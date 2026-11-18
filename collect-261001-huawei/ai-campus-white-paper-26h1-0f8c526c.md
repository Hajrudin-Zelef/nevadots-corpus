Building a Fully Connected,
          Intelligent World
Chief editing organization

Huawei Technologies Co., Ltd.




Participating organizations

Beijing Bridata Technology Co., Ltd.
Beijing Digital Hail Information Technology Co., Ltd.
Chinasoft International Co., Ltd.
Guangdong Flying Enterprise Internet Technology Co., Ltd.
Hangzhou Zhongling Future Technology Co., Ltd.
Hi-Think Technology, Corp.
HMN Smart Co., Ltd.
iSoftStone Smart Technologies Co., Ltd.
Lianfa Group Co., Ltd.
East China Architectural Design & Research Institute Co., Ltd.
Shandong Bittel Intelligent Technology Co., Ltd.
Shanghai Yuankong Automation Technology Co., Ltd.
Shenzhen Webuild Technology Co., Ltd.
SHENZHEN HUAZHAO TECHNOLOGY CO., LTD.
Sichuan Timwave Yunze Intelligent Technology Co., Ltd.
Wanchow Qizhi (Qingdao) Information Technology Co., Ltd.
Zhuhai Xiangyi Aviation Technology Co., Ltd.
Editorial advisors (in alphabetical order)

Chen Zhiwei, Deng Xiao, Dong Shaohua, Gu Rui, Guo Hongfu,
Guo Wanzhou, Han Bing, He Ji, Hu Yanxin, Liu Jiajia, Liu Shuqing,
Liu Song, Liu Xi, Liu Yue, Su Yufeng, Tan Jianxin, Wang Gang,
Wang Jiankang, Wang Jingji, Wang Xiaoan, Wang Zhong,
Wei Zequn, Xiao Guangrui, Xu Bo, Xue Long, Yang Xi, Yi Yangao,
Yu Xunxun, Yue Xuefeng, Zeng Eryang, Zhang Jirong, Zhao Shaoqi,
Zhao Xianghui




Chief editors

Wang Zhiming, Liu Yaoyao, Niu Kun, Zheng Rongjian, Song Junjing,
Zhuang Guanglong, Chen Dan, Jiang Lin, Chen Zhongzhou, Zhang
Jiabao, Xu Yong, Liu Zhipiao, Jiang Yinqing, Pan Feng, Zhang Juan




Contributors (in alphabetical order)

Chen Suixiao, Ge Feng, Guo Jiani, He Wenxi, Hu Xiumin, Lei Shifei,
Li Honggang, Liu Bi, Liu Guanjie, Ouyang Wanru, Sun Shengbai,
Sun Yang, Wang Binbin, Wang Bo, Wang Haijiao, Wang Liangsheng,
Xia Wei, Zhou Yang, Zhu Huizhang
Foreword I




Yang Chaobin

Huawei's Director of the Board and CEO of the ICT Business Group




Since we first defined intelligent campuses alongside our customers, partners, and industry
stakeholders in 2019, Huawei has delivered over 1,500 projects worldwide, driving rapid digital
transformation across various industries.

Today, at a pivotal moment in intelligent industrial transformation, we recognize that AI's true
value emerges from the integration of applications, data, and ICT infrastructure in real-world
settings. Campuses not only provide the physical foundation for these scenarios but also act as
essential testbeds for translating AI potential into tangible industrial advancement.

AI Campus represents a new model for campuses, evolving in alignment with the prevailing trends
of intelligence. Centered on the innovative "intelligent spaces," it combines computing power
and data flows to create an open, collaborative, and co-evolving industrial ecosystem. Working
closely with dozens of leading customers, partners, and research institutions, we have compiled
industry-wide insights into one paper. Drawing on this collective expertise and hands-on campus
experience, we developed the AI Campus concept, defined its core features, and established a
reference architecture and implementation roadmap. This white paper aims to be a practical
guide for industries planning and building intelligent campuses in the AI era; its core value lies in
this blueprint.
Foreword II




Michael Ma

Vice President of Huawei and President of
Huawei's ICT Product Portfolio Mgmt & Solutions Dept




Every advancement in intelligent campuses stems from the integration of technological innovation
and industry practices. Huawei goes beyond offering products and solutions; we drive campus
transformation. Capitalizing on our full-stack cloud, network, edge, and device capabilities, we
collaborate with partners to innovate together and consistently create value for our customers.

AI is now advancing campuses from isolated to all-scenario intelligence. To support this
transition, Huawei has partnered with dozens of customers, partners, and experts from
research institutions to hold more than 20 specialized workshops. Combining industry insights
with Huawei's hands-on experience, we jointly authored the AI Campus white paper. This
paper explores 17 intelligent spaces across 11 business segments, such as intelligent office,
production, intelligent guest rooms, and commercial consumption. It also defines the six
technical features of AI campuses for the first time: HI-AI coexistence, data-AI convergence,
all-domain connectivity, multi-dimensional sensing, resilient security, and eco-sustainability.
Additionally, the white paper outlines the reference architecture of AI campuses and provides
suggestions on integrated planning, construction, and operations.

We recognize that AI evolves rapidly and campus scenarios will continue to change. This paper
summarizes and reviews current technological innovation and industry practices. Looking ahead,
we will remain dedicated to ongoing exploration and reflection and welcome your insights to help
shape the future of AI campuses.
Foreword III




Deng Yiou

Secretary-General of NIDA




Since its establishment, the Network Innovation and Development Alliance (NIDA) has remained
committed to defining the evolution path and network construction standards of global Internet
infrastructure, and driving the upgrade of the data communication industry. As AI deeply
integrates into industries, fixed networks are transitioning from connecting people to connecting
intelligence. Innovative network architectures are now fundamental to maximizing AI's computing
power and enabling intelligent applications.

As core units of urban and industrial development, campuses serve as essential carriers for
translating the value of AI into real-world impact, and as ideal proving grounds for Net5.5G. The
AI Campus white paper outlines future intelligent campus scenarios centered on the concept of
"intelligent spaces." Its key technical features, including HI-AI coexistence, all-domain connectivity,
and multi-dimensional sensing, closely align with NIDA's Net5.5G vision. This alignment offers a
clear architectural blueprint and capability framework for advancing campus networks.

We believe the release of this white paper will strengthen industry consensus and deliver a
definitive roadmap for planning and developing AI campuses. Moving forward, NIDA will continue
to collaborate with industry partners to advance campus networks toward a smarter, greener, and
more sustainable future. Together, we are dedicated to strengthening the network foundation
that supports intelligent upgrades across diverse industries.
CONTENTS



01
Initiating with AI: Driving Forces and the Evolution of AI Campuses                                                                                                                         02

1.1 Four Driving Forces of AI Campuses������������������������������������������������������������������������������������������������������������������������������ 03
1.2 Four Evolution Directions of AI Campuses������������������������������������������������������������������������������������������������������������������ 10
1.3 AI Campus Evolution: From Traditional Campus to AI Campus�������������������������������������������������������������������������� 22




02
Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 26

2.1 Office Campus: AI Enables Higher Quality and Improves Office Experience����������������������������������������������� 27
2.2 Manufacturing Campus: Bringing Intelligence to All Links for Fast and Stable Production������������������� 38
2.3 Industrial Campus: Powering Enterprise Growth and Maturity with Digital and Intelligent
         Technologies�������������������������������������������������������������������������������������������������������������������������������������������������������������������������� 41
2.4 Sports and Exhibition Campus: Activating Venues for an Immersive Experience��������������������������������������� 43
2.5 Hospital Campus: Intelligent Foundation Streamlines Healthcare, Where Every Second Counts�������� 46
2.6 Commercial Campus: Seamless Transactions, Enhanced Immersion��������������������������������������������������������������� 49
2.7 Education Campus: Enlightening Minds with AI to Build Personalized and Immersive Intelligent
         School Campuses����������������������������������������������������������������������������������������������������������������������������������������������������������������� 53
2.8 Community Wellness Campus: Intelligent, Inclusive Care for All Ages���������������������������������������������������������� 55
2.9 Electric Power Campus: Intelligent Electric Power Enables a Zero-Carbon, Safe, and Efficient Power
         Supply System����������������������������������������������������������������������������������������������������������������������������������������������������������������������� 59
2.10 Mining Campus: Intelligent Management to Turn High Risk into High Efficiency������������������������������������ 61
2.11 Research Labs: Intelligent Support for Breakthroughs Without Burnout������������������������������������������������������� 63




03
Building with AI: Key Technical Features of AI Campuses                                                                                                                                     66

3.1 HI-AI Coexistence����������������������������������������������������������������������������������������������������������������������������������������������������������������� 67
3.2 Data-AI Convergence��������������������������������������������������������������������������������������������������������������������������������������������������������� 71
3.3 All-Domain Connectivity��������������������������������������������������������������������������������������������������������������������������������������������������� 78
3.4 Multi-Dimensional Sensing���������������������������������������������������������������������������������������������������������������������������������������������� 82
3.5 Resilient Security������������������������������������������������������������������������������������������������������������������������������������������������������������������ 85
3.6 Eco-Sustainability����������������������������������������������������������������������������������������������������������������������������������������������������������������� 92
04
Architecting with AI: Reference Architecture for AI Campuses                                                                                                                              100

4.1 Agentic Device: Terminal Agents Featuring Autonomous Sensing, Decision, and Evolution���������������102
4.2 Agentic Network: Future-Proof Campus Networks���������������������������������������������������������������������������������������������� 103
4.3 Agentic Platform: Campus Digital Platform Integrating Data, Knowledge, and Capabilities��������������104
4.4 Agentic Applications: Catalyzing Service Innovation and Unlocking the Digital Economy's
        Multiplier Effect����������������������������������������������������������������������������������������������������������������������������������������������������������������� 105
4.5 Agentic Security: Proactive, Intelligent Defense System for Service Continuity and Core Value
        Protection����������������������������������������������������������������������������������������������������������������������������������������������������������������������������� 106
4.6 Agentic O&M: Cross-Domain Full-Stack Convergence, Proactive Service Assurance, and Intelligent
        Fault Self-Healing�������������������������������������������������������������������������������������������������������������������������������������������������������������� 107




05
Advancing with AI: Paths and Evaluation for Building AI Campuses                                                                                                                          110

5.1 AI Campus: Integrated Planning, Construction, and Operations���������������������������������������������������������������������111
5.2 AI Campus Maturity Assessment��������������������������������������������������������������������������������������������������������������������������������� 120




References125

Acronyms or Abbreviations                                                                                                                                                                126
01
01
Initiating with AI:
Driving Forces and the Evolution of AI Campuses
As the digital and physical realms converge at an accelerating pace, the
coming years will witness a transformative leap in the intelligent development
and construction of campuses. This evolution is shaping a clear vision for AI
campuses. This chapter explores four key driving forces and emerging trends
that shape the future of AI campuses. These mutually reinforcing trends form a
core framework for advancing AI campuses toward an intelligent organic entity.




                         02
01 Initiating with AI: Driving Forces and the Evolution of AI Campuses




          1.1 Four Driving Forces of AI Campuses

      1.1.1 Strategic Guidance: The Wave of Digital and Intelligent
      Transformation Propels Campuses to the Forefront of National
      Competitiveness

    Looking across the globe, national strategies for                                         In its AI Action Plan , the US positions AI as the
    digitalization and AI are already being rolled out in                                     core lever for reshaping global technological
    full force. From the US's AI Action Plan to the EU's                                      dominance, driving both technical breakthroughs
    2030 Digital Compass: the European Way for the                                            and industrial implementation. Canada's
    Digital Decade , and from China's "Digital China"                                        Sovereign AI Compute Strategy is built upon a
    initiative to Japan's "Society 5.0" vision, a strategic                                   "sovereign compute + ethical framework," seeking
    race centered on a future of digital and intelligent                                      a balance between deep cultivation in frontier AI
    technologies has fully unfolded. AI has transcended                                       research and the promotion of digital inclusion,
    mere technological advancement to become a core                                           and ensuring that the benefits of technological
    strategic pillar for major powers shaping future                                          progress are broadly shared. The United Kingdom
    productivity. It has emerged as the ultimate arena                                        focuses on the digital bedrock of computing
    where nations compete for industrial dominance                                            infrastructure; its AI Opportunities Action Plan
    and technological sovereignty.                                                            explicitly aims to fortify the foundations of AI
                                                                                              innovation and digital inclusion through large-
    These digital and intelligent strategies                                                  scale capacity expansion. Elsewhere, South Korea
    ultimately require concrete spatial platforms for                                         has moved to safeguard AI development through
    implementation and realization. Campuses can                                              legislation; the introduction of AI Basic Act
    serve as crucial frontlines where these grand                                             has elevated AI to the very core of its national
    visions transition from blueprints to reality.                                            strategy. See figure 1.



                                   Core Policies of AI and Digital Strategies in Major Countries

      Canada: is seeking a strategic balance between           United Kingdom: scales out computing infrastructure         China: promotes AI penetration across all industries,
      pioneering frontier AI research and fostering digital    to fortify the digital bedrock, aiming to catalyze AI       with the "AI+" initiative as the guideline and computing
      inclusion, based on the dual pillars of "sovereign       innovation and realize digital inclusion.                   infrastructure and security governance as the two pillars.
      computing + ethical framework."                          • AI Opportunities Action Plan                              • "AI+" Action Plan
      • Sovereign AI Compute Strategy                          • UK Digital Strategy                                       • 2025 Action Plan for Building a Digital China
      • Digital Ambition




                                                                                                                                               South Korea: firmly cements AI
                                                                                                                                               as a central pillar of its national
                                                                                                                                               competitiveness, based on "AI
                                                                                                                                               legislation + full-stack computing
                                                                                                                                               sovereignty.
                                                                                                                                               • AI Basic Act
      US: positions AI as the core driver                     Saudi Arabia: embraces AI as a                 UAE: invests in its vision        • National AI Initiative
      for reshaping global technological                      driver of national growth, promoting           to become a global hub for
      dominance.                                              the transformation into a global               AI-driven digital economy.
      • AI Action Plan                                        digital computing hub.                         • UAE AI Investment Plan
      • The Digital Solidarity Strategy                       • HUMAIN National AI Company                   • D igital 6G Innovation
                                                                 Program                                        Strategy
                                                              • The Saudi Digital Strategy




                      Figure 1 Summary of global digital and intelligent strategies and policy frameworks


                                                                                       03
                                                                         Initiating with AI: Driving Forces and the Evolution of AI Campuses 01




Amidst the global tide of digital and intelligent                             ecosystem fostering and full-lifecycle security
t ra n s fo r m a t i o n , C h i n a h a s e s t a b l i s h e d a           governance, balancing technological breakthroughs
clear strategic roadmap through the deep                                      and industrial transformation with robust risk
synergy between top-level planning and policy                                 prevention and control.
i m p l e m e n t a t i o n . T h i s a l i g n m e n t p ro v i d e s
definitive guidance and a robust foundation for                               All strategic blueprints for digital and intelligent
the integration of AI across all industry sectors,                            transformation must find their footing in
while offering systematic policy support for the                              concrete practices and real-world applications.
advancement of campus intelligence. The official                              Campuses have evolved beyond mere physical
issuance of the State Council's Opinions on Deeply                            spaces for industry clusters; they are now
Implementing the "AI+" Action Initiative marks                                core vehicles for national AI and digitalization
the elevation of the "AI+" concept to a national                              strategies. They shoulder the direct responsibility for
strategic action. It signifies a critical leap for                            technology transformation, ecosystem cultivation,
China's AI development: transitioning from isolated                           and industrial upgrade, serving as a critical yardstick
technological breakthroughs toward comprehensive,                             for a nation's ability to translate macro blueprints
all-factor empowerment. The policy is characterized                           into improved productivity. The competition in
by three key pillars: first, centering on the "AI+"                           digital and intelligent strategies is increasingly
initiative to drive the shift from isolated applications                      manifesting as a contest of innovation capacity,
to deep penetration across all industries; second,                            resource orchestration, and operational intelligence
deploying intelligent infrastructure and trusted data                         at the campus level. Campuses are being thrust to
spaces with a moderately ahead-of-curve strategy                              the forefront of national competitiveness like never
to strengthen the supply and cross-domain synergy                             before; their development quality and efficiency
of core factors like computing power and data;                                will profoundly influence a nation's standing in the
and third, placing equal emphasis on innovation                               future global landscape.


                                                                         04
01 Initiating with AI: Driving Forces and the Evolution of AI Campuses




      1.1.2 Market-Driven: The Agent Economy Ushers in an Era of Value
      Reshaping and Incremental Gains for Campuses

    History shows that any disruptive technology,                        to curate intelligent space scenarios. Meanwhile,
    following an initial period of excessive hype,                       AI tech firms must extend into tangible campus
    must eventually return to its commercial essence:                    environments to validate their worth by solving
    creating measurable value. As the investment                         specific operational pain points. Ultimately, by
    frenzy cools and the market turns rational, AI                       leveraging a deep understanding of spaces and
    is moving beyond the phase of "technology for                        people, vendors will provide continuous spatial
    technology's sake" and entering a vital period                       services to achieve final value monetization. This
    focused on practical application and ROI delivery.                   transition from "selling products" to "managing
    At this critical turning point, a clear industry                     spaces" and eventually to "providing services" is not
    consensus is emerging: the era of the agent                          just a fundamental business model revolution, but
    economy is approaching. According to the insights                    an inevitable choice for vendors to establish core
    from the 3rd Sequoia Capital's AI Ascent Summit,                     competitiveness in this new era.
    the next wave of AI will not be about selling tools,
    but about selling outcomes.                                          From a user perspective, the evolution of
                                                                         integrated spatial intelligence within campuses
    From a vendor perspective, the competitive                           follows a clear progression: from rationality
    landscape has transformed: It is no longer about                     to sensibility, and from function to value. For
    standalone product performance, but rather                           campus enterprises, the boundaries of demand
    integrated spatial intelligence tailored to campus                   are continuously expanding. While they were
    environments. This race for spatial intelligence                     previously confined to core cost-control needs
    is triggering a profound role shift: hardware                        centered on physical space leasing, the emergence
    vendors, such as smart home companies, must                          of the digital economy as a primary growth engine
    evolve into the architects of spatial value. They are                has fundamentally shifted their priorities. For most
    extending their reach beyond tangible hardware                       enterprises, building in-house AI systems and
    manufacturing into the realm of intelligent services                 computing platforms is cost-prohibitive, and even
    and operations. Their value is no longer confined                    more so for small- and medium-sized enterprises
    to the physical products, but lies in their ability                  (SMEs). Consequently, the digital capabilities




                                                                   05
                                                        Initiating with AI: Driving Forces and the Evolution of AI Campuses 01




provided by campuses have emerged as a core                  shows that 40% of spatial transformation pioneers
business requirement. According to a survey, 65%             have effectively boosted employee satisfaction by
of technology and manufacturing enterprises are              tracking real-time occupancy. Furthermore, the
willing to pay a rental premium of 20% or more               rise of remote and hybrid work has heightened
for space in an AI-powered campus. This means                the demand for spatial flexibility and service
that enterprises are investing in more than just             adaptability, with 76% of Chinese enterprises
square footage; they are securing turnkey digital            willing to invest in AI for their workspaces and
transformation support. The underlying logic                 working styles. According to the core logic behind
of this shift lies in the migration of the value             this change, the emotional value embedded in a
priorities: Enterprise demand has transitioned from          space is now deeply intertwined with productivity.
"renting square footage" to "buying growth," and             When employees work within environments
from "reducing costs and improving efficiencies"             defined by "emotional resonance and adaptive
to "enjoying value-added services." For campus               needs," both work efficiency and retention rates
users, the generational shift in workspaces                  see a boost. Through intelligent capabilities, AI-
combined with the evolution of working styles                powered campuses are uniquely positioned
has elevated demand from basic comfort to                    to fulfill these emotional needs of consumers.
emotional resonance. The redefinition of spatial             Therefore, the ultimate goal of spatial intelligence
value is becoming increasingly evident. Data                 is a return to a human-centric focus.



  1.1.3 Human-Centric: Reconstructing the Essential Connection
  Between People and Spaces in the Intelligent World

AI has emerged as the pivotal force reshaping                at the very heart of intelligent development—that
global competition and driving the evolution                 technology's ultimate mission is not to replace
of both the intelligent economy and intelligent              or dominate humans, but to empower and serve
world. Looking toward 2035, China is poised to               them. It advocates for a humanistic spirit to build a
enter a new era of intelligent economic and social           caring intelligent society defined by the harmonious
development. As the reach of digital and intelligent         coexistence of people, environments, and
technologies extends to every corner of society,             technology. As a core space supporting industrial
there is not only a revolution in efficiency but also        clusters, living communities, and ecological synergy,
an evaluation on value: The future intelligent world         campuses must be firmly anchored to this value.
should not merely be measured by efficiency, but             The intelligent upgrade of a campus must never
should also be characterized by the warmth of                stop at the mere stacking of equipment or the
human connection.                                            flow of data. Instead, it must always take genuine
                                                             human needs as its starting point. In so doing,
From intelligent security and autonomous delivery            intelligent technology becomes a bridge connecting
within the campus to collaborative workspaces and            the space with emotion, and efficiency with care.
operational efficiency for enterprises, technology
is reshaping spatial value and operational logic             "Tech4Good" is not a mere slogan. It is the
at an exponential pace. According to Su Jun from             anchor for spatial value and the promise that
the Tsinghua-Rizhao Joint Research Institute for             every square meter of space serves human
Intelligent Society: "Technology is neutral, and             well-being. Today, "Tech4Good" has become a
algorithms are innocent; human beings must not               global consensus in the technology sector, evolving
become slaves to technology." This assertion strikes         from a grand narrative into the underlying logic



                                                        06
01 Initiating with AI: Driving Forces and the Evolution of AI Campuses




    of technological advancement. David Wang,                            However, while pursuing warmth and
    Executive Director of Huawei, captured the                           efficiency, there must be a clear-eyed
    essence of this consensus with his assertion:                        recognition that AI is a double-edged sword.
    "Technology itself is not the goal; people are." The                 As technology integrates deeply into spatial
    core of innovation—defined by being "inclusive,                      environments, potential risks inevitably follow.
    boundless, and green"—is to realign technology                       From the leakage of personal data to precision
    with genuine human needs. This vision is further                     scams powered by AI, the vulnerabilities of the
    reinforced by Tech Predictions and Future Visions                    digital world are becoming new societal concerns.
    2049 , which states that the ultimate purpose of                     Therefore, constructing a resilient security
    AI advancement is to better fulfill human needs                      space is the indispensable foundation for the
    while enhancing human dignity and well-being.                        harmonious coexistence of people, environment,
    This implies that technological upgrade must                         and technology—a concept embodied by two
    simultaneously embrace flexibility, inclusivity, and                 sides of the same coin: digital and physical
    humanity, ensuring that the warmth of technology                     security. In the digital dimension, campuses
    is accessible to everyone. Consequently, a campus                    establish a digital safety space and enforce end-
    is no longer a mere collection of intelligent                        to-end privacy protection and proactive defense
    spaces, but the critical anchor where these values                   mechanisms, fundamentally addressing the root
    materialize. Its core function is to transform digital               causes of data misuse and network attacks.
    intelligence into perceptible services and tangible                  Simultaneously, the value of security extends into
    warmth within the spatial environment. Here,                         the physical world, where intelligent sensors and
    every square meter of space serves human growth                      AI algorithms provide 24/7 non-intrusive care and
    and well-being, standing as a true paradigm of                       fall detection for the elderly living alone, building
    the harmonious coexistence between technical                         a reliable physical safety net for every individual
    rationality and value rationality.                                   within the campuses.




                                                                   07
                                                         Initiating with AI: Driving Forces and the Evolution of AI Campuses 01




  1.1.4 Technological Leap: Campuses Become the Strategic Nexus for
  AI's Move from Digital to Physical Worlds

Huawei's Intelligent World 2035 report suggests               At its core, an AI campus aims to cultivate a more
that the evolution of thought—progressing from                seamless living and working environment along
experience to science, and ultimately to wisdom—              with a highly efficient operation and maintenance
is the defining thread of human civilization. As              system. The essence lies in leveraging technologies
tools have evolved from abacuses to calculators,              to facilitate deep connectivity and intelligent
computers, the Internet, and now to machine                   collaboration across four campus dimensions:
intelligence, they have consistently shattered the            people, things, spaces, and information.
constraints of biological systems. This evolution is
triggering a leap in creativity and unlocking infinite        There are two main technological leaps that AI
possibilities for human development.                          campuses are undergoing.


Over the next decade, information technology is               Leap 1: AI is entering campuses and permeating
set to undergo a monumental transition.                       various industries, accelerating the convergence
                                                              of the digital and phy sical worlds. The
The advancement of digital and intelligent                    revolutionary breakthrough lies in AI's move
technologies, such as AI and digital twin, has                from the digital realm into the physical
reached a critical inflection point. The core                 world. As Huawei's Intelligent World 2035 report
challenge lies in identifying a core field that can           highlights, artificial general intelligence (AGI) will
harness these disruptive technologies and integrate           be the most transformative driving force of the next
diverse demands, facilitating the pivotal transition          decade. However, significant core challenges must
from technical investment to monetization.                    still be overcome to achieve the AGI singularity.
Campuses are exactly such a field for AI to realize           This singularity will only truly arrive when AI is
this transition.                                              capable of deep reasoning for complex physical
                                                              phenomena, achieving a seamless coupling of
Against this backdrop, "AI" within next-generation            the digital and physical worlds. Professor Fei-Fei
AI campuses should be understood in a broader                 Li, the "Godmother of AI," emphasizes that "the
sense. It represents more than just large language            future of AI lies in evolving beyond the limitations
models and AI agents; it encompasses a cluster of             of pure language models toward a new capability:
advanced digital technologies, including Internet of          understanding the physical world and engaging in
Things (IoT) and digital twin.                                real-world interaction." This breakthrough relies on




     An AI campus is not merely a site where AI technologies are applied. It must be defined
     as an intelligent ecosystem integrated with next-generation information technologies like
     IoT—a new paradigm viewed from the perspective of end users and services.


     Yue Xuefeng, Director of Hi-Think Technology, Corp.




                                                         08
01 Initiating with AI: Driving Forces and the Evolution of AI Campuses



    the synergy of two cutting-edge technologies: world                  with physical campuses: AI is transitioning from a
    models and embodied AI. Campuses—with their                          mere auxiliary tool to an autonomous decision-
    clear physical boundaries, diverse service scenarios,                making agent. Campuses serve as a critical vehicle for
    and quantifiable operational metrics—provide an                      the transition. Campuses will evolve into super agents
    environment for iterative, replicable, and scalable                  that orchestrate multi-agent capabilities, effectively
    digital-physical integration.                                        turning the concepts of "space as a service" and
                                                                         "scenario as a service" into reality. Underpinning
                                                                         this metamorphosis will be the collaborative
          The evolution of embodied AI marks                             empowerment of information technologies. AI agents
          a new transition in social intelligence,                       serve as the core, evolving into the autonomous
          moving from "information connectivity"                         decision-making brain of the super agent. Digital
          to "physical interaction." Its core value                      twins evolve into predictive, dynamic mirrors,
          lies in serving as the critical interface                      providing a high-precision foundation for spatial
          between the physical world and digital                         awareness. IoT terminals, coupled with 6G Wi-
          intelligence, with the potential to                            Fi sensing networks, construct a highly responsive
          systematically reshape the paradigms of                        neural link. Data assetization technologies break
          social production and collaboration.                           down silos, transforming data into a core production
                                                                         factor that is usable, manageable, and value-adding.
          Hangzhou Zhongling Future
          Technology Co., Ltd.                                           By seamlessly interconnecting people, things, and
                                                                         AI entities, campuses function as super agents that
                                                                         operationalize these service models, delivering on
                                                                         the promise of "space and scenario as a service." For
    In the future, embodied AI will be used to interact                  campuses, these technologies form an interlocking
    with physical environments and execute tasks on                      ecosystem that establishes a deeply symbiotic
    campuses, while world models will build a dynamic                    operational foundation for people, machines, and
    digital twin system to map these spaces accurately.                  intelligence. Data assetization ensures the seamless
    This interplay will catalyze a seamless convergence                  flow of information; digital twins and AI agents
    of the physical and digital worlds, helping to realize               dynamically orchestrate resources; and IoT combined
    AGI within real-world campus settings.                               with 6G guarantees instantaneous response.


    Leap 2: AI will transition from "resource-intensive                  The synergy of these four driving forces empowers
    training" to "value creation," empowering                            AI campuses to transcend traditional functional
    diverse industries with campuses acting as the                       boundaries and developmental constraints. Moving
    strategic anchor for this value delivery. As large                   beyond siloed intelligence toward scenario-
    models evolve, AI agents will dominate the future                    wide intelligence, AI campuses will be shaped
    AI narrative. Over the next decade, AI agents will                   into entities that are guided by national strategy,
    improve their understanding of the objective laws                    driven by market demand, rooted in human-
    of the physical world, evolving into action-oriented                 centric values, and underpinned by advanced
    systems capable of autonomous, closed-loop task                      information technologies. As such, AI campuses
    execution. They will advance from mere execution                     will achieve profound integration of strategy, value,
    tools to decision-making partners, ultimately driving                humanity, and technology, ultimately becoming
    an intelligent revolution across all industries. A                   intelligent, organic entities that feature multi-agent
    profound transition is underway as AI starts merging                 collaboration and multi-value coexistence.




                                                                   09
                                                         Initiating with AI: Driving Forces and the Evolution of AI Campuses 01




    1.2 Four Evolution Directions of AI Campuses

  1.2.1 Experience Evolution: Triple-Stage Advancement to Immersion

AI campuses are no longer the simple stacking                 According to Deloitte's Tech Trends 2025 ,
of technologies and tools. Instead, they are more             spatial computing is center stage and triggering
like a user-centric platform for experiences. Their           a d i s r u p t i v e re v o l u t i o n i n i n t e ra c t i o n s . I t
value lies not just in technical sophistication but in        transforms the visualization of ideas and objects,
delivering seamless experiences that truly matter.            shifting from flat 2D planes into dynamic 3D
This trend redefines campus-user relationships by             environments, while integrating voice recognition
advancing across three key dimensions: interaction,           and gesture controls. Its unparalleled ability to
services, and value creation.                                 contextualize data and deliver deeply immersive
                                                              experiences is revolutionizing the way humans
1.2.1.1 Intelligent Upgrade of                                interact with machines.
Interactions
                                                              Embracing this trend, unlocking true campus
Traditional interactions of campus service users              intelligence hinges on the intelligent upgrade of
have long suffered from clunky processes and                  interactions. This upgrade goes beyond adding
fragmented experiences. Users often juggle                    interaction channels. It is more about crafting a
multiple apps, endure repeated verifications,                 smart, intuitive system that deciphers user intent
and navigate complicated operations just for                  and proactively adjusts to individual behaviors by
basic tasks like identity checks, service inquiries,          harmonizing multimodal interaction technologies.
or facility controls. This inefficiency creates               The system anticipates user demands by
unnecessary barriers, widening the gap between                comprehensively analyzing real-time data—voice,
people and their surroundings.                                visual, gestures, and location—delivering solutions




                                                         10
01 Initiating with AI: Driving Forces and the Evolution of AI Campuses



    before user commands arrive and transforming                         Innovative interactions can also be integrated into
    passive interaction into proactive assistance.                       everyday campus life, transforming routine tasks
    This ensures the most natural and seamless                           across multiple domains from managing public
    interaction experiences.                                             facilities to invoking corporate services. Imagine
                                                                         booking a conference room effortlessly through a
    The intelligently upgraded interaction mode                          quick voice prompt, while smart systems prepare
    enables seamless adaptation to every scenario                        the environment ahead of your scheduled time.
    and user, ensuring effortless engagement anytime                     Even public utilities like lighting and climate control
    and anywhere on every interaction interface (see                     intelligently adjust themselves by detecting human
    Figure 2). For example, people unfamiliar with                       activity through advanced visual recognition.
    smart devices or newcomers exploring the campus
    for the first time can initiate intelligent interactions             At its core, the intelligent upgrade of user
    based on their daily habits—there's no need to                       interactions on a campus is not just about
    master complicated steps. Target recognition                         simplifying operations. Rather, it is about blending
    swiftly handles identity checks, while voice prompts                 multimodal technologies with spatial computing
    deliver precise navigation instructions. Every                       to create environments that intuitively grasp
    step flows naturally without manual operations,                      user needs, fostering natural exchanges between
    blending intuitive interactions into the environment.                individuals and their surroundings.




            Engagement Anytime and Anywhere on Every Interaction Interface

                      Voice interaction, visual recognition, gesture control, facial expressions/body movements,
                                             brain-computer interface, and tactile feedback


                                      Multimodal interaction                Seamless operations




                                                                                                                    AI Generated



                                          Figure 2 Intelligent upgrade of interactions




                                                                   11
                                                                    Initiating with AI: Driving Forces and the Evolution of AI Campuses 01




1.2.1.2 Proactive Innovation of Service                                  receive services. Rather, they will become co-
Reach                                                                    creators who shape services and environments.
                                                                         Traditionally, campus services and layouts were
The key direction for campus services is to                              designed with top-down decisions from the
move from responsive demand fulfillment                                  management team, while users merely adapted.
toward proactive value creation. The essence is                          Today's strategies have revolutionized this
to abandon the outdated notion that campus                               model. Real-time user feedback fuels continuous
u s e rs s e e k s e r v i c e s . We c a n e le va t e t h e i r        i m p ro v e m e n t s , e m p o w e r i n g c a m p u s e s t o
experiences—transforming simple satisfaction                             actively shape both service offerings and spatial
into authentic joy.                                                      functionality. Users now co-create the very essence
                                                                         of the campus experience.
Previously, despite relatively swift responses,
services were solely user-driven and providers                           So campuses have evolved beyond service
could only meet requirements passively. But an AI                        providers. They now harness intelligent technologies
campus can use precise data-driven capability to                         to actively engage employees, visitors, and on-site
anticipate needs, crafting an end-to-end service                         enterprises throughout the entire journey, from
cycle—from predictive insights to proactive                              entry to exit. They invite users to fully participate
solutions and exceptional user delight . It                              in the process and dynamically tailor and enhance
redefines convenience by delivering exactly what                         experiences in real time. Campuses and users
users desire, revolutionizing traditional service                        collaboratively craft personalized value that
paradigms.                                                               redefines conventional norms.


The deep integration of technologies like AI                             This pivotal shift in user roles is driven by
agents and big data forms the core technical                             both strategic policy frameworks and robust
foundation for service innovation. AI agents,                            technological infrastructure. On one hand, AI
empowered by autonomous decision-making                                  campuses thrive on harmonizing the demands of
and cross-system collaboration, revolutionize                            diverse stakeholders—such as asset owners, on-
campus ser vices, transforming them from                                 site enterprises, and employees—which is a key
reactive responses to proactive solutions,                               strategy for operational practices. On the other,
woven into every stage of the user journey.                              intelligent technologies, led by AI, streamline this
This proactive innovation of service reach does                          strategy with efficiency. By converting generalized
not only boost operational metrics. It radically                         surveys on requests and satisfaction into precise
redefines the fundamental relationship between                           data insights, these technologies embed a user-
users and campuses.                                                      first philosophy across every phase: planning,
                                                                         operations, and evolution. This shift empowers
1.2.1.3 Collaborative Value Creation on                                  users to transition from recipients to contributors,
the User Journey                                                         ultimately achieving an alignment between
                                                                         campus services and user needs, and fostering
In the future, campus users will no longer just                          collaborative value creation.




                                                                    12
01 Initiating with AI: Driving Forces and the Evolution of AI Campuses




      1.2.2 Space Evolution: Intelligent Spaces with Human-Machine
      Synergy and Dynamic Adaptation

    Campus transformation transcends simple facility                     intelligence core, ultimately forging a synergy
    improvements. It harnesses technologies to redefine                  between humans, intelligent machines, and
    physical design, streamline operational logic, and                   spaces around them.
    amplify operational efficiency, ushering in a new
    age of innovative collaboration.                                     1.2.2.1 Virtual-Reality Spaces with
                                                                         Evolved Forms and Dynamic Twins

          AI technologies' primary mission is                            The form of an AI campus' space is evolving.
          to infuse existing spaces with new                             The limitations of the physical realm are
          meanings, such as enhancing comfort                            shattered, creating a dynamic digital twin. This
          and convenience in residential areas                           is an intelligent space mapping and interacting
          and adapting to varied workplace                               with reality in real time. A digital twin propels the
          demands, from corporate hubs to                                evolution of space through transformative leaps
          collaborative co-working zones, thus                           across three distinct capabilities (see Figure 3).
          creating dynamic and vibrant spaces.
                                                                         • First, all- domain perception and precise
          East China Architectural Design &                                 mapping. IoT and 3D modeling technologies
          Research Institute Co., Ltd.                                      enable real-time digitization of every
                                                                            individual, object, field, and event across
                                                                            t h e c a m p u s , c re a t i n g a d y n a m i c , e v e r-
                                                                            updating digital twin synced with the physical
    This evolutionary trend relies on digital twins                         environment. This unveils the underlying
    as the foundational technology and AI as the                            operational mechanics, precisely pinpointing



                 Virtual-Reality Spaces with Evolved Forms and Dynamic Twins
                                      Real-Time Mapping and Dynamic Interaction


                All-domain perception and precise          Simulation and intelligent               Instruction delivery and
                            mapping                                deduction                           closed-loop control




                                                                                                                           AI Generated



                    Figure 3 Three capabilities for the transition from the real space to virtual space


                                                                    13
                                                                           Initiating with AI: Driving Forces and the Evolution of AI Campuses 01




  resource waste, workflow blockages, and                                       Digital twins bridge the virtual and physical
  lurking hazards, offering crystal-clear direction                             realms, revolutionizing not just technologies but
  for future enhancements.                                                      fundamentally redefining the value of campus
                                                                                spaces. These dynamic replicas offer awareness,
• Second, simulation and intelligent deduction.                                 simulation, and control capabilities. Campus
  A f f o r d a b l e , r i s k- f r e e s i m u l a t i o n s , l i k e        managers can conduct extensive simulations in
  crowd movement modeling, energy usage                                         the virtual space, find ideal solutions, and then
  a s s e s s m e n t s , a n d e m e rg e n c y re s p o n s e                 implement them in the real world. This is both
  rehearsals, are conducted in the digital space.                               a defining characteristic of AI campuses and the
                                                                                cornerstone for achieving efficient, agile, and
• Third, instruction delivery and closed-loop                                   intelligent operations.
  control. Optimization strategies crafted in
  the digital space, like fine-tuning devices                                   1.2.2.2 Upgraded Collaboration
  or planning routes, enable precise control                                    Paradigm with Human-Machine Synergy
  over physical systems, creating a seamless
  bidirectional loop for instant status updates                                 AI campuses represent a profound
  and precise command delivery. Consequently,                                   advancement in the collaboration paradigm
  the space is capable of self-adaptation and                                   between humans, machines, AI agents, and
  self-optimization.                                                            spaces. In this paradigm, humans are the main
                                                                                requesters, with machines serving as efficient
This evolution of space forms emerges from                                      executors, while AI agents act as central hubs for
strategic policies and innovative technical                                     collaborative decisions. Spaces are the foundation,
advancements. Digital twins now stand at the                                    carrying all these elements to craft a harmonious
forefront of the strategic layout of major global                               human-machine-thing system (see Figure 4.)
economies. Initiatives like the EU's Destination
Earth program and the US's technology innovation                                In terms of concepts and practices, cutting-edge
legislation underscore the pivotal role of digital                              design ideas—such as creating robot-inclusive
t wins. Moreover, the introduction of ISO/                                      environments—craft blueprints that foster profound
IEC international standards on the concepts,                                    human-machine synergy, while considering spaces
terminology, and use cases of digital twins has                                 as the cornerstone. At the technical level, the
harmonized global technical frameworks, offering                                progress and market integration of embodied
clear guidelines for integrating virtual and                                    intelligence provide a tangible route to bring these
physical realms in campus-level applications. On                                visionary plans to life.
the technical front, the transformative power of
digital twins has been rigorously proven across                                 As AI expert Fei-Fei Li introduced the concept of
diverse campus environments. Within smart                                       "robot as a service," embodied robots equipped
manufacturing hubs, these systems mirror entire                                 with multimodal perception and 3D spatial
production lines in real time, enabling location of                             interaction capabilities are initially deployed in
equipment exceptions and simulated debugging                                    campus security inspections and facility O&M. This
through virtual interfaces. Practices have proven                               deployment marks a significant shift from human-
that this approach can reduce the troubleshooting                               operated machines to intelligent human-machine
and downtime of key devices by more than                                        collaboration (see Figure 4).
40%, significantly improving asset utilization and
production continuity.




                                                                           14
01 Initiating with AI: Driving Forces and the Evolution of AI Campuses




              Upgraded Collaboration Paradigm with Human-Machine Synergy


              Human (main requester,              Machine             AI agent (multi-       Space (carrying all elements in a harmonious
           AI manager, and collaborator)    (efficient executor)   AI agent collaborator)           human-machine-thing system)




                                                                                                                                 AI Generated



                               Figure 4 Collaboration paradigm with human-machine synergy



    1.2.2.3 Cognition Leap with Organic-                                   Unlocking the organic-like characteristics
    Like Characteristics                                                   hinges on AI's shift from mere perception
                                                                           to true cognition—a leap toward actively
    The ultimate goal of space evolution is to endow                       comprehending the physical world. While
    campuses with organic-like characteristics                             mainstream AI excels at recognizing images, voices,
    encompassing perception, thinking, decision-                           and text, it falls short in deeply understanding and
    making, and action. This transformation thrives                        modeling the causal relationships and evolving
    on key driving elements. One is the deployment                         dynamics within its surroundings. Cognitive
    of pervasive IoT devices as perception nodes that                      intelligence, representing an advanced stage
    continuously capture the intricate states of the                       of AI development, requires systems to master
    physical reality. A multi-AI agent collaboration                       comprehension, reasoning, foresight, and planning.
    system, grounded in world models, handles                              The key lies in the creation of an internal world
    cognition and decision-making, enabling a                              model that mirrors the operational mechanisms of
    profound understanding of the environment, causal                      the real world.
    analysis, and predictive foresight. The intelligent
    platforms function as the operational hub for the                      As Yann LeCun, a Turing Award winner, pointed out,
    integration of virtual and physical realms, offering a                 AI's true evolutionary path lies not in scaling up
    cohesive time-space framework and a simulation/                        data processing but in mastering the world model
    validation environment for perception, cognition,                      to predict environmental shifts and behavioral
    and action. Based on all of these elements, diverse                    outcomes and enable autonomous adaptation in
    multi-AI agent systems can work together as                            ever-changing, open environments. AI campuses
    adaptable execution units, facilitating autonomous                     exemplify this vision. They can serve as a real-world
    operations and feedback across various scenarios,                      training field for continuously refining cognitive
    including office, production, and daily life.                          intelligence models, calling on diverse physical




                                                                     15
                                                         Initiating with AI: Driving Forces and the Evolution of AI Campuses 01




spaces, intricate device interactions, and highly             pinpoint equipment anomalies, trace fault origins,
dynamic activities. These refined models, in turn,            predict their progression, and significantly minimize
offer a more robust and future-ready foundation               downtime risks. Additionally, campus operations can
for intelligent collaboration within the campuses.            optimize resource allocation dynamically, adapting
                                                              to production phases, workforce movements, and
This positive cycle—from data training and                    energy demands through advanced insights.
model evolution to service optimization—enables
campuses to transcend rule-bound responses.                   Ultimately, AI campuses are no longer managed
Instead, they continuously improve through                    spaces. They evolve into intelligent organic entities,
ongoing interactions with the environment, just               capable of perception, thinking, decision-making,
like an organism. For instance, by using cognitive            and action, as well as self-optimization and self-
intelligence for predictive maintenance, systems can          evolution.




  1.2.3 Operations Evolution: A Shift Toward Intelligent Collaboration,
  Security, and Sustainability

The reconstruction brought by AI campuses                     Operations entities are tailored for campuses.
transcends the single operations logic of space               The operations structure of AI campuses is
utilization and basic services that can be still found        a new collaborative ecosystem of "people +
in conventional campuses. As AI technologies                  agents + machines," fundamentally redefining
penetrate the operations model, campuses                      roles and value.
are evolving from reactive management to
proactive operations, from mere spaces to value-              • People can be relieved of mundane tasks and
driven ecosystems. The evolution is not merely                   concentrate on planning strategies, handling
a technological one. It is a full rebuild of the                 complex issues, making rules, and offering
operations logic and structure, as well as the                   ethical oversight. This allows for people to
value system. It is an opportunity to inject new                 provide clearer directions and foster innovation
momentum into the development of campuses.                       in operations.


1.2.3.1 Operations Innovation:                                • AI agents serve as a super-intelligent campus
Proactive, Autonomous, and Intelligent                           s u p p o r t , a n d o f fe r a 1 - t o - N e x e c u t i o n
                                                                 architecture that adheres to emerging protocols
An AI campus brings a great transformation to                    like MCP, A2A, and A2UI. Using data processing
the operations system. It uses AI technologies to                and autonomous decision-making, they can
swap traditional human-centered operations with                  manage systematic tasks (such as requirement
an AI-powered, proactive, autonomous alternative.                analysis and process scheduling), decompose
This will break through the entity boundaries and                tasks, make decisions, and collaborate with
model limitations of conventional operations,                    other agents to execute and complete tasks.
achieving a leap in both operational efficiency and              This makes them the intelligent connectors and
service experiences.                                             optimization engines for operations.




                                                         16
01 Initiating with AI: Driving Forces and the Evolution of AI Campuses



    • Machines serve as the physical counterparts                               1.2.3.2 Green Transformation:
       t o t h e a ge n t s. Th e y ac c u rate l y e xe c u t e                A Digital-Driven, Low-Carbon, and
       standardized tasks—such as inspection, control,                          Resilient Development System
       and distribution—assigned by super-intelligent
       campus agents, with a millisecond-level response                         As global climate governance continues to
       speed, and millimeter-level precision.                                   advance, sustainable development has become the
                                                                                consensus among the international community.
    The reconstruction removes the constraints of the                           More than 130 countries have explicitly set carbon
    conventional human-based operations model.                                  neutrality goals. For example, the EU has released
    It also frees operations processes from the                                 The Green Deal Industrial Plan , and China has
    dependency on individual experiences, achieving                             unveiled a series of policies on carbon peaking
    m o re e f f i c i e n t , p re c i s e , a n d a u t o n o m o u s         and carbon neutrality. These policies are changing
    workflow s and intelligent optimization to                                  low-carbon development from a mere concept
    modernize campus operations.                                                to a systematic practice, providing clear guidance
                                                                                for green transformation in campuses. Green
    O p e ra t i o n s a r e b e c o m i n g p r o a c t i v e a n d            transformation in campuses not only reduces
    autonomous. Driven by multiple collaborative                                costs, but also serves as a strategic choice to
    e n t i t i e s , t h e c a m p u s o p e ra t i o n s m o d e l i s        enhance competitiveness. Its objective is to
    changing from the traditional "passive and                                  build a sustainable development system that is
    reactive" to autonomous "proactive prediction                               green, low-carbon, and highly resilient.
    and autonomous decision-making." As this
    model shifts and the super agents and machine                               The construction of this system focuses on two
    capabilities for campuses evolve, a disruptive                              digital-driven pillars. The first is the green energy
    management model will emerge.                                               structure, which integrates clean energy and new
                                                                                infrastructure such as distributed PV, energy storage,
    The goal of operations innovation is to power the                           and intelligent micro-grids. This helps optimize the
    transformation of both HI-AI collaboration and                              energy supply from the source, reduce the carbon
    proactive autonomy, using AI technologies.                                  footprint, and fundamentally optimize the energy
                                                                                structure. The second pillar is intelligent energy
                                                                                management. The IoT sensing function and the
          The one person department (OPD) or                                    digital twin platform are used to perform panoramic
          one person campus (OPC) model will                                    monitoring, dynamics analysis, and precise control
          emerge as the capabilities of super                                   of energy consumption. AI algorithms are used to
          agents keep improving. In this manner,                                predict loads, intelligently schedule energy, and
          one person can collaborate with the                                   continuously optimize system efficiency. This drives
          agent to manage a department or                                       energy conservation to evolve toward a refined,
          operate a campus.                                                     digital process focusing on continuous optimization.


          HMN Smart Co., Ltd.                                                   I n t h i s m a n n e r, t h e o u t c o m e s o f g r e e n
                                                                                transformation go far beyond energy conservation.
                                                                                As energy conservation technologies keep
                                                                                penetrating and green scenario models—




                                                                           17
                                                           Initiating with AI: Driving Forces and the Evolution of AI Campuses 01




s u c h a s n e t-z e ro a n d re c yc l e m o d e l s —        governance, and emphasizes the creation of a
keep developing, campuses can transform                         dynamic, coordinated governance framework
environmental performance into new                              through legislation and standards. This consensus
measurable, operable value assets. Digital                      profoundly reveals the intrinsic logic of security
technologies are driving the green transition                   governance in the AI era. Security is not the
of the energy structure and the intelligence of                 opposite of innovation or an afterthought, but
energy management. This allows campuses to be                   must be systematically embedded as an intrinsic
transformed into a composite asset that produces                element throughout the lifecycle of AI campus
both ecological benefits and economic value.                    planning, construction, and operations.


1.2.3.3 All-Domain Security: Proactive,
Collaborative Security Governance                                   Low-altitude security is becoming
                                                                    crucial for physical space security,
The security paradigm for campuses is undergoing                    as low-altitude economy develops
fundamental reshaping against a backdrop of                         in recent years. In this vein, this
AI enablement. The focus is on shifting from                        co m p a n y u s e s m u l t i m o d a l , l ow-
passive responses and isolated protection to                        alt itude sensing, along with the
a comprehensive security assurance system                           intelligent analysis of low-altitude
featuring all- domain sensing, proact ive                           activity data to predict and evaluate
defense, and intelligent collaboration. This                        risks in low-altitude campus spaces.
system integrates physical space security, data                     This will become an indispensable
security compliance, and intelligent application risk               part of the intelligent security
prevention and control. It aims to transform security               protection foundation of AI campuses.
from a supporting expense to a core capability that
supports the move toward autonomous campuses                        Sichuan Timwave Yunze Intelligent
in the future.                                                      Technology Co., Ltd.


There is a global consensus that AI security
governance should balance development and
security. In the future, the security governance will           In conclusion, the security governance system of
become crucial for campuses. The EU's Artificial                AI campuses is an all-domain security assurance
Intelligence Act, as the world's first comprehensive            system that can proactively detect risks, analyze
AI regulatory framework, defines the security                   security threats, and collaboratively respond to
baseline of intelligent systems through a risk-                 them, according to the integration of regulations,
based classification framework. Secure A.I. Act                 technologies, and operations. It is usually built
of 2024 released by the United States focuses                   under the guidance of national policies and the
on AI security vulnerability tracking and risk                  support of cutting-edge technology frameworks. It
handling, and strengthens the full-process control              is not only a fundamental for campus operations,
of technology implementation. Opinions of the                   but also the driving force for high- quality,
State Council on Deepening the Implementation                   sustainable campus development. It inserts stability
of the "Artificial Intelligence+" Initiative released           into the evolution of AI campuses.
by China outlines a mutually reinforcing approach
across development, application, safety, and




                                                           18
01 Initiating with AI: Driving Forces and the Evolution of AI Campuses




      1.2.4 Foundation Evolution: Intelligent Foundation with All-Domain
      Connectivity and Adaptability

    The intelligent foundation provides technical                        and collaborative response, driving intelligent
    support needed to implement AI campuses across                       scheduling of resources across all domains, multi-
    intelligence scenarios. It not only meets the core                   scenario collaboration, and autonomous decision-
    requirements of intelligent campus operations,                       making across the entire chain. This enables
    but also reserves technical upgrade interfaces in                    closed-loop transformation from data to intelligent
    advance. It provides solid technical support for the                 services. By combining data and intelligence, the
    long-term evolution of AI campuses and drives the                    digital platform for campuses not only transforms
    continuous upgrade of campus intelligence.                           data into a core production factor, but also
                                                                         gradually breaks through physical boundaries
    1.2.4.1 Evolution from the Digital                                   to achieve full interconnection and intelligent
    Platform to the Intelligent One                                      interactions between people, machines, things, and
                                                                         environments. In the future, it will evolve into an
    The Internet of everything (IoE) is connecting                       organic entity capable of self-sensing, optimizing,
    people, machines, and things in campuses,                            and updating, heralding a new era of intelligent
    achieving extensive connectivity across all domains.                 connectivity between everything.
    However, this achievement only represents a
    breakthrough in connectivity, and its core value is                  The platform focus is shifting from device
    not fully unleashed. Specifically, massive data is                   connectivity to AI-driven connectivity across
    aggregated, but there are still data silos between                   all domains. In conventional campuses, device
    systems due to the lack of effective data integration                connections exhibit single-point access and
    mechanisms and intelligent analysis capabilities. AI                 decentralized management characteristics.
    campus, a leap toward the intelligent connectivity                   Therefore, only basic data can be collected, and
    of everything, focuses on AI penetration and data                    intelligent analysis and cross-scenario collaboration
    integration to bring intelligence to connectivity                    are unavailable. After AI Campus is introduced,
    and create value from data. This transformation is                   there is a foundation for intelligent connectivity
    reshaping the technical foundation of campuses                       across all domains. This solution, centered on the
    from the following two perspectives, under the                       AI model, streamlines the entire process from
    guidance of policies and industries:                                 connectivity to analysis, decision-making, and
                                                                         execution, achieving complete, intelligent closed-
    With the intelligent platform as the core,                           loop management.
    campuses are evolving from information silos
    to spatial agents. The platform serves as a                          In essence, the technological leap toward intelligent
    unified digital foundation and data governance                       connectivity is the systematic reconstruction of
    system, integrating various service systems and                      the campus infrastructure using AI and data
    consolidating data across all domains. It also                       technologies. Driven by industry practices and
    ensures secure data circulation and assetization                     national policies, the reconstruction enables
    using data cleaning, anonymization, and cross-                       campuses to evolve from passive data collectors
    domain collaboration, providing robust data                          to proactive value creators. It provides ongoing
    support for campuses. Additionally, the platform                     technological momentum for intelligence to be
    integrates intelligent analysis, model scheduling,                   implemented across all scenarios.




                                                                   19
                                                      Initiating with AI: Driving Forces and the Evolution of AI Campuses 01




1.2.4.2 Evolution from Single Connection                   personnel dynamics analysis, perimeter security
to the Intelligent Neural Network                          monitoring, and refined energy management,
                                                           unleashing the potential of digitalization. The
The network architecture of AI campuses is bringing        last stage is to load industry-specific apps based
a profound transformation in three progressive             on open built-in containers. By doing so, the
stages. The first stage is to transcend single             network can leverage AI analysis to provide
connections and achieve a leap toward multi-               different intelligent space control capabilities in
dimensional sensing. The conventional network              different scenarios, such as conference rooms,
is merely data transmission pipes, whereas the             hotels, hospital ward wellness centers, and stores.
intelligent network integrates various sensors to          In this manner, the network can offer a complete
form a neural network across campuses. This                security protection system covering multiple
network can track multi-dimensional physical               aspects, such as assets, transmission, space, and
information such as the temperature, personal              privacy. This allows the network to build a future-
dynamics, and device status in real time. This             oriented foundation of intelligent connectivity for
provides situational sensing data for upper-layer          customers in various industries.
intelligent applications.
                                                           The evolution ultimately helps provide an intelligent
The second stage is continuous network                     network system with the capacity for sensing,
evolution. On one hand, the network can offer              decision-making, and execution. It is not only
high-speed connections, based on technologies              a high-speed channel for information flows in
such as Wi-Fi 7 and 50G PON, to allow high-                campuses, but also the hub that drives autonomous
speed, stable connections in all scenarios. On the         campus operations and virtual-physical symbiosis.
other hand, the network can integrate IoT and              T h i s l a y s a s o l i d n e t w o r k fo u n d a t i o n fo r
sensing functions to support spatial positioning,          implementing AI campuses across scenarios.




                                                      20
01 Initiating with AI: Driving Forces and the Evolution of AI Campuses



    1.2.4.3 Evolution from Single Terminal                               reach 90% by 2030. In this vein, embodied agents
    Access to Intelligent Terminal Access                                represented by intelligent robots and drone swarms
    Across Domains                                                       are breaking through the limitations of traditional
                                                                         fixed sensors, and are being scaled up in the
    The implementation of AI campuses is extending                       campuses of multiple domains. They are no longer
    from cloud-based decision-making to precise                          isolated devices, but intelligent units that can move
    execution in the physical world. The key to this                     autonomously and work collaboratively. They are
    is the fast adoption of new intelligent terminals.                   the physical counterparts that allow AI to execute
    This shows that the agents for campuses are                          complex tasks in the physical world.
    evolving from disembodied intelligence to
    embodied intelligence. This, in turn, provides                       The large-scale application of embodied
    unprecedented physical interaction possibilities                     intelligent robots and drones will fundamentally
    for smart applications.                                              reshape the campus operations model and
                                                                         enhance service experiences through all-
    Globally, the large-scale deployment of new                          scenario penetration and efficient collaboration.
    intelligent terminals and the implementation of                      In industrial manufacturing scenarios, embodied
    embodied intelligence technologies are forming                       intelligent robots can penetrate production lines to
    a clear policy consensus and industry trend. This                    do high-precision assembly, inspection, and material
    permits AI campuses to move from cloud-based                         transportation for flexible, automated production
    decision-making to precise execution in the physical                 processes. In security inspection scenarios, drone
    world, opening up a broad application space.                         swarms can build an all-weather, full-coverage,
    For example, Apply AI Strategy released by the                       multi-layered inspection network to automatically
    EU clearly promotes the in-depth integration of                      identify unexpected events and respond to them
    intelligent terminals with AI technologies for the                   quickly. In commercial services and logistics
    intelligent upgrade of industry scenarios. Winning                   distribution scenarios, autonomous, unmanned
    the AI Race: America's AI Action Plan released by                    logistics robots can provide precise guidance and
    the United States specifies intelligent infrastructure               distribution services, dramatically improving user
    deployment as a core pillar and also strengthens                     experiences and operational efficiency.
    the research, development, and implementation of
    embodied intelligent terminals. Singapore's Smart                    In conclusion, new intelligent terminals, as the
    Nation 2.0 sets the Punggol Digital District as a                    physical extension of the intelligent foundation,
    demonstration for the large-scale deployment of                      transform the analysis and decision-making
    devices such as smart robots and autonomous                          of cloud AI into precise, efficient actions in the
    driving terminals, aiming to enable intelligent                      physical world. Through the extensive deployment
    interactions in all domains. Opinions of the State                   of embodied agents, AI campuses can help build
    Council on Deepening the Implementation of                           an organic whole that features virtual-physical
    the "Artificial Intelligence+" Initiative released by                coexistence and dynamic collaboration. It can truly
    China states that the coverage rate of AI agents                     extend intelligent capabilities to every corner of
    and intelligent terminals in each industry should                    campuses.




                                                                   21
                                                                                          Initiating with AI: Driving Forces and the Evolution of AI Campuses 01




         1.3 AI Campus Evolution: From Traditional Campus
             to AI Campus
The evolution to AI campuses represents a shift                                                  1.3.1 Traditional campus:
from physical spaces to intelligent, self-evolving                                               Campus informatization
entities. Traditional campuses evolve toward
Intelligent Campus 1.0 and then Intelligent                                                    Core features: System and service silos and
Campus 2.0, with each stage exhibiting distinct                                                inefficient operations. In this phase, the primary
generational differences in core positioning,                                                  focus is on building the ICT infrastructure. Various
technological support, service models, and value                                               extra-low voltage (ELV) subsystems (such as
logic (see Figure 5).                                                                          security, access control, and energy consumption)



                                                                                                                                      Intelligent campus 2.0
                                                                                                 Leapfrogging                               (AI campus)

                                                  Leapfrogging   Intelligent campus 1.0                                              HI-AI coexistence       Easy-to-access services

  Traditional campus                                                                                  Four
                                                                                                   evolution
                                                                                                                             Proactive intelligent
                                                                                                                                   services
                                                                                                                                                        Twin-based
                                                                                                                                                                                Multimodal natural
                                                                                                                                                                                    interaction
                                                                                                                                                     intelligent space
                                                                                                   directions:
                                                   Platform          Campus application            Experience                         Data-AI convergence                Platform-driven
                                                                                                     Space
                BMS/IBMS                                                                           Operations
                                                                                                                                         Data         Intelligent         Flexible intelligent
                                                                                                                                      convergence     autonomy                computing
                                                                                                                                                                                                     Intelligent




                                                                                                   Foundation
                                                                                                                 Resilient
                                                                                                                 security




                                                                                                                                                                                                        O&M




                                                                   Campus digital platform
                                                                                                                                      All-domain connectivity               Wi-Fi sensing
                                                                                                                                       Connecting    Connecting              Connecting
                                 Access control
  Environment




                                                                                                                                         people                              intelligence
   monitoring




                                                                                                                                                       things
                                                                    Campus connectivity
                Lighting


                           A/C




                                                                                                                                Multi-dimensional sensing            HarmonyOS Connect
                                                                       Campus device                                              Sensing terminal
                                                                                                                                                       Facility
                                                                                                                                                                         Intelligent terminal




 Single-system autonomy                                            Platforms + Services                               Intelligent space and proactive services




                                                                   Figure 5 Evolution to AI campuses


                                                                                          22
01 Initiating with AI: Driving Forces and the Evolution of AI Campuses



    are built and managed independently, achieving                       it comprehensively captures the changing needs
    intelligent applications in specific scenarios, such                 of people, things, and environments. Digital twin
    as target recognition–based access control and                       technology synchronizes the virtual and physical
    video security. However, as these systems are highly                 spaces, providing real-time status updates. Then,
    siloed, the overall campus operations rely heavily                   multiple AI agents collaborate to match these
    on manual coordination, limiting efficiency and                      needs and make scenario-specific decisions,
    user experience. Currently, most campuses in China                   fundamentally transforming the nature of the
    remain in this phase.                                                space. Instead of being a passive physical carrier
                                                                         for services, the campus becomes an intelligent
                                                                         space that can proactively sense requirements and

      1.3.2 Intelligent Campus 1.0:                                      adapt to scenarios, offering immersive interactive

      Campus digitalization                                              experiences and proactive services for people.


    Core features: Unified platform management,                          The level of intelligence in AI campuses is a
    data convergence, and scenario-based                                 gradual evolution process. To better define the level
    collaboration. This phase marks an architectural                     of intelligence in AI campuses, intelligence is divided
    transformation from a collection of isolated systems                 into five levels based on the role and autonomy of
    to platform enablement. The core idea is to build                    AI in campuses. These levels progress from low to
    a unified digital campus platform, which integrates                  high, forming a clear hierarchy (see Figure 6):
    information technology (IT) and operational
    technology (OT) data to visualize, manage, and                       Function level: An auxiliary tool and basic
    control campus services. At the network layer, this                  execution. AI is only used as a passive tool to
    involves the convergence of multiple networks                        execute single instructions. At the sensing level, the
    into one. In terms of value, this phase initially                    tool collects data only in the preset dimension, and
    dismantles data silos, enabling cross-scenario                       triggers a single preset action at the action level. It
    collaborative services (such as all-in-one card-based                cannot handle complex or dynamic scenarios outside
    collaboration). It also begins to explore the value of               the scope of the rules, and manual intervention is
    data assets to support lean operations.                              required to handle exceptions. For example, in a
                                                                         security scenario, AI only performs basic judgments
                                                                         on the collected data and triggers a single alarm

      1.3.3 Intelligent Campus 2.0                                       action, but cannot distinguish risk levels or

      (AI campus): Intelligent                                           automatically associate with other security devices.

      upgrade of campuses
                                                                         Task level: Task execution and inference
    Core features: HI-AI coexistence, all-domain                         assistance. AI can complete single closed-loop
    connectivity, resilient security, and eco-                           tasks with the basic sensing, action, and inference
    sustainability. This phase marks a paradigm shift                    capability. AI supports the deployment of a single
    from platform enablement to intelligent autonomy,                    basic scenario in the campus, forming a fixed closed
    where the campus evolves into an intelligent, self-                  loop of human instruction–AI execution. AI does not
    evolving entity that can sense, decide, and grow.                    have the permission to make autonomous decisions
    Its core features are enabled by technologies such                   beyond the task scope. For example, in energy
    as Wi-Fi 7, multimodal sensing, and digital twins.                   consumption management, AI can independently
    Through intelligent terminals and other devices,                     execute the instruction of turning on the air




                                                                   23
                                                                                          Initiating with AI: Driving Forces and the Evolution of AI Campuses 01




conditioner when the temperature exceeds the                                                     and assist humans in making key decisions.
threshold, but cannot optimize the overall policy.                                               For example, in energy efficiency optimization,
                                                                                                 AI can generate energy saving solutions based
Collaborative level: Collaborative autonomy                                                      on all-domain energy consumption data, assist
and agent response. AI can independently lead                                                    managers in decision-making, and dynamically
the execution of actions in scenario-specific                                                    optimize the effect.
operational processes. It can also independently
memorize and reflect on these operations, and                                                    Wisdom level–autonomous intelligence: All-
collaborate with humans to complete complex                                                      domain autonomy. AI can autonomously iterate
tasks. Its capabilities cover sensing, action,                                                   through all intelligent scenarios within the campus,
inference & decision-making, and memory                                                          achieving autonomous operations in all domains.
& reflection. For example, in warehousing                                                        In addition, AI can execute complex organizational
management, AI can independently execute the                                                     tasks and collaborate with other agents to support
inbound and outbound processes, but warnings                                                     the evolution of the operations system to adapt to
such as overstock need to be manually confirmed.                                                 internal and external changes of the campus. For
                                                                                                 example, in integrated operations, AI can schedule
Advisory level: Expert service and proactive                                                     resources across departments, collaborate with
prediction. AI provides expert-level solutions                                                   multiple agents to process complex tasks, and
and assists in dynamically optimizing the                                                        continuously iterate the operational logic of each
implementation of solutions. AI can independently                                                scenario, achieving autonomous evolution of the
learn and iteratively optimize the solutions,                                                    entire campus.



                   Function level -                    Task level -                 Collaborative level -         Advisory level - Expert                Wisdom level -
                    Auxiliary tool                   Task execution               Collaborative autonomy                 services                    Autonomous intelligence


                     Key features                      Key features                      Key features                    Key features                       Key features
               AI functions merely as a rule-       AI can independently            AI can independently lead         AI has become the                AI autonomously iterates
                based support tool, limited      complete single tasks from        the execution of actions in      core of refined campus                scenarios within the
               to passive execution of single     end to end and has basic        scenario-specific operational      operations. It suggests         campus, achieves all-domain
                tasks without independent          inference and problem-            processes, and campus        expert-level solutions based         autonomous operations,
                decision-making capability.          solving capabilities.          personnel collaboratively       on all-domain data and              and executes complex
                                                                                  handle warnings and to-be-       dynamically optimizes the          organizational tasks while
                                                                                   confirmed items generated        implementation effect.             collaborating with other
                                                                                              by AI.                                                            agents.



                      Capabilities                      Capabilities                     Capabilities                    Capabilities                       Capabilities
                  Instructions need to be       AI deployment in single, basic       AI leads the operational         AI delivers expert-level       AI has global cognition and
               triggered manually or under       scenarios within the campus      processes in regular campus     solutions and autonomously         decision-making capabilities,
               supervision from end to end      supports the basic instruction-     scenarios, autonomously       learns and generalizes them,          just like an entity that
               within the campus. AI cannot         execution cycle. AI can        memorizes and reflects on       assisting humans in making         continuously evolves and
                 autonomously cope with          autonomously complete the        the actions, and collaborates            key decisions.              operates autonomously.
                scenarios beyond the rules.      entire process for a specified     with humans to complete
                                                        single scenario.                  complex tasks.


                 Perception     Basic actions      Perception         Actions       Perception       Actions        Perception         Actions         Perception         Actions


                                                                                   Inference and     Memory        Inference and       Memory         Inference and       Memory
                                                         Basic inference
                                                                                  decision-making   reflection    decision-making     reflection     decision-making     reflection

                                                                                                                   Autonomous       Generalization    Autonomous       Generalization
                                                                                                                     learning         capability        learning         capability

                                                                                                                                                        Personal       Collaboration
                                                                                                                                                        emotions         behavior



Intelligence
    level
                     Basic execution                Inference assistance                Agent response                Proactive prediction              All-domain autonomy




                                                                Figure 6 Intelligence level of AI campuses




                                                                                         24
25
02
Innovating with AI:
Faster Evolution Toward Intelligent Spaces for
AI Campuses
As digital technologies are deeply integrated with the real economy, they are
profoundly reshaping the way we work and live on campuses around the
world. In the future, campuses will no longer be merely a physical collection of
buildings and facilities. Instead, they will become self-evolving entities composed
of numerous intelligent spaces that are closely linked. Each of these entities can
sense, think, grow, and collaborate.




                          26
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses



    We believe that the key to building an AI                          energy. It also details the intelligent production
    campus is to translate the macro vision into                       space that drives physical manufacturing, the
    the construction of specific intelligent spaces.                   industry service space that empowers the industry
    Focused on this evolution blueprint, this chapter                  ecosystem, and the exhibition, healthcare,
    describes 17 intelligent spaces across 11 types                    commerce, education, and community intelligent
    of campus businesses.                                              spaces that enhance humanistic experience.
                                                                       These spaces cover 23 typical scenarios.
    These spaces range from the intelligent office
    space that stimulates individual creativity to the                 Together, these intelligent spaces herald the
    operations assurance space that ensures stable                     arrival of a new era. The value of campuses is
    campus running across all domains, and from the                    shifting from providing basic physical venues
    security space that safeguards both physical and                   to offering intelligent spaces that deeply adapt
    digital security to the transportation and parking                 to scenarios, continuously enable growth, and
    space that redefines the logic of mobility and                     proactively create value.




         2.1 Office Campus: AI Enables Higher Quality
         and Improves Office Experience

      2.1.1 Intelligent Office Space: From Static Workstations to Dynamic
      Enablement Field with Boosted Productivity

    An intelligent office space is driven by digital                   months. Central to organizational collaboration
    technologies such as AI, digital twins, and IoT.                   and decision-making, meetings naturally become
    It reaches new levels in efficiency, collaboration,                a key area for AI agent implementation. As
    and decision-making, creating a new generation                     AI agents, large language models, and IoT
    of office environments with integrated virtual and                 technologies become increasingly mature, they
    physical spaces that combine the thoroughness                      provide new possibilities for reshaping high-
    of individuals' work with the efficiency of team                   frequency and high-value office collaboration
    collaboration. It is designed for the core decision-               scenarios like meeting spaces.
    makers, high-value creators, and all employees
    of enterprises. This helps boost office productivity
    from process execution to value creation.
                                                                             AI collaborat ion throughout the
    Typical scenario 1:                                                      entire meeting process is the core
    Full-process intelligent meetings                                        implementat ion scenario for AI-
                                                                             enhanced meeting spaces in the future.
    According to Microsoft's Work Trend Index 2025 ,
    82% of enterprise executives plan to integrate                           HMN Smart Co., Ltd.
    AI agents into their core teams to empower
    business operations within the next 12 to 18




                                                                  27
                                            Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




Looking ahead, meetings will be an efficient                Using intelligent technologies, it clearly marks
method of collaboration that deeply integrates              the identity, time segment, and speech content of
virtual digital spaces with physical intelligent            each participant, ensuring that participants from
spaces, achieving intelligent collaboration                 different positions and with different business
before, during, and after meetings.                         backgrounds can obtain the meeting information
                                                            in real time. High-precision speech recognition
And full-process intelligent meeting is not just            transcribes speech in real time, providing
a simple automation of traditional meetings. It             multi-language translation to help speakers of
transcends the limitations of physical meeting              different language participate in the meeting and
rooms to cover all stages before, during, and               discussion smoothly. The system extracts the core
after the meeting through the collaboration                 viewpoints and key data from speech in real time,
of multiple AI agents. The core objective is to             and automatically marks the points of contention
transform meetings from complex processes                   in the discussion. It then generates an intuitive
with low efficiency to intelligent collaboration for        list of key points in real time and sends it to the
efficient decision-making (see Figure 7).                   participants. The AI agent monitors the duration
                                                            of each segment based on the meeting agenda
Before a meeting starts, employees can use                  and gently reminds speakers to stay on topic
AI chatbots to clarify the core objectives of the           and to not exceed the allotted time, efficiently
meeting and initiate intelligent collaboration.             advancing the meeting.
The meeting system provides employees with
a checklist for preparing meeting resources.                After the meeting, the system helps campus
It can automatically coordinate schedules of                employees make summaries for future reuse.
multiple parties, intelligently reserve meeting             The system automatically consolidates key
rooms and required equipment, and proactively               points and conclusions of the meeting, generates
integrate relevant background information to                standardized meeting minutes that cover core
generate key meeting briefings. Based on the                content, to-dos, responsibility assignment, and
meeting topic, participants' roles, and business            timeline, and immediately sends the minutes
needs, the system can use intelligent algorithms            to each participant. It also accurately connects
to set core topics and recommended durations,               the to-dos to the participants' daily office and
automatically generating a structured agenda                task management platforms, automatically
and pushing it to all participants. In terms                sends progress reminders, and provides real-
of the environment, the system can adjust                   time updates on the task completion status.
the temperature and humidity of the meeting                 This helps the campus team form a closed loop
room to a comfortable range, preset the light               workflow spanning meeting decisions, task
brightness and curtain position, and automatically          assignment, and progress tracking. Furthermore,
start and debug audio and video devices based               based on the knowledge graph, the system
on the meeting type and number of participants.             classifies and saves meeting minutes, audio and
When participants arrive at the meeting room,               video records, and core viewpoints by topic to
they can immediately enjoy a comfortable and                the enterprise's knowledge base. This allows for
efficient meeting experience.                               keyword search, segment playback, and source
                                                            tracing, making it easy for campus employees
During the meeting, the system provides                     to access the information at any time, and
intelligent real-time services for campus                   facilitating subsequent business review and
employees throughout the entire process.                    task execution.




                                                       28
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses



                Meeting affairs and                                                  Real-time services                                              Summary after
         1                                                                     2                                                              3
                resource preparation                                                 during the meeting                                              the meeting




                            Objective and
                            agenda setting
                                                                                            Core content
                                                                                            extraction              Meeting pace                                  Knowledge reuse
                                                                                                                      control                                     and accumulation
     Intelligent meeting                 Resource coordination
     room reservation                    and preparation                         Real-time information
                                                                                 synchronization                                                  Closed-loop
                                                                                                                                                  management
                                                                                                                                                  of to-dos

                   Meeting resource
                   preparation                                                                                              Summary of
                                                  Meeting room environment                                                  meeting minutes
                                                  and device presetting




                   Before the meeting                                          During the meeting                                          After the meeting
           Employees can use AI chatbots to clarify                    The AI language model provides intelligent                  The AI agent extracts meeting information,
            the core objectives of the meeting and                      real-time services for campus employees.                     providing summaries for future reuse.
               initiate intelligent collaboration.



                                                   Figure 7 Full-process intelligent meeting scenario


    AI agents support the entire meeting process to                                              efficient organization collaboration, and seamless
    build an intelligent meeting loop, significantly                                             process interconnection.
    improving meeting efficiency and the speed
    and quality of decision making. This greatly
                                                                                                           We envision to create a dedicated
    simplifies the meeting organization process while
                                                                                                           campus assistant that can sense,
    reducing the time administrative personnel and
                                                                                                           act, and think in work and learning
    participants spend on transactional work. It also
                                                                                                           through natural language interaction,
    frees participants from tedious work such as note-
                                                                                                           and build a knowledge graph and a
    taking and coordination, so that they can focus on
                                                                                                           dedicated corpus for enterprises and
    core discussions and in-depth thinking.
                                                                                                           organizations to support hierarchical
                                                                                                           and role-based management and query
    Typical scenario 2:
                                                                                                           by individuals or organizations.
    All-scenario collaborative office

                                                                                                           SHENZHEN HUAZHAO TECHNOLOGY
    In the critical phase where AI shifts from
                                                                                                           CO., LTD.
    technological competition to practical application,
    office scenarios are undergoing reconstruction
    at the operating system (OS)–level. According to
    iResearch, the market size of AI office software                                             In the future, the all-scenario collaborative office
    company in China reached CNY30.864 billion in                                                will have a campus AI assistant covering two
    2024, and is expected to exceed CNY190 billion                                               levels: individual enablement and organizational
    by 2028. In the AI era, besides the meeting                                                  collaboration. This assistant will be a dedicated,
    scenario, office work in the future will reach                                               intelligent super office assistant that deeply
    more diversified collaboration scenarios, such                                               adapts to employees' work habits and provides
    as personal task handling, cross-department                                                  end-to-end personal services such as knowledge
    collaboration, and mobile office. AI assistants                                              Q&A, task arrangement, and workflow handling.
    for campuses will be widely used in all these                                                It will also be a team collaboration AI assistant
    scenarios to provide employees with a brand-new                                              that supports efficient individual work and team
    experience spanning intelligent work assistance,                                             collaboration upgrade in all aspects (see Figure 8).



                                                                                          29
                                                                                 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




                                   Personalized intelligent companion                                                         Knowledge co-construction and
                                                                                                                                 sharing system building
                                     Transaction arrangement: AI agents can                                                The NLP technology is used to build a
        Individual                   handle multiple tasks and intelligently                                               cross-department knowledge collaboration                  Organization
                                     schedule individuals' time and resources.                                             platform, promoting the shift from
       enablement                                                                                                          knowledge accumulation by individuals to                  collaboration
                                                                                                                           reuse by organizations.
     Builds a dedicated,                                                                                                                                                            Builds a full-process
  intelligent, and efficient                                                                                                                                                      service system covering
   work partner for each                                                                                                                                                         knowledge co-construc-
          employee                                                                                                                      Forward-looking insight                  tion and sharing, insight
                               Personalized intelligent companion                                                                                                                  enablement, and goal
                                Knowledge Q&A: Provides accurate
                                                                                          Campus AI                                       for decision-making
                                                                                                                                 Integrates multi-source data such as industry           alignment

   Relies on data linkage
                                information query based on data linkage
                                across campus system APIs.
                                                                                           assistant                             policies and market trends, uses AI
                                                                                                                                 algorithms to identify changes, and
                                                                                                                                 generates visualized insight reports.
   across campus system
   APIs and collaboration                                                                                                                                                         Builds a knowledge
   of multiple AI agents.                                                                                                                                                         collaboration platform
                                                                                                                                                                                  using the NLP technology
                                                                                                                                                                                  and AI agents.
                                           Full-process assistance                                                                  Full-process assistance
                                   Workﬂow handling: Assists individuals in                                                Intelligently associates with and visualizes the
   A super assistant that          ﬁlling in forms and transferring workﬂows in                                            KPIs of individuals, teams, and organizations,
   deeply understands              scenarios such as attendance, travel expense                                            and breaks down strategies into executable
   individuals' work habits,       reimbursement, and application approval,                                                tasks to ensure goal alignment.                        Focuses on efficiency and
   proactively provides            and provides real-time feedback on the                                                                                                         capability improvement
                                   handling progress.
   support, and seamlessly                                                                                                                                                        at the team and
   connects to various                                                                                                                                                            organization levels.
   applications
                                                       Intelligent work assistance, efficient organization collaboration, and seamless process interconnection




                                                       Figure 8 All-scenario collaborative office


For individuals, the campus AI assistant is                                                                 collaboration platform built based on natural
designed to provide them with a dedicated,                                                                  language processing (NLP) technology. It
intelligent, and efficient AI-powered                                                                       helps organizations and teams build structured
work partner. It is no longer a simple task                                                                 knowledge bases, and supports easy upload,
remi n der t o ol , b u t a s u p e r as s i s t an t tha t                                                 intelligent search, and real-time collaboration
deeply understands individuals' work habits,                                                                for team members, as well as allowing them
proactively provides support, and seamlessly                                                                to recommend and share knowledge based on
connects to various applications. By leveraging                                                             permissions. This drives the shift from knowledge
the data linkage across campus system APIs                                                                  a c c u m u l a t i o n b y i n d i v i d u a l s t o re u s e b y
and the multi-task processing capability of AI                                                              organizations. In addition, the assistant integrates
agents, the assistant provides services such as                                                             multi-source data such as industry policies and
personal knowledge Q&A , task arrangement,                                                                  market trends, and uses AI algorithms to identify
and workflow handling. It also assists individuals                                                          trends and technology directions, generating
in filling in forms and transferring workflows in                                                           visualized insight reports to provide clear support
scenarios such as attendance, travel expense                                                                for decision-making. This helps teams predict
reimbursement , and application approval.                                                                   and plan, enhance strategic sensitivity, and boost
In addition, the assistant provides real-time                                                               market competitiveness. Moreover, the assistant
feedback on the progress of these tasks.                                                                    intelligently associates and visualizes the KPIs
                                                                                                            of individuals, teams, and organizations, breaks
For teams, the campus AI assistant focuses on                                                               down strategies into quantifiable goals at various
improving the efficiency and capabilities of                                                                levels, tracks progress in real time, generates risk
teams and organizations. Leveraging AI, the                                                                 alerts, and pushes optimization solutions. This
assistant helps to improve the operational                                                                  ensures that macro goals are broken down into
efficiency of campus organizations through                                                                  individual executable tasks, and that the goals of
the construction of a shared knowledge                                                                      the organization are aligned at all levels.
base, forward-looking insight analysis, and
goal-driven organization KPIs. Different from                                                               The core value of this assistant is reflected in
personal information management, this assistant                                                             both employee and enterprise applications, and
for teams is a cross-department knowledge                                                                   comprehensively enhancing office efficiency.



                                                                                                   30
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses



    For employees, it reduces the time spent on                                 human errors and delays, improving overall
    transactional work, allowing them to focus on                               efficiency and decision-making speed. The
    core thinking and creativity. For enterprises,                              accumulated office data can also be used for
    standardized, intelligent processes minimize                                process optimization and system improvement.




      2.1.2 Operations Assurance Space: Intelligent Operations Hub from
      Experience-Driven to Agentic Autonomy

    The operations assurance space is the intelligent                           Against this backdrop, Agentic IOC emerges as
    core that supports the overall efficient operations                         the digital and intelligent brain of the campus.
    of a campus. Using intelligent technologies, it
    provides comprehensive support for campus                                   By building digital twins covering campus buildings,
    operations, addressing pain points in traditional                           devices, pipelines, personnel, and environmental
    campus operations such as delayed response,                                 elements, Agentic IOC can map data from IoT
    poor collaboration, and reactive decision-making.                           sensing, service systems, and video analysis into a
    This ensures the stability and efficiency of                                unified, intuitive, and interactive view. The campus
    campus operations. Agentic intelligent operation                            running status is no longer represented in the form
    center (IOC), as the intelligent core of campus                             of scattered data. Instead, it is continuously sensed
    operations, can accurately gain insight into the                            and dynamically displayed on the same screen.
    campus running status and detect and resolve
    issues in a timely manner.                                                  On this basis, digital twins define the twin
                                                                                data (ledger, time series, event, and model)
                                                                                of the campus, and further serve as visualized
          The effective operation of Agentic IOC                                environments for carrying and verifying the analysis,
          depends on a foundational space that                                  decision-making, and execution results of AI agents.
          can uniformly support campus running.                                 This significantly enhances intelligent decision-
          Digital twins are the key carriers of this                            making (see Figure 9).
          operations assurance space.
                                                                                F i r s t , a l l - d o m a i n o p e ra t i o n a l s i t u a t i o n
          Beijing Digital Hail Information                                      awareness. Agentic IOC integrates data from
          Technology Co., Ltd.                                                  multiple systems, such as campus security, energy
                                                                                consumption, devices, and environment, and uses
                                                                                AI agents to continuously perceive and understand
                                                                                the operational situation in a structured manner.
                                                                                Combined with the digital twin space, it can

    Typical scenario: Agentic IOC                                               intuitively display the status of key devices, personnel
                                                                                movement trajectories, energy consumption
    A s A I t e c h n o l o g i e s s h i f t f ro m t o o l - b a s e d        trends, and risk points, providing a unified basis for
    applications to autonomous collaboration of agents,                         subsequent analysis and decision-making.
    campus operations management is undergoing a
    profound transformation from reactive response                              Second, collaborative analysis and judgment
    to proactive, forward-looking prediction and                                by multiple agents. Centering on the campus
    collaborative governance across all domains.                                operations assurance objectives, Agentic IOC



                                                                           31
                                           Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




                                               All-domain operational
                                                situation awareness
     Collaborative analysis and                                                             Decision-making and
   judgment of multiple agents                                                              closed-loop feedback




                                                   Campus multi-agent
                                                   collaboration system
       Convergence of data from                                                               System collaboration
           multiple sources                       Analysis and judgments                       for decision making
                                                  by professional agents
              AI agent for                                                                     Real-time feedback
         continuous awareness                     Shared situation in the                      on execution effect
                                                       digital twin
           Digital twins for                                                                      Closed loop of
            visualization                        Complex issue breakdown                      intelligent operations
                                                     and analysis...
         Uniﬁed decision-mak-                                                                           ...
              ing basis...




                                           Figure 9 Agentic IOC


builds a collaboration system consisting of                      only identify potential risks in advance through
multiple professional agents. Different agents                   continuous situational analysis, reducing the
focus on the operational analysis and risk                       probability of routine failures, but also quickly
assessment in their respective domains, and                      locate security events, facilitating multi-system
share situational awareness through the digital                  collaboration and coordinated response. In
twin. In this way, complex issues can be broken                  addition, the system supports more refined energy
down into multiple subtasks, which are analyzed                  consumption configuration and device policy
and inferred by different agents in a collaborative              adjustment based on operational data analysis.
manner, improving the efficiency and accuracy of                 In emergencies, the system performs situational
analysis and judgment in cross-system and cross-                 analysis, commanding, and dispatching based
domain scenarios.                                                on the digital twin, significantly improving the
                                                                 efficiency of emergency response and achieving an
Third, decision-making execution and closed-                     intelligent upgrade of campus operations.
loop feedback. After analysis and judgment are
completed, Agentic IOC can associate with the                    Through the integration of Agentic IOC and digital
existing management systems and processes of                     twins, campus operations are gradually shifting
the campus to assist in delivering instructions,                 from a management mode primarily based on
dispatching tasks, and scheduling resources. The                 manual experience to a system centered on
execution results are synchronized in real time to               agent collaboration and spatial situation. Digital
the digital twin, visually displaying the handling               twins provide intuitive feedback and continuous
process and effect. Based on this feedback                       correction for AI decision-making, while AI agents
mechanism, AI agents can evaluate the decision-                  provide proactive analysis and collaboration
making effect as a basis for optimizing subsequent               capabilities for campus operations. This integration
operation policies, gradually forming a continuous               lays a solid foundation for campuses to achieve
improvement loop for intelligent operations.                     more secure, efficient, and sustainable intelligent
                                                                 operations. It also provides important support for
In daily campus operations, Agentic IOC helps                    the upgrade of operations assurance capabilities in
achieve all-round service assurance. It can not                  AI campus scenarios.



                                                           32
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses




      2.1.3 Security Space: Integrated Security from Post-Event Handling
      to Proactive Prevention and Response

    A resilient security space helps to build a 24/7 and                                            Typical scenario 1:
    sustainable security line of defense for campuses.                                              Physical resilient security
    The space provides security for both physical
    spaces and digital ecosystems by deeply combining                                               The campus builds an air-ground integrated
    the air-ground integrated intelligent inspection                                                intelligent inspection and security system that
    system with the resilient command mechanism                                                     combines drones and embodied AI robots,
    that covers both routine services and emergency                                                 creating a robust security shield for personnel
    response, while building a full-link digital security                                           through technology collaboration. Using the
    shield. It provides all-round intelligent assurance                                             agent collaboration technology and integrating
    for personnel, facilities, and data assets at all times                                         aerial patrols and ground inspections, this scenario
    in campuses. This promotes the fundamental leap                                                 focuses on building a three-dimensional security
    of campus security governance from post-event                                                   system covering the entire campus and all time
    reactive defense to pre-event proactive defense                                                 periods. The core objective is to ensure personal
    (see Figure 10).                                                                                safety. In the air, drones perform routine patrols

                  Typical scenario 1: Physical resilient security                                              Typical scenario 2: Digital security shield

           The campus builds an air-ground integrated intelligent inspection                          In the campus-wide security system, data security and network
           and security system that combines drones and embodied AI                                   information security are the core foundation for building a solid
           robots, creating a robust security shield for personnel through                            line of defense.
           technology collaboration.




                                                                                                                                   Data security
                            Aerial drone inspection
                            Identiﬁes risks and generates
                            real-time alarms through thermal
                                                                                                                                             Aerial drone inspection
                            imaging and AI algorithms.
                                                                                                                              AI covers the entire process spanning sensitive data
                                                                                                                              collection, transmission, storage, and destruction.
                                                                                                                              Prevents data leakage and abuse through scenario-speciﬁc
                            Digital twin map                                                                Data center       privacy security, authorized access, and standardized
                                                                                                                              retention and deletion.
                            Supports daily simulation and drills of
         Air-ground         emergencies and plan optimization.
         integration        Synchronizes the operation map in real time in
                            case of emergencies, and AI quickly generates       Resilient                                 Network information security
                            solutions and schedules emergency resources.     security system

                                                                                                                                    Hierarchical isolation and protection
                                                                                                                              A hierarchical permission system is established for IoT
                            Embodied AI robot patrol on the ground                                           Camera
                                                                                                                              devices to implement physical isolation of multiple
                            Form an end-to-end security management                                                            networks. External devices must be veriﬁed and security
                            process through Agentic IOC data                                                                  detection is required to prevent unauthorized intrusion and
                            interworking and task collaboration.                                                              network attacks.
                                                                                                             Inspector




                                                Figure 10 Physical resilient security and digital security


                                                                                               33
                                           Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




at high altitudes, perimeters, and other areas that         practical capabilities at nearly zero cost. When
are difficult for humans to reach. With thermal             an emergency occurs, the system immediately
imaging and AI algorithms, drones identify risks            switches to the intelligent command mode. The
such as objects falling from heights and climbing           digital twin synchronizes with the onsite IoT
behaviors, and generate alarms in real time. On             sensing data in real time, providing the command
the ground, embodied AI robots autonomously                 center with a panoramic, visualized operation map.
patrol key indoor and outdoor areas to detect               Based on the real-time situation and simulation
potential safety risks. Through data interworking           model, the system uses AI to quickly generate
and task collaboration with Agentic IOC, these              and recommend the optimal handling solution,
robots can guide responses, secure areas, and               and automatically collaborates with emergency
evacuate personnel in emergencies like fires, fully         resources and accurately schedules rescue forces,
ensuring the safety of the campus.                          ensuring efficient and well-organized actions. This
                                                            achieves a fundamental shift from experience-
In terms of sensing, this space integrates multi-           based response to scientific handling.
dimensional sensing technologies—such as
channel state information (CSI), millimeter-                Typical scenario 2:
wave (mmWave) radar, 3D optical sensing, and                Digital security shield
spectral sensing—to build comprehensive and
accurate environment sensing capabilities. The              In the campus-wide security system, data
CSI technology enables device-free detection of             security and network information security are
human movement trajectories, mmWave radar                   the core foundation for building a solid line of
penetrates obstacles for effective detection, 3D            defense. To protect sensitive data such as video
optical sensing technology can reconstruct spatial          security data, access control records, and personnel
structures with high precision, and spectral sensing        identity information in the campus, and to mitigate
can identify specific material components. These            network risks caused by IoT device connection and
technologies work together to detect the number             multi-system data interaction, a full-link security
of people and their accurate locations in real time,        system covering all scenarios is built. This ensures
and comprehensively monitor the environment                 the security of the physical space and the reliability
status. This helps detect various risks such as fire        of digital ecosystem in the campus.
hazards, gas leaks, and structural abnormalities in
a timely manner, providing accurate data support            At the network layer, a cyber security system
for physical space security.                                with precise prevention and clear boundaries is
                                                            built to address the risks that arise as a large
In addition, to cope with various emergencies               number of IoT devices connect to and interact
that may occur in the campus, a resilient security          with the campus network. A hierarchical
system is built, which combines routine services            access permission system is established for video
and emergency responses with interaction                    security devices, access control devices, inspection
between the virtual and real worlds. With the               robots, and other IoT devices, implementing
digital twin technology as the core, the system             precise control over all-domain network access.
creates a highly accurate replica of the campus             The office network, security network, and device
in the digital space under normal conditions,               control network in the campus are independently
allowing the security team to perform immersive             separated and isolated to effectively control the
simulation and drills of various emergencies                blast radius of risks. Strict access control and
and optimize emergency plans, and accumulate                security detection are performed on external




                                                       34
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses




    devices such as personal devices and guest devices                 data such as target images, license plates, and
    to prevent unauthorized intrusion and network                      entry/exit trajectories, the entire process spanning
    attacks. Internet access behaviors, such as public                 collection, transmission, storage, and destruction
    Wi-Fi access in the campus, are regulated to                       is protected. Sensitive areas in video security
    control inappropriate comments and ensure the                      scenarios are automatically hidden, and visitor
    stable running of the security system.                             registration data is only accessible to authorized
                                                                       security personnel, avoiding privacy risks and
    At the application layer, a security shield is                     ensuring compliance in data usage. Data retention
    built with a focus on protecting devices used                      and clearance are strictly regulated. Historical data
    for campus office work and operations as                           is retained and periodically cleared according to
    well as key business systems. Security software                    standards, eliminating risks of data leakage and
    is installed on office and campus O&M devices                      abuse in all processes and scenarios.
    to implement comprehensive security control
    measures and prevent information leakage.                          At the O&M layer, personnel and qualification
    Security hardening is performed on key systems                     management is the core to consolidating
    such as access control, turnstiles, and energy                     the securit y foundat ion. Trusted identit y
    systems. The access control mechanism is strictly                  identification and qualification certification are
    implemented to ensure the secure and stable                        performed on office and O&M personnel in the
    running of various application systems and                         campus, and account permissions are strictly
    prevent security risks during application execution.               controlled to prevent privilege abuse. The full-
                                                                       lifecycle security management of certificates is
    At the data layer, a closed-loop security                          promoted to manage the certificate issuance, use,
    system is built to manage the entire lifecycle                     and revocation, providing support for secure and
    of sensitive data in the campus. For sensitive                     standard O&M.




                                                                  35
                                                                       Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




  2.1.4 Transportation and Parking Space: From Inefficient to Efficient
  and Seamless Mobility

The transportation and parking space is an                                                       agent automatically associates with the traffic
intelligent infrastructure that drives smart                                                     data outside the campus, predicts commuting
mobility in campuses, creating an efficient and                                                  congestion, and pushes the optimal combination
seamless integrated ecosystem for mobility,                                                      of travel options, such as subway + campus shuttle
parking, and vehicle charging. It supports campus                                                bus and autonomous shuttle bus. The departure
traffic by helping find parking spaces, alleviating                                              frequency of shuttle buses can be dynamically
traffic congestion, and making charging more                                                     adjusted based on the number of passengers
convenient, providing convenient and efficient                                                   waiting for the bus.
travel support for personnel and vehicles in
campuses. This space focuses on improving                                                        Before arriving at the campus, their vehicles
the smart transportation ecosystem. Mobile,                                                      are automatically verified by road side units
intelligent charging robots enrich the smart                                                     (RSUs) using mmWave radar, allowing them to
services in the transportation and parking space.                                                enter the campus without stopping. The parking
This enables efficient travel and convenient                                                     lot, supported by a digital twin foundation,
charging, continuously optimizing the smart                                                      displays the vacant parking spaces in real time.
mobility experience in campuses (see Figure 11).                                                 Autonomous vehicles can automatically find and
                                                                                                 park in available spaces. Personnel can walk to
Typical scenario 1:                                                                              the office area directly using AR navigation, which
All-domain intelligent mobility                                                                  dynamically adjusts the routes based on the real-
                                                                                                 time crowd flow to avoid congested areas.
Based on the all-domain campus interconnection
of people, vehicles, parking spaces, and objects,                                                Upon entering the office area, a digital identity
the system provides a seamless, adaptive, and                                                    is automatically associated with the access
immersive smart mobility experience for campus                                                   control, elevator, and meeting room permissions.
personnel. It ensures a smooth and enhanced                                                      When a person approaches the elevator, the
experience from the moment personnel enter                                                       system pre-calls the target floor. Meeting sign-
the campus, throughout their workday, to when                                                    ins are automatically completed through target
they leave.                                                                                      recognition, and the agenda of participants is also
                                                                                                 updated. Before leaving the campus, a person
Before setting off, personnel synchronize their                                                  can initiate the departure instruction on their
travel plans on their mobile phones. The AI                                                      mobile phone. An autonomous shuttle bus will

                                                                       Before arriving at the campus:                          Entering the office area and before
                                                                       Seamless campus entry and                               leaving the campus: Seamless
                                                                       intelligent navigation                                  access and intelligent linkage
        Before setting off: Intelligent travel                                           Vehicles can automatically pass                           Digital identities are associated with
                                                                                                                                   Seamless
        planning                                                          Seamless       identity veriﬁcation through
                                                                                                                                digital identity
                                                                                                                                                   access control, elevator, and
                                                                       veriﬁcation for   mmWave radar and RSUs, and                                meeting room permissions, allowing
                                                                        campus entry     quickly enter the campus without       authorization      seamless access in all scenarios.
                            The AI agent synchronizes travel plans                       stopping.
                            with mobile phones and pushes
         Synchronization    optimal combination of travel options,
                                                                                                                                                   When a user approaches the
          of travel plans   such as subway + shuttle bus and                             The parking lot displays vacant           Intelligent     elevator, the AI agent pre-calls the
                            autonomous driving shuttle bus, based                        parking spaces in real time based          elevator
                                                                         Intelligent                                                               target ﬂoor to reduce the waiting
                            on public transportation data.                               on the digital twin foundation,          reservation      time.
                                                                           parking       and autonomous vehicles can
                                                                                         ﬁnd and park in available spaces.
            Dynamic         The AI agent intelligently adjusts the                                                                                 Meeting sign-ins are automatically
                            departure frequency of shuttle buses                                                                Seamless target
                                                                                                                                                   completed through target
          scheduling of                                                                                                           recognition
                            based on the number of passengers                            Users can directly reach the office                       recognition, and the agenda of
          shuttle buses     waiting for the bus.                                         area through AR navigation. The           for sign-in     participants is also updated.
                                                                        Dynamic AR
                                                                                         route is dynamically adjusted
                                                                        navigation       based on the real-time crowd
                                                                                                                                                   A departure instruction can be sent
                                                                                         ﬂow to avoid congestion.
                                                                                                                                  Intelligent      from the mobile phone to reserve a
                                                                                                                                shuttle bus for    shuttle bus in advance. Vehicle
                                                                                                                                  departure        information and locations will be
                                                                                                                                                   synchronized to AR navigation.



                                                             Figure 11 All-domain intelligent mobility


                                                                                         36
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses



    stop in front of the office building in advance.                                                 efficient energy utilization and dynamic balance.
    Vehicle information and real-time location will                                                  Future campuses will build a microgrid ecosystem
    be synchronized to the AR navigation interface,                                                  that integrates parking and charging. The flexible
    allowing the person to board the shuttle bus                                                     deployment of intelligent charging robots and
    seamlessly upon arrival. For car drivers, they can                                               bidirectional interaction with V2G technology will
    also complete autonomous vehicle exit from                                                       facilitate the in-depth integration of traffic flow
    the parking lot and pick-up at the campus exit                                                   and energy flow (see Figure 12).
    through remote instructions.
                                                                                                     Mobile charging robots represent an innovation
    In the future, campus access will no longer be an                                                scenario where smart transportation and new
    independent process node, but an adaptive service                                                energy services deeply converge. They play a key
    that is deeply integrated with office and travel                                                 role in establishing a flexible and efficient mobile
    needs. Through all-domain data collaboration and                                                 charging system. By automatically dispatching
    intelligent decision-making, it will deliver a future                                            charging units to parking spaces, these robots
    access experience characterized by no manual                                                     provide fully automated charging services for
    operation, seamless adaptation, and convenience                                                  vehicles in the campus. This eliminates the
    that follows you everywhere.                                                                     restriction of fixed charging piles on parking
                                                                                                     spaces, enables flexible resource allocation and
    Typical scenario 2: Integrated parking                                                           efficient reuse of charging resources, and provides
    and charging energy network                                                                      a seamless charging experience. All of this allows
                                                                                                     users to charge their vehicles immediately after
    Driven by energy transformation and the rapid                                                    parking and drive away with fully charged batteries.
    development of intelligent connected vehicles,
    the microgrid system that integrates parking                                                     In addition, PV canopies, mobile charging robots,
    and charging with the vehicle-to-grid (V2G)                                                      and tiered energy storage units are integrated. The
    technology is becoming the hub of energy                                                         electricity generated by PV canopies is prioritized
    management in future intelligent campuses and                                                    for parking facilities and charging services. Any
    the core infrastructure of urban transportation.                                                 surplus electricity is stored in energy storage
                                                                                                     units and then sent to the campus energy center
    At the heart of this trend is the transformation                                                 during peak hours. At the same time, parking fees
    of parking lots from mere vehicle storage spaces                                                 and charging costs are linked to carbon credits
    into the "capillaries" of the urban energy network                                               for green travel, forming a low-carbon cycle of
    and distributed energy storage nodes, enabling                                                   parking-charging-energy saving.

                                                                                                                           Seamless
                                                                                                                           charging

                                                      PV canopy                                                                              Automatically identiﬁes positions of
                                                                                                                            Adaptive         vehicle charging ports and precisely inserts
                                                is
                                            ity                                                                             charging         and removes the charging connector,
                                         ric y
                                                               Po r pa argin




                                       ct rg                                                                                                 providing a seamless charging experience.
                                     le ene its.
                                                                 we rki
                                                                 fo ch




                                   e
                                  s in un
                                                                   r s ng g ser




                               lu
                             rp ed ge
                                                                      up




                           Su stor ora                                                                                      Safety           AI monitors parameters such as voltage,
                                                                         ply facili ices




                                   st                                                                                                        current, and battery temperature in real
                                                                                                                         monitoring and
                                                                             is ties .




                                                                                                                                             time, and automatically cuts off power
                                                                               pri




                                                                                                                          scheduling
                                                      Energy                                                                                 supply when an exception occurs.
                                                                                   ori and




                                                                                                  Automatic
                                                                                    v


                                                                                      tiz




                                                     network
                                                                                         ed




                                                                                                  scheduling
                                                                                                                  Automatically identiﬁes charging
           Tiered energy                                                                          Response to
                                                                                                                  needs and parking positions of
            storage unit                                                                           charging
                              Electri                                                                             vehicles, and receives and
                                      city                                                         requests       responds to charging requests.
                                 energ is transmitte
                                        y cente          d to th
                                                r duri
                                                       ng pea e campus                             Intelligent
                                                              k hou                                               Uses laser radar and visual sensing
                                                                    rs.
                                                                                                 navigation and
                                                                                                                  to build maps, plan optimal routes,
                                                                        Mobile charging robot       obstacle      and avoid obstacles.
                                                                                                   avoidance



                                              Figure 12 Integrated parking and charging energy network


                                                                                                37
                                                Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




    2.2 Manufacturing Campus: Bringing Intelligence
        to All Links for Fast and Stable Production

  2.2.1 Production Space: From Silos to Intelligent Manufacturing Hubs
  Powered by Deterministic Connectivity and Robotics Collaboration

Intelligent production spaces connect fragmented                 development of intelligent manufacturing.
processes through smart technologies, creating an
integrated, collaborative ecosystem. The result is a             An integrated production network foundation,
fundamental improvement in operational efficiency,               spanning from individual devices to production
safety and full-link synergy for greater controllability.        lines and the entire campus, can be built on
                                                                 a core of 10 Gbps intelligent connectivity,
Typical scenario 1: Trusted, intelligent                         intelligent edge computing, and all-domain
production networks                                              determinism. This foundation enables end-to-end
                                                                 intelligent collaboration across industrial control,
Driven by a new wave of technological                            flexible production, and R&D innovation while
revolution, the advancement of edge intelligence,                ensuring device-network integrated security via
the industrial Internet, and AI is reshaping                     a trusted production network and HarmonyOS-
manufacturing paradigms and steering the                         powered devices. The security framework covers
industry toward full intelligence. Intelligent                   everything from communication to control, and
manufacturing hubs, which house a large number                   from smart terminals to legacy equipment. This
of companies, are crucial for the deep integration               transforms the production system from a stable,
of digital and real economies. They are the                      reliable setup into one that is self-adaptive and
primary battleground for driving the high-quality                continuously evolving (see Figure 13).




                                                            38
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses



    High-performance collaboration: 10 Gbps                             • AI-based anomaly detection: Leveraging edge
    deterministic communication enables all-domain                          AI for real-time analysis of device behaviors,
    connectivity for production devices, automated                          the system immediately generates alarms and
    guided vehicles (AGVs), and industrial robots                           disconnects links upon detecting risks, such as
    with zero packet loss and low latency. Command                          abnormal valve toggling or motor operation.
    transmission latency measured in milliseconds                           This represents a critical security evolution,
    ensures high-speed synchronization across                               a shift from passive protection to proactive
    production lines. The network is deeply integrated                      prediction.
    with intelligent edge computing, allowing
    automated optical inspection (AOI) to complete                      Driven by both performance and security, the
    the recognition-adjustment-warning loop within                      integrated production network enables real-time,
    seconds. Furthermore, AGVs can synchronize                          precise collaboration and the dynamic optimization
    with production cycles in real time, dynamically                    of production workflows. It ensures trusted,
    optimizing their paths to deliver materials to                      controllable data across all links and provides full
    production lines on demand and boosting logistics                   visibility into device behavior to preempt risks.
    efficiency by over 40%.                                             In doing so, it creates an efficient and reliable
                                                                        digital foundation, allowing intelligent campus
    Proactive security: Legacy devices like traditional                 production to evolve from stable operation toward
    programmable logic controllers (PLCs) and manual                    autonomous evolution.
    valves often lack built-in protection, making them
    susceptible entry points for cyberattacks. To bridge                Typical scenario 2:
    this security gap, HarmonyOS-powered intelligent                    Production robot collaboration
    terminals can act as security agents to establish a
    zero-trust line of defense.                                         As embodied AI rapidly advances, campuses—
                                                                        and especially manufacturing sites—have become
    • Trusted authentication and auditing: All                          the primary arena and ideal testing ground for
       operations require dual authentication via                       deploying robotics. With manufacturing intelligence
       device certificates and user identities to prevent               accelerating and environments growing more
       unauthorized access.                                             complex, traditional manual inspections are no
                                                                        longer sufficient to meet rising safety standards.
    • Distributed end-to-end encryption: Control                        In the future, embodied AI robots (such as
       commands are encrypted throughout the process                    logistics robots) equipped with autonomous
       to prevent eavesdropping and tampering.                          sensing, decision-making, and execution




       Interconnection efficiency                        Field intelligence                 Cross-process collaboration
       Deterministic communication                  Edge intelligent computing               Production-R&D network hub
               technology

             Ensuring precise motion                 Delivering materials to production     Seamlessly connecting R&D testing with
        synchronization during high-speed           lines on demand, boosting material       production line validation, speeding up
         operation of the production line                turnover efficiency by 40%           the trial production of new products



                                       Figure 13 Trusted, intelligent production networks


                                                                   39
                                                                     Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




                                                       Autonomous mobility and sensing
                                                       • Robots: Intelligent planning of autonomous
                                                         inspection routes
                                                       • Inspections with comprehensive multimodal sensing
                                                                                                                   Substantially reducing both missed
                                                                                                                      detections and false alarms




                                        Ensuring production                                                        Implementing around-the-clock,
                                           safety through                                                              360-degree inspections
                                       intelligent protection
                                        with no blind spots

                                                                                                                  Closing security gaps during critical
                                                                                                                    periods like nights and holidays


   Real-time linked response                                 Intelligent identiﬁcation and analysis
   • Automatically triggers correlated alerts                • AI-based anomaly detection through
     upon anomaly detection and                                inspections with comprehensive
     recommends corrective actions                             multimodal sensing


                                                    Figure 14 Production robot collaboration

capabilities will be deeply integrated into every                                           As embodied AI robots play an increasingly important
stage of production. They are set to become                                                 role in inspections and logistics, efficient collaboration
the first line of defense for efficient and safe                                            and unified scheduling become paramount. These rely
production (see Figure 14).                                                                 on a clearly structured, integrated robot management
                                                                                            platform that offers centralized capabilities.
For production safety inspection, robots enable
intelligent, 360-degree protection by leveraging                                            Intelligent task allocation, path planning, and
autonomous mobility, sensing, intelligent                                                   real-time monitoring ensure that robots operate
analysis, and real-time linked response. They                                               efficiently in complex environments without
can handle the entire inspection loop, from the                                             interference or conflicts. This technology enables
planning of autonomous inspection routes and                                                robots to be widely deployed for food delivery,
comprehensive multimodal sensing to AI-based                                                cleaning, material transportation, and tour guiding.
anomaly detection, warnings, and response. This                                             Through efficient scheduling and task execution,
substantially reduces both missed detections                                                these robots can significantly improve service
and false alarms. By providing around-the-clock                                             efficiency and quality, reduce manual intervention,
inspections, robots effectively close security gaps                                         and optimize resource allocation across AI campuses.
during critical periods like nights and holidays.


In terms of logistics and distribution, an AI                                                         This represents an evolution from
campus will be equipped with an integrated                                                            tradit ional intelligent campuses,
logistics system comprising diverse intelligent                                                       where drones and unmanned vehicles
terminals, such as ground-based robots and                                                            perform specific tasks planned for
drones. Within assembly lines and material                                                            specific scenarios, to AI campuses
handling scenarios, intelligent devices (like AGVs                                                    characterized by the autonomous
and robotic arms) will work in synergy with non-                                                      collaboration and on-demand
intelligent devices (like manual workstations and                                                     scheduling of various intelligent
legacy conveyors). The HarmonyOS intelligent                                                          terminals (including embodied AI
terminal acts as a collaboration hub that enables                                                     robots and unmanned devices).
cross-device task scheduling, status synchronization,
and action coordination.                                                                              — HMN Smart Co., Ltd.




                                                                                     40
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses




         2.3 Industrial Campus: Powering Enterprise
              Growth and Maturity with Digital and
              Intelligent Technologies

      2.3.1 Industry Service Space: From Physical Aggregation to a
      Platform for Smoother Collaboration and Faster Innovation

    An intelligent industry service space is an enterprise-            resources, and a passive, wait-for-request approach.
    centric ecosystem built on big data and AI, to                     To address these pain points, intelligent enterprise
    provide intelligent, companion-like, and full-chain                services powered by big data, AI, and digital twin
    services. Such ecosystems use AI algorithms to                     technologies have emerged as the new standard.
    precisely match innovation resources to enterprise
    needs, enable flexible space leasing, and empower
    enterprises to thrive throughout their lifecycle. This                   A true AI campus should allow
    approach not only boosts collaboration across                            every enterprise to enjoy affordable
    the industry value chain but also propels industry                       and accessible services, as if they
    clusters to evolve from a basic state of physical                        w e re s u p p o r t e d b y a d e d i c a t e d
    aggregation to a sophisticated model defined by                          intelligent assistant. This improves
    smoother collaboration and faster innovation.                            tenant satisfaction and loyalty while
                                                                             transforming the campus' role from
    Typical scenario:                                                        a traditional landlord to an industry
    Intelligent enterprise services                                          enabler. In doing so, it creates a
                                                                             digital ecosystem whose value grows,
    With the rapid development of the digital                                becoming more indispensable the
    economy, the competition of industrial parks                             more it is used.
    has shifted its focus from mere policies and
    land to services and ecosystems. Enterprises                             Guangdong Flying Enterprise Internet
    increasingly demand personalized, instant, and full-                     Technology Co., Ltd.
    lifecycle services. However, traditional service models
    often struggle with delayed responses, fragmented



                                                                  41
                                                                 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




Campuses of the future will be more than just                                             sized enterprises.
physical spaces; they are becoming intelligent
partners for enterprise growth.                                                     • Flexible resource leasing: Modular leasing
                                                                                          services are provided to meet enterprises'
Building an enterprise- centric, intelligent ,                                            core requirements, such as their storage and
and companion-like service system is key to                                               computing demands. Startups can rent basic
strengthening a campus' ability to attract tenants                                        compute and storage resources on demand
and investment, as well as boosting its operational                                       to avoid heavy upfront investment, while
vitality. It has become the core pillar for intelligent                                   mature companies can quickly scale their
spaces dedicated to industry investment and                                               advanced compute capacity during peak periods
operations (see Figure 15).                                                               without worrying about idle equipment. This is
                                                                                          supplemented by 24/7 remote O&M support,
Intelligent enterprise services encompass smart                                           which resolves any resource issues in real time to
operational support, resource services, and industry                                      ensure business continuity.
ecosystem collaboration.
                                                                                    • Lifecycle enablement and ecosystem
• Operational support: By integrating third-party                                         collaboration: Using knowledge graphs, the
    services, such as finance and tax, legal affairs,                                     AI system identifies enterprises' technology
    human resources, and supply chain finance,                                            requirements and supply chain gaps, and
    the campus system relies on AI assistants to                                          proactively recommends potential partners,
    provide 24/7 consultations and value-added                                            research institutions, or shared technology
    services. These include intelligent contract                                          platforms within the campus. This accelerates
    reviews, workforce risk warnings, and financing                                       the circulation of resources within the campus,
    requirement prediction, all of which significantly                                    s p e e d i n g u p t h e c o m m e rc i a l i z a t i o n o f
    reduce operating costs for small and medium-                                          innovations and facilitating order matching.


              Before commencing operations:                Ongoing operations:                           After commencing operations:
         1    Resource leasing and support
                                                     2     Operational support services
                                                                                                    3    Industry collaboration and ecosystem linkage




 Resource configuration                             Integration of                                 Resource                              Commercialization of
     solution preview                         third-party professional                         recommendations                           innovation outcomes,
(office spaces, computing                         services and 24/7                            (industry policies,                        and order matching
   power, and storage)                              consultations                             partners, and more)



                       Modular leasing services              Value-added services such as intelligent           Recommendation of research
                   (compute and storage resources)          contract reviews, workforce risk warnings,             institutions and shared
                                                              and financing requirement prediction                  technology platforms




    Affordable and accessible services,                         A balance between resource                             A digital ecosystem whose value
      as if supported by a dedicated                            flexibility and cost control to                      grows, becoming more indispensable
             intelligent assistant                                ensure business continuity                                  the more it is used




                                                Figure 15 Intelligent enterprise services




                                                                              42
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses




         2.4 Sports and Exhibition Campus: Activating
              Venues for an Immersive Experience

      2.4.1 Sports Venue: From Mere Spectatorship to Multi-Viewpoint,
      Immersive Interactions

    Typical scenario:                                                  during, and after a sports game. As a result, it has
    Intelligent spectatorship                                          become a core feature showcasing the value of
                                                                       intelligent sports spaces (see Figure 16).
    Driven by the increasing maturity and deep
    integration of cutting-edge technologies such as                   Before a game, spectators are greeted with
    AI, IoT, and big data, sports venues are undergoing                a seamless entry experience. Services are in
    a systemic evolution, transforming from traditional                motion the moment a spectator sets off for
    competition arenas into comprehensive, intelligent                 the stadium. By synchronizing ticketing and
    hubs for athletics, fitness, entertainment, social                 transportation data, the system offers optimal
    interaction, and data services. According to the                   travel suggestions and parking reservations.
    Global Sports Tech Ecosystem Report 2024, the                      Upon arrival, contactless entry backed by smart
    market penetration of intelligent spectatorship                    recognition allows spectators to pass through
    technologies in commercial sports increased from                   checkpoints instantly, without having to show
    18% in 2021 to 41% in 2024, and is projected                       physical credentials, dramatically reducing wait
    to exceed 60% by 2027. Intelligent spectatorship                   times. Inside the venue, the AR navigation system
    focuses on creating personalized, immersive, and                   automatically plans routes and guides spectators
    engaging experiences. By integrating 6G, AI, AR,                   to their seats. This resolves wayfinding challenges
    and ultra-high-definition (UHD) transmission, it                   in large venues and ensures a smooth start to the
    transforms the entire spectator experience before,                 game-day experience.




                                                                  43
                                                                        Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




During the game, spectators can enjoy a                                                       deployed across the stadium enable the real-
multi-viewpoint, immersive experience. A                                                      time transmission and processing of massive data
multi-cam free-viewpoint video system and 8K                                                  streams. Intelligent vending robots and delivery
UHD displays are deployed in the stadium to                                                   systems automatically calculate paths based on
meet diverse spectator needs. No longer confined                                              spectator locations, delivering food, beverages,
to a single seat-based perspective, spectators                                                and merchandise directly to their seats. This
can use their mobile devices to instantly switch                                              ensures fans never miss an exciting moment by
between different viewpoints, from a player's                                                 leaving their seats to make a purchase.
perspective, to a referee's angle, to a high aerial
view. For those in distant rows, virtual reality (VR)                                         After the game, spectators can preserve their
panoramic streaming allows them to "teleport"                                                 memories by creating digital keepsakes. AI
to the front row for an immersive, stunning visual                                            can be used to automatically generate exclusive
experience. In addition, the stadium also provides                                            highlight reels for each fan, featuring the shining
proactive smart support services. This seamless                                               moments of their favorite stars and clips from
experience is powered by a high-speed, stable,                                                their chosen viewpoints. With just a single click,
and ubiquitous intelligent network. An integrated                                             these personalized highlight packages can be
5G private network, Wi-Fi 7, and IoT system                                                   shared directly to social media.


                                                                                                          Before a game:
                                                                                                      Seamless entry experience


                         Travel suggestions                               Target recognition
             By synchronizing ticketing and transportation             Spectators can pass through                                    AR navigation
                 data, the system offers optimal travel            checkpoints instantly, without having                          The system automatically
                 suggestions and parking reservations.                 to show physical credentials.                               plans routes and guides
                                                                                                                                  spectators to their seats.




                                                             Multi-viewpoint, immersive game watching
    During the game:                                   A multi-cam free-viewpoint video system and 8K UHD displays
    Multi-viewpoint,                                    are deployed, allowing spectators to switch between player,
                                                                                                                                               After the game:
                                                         referee, and other camera angles on their mobile devices.
  immersive experience                                                                                                                       Digital retention of
                                                       Spectators in distant rows can rely on VR panoramic streaming
Proactive smart support services                                       for a fully immersive experience.                                   game-watching memories
Backed by 5G and IoT technologies,
intelligent vending robots and delivery
systems automatically calculate paths
based on spectator locations, delivering                                                                                    AI clipping
food, beverages, and merchandise                                                                               AI can be used to automatically generate
directly to spectators.                                                                                          exclusive highlight reels for each fan.




                                                              Figure 16 Intelligent spectatorship




   2.4.2 Exhibition Venue: From Static Displays to Immersive Narratives
   and Transaction-Enabling Spaces

An intelligent exhibition space is a new venue                                                carbon flow tracking, it is further enhanced by
model that integrates sustainability with digital                                             immersive services like virtual human companions
technologies. Its core purpose is to enhance the                                              and AR navigation. This model not only fosters an
visitor experience while leveraging technology                                                eco-friendly exhibition ecosystem but also delivers
to drive sustainable development. Built on an                                                 a personalized, interactive experience, redefining
infrastructure of self-generated electricity and                                              the paradigm of smart exhibitions (see Figure 17).



                                                                                       44
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses



    Typical scenario:                                                               available to answer route-related questions.
    Immersive digital guides
                                                                                    For exhibit introduction, the virtual human acts as
    Powered by AI and multimodal interaction,                                       a personal tour guide. Using voice interaction and
    personalized navigation and guidance creates                                    a multimodal content engine, it delivers in-depth
    a highly interactive exhibition experience                                      explanations that are engaging, easy to understand,
    tailored to each visitors' unique needs. This                                   and tailored to each visitor's background and
    approach delivers tailored services throughout                                  interests, moving beyond traditional text displays
    the journey, significantly boost ing both                                       or audio clips. More importantly, the service is
    viewing efficiency and overall satisfaction.                                    adaptive: by analyzing visitor behaviors and regional
    Consequently, it is accelerating the evolution of                               heat maps in real time, the system dynamically
    digital guides from a mere conceptual idea to                                   adjusts upcoming tour content and routes to
    large-scale implementation, establishing itself                                 ensure a consistently smooth and engaging
    as a core direction for digital guides within the                               experience. This approach transforms the visit from
    campus domain.                                                                  a series of static displays into an interactive, deeply
                                                                                    personalized journey of exploration.
    A virtual human streamlines entry by welcoming
    visitors and facilitating verification and identity                             Immersive digital guides will significantly
    linking. Using advanced visual recognition, it                                  enhance both the efficiency and quality of the
    can identify each visitor in seconds, replacing                                 exhibition experience, enabling visitors to focus
    complex ticket checks with a seamless and                                       on their specific interests and avoid irrelevant
    efficient access experience.                                                    content. This approach directly addresses the pain
                                                                                    points of traditional exhibitions, such as wayfinding
    During the exhibition, a dedicated virtual guide                                confusion, aimless browsing, and information
    accompanies each visitor throughout the tour.                                   inaccuracy. For organizers, the data accumulated
    Powered by high-precision indoor positioning                                    from these visits provides precise, actionable
    and AR navigation, it automatically plans the                                   insights for optimizing exhibit layouts and planning
    best route and offers intuitive guidance. The                                   future events. This strengthens the personalized,
    virtual guide also provides real-time updates to                                interactive advantages that define the smart
    help visitors avoid crowded areas and is always                                 exhibition model.



                    Entry verification through target recognition                                     AR navigation
                    A virtual human streamlines entry by welcoming                                    A dedicated virtual guide, powered by high-
                    visitors and facilitating verification and identity                               precision indoor positioning and AR navigation,
                    linking in seconds, enabling a seamless and                                       stays by each visitor's side to automatically plan
                    efficient access experience.                                                      the best routes and avoid crowded areas.




                    Multimodal intelligent narration                                                  Data-driven optimization
                    The virtual human acts as a personal tour guide.                                  The data accumulated from visits provides
                    Through voice interaction, it delivers in-depth                                   precise insights for optimizing exhibit layouts
                    explanations tailored to each visitor's interests                                 and planning future events. This reinforces the
                    and dynamically adjusts upcoming content.                                         core advantages of smart exhibitions.




        More efficient viewing experience                          Better user experience                            Enabling smart management
        Allows visitors to focus on their interests           Addresses pain points like wayfinding                 Leverages data to support operations
             and avoid irrelevant content                     confusion and information inaccuracy                    and enables refined management



                                                       Figure 17 Immersive digital guides


                                                                              45
                                               Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




    2.5 H ospital Campus: Intelligent Foundation
        Streamlines Healthcare, Where Every Second
        Counts
  2.5.1 Hospital Ward: From Passive Treatment to Preventive Healthcare

Intelligent healthcare streamlines emergency                  pre-hospital emergency care and in-hospital
treatment and prioritizes warm care services                  diagnosis and treatment, thus improving the
to provide individuals with the best possible                 efficiency and accuracy of emergency care for
treatment. It streamlines the pre- and in-hospital            those in critical condition. With an emphasis on
processes through zero-latency collaborative                  real-time information exchange, resource scheduling
emergency response, seizing the golden window                 in advance, and accurate diagnosis and treatment,
for emergency treatment. In addition, zero-                   intelligent collaboration provides the means for
distance smart wards create a health service                  people to seize the golden 10-minute window for
system that covers all people on campus and                   emergency treatment, and builds a full-chain health
extends healthcare management throughout the                  management service for patients.
entire healthcare lifecycle (see Figure 18).
                                                              Pre-hospital vitals monitoring: Patients' vital
Typical scenario 1: Zero-latency,                             signs, including heart rate, blood pressure, oxygen
intelligent collaborative emergency                           saturation, and ECG, can be collected in real
response                                                      time via IoT devices such as smart monitoring
                                                              terminals. This data is transmitted synchronously
Intelligent collaboration between pre- and in-                to the hospital's emergency center, enabling real-
hospital processes helps seize the golden 10-minute           time visual communication between pre-hospital
window for emergency treatment. It uses intelligent           paramedics and in-hospital teams to provide
technologies to create a strong link between                  remote guidance for on-site emergency procedures.


                                                         46
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses




        Intelligent case archiving upon admission                                                                              Pre-hospital vitals monitoring
                                                                                 d                 Re
    Pre-hospital vital sign data and treatment records are                    an ion                 al                          The IoT terminal collects patients' vital signs in
    automatically synchronized to the hospital's EMR system                     c t                                               real time and transmits them to the hospital's




                                                                          co sis




                                                                                                       -t exch
                                                                               e




                                                                                                         im
                                                              treatment gno
    through AI, eliminating the need for manual data entry                                                                                emergency center, achieving visualized




                                                                            nn




                                                                                                           e i nge
    and ensuring seamless integration of diagnosis and                                                                         communication and remote guidance before and




                                                                        dia




                                                                                                              nfo
    treatment before and during patient admission.                                                                                                     during patient admission.
                                                                                   Zero-latency,




                                                                                                               a
                                                                Precise




                                                                                                                 rmation
                                                                                    intelligent
                                                                                   collaborative
    The intelligent platform triggers an emergency                                  emergency                                    The AI model quickly diagnoses patients based
    response based on pre-hospital data and diagnostic                                                                                on pre-hospital data and medical history,
    recommendations, automatically reserving beds,
                                                                                     response                                       generates treatment recommendations and
    dispatching medical staff, and preparing equipment                                                                           hospital readiness checklists, which are pushed
    and medication. This ensures that patients receive                                                                                                  to related departments.
    immediate treatment upon arrival.                                                 sch
                                                                                   Ad    eduling
     Emergency resource scheduling in the hospital                                                                         AI-powered intelligent disease analysis
                                                                                     vanc
                                                                                          e resource




                                 Figure 18 Zero-latency, intelligent collaborative emergency response


    AI-powered intelligent disease analysis: AI models                                       outcomes. By enabling advance clinical assessment
    are used to construct an auxiliary diagnosis                                             and removing procedural delays, the system
    engine, which analyzes vital signs collected before                                      replaces fragmented data with integrated insights.
    admission as well as patient history to rapidly assess                                   AI-driven analytics and pre-emptive resource
    the condition. It intelligently generates preliminary                                    allocation allow medical teams to seize the golden
    treatment recommendations and hospital readiness                                         10-minute window, ensuring that every second
    checklists, which are pushed simultaneously to ER,                                       counts toward a successful recovery.
    surgery, and laboratory departments to streamline
    the clinical workflow upon arrival.                                                      Typical scenario 2: Smart wards featuring
                                                                                             zero-distance, personalized care
    Emergency resource scheduling in the hospital:
    Leveraging the intelligent resource scheduling                                           Healthcare models are transitioning from
    platform, the hospital initiates an immediate                                            traditional in-hospital treatment to whole-lifecycle
    emergency response based on pre-hospital data                                            health management, integrating out-of-hospital
    and diagnostic insights. It automatically reserves                                       prevention with community-based wellness. Against
    trauma beds, precisely allocates medical teams, and                                      this backdrop, leveraging cutting-edge technologies
    pre-stages necessary instruments and medications,                                        like IoT and AI to transcend spatial-temporal
    ensuring that all life-saving resources are fully                                        boundaries is essential. By extending healthcare
    deployed the moment the patient arrives.                                                 resources to daily living and working environments,
                                                                                             the industry is shifting from reactive to preventative
    Intelligent case archiving upon hospital admission:                                      healthcare. Within this framework, the Smart
    Automated EMR synchronization ensures that vital                                         Ward solution offers zero-distance monitoring,
    signs and first-aid treatment logs are instantly                                         personalized rehabilitation, and integrated campus-
    available upon hospital arrival. With no manual re-                                      wide care to build an intelligent service system
    entry required, the care team can leverage end-to-                                       that delivers precise, efficient, and compassionate
    end data integrity to deliver faster, more accurate                                      healthcare (see Figure 19).
    interventions, streamlining emergency treatment
    and hospitalization.                                                                     This case not only validates the seamless synergy
                                                                                             between visual sensing and device control within
    Integrating pre-to-in-hospital processes through                                         the HarmonyOS architecture but also upholds a
    intelligent technology creates a patient-centered                                        vision-over-sensor and algorithm-driven approach.
    framework designed to optimize critical care                                             By replacing physical sensors with AI vision and


                                                                                        47
                                                     Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




                                                                       Personalized and adaptive rehabilitation and
       By utilizing AI-powered cameras to                              health plans: AI models and MR data acquisition
       calculate window opening angles                                 are applied to precisely capture data about
       and leveraging HarmonyOS device                                 the patient's conditions, such as limb function,
       intercommunication, commands are                                physical constitution, and baseline rehabilitation.
       sent directly to on-site controllers                            By intelligently matching these metrics against a
       to deactivate air conditioning in                               standardized rehabilitation database, the system
       corresponding rooms. This approach                              generates highly adaptive and personalized training
       eliminates the need for additional                              protocols, which are pushed simultaneously to both
       window sensors or complex wiring.                               medical teams and patients.


        S hanghai Yuankong Automat ion                                Collaborative care services based on campus
        Technology Co., Ltd.                                           resources: To enhance care for seniors, intelligent
                                                                       voice assistants leverage personalized profiles
                                                                       to curate daily routines, including entertainment
                                                                       preferences and dietary habits. These empathetic
simplifying deployment, it provides a scalable and                     interventions, from audio-visual content to hydration
replicable roadmap for hospital wards, steering                        reminders, create a supportive environment.
campus operations toward a more lightweight and                        Additionally, by integrating with the hospital social
intelligent direction.                                                 work platform, the system facilitates access to
                                                                       mental health services and emotional guidance.
Zero-distance, real-time health monitoring:
Ward devices like intelligent beds and vital sign                      This intelligent healthcare service model, which
monitors automatically collect data such as                            integrates real-time monitoring, intelligent
heart rate and blood pressure, and synchronize                         rehabilitation, and humanistic care, not only enables
abnormal indicators to the medical workstation                         seamless transfer of healthcare data and efficient
and family members in real time. Patients can                          service response, but also goes beyond the boundaries
call for nurses through voice commands. The                            of conventional healthcare service scenarios. It deeply
hospital campus healthcare platform collaborates                       integrates technology and care, providing a practical
with nearby resources to ensure instant response                       model for the high-quality development of healthcare
to patients' needs.                                                    and wellness services on intelligent campuses.


                                                                                                                                 Health guard

                                                                                          Ward devices (intelligent beds and vital sign monitors)
                                                                                          automatically collect data such as heart rate and blood
                                                                                             pressure, and synchronize abnormal indicators to the
                                                            Zero-distance,                  medical workstation and family members in real time.
                                                           real-time health                     Patients can call for nursing services through voice
                                                             monitoring                    commands. The hospital campus healthcare platform
                                                                                            collaborates with nearby resources to ensure instant
                                                                                                                       response to patients' needs.
Humanistic care

Care services: The intelligent voice
                                                                 Smart
assistant records patients' preferences                          ward                                            Intelligent rehabilitation
                                                 Collaborative
and provides customized care.                                            Rehabilitation
                                                 care services
Mental health support: The social                                             and                        Personalized plan: The AI model collects
                                               based on campus
worker platform is there to provide                                       health plans                      patients' rehabilitation data using MR
                                                   resources
mental health services for patients.                                                                       technologies, customizes training plans
                                                                                                               based on the standard library, and
                                                                                                        synchronizes these plans with the medical
                                                                                                                                team and patients.




                               Figure 19 Smart ward featuring zero-distance, personalized care


                                                                  48
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses




         2.6 Commercial Campus: Seamless Transactions,
              Enhanced Immersion

      2.6.1 Intelligent Guest Room: No Manual Reception, and No Need
      for a Key Card

    S m a r t g u e s t ro o m s a re a n e w m o d e l fo r           24/7 intelligent butler service, with no
    accommodation, centered on contactless, on-                        reception desks or key cards
    demand services. By integrating digital technologies
    such as IoT, AI, and mobile connectivity, these                    Contactless, on-demand response is at the
    spaces dismantle the procedural barriers of                        core of the 24/7 intelligent butler guest room
    traditional hospitalit y. Moving away from                         service. This service reconstructs the guest room
    fragmented and reactive service models, smart                      service model with a full-process contactless
    guest rooms operate on an internal logic of pre-                   experience, significantly improving convenience
    arrival request submission, frictionless access,                   and service efficiency. Built upon the core logic of
    intelligent response, and automated settlement.                    pre-arrival request submission, frictionless access,
    This creates a personalized, automated, and                        intelligent response, and automated settlement,
    invisible service that spans the entire journey, from              this model dismantles the procedural barriers
    check-in to post-departure.                                        of traditional hospitality. It constructs a full-link,
                                                                       frictionless service covering pre-arrival, arrival, stay,
                                                                       and departure, creating a highly efficient, private,
                                                                       and convenient intelligent experience.




                                                                  49
                                                     Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




   Check-in preparation:                Upon arrival:                       During the stay:             Upon departure:
    Intelligent booking                Seamless access                     Intelligent control          Seamless check-out




  With the online intelligent    Intelligent locks are                  The IoT device linkage        The automatic settlement
  service platform and           integrated with target                 and intelligent interaction   system and online payment
  identity verification          recognition. Guests can                engine technologies are       technology are used to
  technology, guests can         quickly verify their identities        used to enable voice/         automatically settle fees,
  select rooms, bind their       and obtain temporary door              panel control of the guest    generate electronic bills,
  booking records, prepay        lock authorization. Instead            room, online reservation      and complete check-out
  for the stay, and submit       of queuing at the front desk           of services, and real-        with one click, saving the
  special requests, while        to get a key card, they can            time tracking of service      trouble of going to the
  the system schedules the       scan the QR code or use                progress.                     front desk.
  necessary service resources.   target recognition to open
                                 the room door.


                 Figure 20 24/7 intelligent butler service, with no reception desks or key cards



Underpinned by an online intelligent service                            maintenance, and tracking the progress of laundry
platform, identity verification, IoT device                             or fitness equipment. Upon receiving a request,
linkage, an intelligent interaction engine, and                         the system automatically dispatches resources and
an automated settlement system, this scenario                           provides real-time updates on service progress.
achieves end-to-end intelligence across the                             Upon departure, the system automatically
entire guest room service workflow. Before                              calculates all room charges and deposit deductions,
arrival, guests can complete booking, room                              generating a detailed invoice pushed directly to
selection, identity verification, and pre-payment                       the guest's smartphone. With one-click payment
via the online platform. The system simultaneously                      confirmation, guests can depart without coming
pushes check-in guides and local service information                    into any contact with the front desk.
while allowing for the early submission of special
requests, such as extra beds or meal reservations,                      Contactless on-demand response allows for
ensuring that service resources are pre-allocated                       a frictionless, end-to-end service journey.
and ready before the guest arrives. Upon arrival,                       This model comprehensively optimizes the
guests can verify their identity via mobile devices                     guest experience while simultaneously driving
or on the kiosk in the lobby, saving the trouble of                     operational excellence for hotel management.
queuing at the front desk. Once verified, the system                    It effectively addresses the most common pain
grants temporary authorization for the intelligent                      points of hotels, such as long check-in queues, lost
door lock. By using QR codes or target recognition,                     key card replacements, and cumbersome checkout
guests can enter their rooms without the need for a                     procedures. By offering a completely self-service
physical key card at any point. During their stay, in-                  journey with zero redundancies, hotels can ensure
room voice assistants and intelligent control panels                    a convenient, efficient, and private experience
enable full-scene environmental control. Guests can                     throughout the guest's stay. For hotels, the data
adjust temperature, lighting, and curtains through                      generated (e.g. user preferences and spending
natural language or a quick tap. Furthermore,                           habits) fuels service optimization and product
the system supports one-touch requests for                              upgrades. This continuous refinement strengthens
services such as in-room dining, housekeeping,                          the competitiveness of smart guest rooms,



                                                                   50
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses



    embodying the core value of intelligent service:
    "Invisibility is the ultimate presence."                                    NearLink, Wi-Fi 7, CSI energy saving,
                                                                                and end-to-end encryption technologies
    The feasibility and evolution of this contactless                           improve service experiences, operational
    service have been validated and promoted across                             efficiency, and security assurance,
    global hotels in their digital and intelligent practices.                   supporting smart hotels in their ongoing
                                                                                evolution toward standardized, large-
    Together, these capabilities form an agile                                  scale growth.
    deployment system for the global market, enabling
    contactless services to balance experience,                                 Shandong Bittel Intelligent Technology
    efficiency, and security across diverse policy and                          Co., Ltd.
    cultural backgrounds, driving smart hotels toward
    standardization and large-scale adoption.




      2.6.2 Commercial Consumption Space: From Shelf Displays to
      Boundless Immersive Consumption

    Commercial consumption spaces serve as lifestyle                       large commercial complexes are evolving towards
    hubs for leisure and shopping. In a boundless                          experience-oriented consumption ecosystems.
    consumption space, AI-based shopping assistants                        Leveraging generative AI, AR/VR, and big data
    are deployed to create interactive experiences, and                    analytics, the framework constructs three core
    data is collected and analyzed to help transform                       scenarios: all-domain personalized consumption,
    commercial venues from mere transactional nodes                        immersive blended experiences, and AI + data-
    into destinations for emotional connection and                         driven operations. This enables a paradigm shift
    value co-creation.                                                     from search-based shopping to proactive product-
                                                                           to-person discovery, and from experience-based
    Boundless immersive consumption                                        intuition to data-informed decision-making,
                                                                           creating a win-win value loop for consumers,
    As digital technologies are adopted by businesses,                     tenants, and campus operators (see Figure 21).


               Generative AI personal                                                              All-domain customer ﬂow
                 shopping assistant                                                                   analysis and heatmap
        Intelligent parsing of vague requirements                                               All-domain customer ﬂow data collection

     Intelligent combination of multi-brand products
                                                                                                 Multi-dimensional intelligent analysis

       Visualized product display and commentary
                                                                                                Operations decision-making data output
            Scenario-based precise navigation
                                                                   Boundless                       Dynamic optimization suggestions
                                                                   immersive
       Interactive marketing games and                            consumption
            instagram-worthy spots
                                                                                                          Data quantization
      Intelligent, lightweight, and quick participation
                                                                                                   Core commercial space indicators
        Immersive and interactive tech experience

                                                                                                    Layout optimization suggestions
          Intelligent generation of social content

            Omni-channel write-off conversion                                                                     ……



                                                  Figure 21 Boundless immersive consumption


                                                                      51
                                           Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




All-domain personalized consumption takes                   venues into social entertainment destinations. This
"one profile for one person" to "mult i-                    shift is effectively redefining the value proposition of
dimensional adaptation." Generative AI personal             modern commercial spaces. This type of commercial
shopping assistants serve as the intelligent service        complex integrates four core technologies: AI
carriers that align LLMs with consumer needs.               computer vision, AR/VR engines, virtual rendering
They help retail services transition from a model           engines, and an omni-channel redemption system.
of passive response to proactive anticipation.              Together, they create a marketing loop for the
According to Gartner, 80% of sales processes                commercial space, smoothing the journey of arrival,
were anticipated to be conducted via digital                experience, sharing, and monetization.
channels by 2025. Meanwhile, a recent report
from Adobe Analytics predicts that by 2028, with            On the hardware side, the mall atrium features ultra-
the maturation of multimodal large models, AI-              HD LED interactive screens and 360° panoramic
driven shopping will account for 56.1% of the               cameras, creating a seamless real-time motion
market. In this landscape, the AI personal shopping         capture zone. At the technical level, the AI computer
assistant serves as the central hub bridging digital        vision system rapidly identifies and extracts consumer
and physical consumption. The shopping assistant            silhouettes. Utilizing motion capture technology, it
sees an integration of LLMs, multi-dimensional              maps human poses onto virtual avatars in real-time,
user profiling systems, and mall digital twin               integrating them into preset interactive scenarios
mapping. Through these core capabilities, it can            such as VR virtual fitting previews, AR photo-ops, and
comprehensively upgrade commercial services,                interactive graffiti. In terms of interaction design, the
encompassing user intent understanding, intelligent         mall can set low-barrier game mechanics; consumers
recommendations, and context-aware navigation.              can participate on the spot via QR codes without
                                                            the need for additional apps. Upon completion,
Immersive, blended experience, from purchasing              the system automatically generates personalized
products to spending quality time: Interactive              posters, branded with the mall's LOGO and event
marketing games and instagram-worthy spots                  theme, alongside digital coupons. During the final
represent an immersive experience ecosystem built           redemption stage, these coupons are synchronized
on tech and entertainment. By using gamification            to the consumer's device, supporting omni-channel
to drive foot traffic and using AR/VR to deepen             usage across both online and offline platforms,
engagement, this model facilitates sales conversion,        ensuring a rewarding experience of consumption.
highlighting the shift in consumption from
functional fulfillment to emotional connection and          AI + data-driven operations and investment
spontaneity. Research by EventMB confirms that              attraction, from experience-based intuition
highly interactive experiences can boost brand              t o d a t a - i n fo r m e d d e c i s i o n - m a k i n g : A l l -
recall by 70%. Furthermore, the Action Plan for             domain traffic analysis and heat maps represent
the Integrated Development of Virtual Reality               a commercial spatial insight system driven by
and Industry Applications sets a clear target for           AI computer vision and IoT technologies. By
the total VR market to exceed CNY350 billion by             performing anonymized tracking and intelligent
2026. This underscores the strategic mandate for            analysis of customer behavior, it drives an
VR/AR and AI to contribute to the real economy,             operational paradigm shift from experience-based
empowering diverse industries across the board.             perception to data-driven quantization. An IDC
These scenarios have become a standard feature              report explicitly states that retailers that optimize
for major commercial complexes to attract foot              operations through data analytics see a 3–5%
traffic, transforming them from simple transactional        increase in sales. This system has now become a




                                                       52
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses



    must-have for leading shopping malls, propelling                           data middle-ends are leveraged to establish a
    commercial management from broad, experience-                              full-link, multi-dimensional analytical capability.
    based judgments into a new stage of precision                              By outputting core commercial metrics alongside
    data-driven strategies, thereby redefining the                             actionable optimization recommendations, it
    operational model of commercial real estate. AI                            ensures that data-driven support permeates every
    vision terminals, IoT positioning networks, and                            stage of the decision-making process.




         2.7 Education Campus: Enlightening Minds with
              AI to Build Personalized and Immersive
              Intelligent School Campuses

      2.7.1 Intelligent Classroom: From Passive Instruction to AI-Enabled
      Personalized Interaction

    Typical scenario: AI + smart classroom                                     Before class, AI assists teachers in teaching
                                                                               design by recommending content, identifying
    This scenario achieves a convergence of AI models,                         key challenges, and providing relevant cases and
    XR, and IoT, constructing a next-gen teaching                              exercises. It also suggests optimal teaching modes,
    space characterized by data-driven instruction,                            lecture workflows, and interaction strategies.
    immersive learning, and omni-channel collaborative                         Furthermore, LLMs or specialized AI assistants
    interaction. It removes the usual physical constraints                     can generate or refine teaching materials, such
    of conventional classes, and makes personalized                            as slide decks, assignments, and case analyses,
    education and immersive learning the new norm,                             based on the syllabus and learning objectives. AI
    establishing an efficient, interesting, and diversified                    also acts as a learning companion during the pre-
    teaching ecosystem (see Figure 22).                                        class preview, guiding students to think critically




            • The dedicated AI assistant generates                    Learning                    Student side
              or refines teaching materials such                      condition                   • XR devices restore abstract content
              as slides, exercises, and case analysis                                               and support gesture-based
                                                                   prediction and
              for teachers based on the teaching                                                    interactive virtual experiments.
              outline and course objectives.                       precise strategy
                                                                      matching                    • AI-powered voice assessment
            • Accompany students in previewing                                                      provides real-time feedback on
              lessons and collect questions.
                                                                     before class
                                                                                                    English pronunciation and generates
                                                                                                    personalized explanations for
                                                                                                    incorrect answers in class tests.
            • The system builds knowledge                Personalized             Immersive
              graphs based on students'                                                           Teacher side
                                                         consolidation          teaching and
              learning tracks and pushes                                                          • Classroom data is synchronized
                                                        and all-domain             real-time
              customized exercises and                                                              to the cloud. Teachers can view
              micro-courses.                            extension after          feedback in
                                                                                                    students' attentiveness and
                                                             class                   class          knowledge mastery on the smart
            • The AI system supports remote
              collaborative discussion.                                                             podium, and dynamically push
                                                                                                    learning resources to students.




                                                        Figure 22 AI + smart classroom




                                                                          53
                                           Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




and come up with their own questions. AI can               advanced learners, and delivering personalized
also collect students' questions and analyzes              guidance for those seeking clarification. AI,
pre-class learning data to evaluate their level            in synergy with XR, can construct curriculum-
of comprehension. By providing corresponding               aligned virtual environments that recreate abstract
recommendations, it enables teachers to deliver            content, such as microscopic molecular structures
more targeted instructions.                                and historical settings, thereby significantly
                                                           enhancing the overall learning experience.
During class, AI can act as a teaching assistant.
For example, it can collect and analyze students'          After class, AI builds knowledge graphs based
classroom performance (e.g. discussion and                 on students' learning tracks, generates exercises
question-raising), which helps teachers better             and assignments of different difficulty levels and
understand students' progress and adjust                   question types, and initiates personalized push
teaching strategies. AI can also generate topics           notifications. Once assignments are completed, the
for interactions, making the class more interactive        system performs intelligent grading and generates
and fun. When teachers need to control teaching            learning reports to help learners track students'
devices or the classroom environment, AI can               progress. Based on individual proficiency and skills,
easily understand teachers' intentions and                 it recommends post-class materials, including
leverage IoT to translate intentions into reality.         high-level content (e.g. academic papers, research
AI also functions as a learning companion for              reports, and case studies), to extend learning
students, offering real-time translation for non-          beyond the classroom.
native speakers, providing extension materials for




                                                      54
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses




         2.8 Community Wellness Campus:
              Intelligent, Inclusive Care for All Ages

      2.8.1 Community Living Space: Friendly Community for All Ages,
      from "Reactive Service" to "Proactive Care"

    The intelligent community space is the heart                       and compassionate care for fifth-generation
    of modern residences. It blends intelligent                        residences (see Figure 23).
    technology with human-centered care to create
    a welcoming, environment for all ages featuring                    Typical scenario: Seamless,
    HI-AI coexistence. This leads to a comprehensive                   comprehensive intelligent protection
    upgrade in living standards, service efficiency, and
    sense of community.                                                Intelligent monitoring of playgrounds and
                                                                       outdoor areas combines AI visual recognition
    Leveraging digital tools like AI agents, the                       with community services to ensure the safety of
    space offers proactive, personalized services—                     children in public spaces. A survey predicts that by
    understanding residents' needs before they do                      2025, 62% of communities in China will have these
    themselves. Acting as an "invisible life assistant,"               systems installed, with over 80% featuring functions
    it matches demands precisely and resolves                          that detect dangerous behavior—making them a
    concerns seamlessly, embedding intelligent                         standard configuration for new communities.
    support into every aspect of daily life without
    requiring residents to ask. Building on this, the                  This solution uses an "AI visual analysis + mobile
    space will focus on implementing scenarios such                    connectivity" framework to provide comprehensive,
    as Intelligent Guardian and Super Community                        intelligent monitoring of playgrounds and other
    Butler, addressing the key needs of all age groups                 outdoor areas, ensuring safety without blind
    and strengthening both intelligent livability                      spots. AI cameras equipped with advanced



                                                                  55
                                                         Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




       Intelligent identification of                   Dual system intervention                       Real-time parent linkage
           high-risk behaviors                       On-site voice alerts combined with              Status updates via the app and
   24/7 AI analysis, accurately identifying          warnings sent to the nearest safety                 instant geofence alerts
       climbing, jumping, and more                     officer for quick intervention




                                     Figure 23 Seamless, comprehensive intelligent protection

object detection algorithms and a database of                                manner, helping them establish safety awareness. In
risky behaviors are installed in public spaces                               addition, the system makes up for the shortcomings
like playgrounds and community squares. They                                 of manual care and greatly reduces the risk of
accurately identify dangers such as climbing on                              accidental injuries. The community management
high structures or over fences, touching sharp                               also achieves remarkable results. The intelligent
objects, or fighting, enabling timely safety alerts.                         care system is more effective than manual patrols
When a risk is detected, alert details—including                             alone, making security management more accurate
footage and location—are sent to parents who link                            and efficient. In addition, the construction of a safe
their child's profile via the community app, allowing                        community environment enhances residents' sense
them to monitor the real-time status in public                               of belonging and happiness.
areas. Additionally, the system's "electronic fence"
feature sends immediate notifications to parents                             Typical scenario: All-round and
if their child leaves the designated safe zone,                              proactive super community manager
providing an extra layer of protection.
                                                                             The proactive AI-powered super community
By building a dual safety network of "real-time                              manager, centered on agents across all spaces and
warning + parent collaboration," the system                                  focused on "insight-driven, service-first" goals, is
ensures children's outdoor safety. The dual safety                           making community services more proactive. It's set
network provides real-time voice reminders to                                to become a key feature in enhancing residents'
correct children's dangerous behaviors in a timely                           quality of life (see Figure 24).


Basic services

Multimodal sensing technologies, sensors, human
body sensing devices, and smart home appliance
data interfaces
                                                             Adaptive
• Precisely align with residents' daily habits and
                                                            community
  automatically adjust temperature, humidity,
                                                           environment
  lighting, and other environmental settings
  in homes and communities, ensuring a
                                                              control                                                         Robot services
  comfortable living environment.
                                                                                                     Self-service order placement and automatic
                                                                                                         closure, replacing manual tasks to fulfill
                                                                   An ideal        Intelligent    personalized intelligent service needs in homes
                                                                    home          community                                      and communities.
Personalized care                                                                    services    • Robots for coffee preparation, cleaning, item
                                                                                                    delivery, garbage collection, and inspection
Behavior analysis system, health data interface,       Personalized living
and voice interaction module                           and attentive care
• Intelligently recommend recipes based on                  services
  health data and available ingredients.
• Offer care services such as medication
  reminders and device parental controls.




                                 Figure 24 All-round and proactive super community manager


                                                                      56
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses



    This scenario addresses residents' full range of                   In personalized care, AI agents operate 24/7
    living needs by focusing on three core areas:                      to address service needs and respond swiftly
    basic community services, supply assurance, and                    to residents' needs. By analyzing user behavior,
    personalized care.                                                 health data, and voice inputs, the system delivers
                                                                       tailored services—such as intelligent recipe
                                                                       recommendations based on family health and
         As demonstrated by the New Youth                              ingredient availability, medication reminders for
         Community model, the NewLife OS                               seniors and children, freshness monitoring, and
         serves as the core platform to build                          automatic restock alerts.
         agents across all spaces, deeply
         reshaping user experience. These agents                       Robotic services handle daily tasks like coffee
         integrate with smart homes, combining                         making, cleaning, deliveries, waste disposal, and
         whole-house voice control to enable                           inspections. Residents can place their own orders,
         seamless, personalized interactions                           enabling fully automated fulfillment that meets
         across devices and scenarios.                                 personalized smart service demands for both home
                                                                       and community.
         Lianfa Group Co., Ltd.

                                                                       Data-driven AI decision-making combined with
                                                                       robotic execution enables seamless collaboration
    For basic community services, multimodal sensing                   between spatial agents and robots. This integration
    technologies—including sensors, motion detectors,                  equips communities with both a "smart brain" and
    and smart appliance interfaces—precisely adjust                    "active hands," creating safe, green, comfortable,
    temperature, humidity, and lighting to create a                    and intelligent living spaces. It also offers a
    comfortable environment. Around-the-clock robot                    replicable model for upgrading community
    patrols equipped with AI risk detection provide                    environmental control from single function to all-
    real-time alerts, strengthening safety. AI analytics               domain intelligence.
    coordinate with robots to execute tasks efficiently.




      2.8.2 Intelligent Elderly Care Space: From Basic Support to Warm
      Companionship

    Typical scenario: Proactive health                                 human warmth, using lightweight intelligent
    management with seamless care                                      devices and AI to automatically track health
                                                                       data, proactively deliver care, and accurately
    As populations age and health awareness                            address emotional needs.
    grows, traditional methods for monitoring
    the health of elderly like making regular calls                    T h i s a p p ro a c h ce n t e r s o n t h re e p i l l a r s :
    are not always effective. The industry's future                    safety monitoring, health management, and
    lies in blending technology with empathy.                          companionship. It's poised to become one of the
    True intelligent elderly care spaces will use                      most widespread AI applications (see Figure 25).
    technology as a "silent guardian" wrapped in




                                                                  57
                                                    Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




       Safety monitoring & prevention                                             All-around life companion



   Data collection                                                                                       Health management

Intelligent devices seamlessly and                                                                     Recommend personalized dietary
automatically collect health data, such                                                              programs and recipes based on the
as sleep and breathing patterns, from                                                                          elderly's health records.
the elderly.


                                                        Seamless,
   Risk identification                                                                                              Life assistant
                                                     proactive health
Flawlessly detects potential
emergencies such as falls,
                                                      management                                       AI agents autonomously manage
                                                                                                            essential living supplies and
prolonged bedrest, and extended                                                                                     medical necessities.
bathroom visits.



   Tiered warning                                                                                Emotional companionship

Push warnings based on severity and automatically                                              Utilize AI voice interaction and intelligent
notify family members and community staff via                                                     dialogue to engage the elderly in daily
phone calls, WeChat, and other channels.                                                       conversations, helping to ease loneliness.




                                  Figure 25 Proactive health management with seamless care



S a fe t y m o n i t o r i n g & p re ve n t i o n : U s i n g        in-one life companions" attuned to seniors'
contactless intelligent devices like mmWave                           needs. By integrating health records (e.g., high blood
radar, there's no need for wearables or cameras.                      sugar), AI recommends low-sugar meal classes and
These devices detect falls and unusual bed rest                       recipes. It also offers emotional support through
without intrusion, sending tiered alerts to family                    conversational AI, helping reduce loneliness.
and community. They automatically collect sleep
and breathing data without disturbing daily                           This model fundamentally transforms elderly care:
routines, triggering immediate warnings for                           • For seniors, it ensures dignified, private, 24/7
events like nighttime falls or apnea. Sensors in                         safety and personalized health support, boosting
bedrooms notify family if someone stays in bed                           autonomy and well-being.
too long; bathroom falls that are unattended for                      • For families, it provides transparent health data,
over a minute trigger high-level alerts via calls and                    easing caregiving stress.
messages; and leaving the bed for over 30 minutes                     • For providers, it shifts care from reactive to
at night generates reminders.                                            proactive, optimizing resources and cutting costs.
                                                                      • For society, it builds an intelligent elderly care
Companionship & health management: AI                                    ecosystem, a vital infrastructure to address aging
evolves beyond simple voice assistants into "all-                        populations.




                                                                 58
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses




         2.9 Electric Power Campus: Intelligent Electric
              Power Enables a Zero-Carbon, Safe, and
              Efficient Power Supply System

      2.9.1 Transmission and Distribution Network Space: From "One-Way
      Delivery" to "Zero-Carbon Autonomous" Intelligent Control

    Typical scenario: Autonomous and                                   comprehensive campus energy needs across all
    efficient zero-carbon power grid space                             scenarios. Supported by digital twins and AI-
                                                                       based scheduling, the system provides full-
    Driven by China's "dual carbon" strategy (reaching                 chain visualization, flexible energy management,
    peak carbon dioxide emissions before 2030 and                      and zero-carbon coordination, establishing a
    carbon neutrality before 2060), leading power                      safe, efficient, and green demonstration model
    companies such as State Grid and China Southern                    for campus power networks. Looking ahead,
    Power Grid, together with major power generation                   transmission and distribution network spaces
    groups, are leveraging their core grid assets and                  will evolve beyond electricity delivery into
    integrating distributed energy units (e.g., solar                  hubs for showcasing zero-carbon technologies,
    PV and energy storage) within campuses. This                       intelligent dispatch, and carbon-electricity
    enables the development of a unified smart                         transaction, offering replicable models for
    transmission and distribution system—generation-                   building future power systems (see Figure 26).
    grid-load-storage-charging—designed to meet




                                                                  59
                                                        Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




Intelligent microgrid autonomy                                                                                     Digital twin O&M
Focus on distributed PV on campuses to                                                             Surplus power storage and peak shaving
deliver clean, green energy.                                                                                     improve energy flexibility.
                                                               Dual carbon
                                                                  strategy
                                                               Generation-
                                                                 grid-load-
                                                             storage-charger
Collaborative electricity and                                   integration
carbon management                                                                                   Safe and efficient operations
Leverage core grid resources to build                                                             Deeply integrate charging piles to support
an efficient power transmission and                                                                    the development of electric vehicles.
distribution network.


                                        Comprehensively meeting power consumption requirements


                                                                                                     Accurate carbon emission accounting
          Visualized monitoring and                       Real-time generation-load matching
                                                                                                     achieves zero-carbon closed-loop
          intelligent inspection increase                 and optimized energy distribution
                                                                                                     management throughout the
          O&M efficiency by 50%.                          ensure stable power supplies.
                                                                                                     entire process.




                             Figure 26 Autonomous and efficient zero-carbon power grid space



Intelligent autonomy of campus microgrids:                                Carbon-electricity coordinated control: A cloud
The transmission and distribution network deeply                          platform collects energy usage data across the
integrates with the distributed PV, ESS, and                              grid, accurately calculating carbon emissions from
chargers on the campus to form an independent                             losses and equipment. Linked to carbon trading
and controllable microgrid system. AI algorithms                          markets, it smartly recommends green power
balance PV output with office and production loads                        purchases and offset plans, ensuring precise
in real time. Excess power is stored or fed back                          carbon accounting and control. Dynamic pricing
to the grid. During extreme weather, the system                           guides load shifting within the campus, optimizing
switches to "island mode" to maintain critical loads,                     energy use and enabling full zero-carbon lifecycle
ensuring self-sufficiency and flexible interaction.                       management.


Digital twin O&M of power transmission and                                Safe and efficient operat ion: Leveraging
distribution networks: A 1:1 digital twin replicates                      intelligent inspection robots and digital twins,
transmission lines, distribution equipment, and                           the system remotely diagnoses equipment and
underground networks. Real-time data enables                              predicts faults, reducing losses and ensuring stable,
visual monitoring, load simulation, and fault                             continuous power supply.
tracing. Virtual simulations predict overloads and
aging risks, automatically generating optimal O&M
plans. Intelligent robots then perform inspections
and repairs, boosting efficiency by over 50%.




                                                                     60
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses




         2.10 Mining Campus: Intelligent Management to
               Turn High Risk into High Efficiency

      2.10.1 Mine Operation Space: From High-Risk Operations to Zero-
      Blind-Spot, Safe, and Efficient Mining

    Typical scenario: 24/7 intelligent mine
    control with zero blind spots                                            We deployed the real-time video
                                                                             analysis platform for hazard warnings,
    Mining environments are harsh and geologically                           the intelligent O&M assistant for
    complex, making traditional manual supervision                           predictive device maintenance, and the
    or single-channel communication inadequate for                           intelligent Q&A system for data-driven
    balancing safety and efficiency. Accelerated digital                     decision-making to shift mining from
    innovation is now driving rapid intelligent mine                         reactive to proactive management—
    development. The key challenge is strengthening                          setting a replicable standard for safe,
    safety underground and in open pits, improving                           efficient, green mining.
    full-process efficiency, and achieving precise
    operational control (see Figure 27).                                     Wanchow Qizhi (Qingdao) Information
                                                                             Technology Co., Ltd.




                                                                  61
                                                              Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




Precise to the centimeter positioning                                                                 Precise positioning in blind spots
and control                                                                                                      UWB with high positioning accuracy
BeiDou + RTK
                                                                                                    Wearables with positioning tags and safety guard
                                                                                                    functions
 Operating vehicle and personnel
 positioning, accurate to centimeters                                                               AI-triggered hierarchical alarms (e.g., falls, excessive
                                                                                                    loitering, and unauthorized entry into hazardous zones)

 Command center: Captures
                                                                         Underground
 accurate coordinates of people and           Open-pit mine                                             Command center management
 vehicles in real time and accurately                                     operations
 defines operational boundaries
                                                                                                  5G high-bandwidth network + Mine private network

                                                                                                    AI-based real-time monitoring and analysis of personnel
                                                                                                    distribution heat maps and density in specific areas


                                                                                                    Optimized inspection routes and frequency, enabling
                                                                                                    transparent management and instant response to
                                                                                                    underground operations




                                        Figure 27 24/7 intelligent mine control with zero blind spots


Building a "zero-blind-spot, 24/7" intelligent                               prevent unauthorized operations, ensuring safe,
personnel management system is essential for                                 regulated workflows.
future intelligent mine campuses. With mature
integration of BeiDou positioning, 5G, and ultra-                            Underground, where satellite signals can't reach,
wideband (UWB) ranging technologies, previously                              UWB technology offers interference-resistant, sub-
in visible mining operations are becoming                                    30-cm precision positioning. Coupled with 5G and
transparent, visualized, and controllable. This not                          dedicated mine networks, personnel locations,
only meets safety compliance but also forms the                              movement paths, and environmental data (e.g.,
foundation for lean, safe operations—tracking                                gas, dust levels) are transmitted in milliseconds
every miner's location and safety status in real                             to command centers. Miners' intelligent devices
time to ensure a safe working environment.                                   serve as both location tags and safety monitors—
                                                                             triggering tiered alarms and linking to video
In open-pit sites, BeiDou satellite combined with                            security if falls, prolonged inactivity, or entry into
real-time kinematic (RTK) technology delivers                                restricted zones occur. Managers can view heat
centimeter-level positioning accuracy for vehicles                           maps of personnel density, review historical routes,
and personnel. Whether in complex blasting zones                             and optimize the inspection path and frequency,
or busy haul roads, command centers track precise                            ensuring transparent underground operations and
coordinates, enforce boundary controls, and                                  rapid response to any incidents.


                                                                             This approach shifts mine safety from reactive
       Autonomous drones create a dynamic                                    accountability to proactive prevention. For miners,
       low-altitude monitoring network,                                      intelligent devices and full-area positioning
       automatically capturing mining progress                               create a robust safety net, significantly reducing
       and detecting risks like deformation,                                 accidents. For mining companies, visualized
       cracks, collapses, and water ingress on a                             "people, machine, environment" data enables
       regular basis, enabling timely alerts and                             refined decision-making, boosting operational
       risk prediction.                                                      efficiency by over 30% and cutting emergency
                                                                             response times by half. Ultimately, this tech-driven
       Sichuan Timwave Yunze Intelligent                                     model advances mining toward intrinsic safety and
       Technology Co., Ltd.                                                  sustainable growth.




                                                                        62
02 Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses




         2.11 Research Labs: Intelligent Support for
               Breakthroughs Without Burnout

      2.11.1 Research and Innovation Space: From Purely Manual to
      Human-Machine Collaboration as a Research Engine

    Typical scenario: AI for Science (AI4S)                            compute-driven models, leveraging AI to handle
    innovation and R&D                                                 massive calculations and pattern recognition.
                                                                       This enables scientists to focus on hypotheses
    As seen in Nobel-winning molecular dynamics                        and breakthroughs, achieving seamless human-
    research based on deep learning, AI has evolved                    machine synergy.
    from a mere tool to a core driver of discovery
    (see Figure 28). National research labs, key to                    AI-powered, full-scenario collaboration frees
    overcoming critical technology bottlenecks and                     researchers from time and location limitations,
    building strategic scientific strength, face challenges            providing instant access to data, literature, and
    like explosive data growth, soaring compute                        tools. AI accelerates meeting scheduling, report
    demands, complex workflows, and cross-institution                  generation, approvals, and document management,
    collaboration barriers.                                            tripling office efficiency and letting scientists focus
                                                                       on innovation. For example, a Shenzhen brain
    Against this backdrop, integrating AI under the AI4S               science lab remotely adjusts neuron imaging
    vision will create a unified system of intelligent                 parameters and shares progress in real time,
    research, efficient management, compliance,                        significantly shortening project cycles.
    and open collaboration, shifting research from
    experience-driven to data- and AI-driven.                          This transformation empowers research on
                                                                       multiple levels.
    Future labs will move from labor-intensive to



                                                                  63
                                                        Innovating with AI: Faster Evolution Toward Intelligent Spaces for AI Campuses 02




           Efficient office                          Scientific research security                           Scientific research services

  AI-driven collaboration, unlocking      Dual-dimensional protection system, ensuring E2E                 Full-process closed-loop enablement,
  researchers' core productivity          security                                                         supporting decision-making
  AI + Office collaboration platform      AI deeply integrated into the entire process of basic            Scientific research database + AI
  • Researchers can access experimental   research and technological breakthroughs                         assistant
    data, literature, and collaboration   • Using supercomputing power, digital twin, and research         • Provide closed-loop services for the
    tools anytime, anywhere.                databases, the system processes vast amounts of data,            entire process of topic exploration,
  • Intelligent office processes            executes experimental simulations and optimization, and          data collection, achievement
    accelerate conference scheduling,       manages scientific research risks, pushing the frontiers of      output, and paper publication.
    report generation, and process          fields like quantum computing, fusion, and climate science.    • The AI assistant analyzes millions
    approvals.                            Intelligent computing power and resource management                of documents, constructs an
                                          • The collaborative scheduling of intelligent computing,           interdisciplinary knowledge
                                            supercomputing, and general-purpose computing, along             graph, and automatically organizes
                                            with AI-based dynamic allocation algorithms, boosts the          experimental data while generating
                                            computing power utilization from about 40% to over 85%.          analysis reports.




                                            Figure 28 AI4S innovation and R&D



                                                                            comprehensive safeguards.
     AI is becoming deeply embedded
     in fundamental research and tech                                       • For data governance, a cross-institutional
     breakthroughs. Using supercomputing                                        sharing platform compliant with the findable,
     power, digital twin, and research                                          accessible, interoperable and reusable (FAIR)
     databases, the system processes vast                                       principle is established, utilizing unified
     amounts of data, runs experimental                                         metadata and blockchain-based storage
     simulations and optimization, and                                          to break down data silos while enabling
     manages scientific research risks,                                         traceability and secure reuse.
     pushing the frontiers of fields like
     quantum computing, fusion, and                                         • In experimental safety and compliance, an
     climate science.                                                           AI-driven, full-process intelligent protection
                                                                                system provides real-time risk alerts within
     iSoftStone Smart Technologies Co., Ltd.                                    s e c o n d s . I t a s s i s t s p e r s o n n e l i n h a z a rd
                                                                                avoidance through image recognition, sensor
                                                                                linkage, and robotic substitution. Blockchain-
                                                                                based storage is further employed to enhance
The collaborative scheduling of intelligent                                     intellectual property protection.
computing, supercomputing, and general-
purpose computing, along with AI-based dynamic                              AI's role in empowering national scientific
allocation algorithms, can boost computing                                  laboratories extends beyond boosting efficiency
power utilization from about 40% to over 85%.                               and cutting costs. It also creates a replicable,
Combined with IoT and predictive maintenance,                               measurable framework for intelligent research.
equipment failures drop 75%, repair costs fall                              This framework charts a clear path for labs to
40%, and digital twins enable full lifecycle asset                          evolve and collaborate within their ecosystems,
tracking for simplified O&M.                                                positioning AI as both a supercomputing engine
                                                                            and an innovation accelerator that underpins
Scientific research security is ensured through                             technological self-reliance and advancement.



                                                                      64
65
03
Building with AI:
Key Technical Features of AI Campuses




               66
03 Building with AI: Key Technical Features of AI Campuses




         3.1 HI-AI Coexistence
    In the evolution of the AI campus, HI-AI coexistence                  shift in campus intelligence—from automation
    transcends traditional human-machine interaction,                     t o a u t o n o m y, a n d u l t i m a t e l y t o wa rd t r u e
    forming a deeply collaborative, mutually enhancing                    symbiosis. The following sections examine this HI-
    partnership between human and AI. Rather than                         AI coexistence through three dimensions: HI-AI
    merely assigning tasks to machines, it creates                        interaction, AI-AI interaction, and adaptive multi-
    an ecosystem where humans and AI systems                              agent. We delve into its essence, implementation
    integrate seamlessly at cognitive, decision-making,                   pathways, and future trajectory.
    and creative levels. This signifies a fundamental


                             HI-AI interaction                                              AI-AI interaction
                      Proactive intent     Multimodal natural                    Autonomous service       Distributed resource
                        recognition            interaction                          collaboration             collaboration




                                                            HI-AI coexistence
                                     Establishing a new partnership between human intelligence (HI) and AI,
                                    featuring deep collaboration, bidirectional enhancement, and co-evolution




                                                         Adaptive multi-agent
                                         Distributed solving of                Knowledge co-creation
                                          complex problems                      and open evolution




                                                        Figure 29 HI-AI coexistence




                                                                     67
                                                                       Building with AI: Key Technical Features of AI Campuses 03




  3.1.1 HI-AI Interaction: From Passive Response to Active Recognition

HI-AI interaction is the foundation for HI-AI                    knowledge solidification algorithms, individual
coexistence, evolving from precise command                       experience is transformed into reusable rules and
execution to profound intent recognition. The goal               strategy libraries, turning personal expertise into
is for AI to truly understand human needs, enabling              organizational assets and providing robust support
natural, efficient collaboration to create a new                 for real-time warnings and assisted decision-making
interaction paradigm of mutual empowerment                       in production processes.
between humans and machines.
                                                                 3.1.1.2 Multimodal Natural Interaction
3.1.1.1 Intent Recognition
                                                                 Multimodal natural interaction creates an intelligent
AI is advancing from interpreting explicit commands              environment that deeply merges the physical and
to comprehending dynamic and implicit user intent.               digital worlds, enabling natural perception and
Central to this effort is building a continuously                control of information. It integrates high-fidelity
learning cognitive model that closes the loop from               sensing and low-latency multi-channel feedback,
command parsing to intent understanding, and                     upgrading from one-way commands to two-way
finally to proactive enablement. By integrating                  intelligent interaction.
multimodal perception, domain-specific knowledge
graphs, and behavior modeling, the system can                    Inputs include voice, gestures, eye tracking, and
deeply analyze user roles, historical preferences,               haptic feedback for seamless, precise intent
context, and multi-source operational data. This                 capture. For example, eye tracking confirms focus,
enables a transition from reactive response to                   gestures enable precise object control, and haptic
proactive anticipation.                                          gloves provide tactile feedback, creating immersive
                                                                 interaction beyond conventional inputs.
In office settings, this ability translates into context-
aware intent inference and decision support. The                 Outputs use holograms, AR/VR, and adjustable
system analyzes speech, text, and nonverbal cues                 environmental controls (light, temperature,
during meetings, alongside intent classification                 sound) to create immersive, mixed-reality spaces.
models and association-rule mining, to detect                    For instance, in emergency command centers,
decision-making needs. For example, it can identify              commanders can outline an area on a 3D digital
an implicit request for solution pathways within a               sandbox using gestures and issue a voice command
discussion about growth bottlenecks, then trigger                like "show personnel distribution and structural
the knowledge-retrieval engine and automated                     safety here." The system instantly overlays real-
reporting module to proactively deliver structured               time heat maps, structural stress visualizations,
industry benchmarks and actionable options.                      and dynamic evacuation simulations, streamlining
                                                                 command execution and feedback.
In production, intent recognition focuses on
extracting and systematically codifying tacit                    Huawei's agent Celia exemplifies this multimodal
knowledge. Leveraging visual analytics, operational              interaction.
sequence modeling, and real-time linkage
with IoT data, the system accurately interprets                  • Context-aware sensing and proact ive
experts' decision logic under specific working                      adaptation: Celia detects the user's setting (e.g.,
conditions. Through event graph construction and                    meeting room or private office) and calendar




                                                            68
03 Building with AI: Key Technical Features of AI Campuses



       events (e.g., product review), and automatically                         available in 10 minutes, as most members are
       switching modes accordingly. In meetings, it                             close to this room. Agenda and materials sent to
       silences non-critical notifications and, based                           all members. Remote attendee detected. Cloud
       on discussion keywords, surfaces relevant past                           meeting and screen sharing activated."
       decisions or data dashboards on a sidebar.
                                                                             • Task collaboration and process assistance:
    • Tra n s p a re n t o p e ra t i o n a n d s e a m l e s s                 For multifaceted requests like "compile last
       coordination: When asked to gather the project                           week's customer feedback and highlight key
       core team, Celia suggests and books the nearest                          pain points," Celia can gather data across
       available room using the team members' location                          systems (e.g., CRM, email), organizes it into a
       and schedules, coordinates device connections,                           structured draft summary, and flags ambiguous
       and explains clearly: "Room A301 booked and                              or contradictory items for user review.




      3.1.2 AI-AI Interaction: Autonomous Collaboration Among AI Systems

    A I - A I i n t e ra c t i o n m a r k s a c h a n g e t h e A I         3.1.2.2 Distributed Resource
    campuses from isolated AI agents to collaborative                        Collaboration
    agent net works, building an ecosystem of
    agents connected via standardized interfaces                             Effective agent collaboration depends on dynamic
    a n d d y n a m i c re s o u rc e s c h e d u l i n g . T h e s e        access to computational, data, and network
    heterogeneous agents (security, energy, service                          resources. The key is unified management and
    robots) autonomously negotiate tasks, share                              intelligent awareness of campus-wide cloud-edge-
    resources, and exchange knowledge—forming a                              device compute capacity. When a collaborative
    flexible, evolving intelligent society.                                  task demands heavy processing (such as real-time
                                                                             multi-stream video fusion), the system automatically
    3.1.2.1 Autonomous Service                                               splits the task and routes computing resources to
    Collaboration                                                            the nearest edge node or campus cloud, making
                                                                             compute follow the task. Resource scheduling also
    Semantic and intent-based collaboration forms the                        incorporates QoS guarantees, dynamically adjusting
    language and social rules of AI interaction. Agents                      network bandwidth and compute priority according
    publish functions through standardized APIs,                             to task urgency (e.g., routine monitoring vs.
    creating a unified AI service catalog for discovery                      emergency response) to ensure reliable critical-task
    and capability sharing. When complex tasks arise                         execution. Data virtualization and lakes abstract
    (e.g., securing VIP routes), agents form temporary                       scattered data into secure, on-demand services
    virtual teams via lightweight distributed negotiation.                   accessible via standard APIs—allowing AI agents
    For example, navigation, elevator control, crowd                         to collaborate without moving raw data, balancing
    analysis, and lighting AI systems coordinate                             efficiency and privacy.
    autonomously to optimize guidance, overcoming
    rigid centralized control and enhancing resilience in
    responding to unexpected events.




                                                                        69
                                                                                    Building with AI: Key Technical Features of AI Campuses 03




  3.1.3 Adaptive Multi-Agent: Emergence and Ecosystem Evolution

Adaptive multi-agent elevates AI campuses from                                 3.1.3.2 Open Knowledge Co-Creation
coordinated execution to ecosystem evolution.                                  and Evolution
The system self-organizes in a decentralized
way to tackle complex global challenges and                                    The campus will become a living knowledge
c o n t i n u o u s l y re f i n e k n o w le d g e , a c h i e v i n g        community, making tacit knowledge explicit and
capabilities beyond individual AI agents.                                      continuously accumulating collective wisdom via
                                                                               an open human-machine collaboration platform.
3.1.3.1 Distributed Problem Solving                                            For challenges like optimizing human-factor
                                                                               efficiency in R&D environments, the platform
Instead of relying on central commands, the                                    breaks tasks into micro-tasks for cross-domain
system achieves a global optimum through local                                 experts and domain AI (e.g., spatial analysis and
decision-making and interaction among many                                     behavior modeling). Experts provide insights and
agents. For example, in campus-wide energy                                     judgments; AI generates solutions, simulates
optimization, building-energy AI agents act                                    outcomes, and uncovers patterns. Through mixed
autonomously, balancing cost and carbon signals                                human-machine evaluation and integration, the
from microgrids. Operators only set top-level                                  platform yields not only innovative solutions but
constraints (e.g., total carbon cap), and then the                             also captures decision logic and optimization
system dynamically balances supply and demand                                  parameters as reusable knowledge assets, feeding
across the energy network, enabling efficient                                  a shared knowledge base that drives ongoing
peak shaving and green energy use.                                             system evolution.




                                                                          70
03 Building with AI: Key Technical Features of AI Campuses




         3.2 Data-AI Convergence
    At the heart of AI campuses, data-AI convergence                    intelligent ecosystem. Within two to three years,
    unlocks data's full value. It moves beyond                          powered by HarmonyOS, edge computing, and AI
    the question of whether data can be used to                         autonomy, this development will transform passive
    ask, is this data useful and valuable? Through                      data management into active value creation. This
    standardization, fusion, and assetization, data                     will substantially boost operational efficiency and
    within campuses evolve from isolated silos to                       innovation capacity, advancing toward a new model
    intelligent flows, ultimately forming an organic                    of data-driven operations and revenue.




                                                               Data-AI
                                                             convergence




               Data-AI convergence                  Interconnection and sharing               Asset accumulation

                One thing, one model                    Middle-end foundation                  Asset encapsulation


                 Semantic alignment                    Closed-loop collaboration                 Internal training


                    Simplified IoT                            Secure sharing                    Asset monetization




                                                 Figure 30 Data-AI convergence


                                                                   71
                                                                                                                Building with AI: Key Technical Features of AI Campuses 03




  3.2.1 Unified Thing Model

Diverse devices and protocols create a Tower of Babel                                                as popular ontologies) and standards like IFC
where data can't interoperate. Data standardization is                                               and Brick Schema to semantically tag terms
a linguistic foundation of sorts for AI campuses, in this                                            within models. For example, energy consumption
way. Without unified semantics, data becomes digital                                                 consistently means kWh, and zone links to
waste. Only a consistent thing model lets thousands                                                  GIS coordinates. This semantic layer decouples
of devices speak the same language, providing a                                                      business logic from data formats, enabling precise
trustworthy base for intelligent applications. This                                                  cross-system queries and inference.
section examines how native protocol unification
and semantic modeling enable data to be universally                                                  For legacy non-standard devices, lightweight expert
interpretable from the point of generation.                                                          models at the edge identify proprietary messages in
                                                                                                     real time and automatically map them to a unified
3.2.1.1 One Thing, One Model                                                                         campus dictionary. This AI-driven format conversion
                                                                                                     replaces manual plugin development, greatly
Each physical entity (e.g., access control, meters,                                                  reducing digital transformation effort and ensuring
cameras) is abstracted into a structured digital                                                     a highly standardized data foundation.
template capturing their core attribute, state, and
behavior. For example, the intelligent lighting                                                      Semantic alignment also involves spatial modeling,
model standardizes fields like on/off status, color                                                  upgrading traditional key-value data into attribute
temperature, power consumption, and location.                                                        sets enriched with spatial and temporal context. For
Regardless of their vendors, all lights are mapped into                                              instance, a model not only records AC temperature
a common logical structure. This avoids redundant                                                    25°C but also links it to its building information
modeling and enhances system interoperability.                                                       modeling (BIM) coordinates, workstation ID, and
                                                                                                     energy consumption weight. This self-identifying
3.2.1.2 Semantic Alignment                                                                           data structure provides ready-to-use inputs for AI-
                                                                                                     driven indoor path planning and precise energy-
We integrate vertical industry modeling (such                                                        efficiency-based scheduling.

                                                   Energy
                                                                                         Asset               Network
                                                consumption                                                                                               ...
                                                                                       application          management
                                                 application

                       Digital world                                                      Spatial data openness
                                                              (spatial relationship, devices, indicators, events, and intelligent interaction)

                                                              Spatial assets                                             Soft spatial indicators
                                                    Region, location, description,                                     People ﬂow, energy consumption,
                                                          and association                                                      asset, and event                         Data task
                                                                            Floor
                                                   Building




                                                                                            Room
                                                                            Floor




                                                                                                                                    Physical device points
                                                                                                         Seat




                                                                                            Room                                  Status, battery level, and location




                                                 Building             Floor          Room     Location          IH      IPC            Tablet               AP
                                                                                                                                                                        Device
                                        Space




                                                   G1                  06            D25       01S          ...         ...              ...                ...




                       Physical space




                              Figure 31 Spatial semantic alignment of campus data


                                                                                            72
03 Building with AI: Key Technical Features of AI Campuses



    Using HarmonyOS's distributed soft bus                        secure channels. The soft-bus abstracts protocol
    technology, campus devices achieve discover-                  differences, allowing developers to focus on calling
    and-connect functionality, moving away from the               capabilities rather than connection methods.
    conventional connect-then-convert approach.
    Native protocol support eliminates conversion                 Unified thing modeling is the bedrock of data-
    losses common with Modbus, BACnet, and similar                AI convergence. Through one thing, one model,
    protocols. Devices are no longer isolated hardware            semantic alignment, and simplified IoT, the campus
    units. Instead, they become virtual super-terminals           builds an intelligible, scalable, and collaborative
    linked via the soft bus, naturally aligning data              digital twin foundation, ensuring data carries
    formats at the link layer for plug-and-play and               intrinsic value for exchange from the source.
    automatic networking.
                                                                  From HarmonyOS soft bus to semantic thing
    Through near-field communication such as                      models, AI campuses achieve a transformation
    Bluetooth, Wi-Fi, or NearLink, devices register               from universal connectivity to universal semantic
    automatically with the unified thing model                    alignment, laying a robust, high-quality data
    repositor y upon connection and establish                     foundation for intelligent campus decision-making.




      3.2.2 Interconnection and Sharing

    Campus operations in volve more than 10                       foot traffic prediction and device fault diagnosis)
    subsystems, such as security, energy, property                and provides services in the form of APIs. The data
    management , and transportation. The                          and AI middle-ends become the "converter" and
    traditional siloed architecture leads to data                 "accelerator" for system interconnection.
    fragmentation and slow responses. The essence of
    interconnection, then, is to break the boundaries             The data middle-end uses real-time lakehouse
    of vertical systems, implement cross-domain                   architecture that integrates batch and stream
    convergence, and unleash the synergy of "1+1>2".              processing to integrate security video streams,
    This section focuses on how to use the middle-                batch personnel access data, and real-time IoT
    end architecture that combines batch and stream               sensor data. The technology eliminates logical
    processing and the cross-system AI collaboration              silos between systems. For example, when the
    mechanism. Once combined, they can transform                  security system triggers an alarm for elevator
    isolated security, energy, and office systems into            congestion, the AI middle-end, along with the
    an organic, collaborative whole.                              elevator and air conditioning control systems,
                                                                  are instantly notified. The AI middle-end then
    3.2.2.1 Middle-End Foundation                                 automatically adjusts the elevator and air
                                                                  conditioning, demonstrating smart collaboration
    The data middle-end and AI middle-end are built               across scenarios.
    as dual engines. The data middle-end aggregates
    multi-source heterogeneous data; cleans, aligns,              3.2.2.2 Closed-Loop Collaboration
    and labels the data; and forms three theme libraries
    covering people, space, and events. The AI middle-            The AI middle-end can implement intelligent
    end encapsulates algorithm capabilities (such as              orchestration across systems by using the shared



                                                             73
                                                                    Building with AI: Key Technical Features of AI Campuses 03




data context. For example, a visitor reservation              3.2.2.3 Secure Sharing
can trigger the entire automated process including
access control, calling the elevator, pre-starting air        Privacy computing (such as federated learning
conditioning in the conference room, and guidance             and secure multi-party computation) and fine-
in the parking lot. Such collaboration is not hard-           grained permission control are used to ensure
coded, but dynamically configured through the                 that data is available but invisible, controllable,
low-code process engine, supporting quick iteration           and auditable. For example, the energy
and personalization.                                          consumption platform can obtain the conference
                                                              room usage status to optimize policies, but
Data and AI sharing can also enable closed-loop               cannot identify the specific users. The security
evolution. Through online continual learning,                 mechanism ensures that data convergence and
campus spatial data (such as Wi-Fi, Bluetooth                 sharing do not result in data leakage.
beacon locations, foot traffic, and tracks) can be
used not only for display, but also for AI training           Interconnection and sharing allow a campus to
and learning. The system automatically corrects               evolve from "function stacking" to "smart sharing".
the algorithm model based on the feedback data.               Through the middle-end foundation, closed-loop
This mechanism of "data-driven algorithms and                 collaboration, and secure sharing, data can flow
algorithm-driven services" allows campus data and             freely under control. This creates efficiency, agility,
AI to be self-optimizing.                                     and a user-friendly service experience.




  3.2.3 Data Accumulation

Data is not only a factor, but also a resource that           3.2.3.1 Data Encapsulation
can be owned, priced, and traded. The AI Campus
solution transforms data from a cost center to a              Based on the concepts of model context protocol
value center, explores sustainable business models,           (MCP), agent to agent (A2A), and skills, campus
and capitalizes on the potential of a campus                  data and capabilities can be encapsulated into AI
digital economy. Data assetization is a closed-loop           assets. For example, vertical capabilities such as
business for the sustainable development of the               device management, space services, and indoor
AI Campus solution. This section discusses how                navigation can be encapsulated into standard
to transform the discrete data accumulated in a               AI toolkits. These toolkits are accumulated as AI
campus into quantifiable, assessable, and tradable            assets, which can be invoked by internal agents and
assets, and how to transform the operation mode               provided to enterprises as standard components. For
from "operating space" to "operating assets".                 example, high-precision positioning services can be
                                                              encapsulated into assets and provided to logistics
                                                              enterprises for the instant monetization of data.




                                                         74
03 Building with AI: Key Technical Features of AI Campuses



                                                                          MCP protocol:
                                                                                                    MCP service asset
                                                                          1) List
                                                                          2) API description        Data or API tools
                                                       MCP host
                                                                                                    MCP service asset
          Large
                           Natural language                                                         Data or API tools
          model               or Function
                            Calling protocol             MCP
                                                         client                                     MCP service asset
                                                                                                    Data or API tools



                                                                                                           ...

         Figure 32 MCP, a method for encapsulating data into assets for large models or agents to invoke



    A campus data asset catalog can also be                            rooms into small, flexible spaces, and adding
    established to manage metadata, mark ownership,                    collaboration and negotiation areas) and optimizing
    and rate the quality of high-value datasets (such                  workstation layouts (for example, implementing
    as foot traffic heat maps, carbon emission curves,                 flexible workstations) to improve space utilization
    and device health indexes). This provides a                        and return on investment (ROI). Using employee
    technical basis for the assetization of campus data                movement data, suggestions can be provided on
    by using end-to-end evidence tracking for data                     optimizing canteen lines, shuttle bus routes, and
    generation, transfer, and use.                                     bus stops, to reduce employees' commuting costs
                                                                       and enhance the satisfaction of commercial tenants
    3.2.3.2 Data Enablement                                            and employees.


    Data enablement is the core scenario for releasing                 In the energy management scenario, assets
    the value of data assets. It aims to feed back the                 such as energy consumption benchmark data,
    encapsulated digital assets to the entire campus                   carbon emission curves, and device energy
    operation process, verify the asset value through                  efficiency can be used to drive green operations.
    scenario-based usage, and optimize asset iteration.                An AI optimization model built based on energy
    This results in an upward spiral of "assets enabling               consumption assets can accurately locate high
    services and services feeding back to assets".                     energy consumption phases and create relevant
    Through the integration of data assets and service                 energy saving solutions. The energy consumption
    scenarios, the operation efficiency can be improved,               data is combined with lease management to
    costs can be controlled, service experience can be                 introduce green lease clauses, and provide
    upgraded, and decision-making can be optimized.                    personalized energy consumption analysis reports
    In the space operation scenario, data assets relating              and energy saving suggestions for enterprises. This
    to space utilization, workstation occupancy, and                   reduces the overall carbon emissions of a campus
    meeting room reservations can be used for space                    and raises the loyalty of commercial tenants
    resource optimization. By analyzing the space                      through green services. The predictive maintenance
    usage data in different time periods and areas,                    model uses device energy consumption assets to
    suggestions can be provided on conference room                     predict exceptions in advance, reducing energy
    layouts (for example, splitting large, idle conference             waste and loss caused by faults.




                                                                  75
                                                                     Building with AI: Key Technical Features of AI Campuses 03




In the O&M and service scenario, data assets                  can be optimized using data assets to accurately
relating to device health, service tickets, and user          locate target enterprises. The mid- and long-term
feedback promote the transformation of the                    reconstruction plan can be made for a campus
O&M mode from "passive response" to "proactive                using space usage and energy consumption data
prediction". A predictive maintenance system is               assets. The asset iteration and resource allocation
built based on device health data assets to warn of           can be optimized based on the data such as the
faults in devices such as elevators, air conditioners,        asset usage frequency and value contribution for
and security equipment in advance. Once the                   refined decision-making.
faults are found, it can arrange maintenance. The
service ticket and user feedback data assets are              3.2.3.3 Data Assetization
analyzed to optimize the shift scheduling and
service process of the O&M team and improve                   Data assetization is the value outlet of data-
the efficiency in resolving problems. Using service           AI convergence. The key is to monetize high-
data assets, customized services are provided for             quality data assets that have been verified
on-premise enterprises, such as pushing technical             internally through productization, transforming
resource interconnection information to R&D                   them into a service, and cooperation under the
enterprises, and providing foot traffic analysis and          compliance framework. Supporting the upgrade
marketing suggestions to commercial enterprises.              of campus infrastructure and the optimization of
                                                              the asset system are also monetization factors.
In the decision-making management scenario,                   The monetization process strictly complies with
all data assets, for example regarding investment             laws and regulations such as the EU GDPR and
promotions, leases, energy consumption, and                   China's three data security laws, and adheres
services, are integrated and analyzed from multiple           to the bottom line of data security and privacy
dimensions using the AI middle-end to provide a               p ro t e c t i o n . Te c h n o l o g i e s s u c h a s p r i va c y
panoramic operations report for the management.               computing and permission control are used to
For instance, the investment promotion strategy               transfer value in compliance with regulations.




                                                         76
03 Building with AI: Key Technical Features of AI Campuses



    In terms of product-based monetization, campus                In terms of cooperation-based monetization,
    data assets are converted into standardized                   cooperation can be carried out with enterprises and
    products and provided as services for external                scientific research institutes in the industry chain to
    organizations. For example, the "regional office              jointly build a data asset ecosystem. For example,
    health index" and "space utilization efficiency               Huawei cooperates with new energy enterprises to
    analysis report" can be provided to the urban                 share asset data such as campus carbon emissions
    planning department to provide data support for               and green power consumption, and jointly develop
    urban function layout and transportation planning.            green energy management solutions. Huawei also
    The "green building energy efficiency rating" and             cooperates with technology enterprises to train AI
    "health profile of enterprises in campuses" can               models based on campus scenario data, promote
    be provided to financial institutions to help them            technology iteration, and share the commercial
    accurately provide green credit and enterprise                benefits of models. In addition, Huawei cooperates
    financing services. Anonymized industry datasets              with government departments to construct markets
    can be provided to scientific research institutes             for city data assets, undertake regional data asset
    to jointly carry out research in intelligent campus           projects, and build a benchmark for city-level digital
    and low-carbon operations, and share research                 asset operation.
    achievements and business benefits.
                                                                  Asset monetization is not the end, but a key step
    In terms of service-based monetization,                       in forming a value chain. The revenue generated
    encapsulated AI capability assets can be provided             from monetization will be reinvested into
    to external organizations in service subscription             upgrading the campus data foundation, optimizing
    mode. Customized asset services can be provided               the asset system, and improving services. This
    for campuses and enterprises of the same type.                will further improve the quality and variety of
    For example, the data asset encapsulation system              data assets, and promote the continuous cycle
    construction service can be provided for new                  of "encapsulation, enablement, monetization,
    intelligent campuses, and the lightweight energy              and iteration," transforming campuses into micro
    consumption management and device O&M                         engines and innovation test fields for the digital
    services can be provided as asset subscription                economy of cities.
    tools for small- and medium-sized enterprises. In
    this way, continuous revenue can be generated                 In summary, model integration is foundational
    through charging by usage, month, or scenario.                to data-AI convergence and makes data usable.
    In addition, the asset transaction matching                   Interconnection and sharing are the core processes
    mechanism can be built using the campus                       for aggregating scatter data. Asset monetization
    platform. As a data broker, the platform connects             is the ultimate goal for transforming data
    the supply and demand parties, provides auxiliary             from resources to assets. These three elements
    services such as consulting on asset pricing,                 form a complete value loop, providing a solid
    compliance reviews, and storage of transaction                data intelligence foundation and sustainable
    evidence, and charges service commissions.                    development momentum for AI campuses.




                                                             77
                                                                                                                                                                       Building with AI: Key Technical Features of AI Campuses 03




    3.3 All-Domain Connectivity
All-domain connectivity is a neural network and                                                                                                            knowledge, events, and intelligence can flow and
circulatory system for the intelligent operation of an                                                                                                     aggregate seamlessly and losslessly—as driven by
AI campus. It transforms traditional communication                                                                                                         intent, among heterogeneous elements in a campus.
connectivity into intelligent connectivity that                                                                                                            This provides a unified, agile, and self-evolving
integrates physical entities (things), roles (people),                                                                                                     connection foundation for smart applications in all
and all-domain intelligence, for autonomous                                                                                                                scenarios, such as education and scientific research,
interactions and collaborative evolution. By building                                                                                                      medical services, industrial production, and business
a borderless, highly elastic, and adaptive converged                                                                                                       operations. It is the lifeline of data-AI convergence
network, all-domain connectivity ensures that data,                                                                                                        and also a bridge to advanced HI-AI coexistence.

                                                                                                                                Immersive collaboration space
                                                                                                                                 Human-machine interaction network

                                                                                                                                        3D vision, tactile, and
                                                                                                                                        auditory interaction




                                                        Seamless, trusted access                                                                                              Emergence of
                                                                    Con                Dyn               Acce
                                                                                                                                                                           adaptive multi-agent
                                                                   auth tinuo        auth amic         prot ss
                                                                       entic us
                                                                                                                     Resource




                                                                                         oriza             ectio
                                                                            atio               tio               n
                                                                                 n                n
                                                                                                                                                                                Secure cognitive interaction,
                                                        Client




                                                                                                                                          Connecting                            knowledge sharing, and
                                                                                                                                                                                task collaboration
                                                                                                                                            people




                        Heterogeneous IoT networks                                                                                                                                                     Intelligent scheduling of
                                                                                                                                                                                                           computing power
                                                                                                                                                                                                                    云

                                                                                                                                                                                                                                   Heterogeneous
                                                                                                                                        All-domain
                                                                                                                                                                                                             SRV6

                                                                                                                                                                                                                                   computing power
                                                                                                                                                                                                         边                     边

                                                                                                                                                                                                                        APN6

                                                                                                                                                                                                                                   collaboration
                          5.5G      Wi-Fi    NearLink       FTTO
                                                                                                                                        connectivity                                                         端           网




                                                                                                      Connecting                                                      Connecting
                Edge intelligent autonomy                                                               things                                                        intelligence                                         Edge intelligent autonomy
                                                                                                                                                                                                                                           Intelligent routing
                                                                                                                                                                                                                                           Digital twin
                                                                                                                                                                                                                                           Automatic traffic
              Event aggregation, local inference, and                                                                                                                                                                                      optimization
              real-time closed-loop management
                                                                                Intelligent spatial interaction                                                   Trusted data circulation
                                                                                                                      Virtual space
                                                                                                                                                                          Data isolation,
                                                                                       Physical space                                                                     metering, and auditing,
                                                                                                                                                                          supporting data asset
                                                                                A high-fidelity digital twin for mirroring                                                transactions
                                                                                and interaction




                                                                                      Figure 33 All-domain connectivity


                                                                                                                                                78
03 Building with AI: Key Technical Features of AI Campuses




      3.3.1 Connecting Things: Intelligent Connectivity of Everything and
      Spatial Collaboration

    The massive number of physical facilities, devices,           3.3.1.2 Edge Intelligent Autonomy
    and environment elements in a campus can be
    transformed from passively managed objects to                 Intelligent nodes based on the cloud native concept
    autonomous and sensible network nodes that                    and AI Agent framework are deployed at the
    support interaction and collaboration. In this way,           network edge to form distributed edge agents.
    a digital reflection is built for the physical world          These agents can understand the local context;
    for both thing-thing and thing-environment                    aggregate, infer, and make decisions on real-time
    intelligent linkage.                                          events generated by dense device clusters; and
                                                                  instruct device clusters (such as production line
    3.3.1.1 Heterogeneous IoT Networks                            robots and building environment systems) in the
                                                                  local region to complete collaboration tasks. They
    With 5G-Advanced and 6G integrated networks                   are an evolution from edge computing to edge
    featuring communication, sensing, and computing,              intelligence. Highly reliable and low-latency service
    and NearLink technology at the core, a controllable           autonomy and closed-loop management are
    deterministic IoT foundation can be built to                  formed in a local region, improving the operational
    integrate Wi-Fi 7 Advanced and fiber to the office            resilience and response efficiency of each functional
    (FTTO) networks. This foundation not only provides            unit, and effectively reducing the pressure on the
    ultra-high bandwidth and ultra-low latency                    core network and central cloud.
    connections, but also features the revolutionary
    functions of proactive space sensing and object               3.3.1.3 Intelligent Spatial Interaction
    identification, allowing "connectivity for sensing".
    NearLink technology provides microsecond-                     A high-fidelity digital twin that mirrors and
    level latency and synchronization precise to                  interacts with a physical campus in real time is
    the micrometer-level for scenarios with high                  built as the core platform for connectivity, control,
    requirements, such as robot cluster control and               and simulation. The all-domain sensing network
    precision instrument synchronization. Distributed             continuously injects real-time data into the digital
    OSs such as HarmonyOS, as neural protocols, use               twin, enabling it to accurately simulate, predict,
    seamless plug-and-play in complex environments                and even rehearse the system status. With the
    across brands and types of terminals, solve the               intent-driven network (IDN) technology, service
    problem of terminal fragmentation in multi-                   policies (such as energy use optimization and
    business campuses, and provide high-fidelity                  security linkage) of managers can be automatically
    and real-time data sources for all-domain digital             converted into network policies and device control
    twins. In addition, the FTTO architecture enables             instructions that can be executed by machines.
    fiber connections from the campus core to edge                Atomic services such as lighting, energy, and access
    terminals, supporting 10 Gbps to the building and 1           control can be dynamically scheduled. In this way,
    Gbps to the desktop. In addition, the energy-saving           people do not need to find services, but services can
    technology of optical modules greatly reduces the             accurately adapt to people and things, creating a
    energy consumption of operating the network.                  secure, comfortable, and efficient spatial experience.




                                                             79
                                                                              Building with AI: Key Technical Features of AI Campuses 03




  3.3.2 Connecting People: Intent-Driven and Immersive Coexistence


By eliminating interaction barriers between                             In addition, multicast enhancement technology is
different roles and digital systems, a human-                           used in large-scale and high-concurrency immersive
centered intelligent environment that understands                       interaction scenarios, making remote collaboration
intent, adapts to habits, and expands capabilities is                   as efficient and effective as if it were face-to-face.
created. It promotes human-machine collaboration
and helps technology to meet the requirements of                        3.3.2.3 Collective Intelligence
its users, as well as improving their experience.
                                                                        A knowledge federation network (KFN) is built
3.3.2.1 Seamless and Trusted Access                                     to provide secure and efficient collaboration
                                                                        channels for each user's AI agents. Thanks to
The traditional network access mode is re-invented                      privacy computing technologies, these agents
by both the digital identity network (DIN), as                          can use secure cognitive interaction, knowledge
well as AI-driven intrinsic security with zero-trust                    sharing, and task collaboration at the network layer
architecture. Regardless of where users are or                          without exposing raw data and privacy. In this way,
what terminals they use, the network continuously                       scattered individual wisdom can be connected,
and dynamically evaluates their trustworthiness                         cross-disciplinary and cross-organizational cognitive
and intelligently authenticates their identities,                       integration can be more efficient, and adaptive
device status, and environmental risks. Based on                        multi-agent can be accessed. This is particularly
the Wi-Fi 7 Advanced air interface awareness                            helpful in scenarios such as complex scientific
and imperceptible identification technologies, the                      research, emergency decision-making, and
solution provides users with a seamless, secure,                        innovative designing.
and optimal access experience across all domains
with policy mobility. It keeps networks available and
accessible for roles such as medical staff, engineers,
and visitors.


3.3.2.2 Immersive Collaboration Space

To support seamless interactions in scenarios
s u c h a s re m o t e s u rg e r y d e m o n s t ra t i o n s ,
holographic teaching, and augmented-reality-
based collaborative R&D, a next-generation
infrastructure needs to be built for holographic
media networks. The deterministic IP network
(DetNet/TSN) slicing technology is used to provide
dedicated transmission channels with latency and
jitter guaranteed for multimodal data flows such
as XR vision, spatial audio, and haptic feedback.




                                                                   80
03 Building with AI: Key Technical Features of AI Campuses




      3.3.3 Connecting Intelligence: Ubiquitous Computing Power and a
      Symbiotic Ecosystem

    An "AI Internet" can be built to make intelligence            3.3.3.2 Trusted Data Circulation
    as accessible, schedulable, and collaborative as
    other utilities. In addition, an open, symbiotic,             The deterministic network, TSN security isolation
    and continuously evolving innovation ecosystem                channel, privacy computing, and blockchain
    can be created for the all-scenario smart upgrade             technologies are integrated to build a trusted
    of campuses.                                                  infrastructure for the circulation of data value. This
                                                                  system ensures that sensitive data in healthcare,
    3.3.3.1 Intelligent Scheduling of                             R&D, and operations is available but invisible. It uses
    Computing Power                                               blockchain technology for metering, auditing, and
                                                                  the automatic settlement of rights and interests
    A campus-level computing network can be built to              during data use. It provides reliable support for cross-
    provide unified identification, global awareness, and         departmental, cross-enterprise, and cross-campus
    intelligent scheduling of heterogeneous computing             modeling, as well as collaborative research, data
    power across the cloud, edge, device, and network             asset transactions, and realizing the value of data.
    using protocols such as SRv6 and APN6. Users can
    describe their tasks and intentions using natural             3.3.3.3 Network Intrinsic Intelligence
    languages. The computing network can then
    intelligently match and combine the optimal AI                Network infrastructure is quickly evolving towards
    model and service chain, automatically orchestrate            AI-Native. Technologies such as in-network
    the task flow, and schedule the task to the most              computing, intelligent routing protocols, and
    appropriate computing node for execution. This                digital twin networks can be used to optimize
    achieves on-demand computing power and                        network traffic, recover faults within seconds,
    smarter services, greatly lowering the threshold              proactively defend against security threats, and
    for obtaining and using advanced intelligence.                generate intent-driven resource policies. In this
    This scheduling mechanism can be linked with the              way, unlike passive connection pipes, a network
    "energy-computing-carbon" collaboration system                can proactively detect services, dynamically
    to preferentially schedule computing tasks to edge            optimize them, and support the evolution of
    nodes with abundant green electricity, balancing              the upper-layer ecosystem, underpinning the
    efficiency and green objectives.                              continuous evolution of AI campuses.




                                                             81
                                                                  Building with AI: Key Technical Features of AI Campuses 03




    3.4 Multi-Dimensional Sensing
An intelligent campus must have all-round sensing            network uses full-dimensional and multi-angle
capabilities. Optical sensing, Wi-Fi sensing, and            sensing and identification of people, machines,
spatial capabilities are coordinated to build a              objects, events, and spatial environments,
digital, precise sensing network that features real-         providing solid data support for smart decision-
time visualization and efficient operation. The              making and AI campus services.




  3.4.1 Optical Sensing: Core Foundation of Domain Awareness

Optical sensing uses light as the core carrier and is        of a campus sensing system. Large models are
the root technology of a sensing system. It covers           used to break through the limitations of traditional
three dimensions: visual sensing, optical fiber              sensing for high-precision and all-dimensional
sensing, and spectral sensing. With continuous,              visual cognition. The core technologies are focused
distributed, and highly sensitive detection features,        on two areas:
optical sensing has an in-depth perception of
the physical world and provides the underlying               The vision-language model based on Transformer
technical support for campus scenarios.                      architecture breaks through the limitations of the
                                                             traditional convolutional neural network (CNN).
3.4.1.1 Visual Sensing: Large Models                         Through large-scale image data pre-training
Enable All-Dimensional Visual Cognition                      and scenario fine-tuning, the accuracy of object
                                                             detection, behavior recognition, and tracking is
With the architecture of device-side collection,             improved to over 95% and the false negative rate
edge computing, and cloud collaboration, visual              is reduced by 80% in extreme scenarios, such as
sensing is the most intuitive and important part             densely populated areas and sudden light changes.



                                                        82
03 Building with AI: Key Technical Features of AI Campuses



    The lightweight computer vision (CV) model is                 3.4.1.2 Fiber and Wi-Fi Sensing for
    deployed on edge intelligent computing nodes                  Distributed Detection
    for millisecond-level real-time inference, meeting
    low-latency requirements in scenarios such as                 By sensing the optical fiber topology and status,
    campus security warning and device inspection.                optical cables used for communication are
    In addition, the multisensory model uses cross-               transformed into distributed fiber optic sensors
    modal semantic alignment technology to integrate              (DFOS) to detect information such as perimeter
    multi-dimensional data such as images, videos,                vibrations, environmental changes, and object
    text, and audio, and build a complete link from               movements. This unlocks services such as pipeline
    visual understanding to semantic interaction and              O&M, security protection, and environment control
    decision generation. This enables the model to                for campuses.
    accurately identify specific targets based on text
    instructions, and convert visual scenarios into               In terms of vibration sensing, distributed acoustic
    structured text reports. This, in turn, addresses the         sensing (DAS) technology is used to transform
    problem of a single visual modality's inadequate              the perimeter optical cables of a campus into
    semantic understanding.                                       a passive real-time vibration sensing array that
                                                                  can cover tens of kilometers. The AI pattern
    In the future, visual sensing on AI campuses will             recognition algorithm is used to accurately identify
    be upgraded to all-domain sensing, intelligent                intrusion behaviors such as trespassing, cutting,
    prediction, and proactive services. The CV model              and digging. This technology can also work with
    and multisensory model will be integrated for                 video security to build a three-dimensional security
    cognitive inference in complex scenarios. In                  system supporting vibration-triggered alarms and
    addition, digital twin technology will be used to             visual verification.
    build virtual campus mapping, providing visual
    support for planning and emergency drills, and                In terms of environment and resource sensing, the
    transforming campus management from "passive                  distributed optical fiber temperature and humidity
    response" to "proactive prediction".                          sensor, along with optical time domain reflectometry
                                                                  (OTDR) technology are used to continuously
                                                                  monitor the temperature and humidity of pipelines,
                                                                  visualize and manage optical power, and locate
                                                                  faults with meter-level precision. This helps identify
                                                                  link interruptions, deterioration, and device faults,
                                                                  ensuring smooth connectivity across campuses.


                                                                  In terms of 3D sensing, technologies such as 3D
                                                                  structured light, Time-of-Flight (ToF) lasers, and light
                                                                  detection and ranging (LiDAR) are used for high-
                                                                  precision scanning over short distances, real-time
                                                                  depth information, and large-scale 3D point cloud
                                                                  generation. As a result, they are useful for multiple
                                                                  scenarios such as industrial inspections, robot
                                                                  obstacle avoidance, and real-world modeling. In
                                                                  healthcare and elderly care, this technology can also
                                                                  be used to monitor active falls, including from bed.




                                                             83
                                                                                          Building with AI: Key Technical Features of AI Campuses 03




                        Vibration @                               Temperature @                                  Stress @
                     Rayleigh scattering                         Raman scattering                           Brillouin scattering




                                               Changes in
                                               glass structure




                              Optical signal
                              transmission



                                         Backscattered light reception
                                             and demodulation

                                                                                                   External environmental
                                                                                                   variables



                                                            Figure 34 Fiber sensing



3.4.1.3 Spectral Sensing: Refined                                              can selectively monitor the concentration of
Environment and Feature Detection                                              specific gases such as CO2, DAS can capture noise
                                                                               distribution, and grid-based light sensors can draw
The environmental parameters and material                                      dynamic light maps. All these technologies provide
features of campuses can be measured in a highly                               accurate data support for campus environment
selective and refined manner using different optical                           control, energy saving optimization, and pollution
sensing methods and spectral analysis technologies.                            warning. In addition, they can be used to identify
For example, laser gas sensors, such as tunable                                the spectra of minor defects in industrial scenarios,
diode laser absorption spectroscopy (TDLAS),                                   further expanding the application scope of sensing.




  3.4.2 Wi-Fi Sensing: Technology That Converges Capabilities Across
  Scenarios

Wi-Fi sensing is based on the concept of                                       outdoor scenarios, uses the joint scheduling of
"connectivity + sensing" and extends the connectivity                          communication and sensing resources, and provides
of traditional infrastructure to integrate Wi-Fi and                           an integrated, intelligent sensing solution for
5.5G technologies. It is applicable to all indoor and                          campuses.

                                                 Wi-Fi carrier changes caused                                Carrier aggregation and
       Wi-Fi CSI sensing
                                                    by object movement                                      interference cancellation




    Human presence detection in                     AP as a radar that can sense the                    Innovative algorithms for carrier aggregation
  seconds based on the linkage with               environment in a way similar to sonar                 and interference cancellation, enabling static
         third-party systems                                                                                           user detection


                                                            Figure 35 Wi-Fi sensing




                                                                         84
03 Building with AI: Key Technical Features of AI Campuses



    3.4.2.1 Wi-Fi Sensing: Converged                              3.4.2.2 5.5G-Based Wi-Fi Sensing: 3D
    Sensing for Indoor Scenarios                                  Sensing in All Indoor and Outdoor
                                                                  Scenarios
    Wi-Fi sensing integrates IoT access, environmental
    awareness, and location tracking for an intelligent           In the 5.5G/6G era, the communication spectrum is
    indoor network that covers connections, perception,           expanded to millimeter wave, terahertz, and visible
    and space, supporting all-scenario integrated                 light, which overlaps with the traditional sensing
    sensing and communications.                                   spectrum. This makes it possible for Wi-Fi sensing
                                                                  to cover indoor and outdoor areas and adapt to
    It uses Wi-Fi signals as the sensing medium, and              extreme environments.
    uses millimeter-wave radar technology for object
    detection in addition to connectivity. It can sense           In terms of space and object perception, Wi-Fi sensing
    space occupation and group behavior based on                  can accurately capture the location, motion status,
    Wi-Fi channel state information (CSI). Using the              and distribution of objects in water, land, and air;
    sensing result, the lighting and air conditioning             track objects such as drones, vehicles, and ships at a
    systems can be automatically adjusted to save                 submeter level; and provide multi-object synchronous
    energy and reduce power consumption. Signal                   monitoring services for low-altitude security
    features from Wi-Fi sensing can be used to deploy             protection, smart transportation, and port control.
    an electronic perimeter and detect security risks             In terms of environment and status perception, Wi-
    such as abnormal intrusions and hidden cameras,               Fi sensing works stably in extreme environments
    serving as a proactive security system. Millimeter            such as rainstorms, fog, and low-light by leveraging
    wave technology can be used to monitor vital                  the advantages of radio wave propagation. It can
    signs such as breathing and heart rate in non-                collect meteorological parameters such as rainfall and
    contact mode, providing all-weather care services             visibility in real time, monitor the vibration frequency
    for the elderly that protect privacy. In terms of             of industrial devices and the structure status of bridges
    protocol convergence and asset management,                    and power transmission lines, and generate fault
    Wi-Fi 7 efficiently collaborates with Bluetooth,              warnings. In terms of refined feature perception, Wi-
    NearLink, and RFID protocols to implement                     Fi sensing is more precise and can distinguish vehicle
    unified access management for multiple protocols.             models, capture license plate information, and identify
    This reduces the IoT deployment costs, and                    minor defects in industrial products. In addition, it
    enables automatic fixed asset audits, tracking of             can monitor human vital signs through non-contact
    mobile materials, and real-time management of                 technologies and distinguish personnel identities
    a large number of electronic shelf labels (ESLs),             through gait recognition, making it applicable to
    empowering better and smarter business and                    various scenarios such as security protection, industrial
    asset management.                                             production, and healthcare.



                                                                                        500 m



                                                                                        300 m



                                                                                        120 m




                                                                                        0m




                                              Figure 36 5.5G-based Wi-Fi sensing


                                                             85
                                                                 Building with AI: Key Technical Features of AI Campuses 03




  3.4.3 Spatial Services: Implementation of Sensing Technologies

Fiber and Wi-Fi sensing provide spatial capabilities        seamless positioning network that covers both
in identity verification, location tracking, and            indoor and outdoor areas. It can also interwork
scenario-based services to improve campus                   with security systems to generate real-time alarms
operations relating to labor, assets, and space             for personnel and vehicles that trespass boundaries,
                                                            sharpening the all-domain management and
3.4.3.1 Positioning Services: Hierarchical                  control capabilities of a campus.
and Accurate All-Scenario Positioning
                                                            3.4.3.2 Identity Verification Service:
Using technologies such as Wi-Fi, 5.5G, and fiber           Multi-Dimensional and Accurate
optics, a hierarchical positioning system from area         Identity Verification
statistics to centimeter-level tracking can be built
to adapt to different indoor and outdoor service            A multimodal identity verification system can be
requirements and maximize infrastructure value.             built by integrating sensing technologies such
RSSI-based positioning technology (with a precision         as vision, millimeter wave, and 5.5G, ensuring
of 5–10 meters) is applicable to situations such as         both accuracy and privacy, and meeting identity
customer flow heat map analysis in shopping malls           verification requirements in all campus scenarios.
and supermarkets, collecting foot traffic data, and         Target recognition technology based on visual
congestion warnings during peak hours in a cost-            perception is used for basic applications such
effective manner. As a result, it can be used to            as unlocking smart terminals, verifying campus
provide data support for optimizing campus routes           access control, and elevator floor permissions
and adjusting store layouts.                                management. In addition, the technology supports
                                                            quick identification in complex scenarios such
Fine timing measurement (FTM) positioning                   as targets wearing masks and in backlit areas,
technology (with a precision of 1–3 meters) is              improving access efficiency. 5.5G gait recognition
supported by smartphones and complies with                  and millimeter value biometric sensing technologies
international standards. It provides seamless               can be used to distinguish family members and
personnel and asset positioning, indoor                     monitor the activity status of elderly people without
navigation, geo-fencing management, and visitor             contact, which is applicable to privacy-sensitive
route tracing in scenarios such as office buildings         scenarios such as elderly care communities and
and exhibition halls.                                       employee dormitories.


Ultra-wideband (UWB) and Wi-Fi hybrid positioning           3D optical sensing technology can be used to
technology (with centimeter-level precision) provide        identify the three-dimensional features of personnel,
reliable support for high-precision scenarios such          and RFID identity tags can be used to identify the
as valuable asset tracking, AGV navigation, and             assets they carry. This can be used in scenarios
displacement monitoring of precision equipment,             such as visitor management, access control of
ensuring accurate management and control in                 construction personnel, and personnel tracking in
industrial production and warehousing and logistics.        confidential areas. In this way, identities can be
                                                            accurately verified and the tracks of associated
5.5G positioning technology enables synchronous             assets can be recorded. For temporary visitors,
tracking of multiple objects in all outdoor areas.          the entire process from target feature collection,
It works with campus electronic maps to build a             identity registration, permission assignment, to exit




                                                       86
03 Building with AI: Key Technical Features of AI Campuses



    deregistration can be automatically managed. In               security system. In terms of space management and
    addition, the positioning system can be used to               control, the lighting, air conditioning, and ventilation
    restrict the activity area, ensuring both convenience         systems in a campus can be automatically adjusted
    and security, and providing all-round identity                according to the environment data sensed by
    support for campus security, elderly care, and the            optical and Wi-Fi technologies for energy saving
    confidential management of information.                       and optimization. In addition, smart reservation
                                                                  and environment linkage are supported for spaces
    3.4.3.3 Asset and Scenario-Based                              such as conference rooms and exhibition halls.
    Services: Intelligent, Full-Lifecycle                         After a reservation succeeds, the temperature and
    Management and Control                                        humidity are automatically adjusted and devices are
                                                                  automatically started. After the reservation ends,
    Intelligent services throughout the lifecycle are             energy-consuming devices are automatically shut
    provided for personnel, assets, and space scenarios,          down. Dynamic space scheduling and emergency
    allowing for the efficient utilization of campus              services can be used to monitor the space usage
    space resources and proactive service upgrades.               of each area in real time by using positioning
    In terms of asset management, Wi-Fi 7 and RFID                and sensing data, and flexible resources such as
    technologies are used to automatically audit the              temporary office areas and warehousing spaces
    inventory of fixed assets, track and manage mobile            can be intelligently allocated. In emergencies,
    assets, and use visualized inventory management               trapped people can be quickly located and the
    and control, as well as warnings for idle assets.             optimal evacuation route can be planned, and
    In addition, a spatial positioning system is used             broadcast and lighting systems can be linked to
    to quickly locate the storage positions of assets,            guide the evacuation. Dangerous areas can be
    improving asset utilization efficiency and O&M                marked, and the travel paths of personnel can be
    convenience. This is applicable to scenarios such             traced, providing accurate support for emergency
    as factory warehousing and the management of                  rescue. In smart office scenarios, automatic
    equipment in office buildings. In terms of personnel          linkage can be made, from personnel arrival to
    services, Wi-Fi mmWave sensing and 5.5G vital                 device startup and environmental adaptation.
    sign monitoring technologies are used to provide              Office permissions and personalized settings can
    elderly care services, such as warnings about                 be automatically synchronized using identity
    falls, monitoring of sleep quality, and alarms for            recognition, improving the smart experience
    abnormal behavior. Visual behavior analysis is used           in office scenarios. The campus environment
    to detect and warn of abnormal behaviors such as              profile and service profile can be made according
    high foot traffic, running, and loitering, and quickly        to multi-dimensional sensing data, for better
    respond to the alarms by interworking with the                decision-making and service optimization.




                                                             87
                                                                     Building with AI: Key Technical Features of AI Campuses 03




    3.5 Resilient Security
Due to such rapid development, campuses are               to "withstanding shocks and recovering quickly."
facing more varied security threats than ever.            This shift concerns building a dynamic protection
In addition to threats to physical security, data         s y s tem tha t can ma i nta i n the conti nu ou s
security, and from natural disasters, there are           operation of core functions and learn from and
now new AI attacks. As a result, the traditional          adapt to interference, pressure, or even damage.
security paradigm, which pursues defense in static        This system integrates three capabilities—AI-
boundaries, is no longer sustainable. Resilient           powered smart defense, digital twin security, and
security has emerged, marking a fundamental               adaptive evolution—to build a full, dynamic,
shift in security concepts: from "avoiding faults"        intrinsic security barrier for the AI campus.




                                          Resilient security




                AI vs. AI                    Spatial security                            Adaptive evolution
                                                                                           Attack analysis and
       Threat detection and defense         Physical space security
                                                                                            knowledge graph
                                                                                          Dynamic optimization
        Model and data protection           Digital space security
                                                                                           of defense policies
                                                Cross-domain                               Elastic self-healing
            Compute security
                                            collaboration security                         and reconstruction



                                       Figure 37 Resilient security



                                                     88
03 Building with AI: Key Technical Features of AI Campuses




      3.5.1 AI vs. AI: A Dynamic Defense System Using Intelligence to
      Counter Intelligence

    Faced with increasingly sophisticated and                      the AI itself is not corrupted or stolen. On one hand,
    automated cyber threats, especially AI-driven                  federated learning is used to keep data available
    attacks (such as deepfakes, automated vulnerability            but invisible, ensuring privacy from the outset.
    discovery, and AI-driven APTs), the defense system             Differential privacy, homomorphic encryption, and
    must be upgraded for "using AI to counter AI".                 trusted execution environment (TEE) technologies
    More advanced AI algorithms are used to build                  are used to build multiple security barriers during
    dynamic and proactive defenses on a broad range                data use and transfer, especially for the computing
    of models, data, and infrastructure to cope with               and storage of sensitive data for hardware-level
    unknown threats and keep campuses safe.                        security isolation. On the other hand, full-lifecycle
                                                                   protection is implemented for AI models, including
    3.5.1.1 Adversarial Defense and                                data corruption detection in the training phase,
    Unknown Threat Detection                                       model watermarking and integrity verification in
                                                                   the deployment phase, and improved defense using
    Traditional rule libraries are unable to cope with AI-         adversarial samples during running, to prevent
    generated zero-day attacks or malicious inputs that            models from being stolen, tampered with, or
    are carefully disguised using adversarial samples.             maliciously misled. In addition, model distillation
    The AI-powered defense system uses deep neural                 and lightweight technologies are used to reduce
    networks and anomaly detection algorithms to                   the computing power consumption and data
    establish dynamic behavior baselines for users,                exposure at the edge while ensuring the model
    devices, and traffic. By continuously analyzing                performance, and for collaboration with the smart
    microscopic patterns in network traffic, API call              edge autonomous system.
    sequences, and user operation logs, the system can
    quickly identify abnormal behaviors that deviate               3.5.1.3 Computing Security and Elastic
    from the baselines, even if they have never appeared           Resource Scheduling
    in historical attack samples. For example, for deepfake
    images generated by AI, which are designed to                  Computing is the "power plant" of an AI campus,
    deceive visual recognition access control systems, the         and its security is critical. The campus monitors
    defense system can integrate adversarial detection             the status of computing resources in real time, and
    models to analyze the unnatural statistical features           intelligently identifies and mitigates computing
    of pixels in the images, and intercept such high-level         flood attacks aimed at exhausting resources.
    penetration attempts in real time.                             Through dynamic resource orchestration and
                                                                   quality of service (QoS) assurance, the system
    3.5.1.2 Model Security and Data Privacy                        ensures that computing is preferentially allocated to
    Protection                                                     crucial security AI tasks (such as threat analysis and
                                                                   intrusion blocking) during attacks, and non-critical
    An AI campus is powered by vast amounts of                     loads are elastically migrated or degraded, giving
    models and data, which can be used to ensure that              elastic defense at the computing layer.




                                                              89
                                                                     Building with AI: Key Technical Features of AI Campuses 03




  3.5.2 Spatial Security: Predictive Defense Using Virtual-Physical
  Collaboration

People: Precise behavior control                                                                     Identifiable identities
Dynamic permission management,                                                              Unique identity across the system,
abnormal behavior identification                                                                     with its source traceable


Things: Visible and                                                                              Manageable permissions
manageable                                                                                          Hierarchical permissions,
Online asset registration,
                                     Physical space                Digital space                   dynamic adjustments, and
full-lifecycle management                                                                            minimum authorization


Environments: Proactive risk                                                                            Traceable behavior
prevention and control                                                                          Transparent behavior, with its
Controllable gas, water quality,                  Cross-domain                                     historical paths queryable
and noise                                         collaboration


                                           Figure 38 Spatial security


Spatial security is achieved by building a virtual twin        cabinets and server cabinets) by comparing real-
that is completely mirrored and synchronized with              time data with the preset healthy digital model,
a physical campus in real time. This allows for early          and generates warnings for tampering. When a real
risk detection and countermeasure simulation in                physical event (such as a fire or hazardous material
the digital space, and provides guidance for precise           leakage) occurs, the twin can instantly visualize the
responses in the physical world. This transformation           distribution of emergency resources, optimal rescue
powers a shift from post-event emergency response              routes, and affected areas, and directly instruct
to pre-event warning, and in-event optimization.               robots or personnel to implement accurate and
                                                               efficient onsite handling.
3.5.2.1 Physical Space Security
                                                               3.5.2.2 Digital Space Security
The core of physical security lies in holographic
awareness for a proactive approach based on                    Security of digital space focuses on risk-free attack
digital twins. A dynamic digital image of the                  simulation and vulnerability control in a virtual
physical world is constructed by integrating the               environment. This allows for the formation of a
real-time status of all IoT devices (cameras, access           virtual-physical closed-loop defense mechanism.
control devices, sensors, and production devices) in           High-fidelity attack drills and vulnerability discovery
the campus. By using deep learning and simulating              are performed in an identical digital twin which
the rules of physical operation, the system can                runs in parallel with the physical world. This
identify abnormal behavior and potential intrusions            includes simulating advanced persistent threat
(such as unauthorized access to restricted areas               (APT) attacks from end to end, testing the
and abnormal device vibrations) in real time. Then,            effectiveness of the existing defense system, and
it can intelligently associate the intrusions with             exposing blind spots and logical defects in advance.
network attack logs to determine whether they                  Network functions virtualization (NFV) and security
are new types of physical attacks. In addition, the            orchestration technologies can quickly deploy
system continuously monitors the physical integrity            security components such as virtual firewalls and
of critical infrastructure (such as power distribution         intrusion detection systems, and flexibly adjust




                                                          90
03 Building with AI: Key Technical Features of AI Campuses



    defense policies to cope with ever-changing threats.           collaboration and automatic responses between the
    When new software or hardware vulnerabilities                  physical and digital spaces. When the digital twin
    are discovered, the impact scope can be quickly                system detects a cyber attack in the virtual space
    and extensively verified in the twin. The service              and proves that the attack may cause a breakdown
    interruption chain and data leakage path that may              of a key physical device (such as a precision machine
    be caused by the exploitation of the vulnerabilities           tool controller) through simulation, the system
    can be calculated. This provides a scientific basis for        can automatically trigger a preset response plan.
    formulating accurate patch policies and network                The system performs operations such as isolating
    isolation solutions. In addition, any new security             infected hosts and blocking malicious IP addresses
    policy (such as firewall rules and access control              in the digital space. At the same time, the system
    lists) can be verified and optimized in this virtual           delivers collaboration instructions to the physical
    sandbox before being applied to the production                 space, schedules the nearest inspection robot to the
    environment. This ensures the effectiveness of the             site to check the device status, and automatically
    policy and prevents unexpected service interruptions           adjusts the process parameters of related production
    caused by policy conflicts.                                    lines to mitigate risks. This integrated, closed-loop
                                                                   response, which involves detection, judgment, and
    3.5.2.3 Cross-Domain Collaborative                             control, is completed within seconds. It seamlessly
    Security                                                       connects risk identification in the virtual space with
                                                                   risk handling in the physical world, greatly improving
    Cross-domain collaborative security is the ultimate            the efficiency and precision of responses to cross-
    goal of digital twins, aiming to achieve intelligent           domain composite threats.




      3.5.3 Adaptive Evolution: A Dynamic Immune System That Learns
      from Experience

    The ultimate goal of resilient security is for a               reveal attacker profiles and activity patterns, and
    security system to have the learning and evolution             convert isolated incidents into learnable tactical
    capabilities similar to those of a biological immune           intelligence.
    system. The adaptive evolution mechanism ensures
    that each security incident becomes a "vaccine"                D ynamic optimization and the automatic
    for system enhancement, driving the continuous                 deployment of defense policies: The system uses
    optimization of defense policies, architectures, and           machine learning to automatically check whether
    even the entire security system.                               existing security policies are effective, according
                                                                   to continuously updated threat intelligence and
    Automated analysis of kill chains and                          reviews of attacks. It can recommend and (after
    knowledge graph construction: All-link data                    approval) automatically deploy optimized detection
    (attack vectors, tactics, techniques, and procedures)          rules, adjust access control policies, or update the
    of any security incident is automatically captured,            blacklist and whitelist, implementing adaptive
    analyzed, and structured into a graph. The system              parameter adjustment for defense policies, and
    can automatically associate different incidents,               making the defense system more intelligent.




                                                              91
                                                                   Building with AI: Key Technical Features of AI Campuses 03




Self-healing and reconstruction capabilities of             A resilient security system is the cornerstone
the elastic architecture: The system has certain            for an AI campus to cope with new threats.
self-healing capabilities in the case of severe             "AI defending against AI" resolves the latest threats
damage. For example, when a security analysis               through dynamic, intelligent attack-defense games.
node is paralyzed due to an attack, the system can          Digital twin security implements risk prediction
automatically switch its functions to a standby node        and control through "holographic monitoring in
or distributed edge node. Furthermore, by using             the physical space, lossless deduction in the digital
software-defined networking (SDN) and software-             space, and precise collaboration across domains".
defined perimeter (SDP) technologies, the system            Finally, the entire system achieves continuous
can dynamically adjust the network topology and             immunity and growth through adaptive evolution.
security perimeter according to real-time threats,          The three elements are closely linked, providing a
so that the attack surface keeps changing. This             reliable security foundation for the prosperity of AI
dramatically increases the cost and difficulty for          campuses.
attackers and improves the overall survivability.




    3.6 Eco-Sustainability
AI campuses promote green and sustainable                   "bits" and "watts." By deeply integrating these
development by practicing the core principles               in intelligent systems—where bits save, manage,
of low consumption, low emissions, and high                 and monetize watts—campuses can achieve zero-
efficiency. AI enables every phase of energy                carbon goals. This promotes green, intelligent
production, transmission, consumption, and                  campuses with balanced energy use, measurable
management, creating an intelligent, self-                  c a r b o n fo o t p r i n t s , a n d a d v a n c e d e n e rg y
sustaining, and evolving green ecosystem. The               management, supporting sustainable economic
two core pillars of zero-carbon sustainability are          growth alongside environmental protection.




               Bits Monetize Watts                                             Bits Save Watts




                     Value                                                                  Value




                                          Bits Manage Watts



                       Figure 39 Bits and watts enable zero carbon and sustainability




                                                       92
03 Building with AI: Key Technical Features of AI Campuses




      3.6.1 Bits Save Watts

    Campus infrastructure such as buildings, data                 with the IOC to precisely predict localized energy
    centers (DCs), communications networks, and                   demand. This enables cross-system collaboration
    IoT terminals is the primary source of energy                 and dynamic parameter optimization. For instance,
    consumption and a critical frontier for de-                   the system can automatically lower air conditioning
    carbonization. Symbiotic intelligence from AI                 loads and switch off unnecessary lighting during
    agents and the IOC, along with weather data and               low occupancy. It also adjusts artificial lighting
    AI models, accurately predicts energy demand and              levels in response to natural light changes, ensuring
    optimizes usage in real time. This minimizes energy           energy supply aligns precisely with demand. In
    consumption at its source and supports carbon                 practice, this technology can cut building energy
    emissions reduction across the campus lifecycle.              consumption by 15% to 30%.


    3.6.1.1 Green Building Energy Saving                          3.6.1.2 Green DC
    Control
                                                                  DCs serve as the computing core of AI campuses
    As building energy use accounts for the lion's                and their high energy consumption is a key focus
    share of campus overall consumption, using AI                 in campus-wide de-carbonization initiatives. The
    for precise control of building energy systems                convergence of AI and green cooling technologies
    is pivotal for mitigating infrastructure-related              supports a systematic and intelligent approach to
    carbon emissions. An integrated AI prediction and             energy optimization.
    optimization framework employs multi-source data,
    such as weather, system operations, and historical            Technically, an AI- driven energy efficiency
    energy consumption, along with the analytical                 management platform continuously monitors
    and reasoning capabilities of AI models to create             multi-dimensional parameters like server loads,
    a closed-loop decision system. Powered by the                 equipment room temperature and humidity, and
    synergy between AI agents and the IOC, the system             cooling system status. The platform employs
    delivers fine-grained, end-to-end control over                algorithms and models to dynamically optimize
    primary energy consumption systems.                           cooling strategies. It adjusts air conditioner
                                                                  temperature and airflow based on real-time heat
    For large, energy-intensive front-end facilities              maps and intelligently links with natural cooling
    like central chiller plants and compressed air                sources like fresh air and cooling towers when
    systems, AI prediction models and optimization                conditions allow, minimizing reliance on mechanical
    algorithms simulate operating conditions using                cooling and lowering energy use. Additionally,
    real-time and predictive data. This enables                   the AI scheduling system consolidates low-load
    dynamic, energy-efficient control strategies that             computing tasks onto fewer physical servers using
    eliminate inefficiencies and significantly boost              server virtualization and resource pooling. This
    overall operational performance. At the control               allows idle servers to enter low-power sleep mode,
    layer of distributed devices like lighting and air            significantly reducing baseline energy consumption.
    conditioners, the system integrates IoT sensing data
    on indoor temperature, humidity, crowd density,               Fully liquid-cooled equipment rooms, paired with
    and illuminance, with inputs from AI vision and Wi-           AI-based precise temperature control, can cut the
    Fi sensing. Capitalizing on large models' contextual          energy consumption of cooling systems and lower
    understanding, AI agents collaborate in real time             DC power usage effectiveness (PUE) from 1.5–1.8




                                                             93
                                                                                     Building with AI: Key Technical Features of AI Campuses 03




to below 1.2. In advanced cases, PUE can drop to                               security over a single optical fiber. This eliminates
1.1 or lower, advancing the green transformation of                            extra energy consumption and resource waste from
computing infrastructure.                                                      repeated network construction, thereby reducing
                                                                               the carbon footprint of network deployment and
3.6.1.3 All-Optical Green Networking                                           operations at its source.


As the green foundation of communication                                       3.6.1.4 IoT Communication Adaptation
networks, all-optical networking significantly
reduces link energy consumption and enhances                                   A c a m p u s t y p i c a l l y h o s t s a va s t a r ra y o f
re s o u rc e u t i l i z a t i o n b y o p t i m i z i n g m e d i a ,        heterogeneous IoT terminals, so reducing
architecture, and service bearing modes.                                       communication energy consumption is an
                                                                               essential complement to the infrastructure de-
Regarding media optimization, all-optical                                      carbonization system.
networking replaces traditional copper cables with
quartz fibers. The carbon footprint of producing                               O pt imizat ion at the transmission layer :
optical fiber is just one-third that of copper per                             Intelligent data compression and edge filtering
unit length, while its transmission speed is over                              technologies precisely filter and efficiently compress
100 times faster than traditional network cables.                              collected data on the terminal side or edge
Additionally, its superior corrosion resistance                                gateway. This reduces the amount of invalid or
and durability allow for up to 30 years of use                                 redundant data transmitted over the network,
without replacement, greatly reducing resource                                 lowering communication energy consumption
consumption and carbon emissions.                                              between terminals and the network.


For architecture optimization, all- optical                                    Optimization at the orchestration layer:
networking deploys passive optical networks (PONs)                             Distributed OSs like HarmonyOS enable intelligent
combined with a two-layer flattened architecture.                              collaboration, allowing sensing, computing, and
This simplifies the network hierarchy and reduces                              control devices in a region to work together and
both the number and power consumption of active                                make local decisions. This substantially cuts the data
aggregation and switching devices. The point-to-                               sent to the central cloud, lowers energy consumption
multipoint (P2MP) deployment mode replaces                                     during cross-region transmission, and significantly
traditional active aggregation devices with passive                            enhances real-time service responsiveness.
optical splitters, cutting optical module usage by
approximately 50% and reducing overall network                                 Optimization at the protocol layer: An AI-
energy consumption by over 30%.                                                driven communication policy engine automatically
                                                                               selects and dynamically adjusts optimal protocols
In addition, the innovative integrated bearer                                  (e.g., NB-IoT and LTE Cat 1), transmit power, and
mode—that is, one fiber for multiple services—                                 transmission frequency based on the terminal
effectively optimizes service bearing. Hard pipe                               type, service requirements, and network conditions.
isolation and slicing technology creates logically                             This minimizes terminal wireless overhead while
independent, securely isolated transmission                                    ensuring data reliability, enabling a low-power,
channels for services like office, production, and                             efficient IoT communication ecosystem.




                                                                          94
03 Building with AI: Key Technical Features of AI Campuses




      3.6.2 Bits Manage Watts

    AI-enabled refined campus energy management                   green development from a vision into a practical,
    creates a comprehensive carbon management                     verifiable operational cycle.
    system that supports baseline data governance
    and intelligently optimizes emission reduction. This          3.6.2.2 Carbon Emission Data Baseline
    implements global coordination, precise control, and          Management
    ongoing optimization of energy consumption and
    carbon emissions, providing data-driven insights for          A carbon emission data baseline management
    sustainable, low-carbon campus development.                   system is established using carbon footprint
                                                                  accounting data. The system provides online,
    3.6.2.1 Carbon Footprint Accounting                           visualized storage and detailed analysis of multi-
    Management                                                    dimensional energy consumption and carbon
                                                                  emission data across the campus. By integrating
    An intelligent carbon footprint monitoring and                AI models with domain-specific knowledge bases,
    management platform for campuses integrates                   it automates complex data consolidation, insights,
    multi-source data, such as energy consumption,                and mining, creating a refined management
    production and operations, transportation, and                baseline covering total campus emissions as well as
    logistics, to manage carbon emissions in a                    per-capita and per-area intensity.
    visible, measurable, and traceable manner. The
    system uses AI to automate data collection,                   Thanks to the collaboration between AI agents
    cleansing, integration, and accounting in line with           and the campus IOC, the system monitors carbon
    international standards like ISO 14064. It generates          emission data anomalies in real time, proactively
    a dynamic carbon account for the entire campus                analyzes root causes, and provides tailored
    and key enterprises, precisely pinpointing emission           optimization suggestions. This shifts management
    hotspots and critical phases.                                 from static data statistics to dynamic, proactive
                                                                  control, delivering a solid and quantitative data
    Building on the carbon footprint accounting                   basis to support targeted emission reduction
    system, an integrated digital twin model of energy,           policies and management appraisal.
    computing, and carbon is created to track and
    analyze the origin and flow of every kilowatt-                3.6.2.3 Intelligent Energy Loss Diagnosis
    hour of electricity, the energy consumption and
    emissions per watt of computing power, and                    Effective energy loss control is vital for efficient,
    the source and cause of every gram of carbon                  cost-effective campus energy operations and
    emitted. Accordingly, a green computing power                 embodies the "bits manage watts" principle.
    certification mechanism is introduced to help the             Intelligent monitoring and analysis through
    system automatically detect, reject, or intelligently         digital technologies (bits) enables precise
    reschedule high- carbon- emission compute                     energy ( watts) loss control, reducing waste
    tasks. This ensures computing resource use                    and optimizing energy transmission and usage.
    dynamically aligns with green energy availability             The AI-enabled system for real-time, network-
    and overall carbon management goals, turning                  wide energy loss monitoring, accurate location,




                                                             95
                                                                   Building with AI: Key Technical Features of AI Campuses 03




and warning minimizes energy waste during                    3.6.2.4 Emission Reduction Solution
transmission, distribution, and consumption.                 Simulation and Optimization

In pipeline loss diagnostics, AI algorithms analyze          Based on the campus carbon emission baseline,
operational data on flow rate, pressure, and                 AI delivers advanced simulation and optimization
temperature from energy pipelines like water,                capabilities to support emission reduction path
heating, and gas systems. This allows for real-              planning and decision-making. AI dynamically
time detection of abnormal loss points like                  s i m u la t e s t he i m pa ct of va ri ou s e m i s s i on
pipeline leaks or equipment failures, identifying            reduction measures, such as energy-saving device
faults within seconds and locating them within               upgrades, renewable energy adoption, and green
minutes for prompt responses. Compared to                    infrastructure improvements, across multiple
traditional manual inspection, this technology               scenarios. It quantitatively assesses investment costs,
boosts response and handling efficiency by over              implementation timelines, and emission reduction
80%, significantly reducing unnecessary carbon               outcomes to create a comprehensive economic and
emissions from energy leaks.                                 environmental benefit evaluation model.


In addition, an AI-based predictive maintenance              Aligned with the campus' strategic development
strategy is employed. IoT sensors on critical devices        plans and phased carbon emissions reduction
continuously collect running parameters from                 targets, AI generates optimal emission reduction
multiple dimensions, including current, voltage,             paths and priority recommendations, clarifying
vibration, temperature, and noise, to establish              key tasks and actions at each stage to maximize
baseline AI models for device health assessment.             benefits within resource limits. Practical experience
The system detects potential faults like bearing             shows that AI-driven simulation and optimization
wear, pipeline blockages, and motor insulation               of integrated solutions, covering PV, ESS, intelligent
aging in advance through real-time comparison                scheduling, and liquid-cooled DCs can boost
and pattern recognition. This helps prevent energy           overall emission reduction by approximately 15%
loss and safety hazards from device degradation or           compared to single-technology upgrades. This
unexpected failures, extends device lifespan, and            delivers efficient, reliable decision-making support
ensures the campus energy system operates reliably           for green, low-carbon campus transformation.
over the long term.




                                                        96
03 Building with AI: Key Technical Features of AI Campuses




      3.6.3 Bits Monetize Watts

    "Bits monetize watts" represents the high-level               AI throughout the entire lifecycle: The technology
    goal for the green, sustainable development of AI             precisely predicts wind and PV power generation
    campuses. It emphasizes using digital technologies            curves alongside electricity demand, optimizing
    (bits) to unlock the additional value of energy               collaborative PV and ESS dispatch to increase
    (watts). This moves energy saving and carbon                  green power usage. Intelligent tracking algorithms
    emissions reduction away from mere cost control,              improve the power generation efficiency of PV
    transforming watts into a value-generating asset.             modules, while intelligent charging scheduling
    At its core, it means empowering watts with bits              encourages new energy vehicles to charge during
    by integrating digital and energy technologies. This          peak green power periods, extending the green
    fosters a virtuous cycle where energy savings yield           power value chain. Moreover, AI-based predictive
    returns, green electricity holds intrinsic value, and         O&M slashes labor costs, enhances microgrid
    carbon emissions become monetizable assets. It                stability, and facilitates the transition from an
    shifts green campus development from passive                  energy supply system to a green power value
    carbon emissions reduction to proactive value                 creation system.
    creation, with the smart microgrid as the key
    enabler across all dimensions.                                3.6.3.2 Value Enhancement in Green
                                                                  Computing Power
    3.6.3.1 Value Enhancement in Green
    Power Generation                                              Value enhancement in green computing power is
                                                                  the key breakthrough. Grounded in the energy-
    Value enhancement in green power generation                   computing-carbon synergy framework, it integrates
    underpins "bits monetize watts." The former centers           green power with compute resources and uses
    on using digital technologies to enable the entire            digital technologies for coordinated scheduling
    lifecycle of green power generation and dispatch,             and optimization. This enables green power to
    boosting green power output, improving allocation             fuel computing, while computing amplifies value,
    efficiency, and unlocking the inherent value in the           demonstrating the seamless convergence between
    production process.                                           bits and watts.


    As the core infrastructure, the smart microgrid               Central to this convergence is a collaborative
    comprehensively deploy s building applied                     dispatch center that integrates energy, computing,
    photovoltaics (BAPV) for rooftops and building                and carbon. Using AI reinforcement learning, it
    integrated photovoltaics (BIPV) for facades,                  predicts computing demand, PV power generation
    turning unused surfaces into distributed green                curves, and ESS status in real time, enabling
    power plants. This supports on-site green power               dynamic alignment between computing and green
    generation and consumption, cutting the costs and             power. During peak demand, computing tasks
    carbon emissions linked to purchased fossil fuel–             are routed to regions with sufficient green power,
    based electricity.                                            producing green computing power that commands




                                                             97
                                                               Building with AI: Key Technical Features of AI Campuses 03




higher market value through certification. In off-        model precisely measure emission reduction and
peak times, idle computing nodes enter sleep              generate traceable data records, providing a strong
mode, while ESS charging and discharging are              foundation for carbon asset certification.
optimized to prevent resource waste. This model
facilitates a leap from "green power to computing         Carbon assets can be monetized by trading
power" and from "computing power to value                 allowances and credits in carbon markets for
creation," maximizing the synergistic value-added         direct financial returns. Additionally, using these
effects of both.                                          assets boosts the campus' green brand influence,
                                                          attracting low-carbon enterprises and creating
3.6.3.3 Value Enhancement in Carbon                       indirect value. AI models unlock carbon asset value
Assets                                                    by optimizing green power and computing power
                                                          scheduling, creating a virtuous cycle of green
This highlights the true value of "bits monetize          power generation, emission reduction, carbon asset
watts." It involves using digital technologies to         creation, and revenue feedback, thereby supporting
convert carbon emissions reduction from green             long-term green development.
power generation and green computing power
into measurable, tradable, and monetizable                In summary, green power generation lays the
carbon assets, turning ecological benefits into           foundation, green computing power serves as
economic gains.                                           the core, and carbon assets embody the ultimate
                                                          value. Together, they create a comprehensive "bits
The campus carbon footprint accounting platform           monetize watts" framework that drives green
and the energy-computing-carbon digital twin              campus development forward.




                                                     98
99
04
Architecting with AI:
Reference Architecture for AI Campuses
The demand for digital and intelligent campuses is intensifying across industries,
bolstered by global policies on green operations, the digital economy, and
new infrastructure. Core technologies—notably HI-AI coexistence, data-AI
convergence, all-domain connectivity, multi-dimensional sensing, resilient
security, and eco-sustainability—are rapidly being integrated and implemented,
driving the evolution from intelligent to AI campuses.




                         100
04 Architecting with AI: Reference Architecture for AI Campuses



    This profound transformation presents systemic                                                                                                AI campus reference architecture powered by six
    challenges to the full-link architecture of                                                                                                   core engines: Agentic Device, Agentic Network,
    traditional campuses, comprising devices,                                                                                                     Agentic Platform, Agentic Application, Agentic
    networks, platforms, applications, security, and                                                                                              Security, and Agentic O&M. Together, they
    O&M. Issues like isolated devices, limited network                                                                                            create an integrated, intelligent closed loop
    bearing capacity, fragmented data, passive                                                                                                    of sensing, data, knowledge, and capability.
    application services, delayed security, and slow                                                                                              Autonomous sensing, collaborative decision-
    O&M responses are increasingly apparent. These                                                                                                making, and dynamic evolution across multiple
    shortcomings hinder campuses from meeting                                                                                                     agents transform campuses from being simply
    future demands for agile response, intelligent                                                                                                "perceptible" to truly "understandable," and
    collaboration, and ongoing evolution.                                                                                                         from "manageable" to continuously "evolving,"
                                                                                                                                                  ultimately creating an intelligent, self-evolving
    In response, Huawei proposed a future-oriented                                                                                                entity that can sense, think, act, and grow.




      Office campus                                               Industrial         National lab        Manufacturing       Electric        Energy campus         Venue campus    University campus      Hospital                     Commercial
                                                                   campus                                  campus          power campus                                                                   campus                        complex


                                                               Office space                                        Production space                                        Service space                                         ……


                                                                                                    HI-AI coexistence                    Easy-to-access services
                                                               Proactive intelligent services                                                                                         Multimodal natural interaction
                                                                                                                          Twin-based intelligent space
                                                                               Office agent                                                                                            Classroom agent
                                                                                                       Conference agent            Agent marketplace          Guest room agent




                                                                                                                                                                                                                                       assurance, and eco-sustainability
                                          Resilient security




                                                                                                                                                                                                                                       Full-stack convergence, proactive
        defense, and adaptive evolution




                                                                                                                                                                                                                     Intelligent O&M
                                                                                                       Data-AI convergence                         Platform-driven
         Intelligent sensing, proactive




                                                                        Data convergence                                                                                   Flexible intelligent computing
                                                                                                                                Intelligent autonomy
                                                                                       Data services                                                                                Super models
                                                                                                                    Spatial services               Intelligent services


                                                                                                    Wi-Fi sensing                      All-domain connectivity
                                                                       Connecting people                                                                                         Connecting intelligence
                                                                                         WLAN
                                                                                                                                 Connecting things                                      5.5G
                                                                                                                          POL                           NearLink




                                                                                                               Multi-dimensional sensing
                                                                                                         Intelligent connectivity of everything
                                                                        Sensing terminal                                                                                              Intelligent terminal

                                                                                                                                       Facility

                AI vision                                                                                                                                                                                                      Intelligent
                                                                                                                                                                                                                               distribution
                                                                  Environment                                                                                                                          Intelligent
                                                                    sensing            Location                                                                                      Intelligent         services
                                                                                                           Environment                         Energy              Information
                                                                                       awareness                            Security                                                  inspection
                                                                                                             control                         management               display
                                                                                                                           management




                                                                                                       Figure 40 AI campus reference architecture




                                                                                                                                        101
                                                                                Architecting with AI: Reference Architecture for AI Campuses 04




    4.1 Agentic Device: Terminal Agents Featuring
    Autonomous Sensing, Decision, and Evolution
Campus terminals are entering an era of universal                                connectivity, as demonstrated by standards like
intelligence, upgrading from basic devices into                                  Matter (Europe and the US), CoAP (China), and
fully autonomous agents. As they become more                                     the GIIC interoperability protocol (China).
widespread, interconnected, data-extensive, and
service-driven, we expect them to evolve along three                           • Unified models: Standardized device data models
key paths: wireless, standardized, and intelligent.                              unify all terminal data into a consistent format,
Together, these shifts create the neural foundation                              enabling seamless device-to-cloud connection
for all-domain sensing, instant connectivity, and                                and data sharing. This ensures devices and
autonomous evolution across the campus.                                          campus digital OSs support plug-and-play with
                                                                                 universal connectivity.
Wireless
                                                                               Intelligent
Terminals will become fully wireless to tackle
the high costs and reconstruction challenges                                   Advances in AI and breakthroughs in key
of traditional cabling. A wide range of wireless                               technologies like intelligent cognition are driving
technologies like Wi-Fi, NearLink, Bluetooth, passive                          campus terminals—the key touchpoints for
RFID, and RedCap allow terminals to quickly                                    spatial awareness—to rapidly evolve from simple
become IP-based and agilely connect to campus                                  devices into agents. Terminal intelligence spans
networks and digital OSs, offering instant network                             three key dimensions. First, AI-native intelligence.
connectivity upon power-on. This significantly                                 Built-in lightweight large models enable local
lowers deployment and long-term O&M costs while                                inference and edge decision-making, accelerating
eliminating reliance on fixed physical locations,                              the widespread adoption of AI-native terminals.
enabling intelligent services to be accessible on                              Second, multimodal fusion and sensing. Terminals
demand anytime, anywhere.                                                      become sensing agents capable of seeing, hearing,
                                                                               and perceiving by integrating computer vision (CV),
Standardized                                                                   automatic speech recognition (ASR), mmWave
                                                                               radars, and thermal imaging. Third, seamless
Standardization is the foundational pillar of a true                           interaction and intent recognition. Terminals can
IoE ecosystem. By creating a common language and                               proactively identify user intents (e.g., "I want to
shared standards for campus terminals, it enables                              hold a meeting" and "I'm cold today") through
seamless plug-and-play functionality and universal                             behavior modeling and context understanding
connectivity through unified protocols and models.                             and then automatically deliver targeted services.
                                                                               Ultimately, campus terminals will use cloud-edge
• Unified protocols: A standardized protocol                                   synergy, self-monitoring, and self-feedback to
  framework speeds up cross-vendor                                             facilitate group-based collaborative autonomy, for
  I o T i n t e ro p e ra b i l i t y, e n a b l i n g s e a m l e s s         example, interactive adjustment of lighting, fresh
  communication among diverse devices and                                      air, and air conditioning systems. This way, they can
  platforms. One protocol stack supports                                       advance the campus toward ongoing autonomous
  both device-to-device and device-to-cloud                                    decision-making and coordination.




                                                                         102
04 Architecting with AI: Reference Architecture for AI Campuses




         4.2 Agentic Network: Future-Proof Campus
         Networks
    As campus digital and intelligent transformation                    This delivers actionable insights that drive intelligent
    constantly advances, campus networks are                            campus transformation.
    undergoing a fundamental capability leap: evolving
    from connecting people to connecting things, and                    Ubiquitous IoT
    ultimately, to connecting intelligence. Connectivity is
    transcending basic people-to-people communication                   All-domain ubiquitous IoT access is made possible
    to embrace Wi-Fi sensing. This allows networks                      by Wi-Fi 7's high bandwidth, low latency, and
    to perceive human states, evolving toward a                         massive concurrency and the integration of multiple
    human-centric "connectivity + sensing" paradigm.                    protocols like Wi-Fi, SparkLink, Bluetooth, Zigbee,
    Simultaneously, connectivity methods are shifting                   and RFID. This contributes to an IoT access anchor
    from single-mode, standardized human-to-human                       system for campus-wide coverage and multi-
    links to ubiquitous IoT. By supporting multi-protocol,              network collaboration, enabling widespread IoT
    multi-form, and multi-access connectivity, networks                 terminal onboarding and centralized management.
    provide unified access capabilities and high-load                   Backed by ultra-broadband all-optical PON/PEN
    service bearing capacity. Finally, interaction models               technologies, an all-optical network foundation
    are progressing from human-machine to HI-AI                         featuring 10GE access and a 100G backbone
    and AI-to-AI, reshaping campus network traffic                      facilitates large-scale deployment of countless
    patterns and continuously transforming network                      terminals. It lays a solid IoT base for intelligent
    architecture. Propelled by the synergy of diverse                   campus operations and drives digital upgrades and
    service and technical requirements, campus networks                 value reconstruction across all campus spaces.
    are undergoing a trifold evolution toward human-
    centric Wi-Fi sensing, ubiquitous IoT, and intelligent              Intelligent communication
    communication.
                                                                        Built on a full-mesh networking architecture, a
    Human-centric Wi-Fi sensing                                         unified interaction system supports standard agent
                                                                        communication protocols like A2A, MCP, and IOA.
    Centered on human needs and driven by Wi-Fi                         This enables efficient collaboration among campus
    sensing, sensing capabilities like 5.5G, Wi-Fi, and                 agents and fully supports evolving interaction
    optical sensing are embedded into the network                       models both between humans and agents and
    communication system. This creates converged                        among agents themselves. Additionally, cloud-
    network architecture that advances from supporting                  based intensive computing power collaborates
    communication to actively perceiving human states,                  with localized edge-device inference, establishing
    seamlessly integrating network communication and                    a streamlined cloud-edge-device synergy network
    all-domain sensing. In addition, a wide range of                    that delivers low latency within milliseconds. The
    advanced technologies like edge computing, Wi-Fi                    network efficiently supports collaborative cloud-
    sensing algorithms, spatial positioning, and digital                edge-device inference and distributed intelligent
    twin support multi-dimensional, multi-type massive                  decision-making, fulfilling the stringent latency
    communication, real-time processing of sensing                      and compute orchestration needs in digital and
    data, intelligent simulation, and in-depth analysis.                intelligent campus scenarios.




                                                                  103
                                                               Architecting with AI: Reference Architecture for AI Campuses 04




    4.3 Agentic Platform: Campus Digital Platform
    Integrating Data, Knowledge, and Capabilities
The extensive application of AI is transforming the           O&M records, facility parameters, and personnel
campus digital platform from a traditional data               behavior patterns—both structured and
foundation into an intelligent hub. By integrating            unstructured knowledge (including device manuals,
state-of-the-art technologies, the platform can               emergency plans, and regulations) are organized
aggregate and understand data from multiple                   into an associated and traceable knowledge
sources in real time, including people, things, and           graph. Combined with a rule engine, this forms
environments. It then integrates the data with the            an explainable, inference-ready knowledge system
digital twin space, making the physical space an              that powers a domain-specific model tailored for
intelligent entity with sensing capabilities. On this         campuses. The large model automatically identifies
basis, the platform works with diverse AI agents              rules and extracts experience from new events and
to build a core architecture that integrates data,            service tickets. It integrates multimodal information
knowledge, and capabilities, ultimately creating              (text, images, and videos) to associate knowledge
an intelligent hub capable of sensing, cognition,             across different dimensions. The results of agent
decision-making, and action.                                  execution feed into correcting and optimizing the
                                                              knowledge models, creating a closed-loop evolution
All-domain data collection and                                mechanism of knowledge-action-feedback-update
convergence                                                   that drives dynamic knowledge growth, ongoing
                                                              evolution, and self-improvement.
The platform collects data on all campus elements,
such as people (locations and behaviors), events              Intelligent services and efficient
(incidents and service tickets), things (devices and          collaboration
assets), environments (temperature, humidity, and
illumination), and video streams. It then cleans,             The agent enablement platform abstracts campuses
deduplicates, standardizes, and labels this raw data          into high-precision digital space models. By
to ensure accuracy, consistency, and accessibility.           integrating real-time data on people, events, and
Next, the platform performs spatiotemporal                    things, it accurately senses the campus environment
alignment and convergence of key data about                   and delivers space-based services. An open
entities, rules, behaviors, relationships, time, and          architecture, unified semantic interfaces, and a full
space to build a dynamic sensing ecosystem of                 set of agent toolchains enable autonomous sensing,
people, things, and spaces, delivering a high-quality         context understanding, intelligent task distribution,
data foundation for upper-layer applications.                 and dynamic workflow scheduling. This ensures
                                                              efficient collaboration and elastic resource scaling
Model-driven and knowledge                                    across agents, supports plug-and-play, autonomous
accumulation                                                  evolution, and efficient co-governance of all roles
                                                              in the ecosystem, and defines a new paradigm for
Capitalizing on the capabilities of AI models                 intelligent campuses that are perceptive, cognitive,
alongside campus-specific data—like historical                adaptive, and co-evolving.




                                                        104
04 Architecting with AI: Reference Architecture for AI Campuses




         4.4 Agentic Applications: Catalyzing Service
         Innovation and Unlocking the Digital Economy's
         Multiplier Effect
    Campus applications powered by agents will                          time heterogeneous data from diverse sources
    evolve from single-point intelligence to all-domain                 such as IoT sensors, HD video security, ambient
    autonomy, consistently driving service innovation                   temperature and humidity, and human movement
    and ecosystem upgrades. Moving forward, intelligent                 patterns. AR technology will let managers virtually
    campus applications will embrace multimodal                         navigate the park to easily monitor facility status,
    natural interaction, twin-based intelligent spaces, and             energy consumption, and crowd heat maps, enabling
    proactive intelligent services, building a self-evolving            truly transparent and intelligent management. This
    agent ecosystem that amplifies the digital economy's                spatially grounded, multi-agent intelligent service
    multiplier effect.                                                  model not only redefines campus operations but
                                                                        also powers digital transformation across various
    Multimodal natural interaction                                      industries, ushering in a new era of intelligent urban
                                                                        development.
    Future campuses will reengineer their services
    around agents, creating an open, well-operated                      Proactive intelligent services
    agent marketplace that seamlessly converts
    intelligent models into online services. This will                  Campus applications are evolving from mere "tools"
    quickly turn innovative ideas into reality and foster               to true "partners," powered by agents' precise intent
    a thriving, ongoing campus innovation ecosystem.                    recognition and customers' real-time spatiotemporal
    Multimodal interactions like voice, gestures, AR/                   data. Previously, campus services were mostly
    holography, and naked-eye 3D will become standard                   reactive and limited, offering the same experience
    features, allowing users to engage with campus                      to all users and struggling to meet personalized
    facilities through a "conversation as a service" model              needs. Today, agents use voice, behaviors, locations,
    where average response times drop from hours                        preferences, and other dimensions of data, coupled
    to seconds. Natural interaction makes technology                    with the user journey, to accurately identify users'
    imperceptible and the experience seamless, allowing                 potential needs across numerous business scenarios.
    spaces to "hear, see, and understand" users.                        More importantly, thanks to reinforcement learning,
                                                                        agents continue to enhance their judgments and
    A twin-based intelligent space                                      recommendations with every user interaction,
                                                                        becoming smarter over time. They have evolved
    Agents are more than just technical tools. They                     beyond simple command executors into intelligent,
    represent a new digital entity triggering continuing                context-aware assistants who understand user
    innovation on campuses. On future intelligent                       habits and anticipate behaviors, allowing services
    campuses, high-fidelity digital twins will act as a                 to integrate seamlessly into daily life and provide
    "digital mirror" of physical spaces, integrating real-              effortless, easy-to-access support.




                                                                  105
                                                                 Architecting with AI: Reference Architecture for AI Campuses 04




    4.5 Agentic Security: Proactive, Intelligent
    Defense System for Service Continuity and Core
    Value Protection
AI campuses establish a proactive security                      Personnel security
architecture encompassing five dimensions—physical
space, personnel, events, assets, and information—              A full-lifecycle precise management system for
with a focus on intelligent sensing, data-driven                employees and visitors is established using unified
insights, and collaborative response. Powered by a              identity authentication and multimodal biometric
unified data platform and AI, the system enables                recognition technologies. Access control based
converged analysis and intelligent decision-making              on target recognition, trajectory tracking, and AI-
across multiple data sources, transforming security             based exception analysis ensures traffic efficiency,
operations from passive response to proactive alerts            promptly evaluates and proactively handles
and automatic handling. This significantly enhances             personnel risks, and provides quick positioning and
campus security, resilience, operational efficiency,            evacuation guidance in case of emergencies.
and service assurance, providing strong support for
sustainable campus development.                                 Event security

The entire security system features learning and                An E2E intelligent emergency response mechanism
adaptive evolution capabilities similar to a biological         covering monitoring, warning, handling, and review
immune system. Each security event acts like a                  is available. The system integrates data from multiple
vaccine, strengthening the system and continuously              sources, including videos, sensors, and alarm systems,
refining defense strategies, architectures, and the             and employs AI algorithms to intelligently detect
overall security framework.                                     security events, fires, and accidents, as well as assess
                                                                their severity levels. It then automatically activates
Spatial security                                                contingency plans, schedules resources, coordinates
                                                                with other systems, and logs the entire process,
An all-domain IoT sensing network coupled with                  vastly enhancing the efficiency and standardization
a 3D digital twin platform enables the multi-                   of emergency response.
dimensional monitoring and visualized management
of buildings, roads, pipelines, and public spaces. This         Asset security
facilitates cross-domain collaboration and intelligent
linkage between physical and digital environments.              The system employs IoT, RFID, Wi-Fi FTM, NearLink,
Spatial security integrates a host of technologies like         UWB, and other positioning technologies to
AI video analysis, intelligent sensors, and geofences           monitor campus facilities, devices, critical materials,
to automatically detect anomalies, including                    and vehicles throughout their lifecycles. Predictive
perimeter intrusions, parking violations, and over-             maintenance models analyze device data to forecast
occupancy. It also enforces dynamic permission                  potential faults and issue early warnings. Geofences
control by area and level, creating an intelligent              and trajectory tracking prevent unauthorized asset
protection system characterized by seamless access              movements and losses, enhancing visibility, risk
and proactive alerts.                                           control, and traceability in asset management.




                                                          106
04 Architecting with AI: Reference Architecture for AI Campuses



    Information security                                                           architecture covers networks, terminals, applications,
                                                                                   and data, ensuring information security. AI-powered
    The defense system has entered a new phase of                                  threat detection, data encryption, access control, and
    AI vs. AI. It faces increasingly sophisticated and                             user behavior analysis detect network attacks in real
    automated network threats, especially deepfakes,                               time, prevent data leaks, and dynamically manage
    automated vulnerability detection, and APT attacks.                            intranet risks, ensuring campus service systems run
    An in-depth defense system based on the zero trust                             stably and core data is secure and compliant.




         4.6 Agentic O&M: Cross-Domain Full-Stack
         Convergence, Proactive Service Assurance, and
         Intelligent Fault Self-Healing
    T h e t ra n s fo r m a t i o n t o wa rd A I c a m p u s e s ,                assurance, and intelligent decision-making—creates
    characterized by pervasive terminals, diversified                              an AI-driven, highly automated, and simplified
    s e r v i c e s , a n d t i g h t I T / C T c o n v e rg e n c e , i s         O&M system, which provides a solid, reliable, and
    stretching traditional O&M to its limits. Major                                efficient technical foundation for the ongoing
    challenges such as isolated domains, passive                                   growth of AI campuses.
    responses, high dependency on manual
    operations, and slow decision-making are major                                 Full-stack convergence
    obstacles to the stable and green operation of
    fully intelligent campus services.                                             Full-stack convergence is the cornerstone of
                                                                                   the Agentic O&M system. It moves beyond
    The Agentic O&M reference architecture is an                                   the limitations of traditional IT, network, IoT,
    ideal choice. It deeply integrates cutting-edge                                and service O&M to build a unified, AI-driven
    AI technologies like big data, large models, and                               platform for campus-wide data integration and
    agent engines to simplify O&M, enable services,                                collaborative analysis, enabling a shift from
    and proactively detect risks. By redesigning O&M                               device- centric to service- centric O&M. The
    processes, this architecture eliminates domain                                 system consolidates multi-domain O&M data,
    silos and delivers full-stack service visibility. It                           and employs AI algorithms to create a full-
    precisely addresses AI campus service needs,                                   stack dependency graph spanning devices, links,
    ensuring the efficient operation of intelligent                                applications, and services. It incorporates expert
    applications. Additionally, it shifts the focus of                             knowledge into an intelligent O&M knowledge
    O&M toward intelligent self-healing to minimize                                graph and leverages a converged computing
    manual intervention and enhance system resilience.                             engine to enable AI-based decision-making with
    Built-in low-carbon features leverage all-domain                               cross-domain data, effectively eliminating O&M
    sensing, competitive optimization, and intelligent                             blind spots. Additionally, the system integrates
    scheduling to enable efficient, clean energy use                               AI natural language interaction interfaces to
    across campuses. Collaboration between three                                   intelligently interpret O&M intents, streamlining
    core capabilities—full-stack convergence, proactive                            operations and laying a solid foundation for all-




                                                                             107
                                                               Architecting with AI: Reference Architecture for AI Campuses 04




domain collaborative O&M on AI campuses. The                  Intelligent decision-making
deep convergence of IT and OT device data with
advanced intelligent optimization algorithms                  Intelligent decision-making is central to Agentic
creates a closed-loop control system for IT-OT                O&M system's efficiency. It shifts O&M focus
collaboration, ensuring operational reliability while         to proactive prediction and enables intelligent
effectively reducing energy consumption.                      self-healing through automated closed-loop
                                                              management, covering fault detection, location,
Proactive assurance                                           resolution, and verification. This minimizes manual
                                                              intervention and ensures the high reliability
Proactive assurance lies at the heart of the Agentic          demanded by AI campuses. The system leverages AI
O&M system. Guided by AI campus service needs,                prediction models and intelligent decision-making
it transitions O&M from passive response to                   engines to continuously analyze O&M indicators,
proactive assurance, enabling the agile deployment            proactively warning of faults based on scenario-
and stable operations of AI services like intelligent         specific AI campus models, such as service peaks and
security and energy management. The system                    large-scale events. When a fault occurs, the AI root
employs an AI operations agent to collect and                 cause analysis engine rapidly correlates cross-domain
analyze service data in real time, accurately                 data to locate the source. The intelligent handling
identifying core requirements and creating                    engine then selects the optimal solution and creates
requirement profiles. Meanwhile, the AI scheduling            a standardized process. After resolution, the AI
engine dynamically allocates compute, storage,                verification engine automatically confirms the fix
and network resources to flexibly support service             and, if it fails, restarts the process, vastly improving
scaling. The system also establishes an E2E AI                handling efficiency and system resilience. Moreover,
campus service experience evaluation framework.               the precise load prediction model comprehensively
It monitors indicators in real time, automatically            schedules and optimizes the campus generation-
detects experience bottlenecks and potential risks,           grid-load-storage system, ensuring stable operations,
and generates intelligent optimization strategies to          enhancing energy efficiency, and supporting zero-
proactively identify and address service issues.              carbon campus construction.




                                                        108
109
05
Advancing with AI:
Paths and Evaluation for Building AI Campuses




              110
05 Advancing with AI: Paths and Evaluation for Building AI Campuses




         5.1 AI Campus: Integrated Planning,
         Construction, and Operations
    The construction of AI campuses has evolved                                                            essential for successful AI campus development.
    beyond stacking isolated technologies; it is now a                                                     It breaks down the long-standing silos between
    transformation underpinned by full-lifecycle and                                                       campus planning and operations, as well as the
    systemic thinking. Fueled by rapid technological                                                       gaps between construction and real-world needs
    advances and evolving demands, an integrated                                                           in traditional projects. This ensures the intelligent
    planning-construction-operations framework is                                                          campus value is firmly established from the start.




                                                                                                                 Coor
                                                                                  ns                                 din
                                                                               tio                                      atin
                                                                            era                   Planning                  g     co
                                                                          op                                                        ns
                                                                   g                                                                  tr
                                                                 in
                                                                ur




                                                                                                                                          uc
                                                              td




                                                                                                                                            tio
                                                                                                                                               na
                                                           rin




                                                                                                        Do




                                                                                                                                                 nd
                                                         ep
                                                      blu




                                                                                                                                                    ope
                                                                                                           the
                                                  the




                                                                                                                                                       ratio
                                                                                                               rig
                                     Implementing




                                                                                                                  ht




                                                                                                                                                            ns durin
                                                                                         lue




                                                                                                                     thi
                                                                                       va




                                                                                                                        ng
                                                                                    ze




                                                                                                 Intelligent
                                                                                                                                                                    g planning
                                                                                                                          s
                                                                                 ali




                                                                                                  Campus
                                                                               Re




                                                                                                      Do things right

                                        Operations                                                                                   Construction

                                                              D
                                                                     ee                                                                       io
                                                                                                                                         n




                                                                          pl                                                             ct
                                                                               ye                                             s   tr u
                                                                                    ng                                   c on
                                                                                         agin
                                                                                                g operators during


                                     Figure 41 Intelligent campus construction phases


                                                                                                    111
                                                                        Advancing with AI: Paths and Evaluation for Building AI Campuses 05




Against this backdrop, the rise of agentic AI is                             basic management needs. Looking ahead, AI's
accelerating the intelligent campus' transition                              inherent iteration and connectivity transform
from phase 1.0, focused on device networking                                 the integrated planning-construction-operations
and data collection, to phase 2.0, emphasizing                               framework from a front-loaded process into
intelligent interaction and value creation.                                  a full-cycle closed loop. Planning sets the
This enriches the concept of integrated planning,                            value and vision, construction drives technology
construction, and operations, moving beyond                                  implementation, operations generate high-value
process alignment to prioritize high-value scenarios.                        data, and this data continuously informs and
The aim is to create a campus ecosystem that can                             optimizes planning, creating a complete self-
sense, think, and evolve, rather than just meet                              reinforcing, iterative logical system.




  5.1.1 Planning: AI Campus as a Systematic Endeavor—Vision-Driven,
  Holistically Planned, and Led by Top-Level Design

Top-level design is an engineering concept                                   The value and necessity of top-level design lies
that involves systematic planning to coordinate                              in tackling the three fundamental questions
all project layers and elements holistically.                                about intelligent campuses. Intelligent campus
In intelligent campus development, it guides                                 construction is a complex, system-wide effort that
evolution through an intelligent vision,                                     turns uncertainty into clarity. The core value of top-
technological enablement, and alignment with                                 level design is the way it can ground processes in
campus services, ecosystems, and stakeholders.                               the campus' real needs. It does this by answering
Top-level design delivers a clear, comprehensive                             three key questions: Where are we now? Where
strategic blueprint for intelligent campuses, serving                        are we heading? And how do we get there? This
as a practical guide to turn the vision into a reality.                      approach enables a coordinated, comprehensive
As the foundation of the integrated planning-                                blueprint for intelligent campuses.
construction-operations framework, the planning
phase is vision-driven, holistically planned, and                            • Question 1—Where are we now: industry
led by top-level design. Developing an AI campus                                insights and requirement understanding. Rooted
is not a series of isolated projects but a strategic,                           in the campus' service status quo and core
long-term value-driven effort.                                                  user needs, and guided by external policies
                                                                                and technology trends, identify pain points and
Today, technological advances are turning                                       requirements to establish a clear starting point
i n t e l l i g e n t c a m p u s e s f ro m c o n c e p t s i n t o            for intelligent transformation.
reality, with top-level design guiding their
development and operations. By adopting a                                    • Question 2—Where are we heading: vision,
forward-thinking approach and a cohesive set of                                 objectives, and blueprint architecture. Define the
initiatives, it sets the direction and roadmap for                              campus' future goals for intelligent development
intelligent campuses. As an old Chinese saying                                  and present a vision blueprint encompassing
goes, "preparedness ensures success, while                                      services, technologies, and user experiences to
unpreparedness spells failure." Top-level design                                align the overall development direction.
acts as the compass for ICT-based and intelligent
campus construction, shaping the project's value                             • Question 3—How do we get there: design and
goals and ensuring effective execution.                                         implementation strategies for service scenarios.



                                                                       112
05 Advancing with AI: Paths and Evaluation for Building AI Campuses



       Outline the roadmap from the status quo to                       trends, technology applications, and global
       the vision, detailing service scenarios, technical               best practices, while gathering core customer
       architecture, and implementation steps to ensure                 requirements through surveys and observations.
       successful execution.                                            Identify key intelligent campus needs, such as
                                                                        simplified and convenient visitor experiences and
    The intelligent campus top-level design boasts three                high-speed network coverage. Finally, drawing
    core characteristics. First, top-level determinism is a             from internal and external analysis, develop
    top-down design where service requirements and                      intelligent construction recommendations that
    core objectives guide all solutions and resources,                  align with customer needs, and define the
    ensuring alignment to achieve goals. Essentially,                   intelligent campus vision and objectives to guide
    the foundation is set from the top. Second, digital                 future planning.
    technology enablement. A digital platform integrates
    ICTs to accelerate application development and                    • Step 2: Blueprint design—Crafting the intelligent
    facilitate cloud-pipe-device synergy, bridging the                  campus blueprint. Based on the vision and
    physical and digital worlds to drive intelligent                    objectives, center on user experience and use
    services and operations. Third, practical operability. It           digital technologies to design the service and
    focuses on a scenario-oriented design that bridges                  system framework. Service scenario and user
    theoretical and real-world practices, creating a                    journey designs should target campus business
    viable blueprint to turn vision into reality and enable             departments, registered enterprises, and
    intelligent transformation. Thanks to this top-level                residents. Following an ethos of human-centered
    design, intelligent campuses achieve a unified,                     design and sustainability, employ methods
    comprehensive layout—progressing construction                       like Design Thinking to craft experience-driven
    and operations from the top down and ultimately                     journeys, highlighting core scenarios such as
    completing its grand voyage toward full intelligent                 intelligent office and seamless access. For
    transformation.                                                     system architecture design, prioritize technology
                                                                        implementation by clearly defining intelligent
    Top-level design follows a three-step approach                      capabilities, core technologies, and required
    from insight to implementation: Drawing on                          software and hardware. Build an integrated
    its extensive practical experience, Huawei has                      architecture that comprises the application
    developed a three-step approach for intelligent                     system, digital platform, and intelligent
    campus top-level design, creating an interlocking                   infrastructure to closely match the system with
    process that guides planning through to effective                   service requirements.
    implementation.
                                                                      • Step 3: Implementation path planning—
    • Step 1: Insight and status quo analysis—                          Specifying the implementation pace. Using
       Defining the starting point. Identify the pain                   the blueprint framework, develop a phased
       points and requirements for building intelligent                 implementation plan prioritized by urgency and
       campuses through thorough internal and                           feasibility. Specify the list of implementation
       external analysis, establishing a clear starting                 projects. Break down the vision architecture into
       point for development. Internally, survey each                   specific intelligent applications, platforms, and
       business department's strategies, pain points,                   infrastructure projects to ensure full alignment
       and needs to clarify intelligent construction                    with the plan. Phase the implementation by
       priorities aligned with overall campus objectives.               assessing each project's service urgency and
       Externally, analyze higher-level policies, industry              complexity, categorizing them as primary priority,




                                                                113
                                                              Advancing with AI: Paths and Evaluation for Building AI Campuses 05




   secondary priority, or long-term. For example,                    organizational and process adjustments.
   the digital platform and network are primary
   priorities; intelligent office is a secondary priority.         • Second, top-level design should align with
   Develop the implementation schedule and                           national strategies and support campus service
   budget that reflect service priorities, customer                  strategies. It maps the entire service landscape
   needs, and funding availability, clearly defining                 and designs solutions grounded in requirements,
   short-, mid-, and long-term milestones, scope,                    value, and technology maturity to ensure
   and investment estimates to ensure the plan is                    effective implementation. It must also respond
   practical and executable.                                         to national strategies like Dual Carbon and
                                                                     Digital China by integrating green, low-carbon
The essence of top-level design is to ensure                         development and digital governance. Through
effective plan execution. Top-level design does not                  scenario-specific planning, such as carbon
guarantee success but maximizes its likelihood. To                   emission management and digital governance,
do so, three key principles must be strictly followed.               top-level design ensures that intelligent
                                                                     campuses operate in sync with national directives.
• First, top-level design is a top-down project. Top
   leaders' understanding and attitudes toward                     • Last but not least, top-level design must be
   intelligent campuses shape the scope and depth                    led by a professional team. Top-level design
   of campus construction. Top leaders should                        for intelligent campuses is a cross-domain
   engage in the process and review of top-level                     undertaking that demands deep collaboration
   design, approve project initiation and funding,                   between the owner, the solution provider,
   and incorporate intelligent campus construction                   and professional institutions. The core project
   into the enterprise's development plan. They                      team typically encompasses a top-level design
   must also coordinate the benefits and resources                   consulting team, an intelligent campus solution
   throughout the entire lifecycle of planning,                      team, specialized technical units, and the owner's
   construction, and operations, while managing                      project management team.




                                                             114
05 Advancing with AI: Paths and Evaluation for Building AI Campuses




      5.1.2 Construction: Prioritizing Architecture and Integrating
      Digital and Intelligent Capabilities to Build an Intelligent Campus
      Construction System

    AI campus construction is a systematic project                               inspections. The operations agent cuts costs
    guided by top-level design and centered on                                   and improves efficiency via energy consumption
    operational feasibility. It offers a practical path for                      control and security management. Meanwhile,
    campus digital transformation by breaking down                               the security and O&M agent safeguards
    data silos and enabling seamless technology                                  campus networks, devices, and facilities while
    integration and service collaboration.                                       minimizing manual intervention.


    The project encompasses four key dimensions.                               • Second, the project involves creat ing
                                                                                 an intelligent platform that is visible,
    • The first is the application layer, where AI                               manageable, and controllable.
       powers campus applications and enables the
       deployment of all-scenario agents.
                                                                                  High-fidelity data is crucial for effective
                                                                                  AI inference. It must completely reflect
         A collaborative system of diverse                                        real-world operational conditions and
         AI agents should be developed to                                         environments while ensuring unity,
         support core campus businesses like                                      dynamism, and interoperability.
         m a n a g e m e n t a n d s e r v i ce s . E a c h
         agent possesses autonomous sensing,                                      Chinasoft International Co., Ltd.
         semantic understanding, task decision-
         making, collaborat ive execut ion,
         and self-learning capabilities,
                                                                                 Typical examples, including HarmonyOS-
         creating a novel campus operation
                                                                                 based IoT device data and core campus service
         and governance model centered on
                                                                                 data, can digitally restore the entire campus
         human-machine collaboration and
                                                                                 service process. To fully maximize data value,
         autonomous decision-making.
                                                                                 an intelligent platform is developed to process,
                                                                                 analyze, and orchestrate data, offering four
         Shenzhen Webuild Technology Co., Ltd.
                                                                                 core service capabilities. The super soft bus
                                                                                 streamlines communication interfaces between
                                                                                 diverse systems and devices, enabling seamless
       Looking ahead, numerous agents throughout                                 cross-platform data exchange. The space
       the campus deliver intelligent space capabilities,                        service digitally models the campus' physical
       c o v e r i n g t h e e n t i re c a m p u s o p e ra t i o n s           environment, supporting positioning and resource
       process. Specifically, the office agent boosts                            scheduling. The AI service integrates machine
       work efficiency with AI conference assistants                             learning and deep learning to intelligently analyze
       and process automation tools. The life agent                              massive amounts of data and deliver decision
       focuses on smart travel and commute planning                              insights. The data service manages the entire data
       to optimize user experience. The production                               lifecycle (storage, cleansing, anonymization, and
       agent enables industrial production through                               sharing), ensuring campus operations are visible,
       predictive maintenance and AI-powered quality                             manageable, and controllable.



                                                                         115
                                                                                                                                                                        Advancing with AI: Paths and Evaluation for Building AI Campuses 05




• The third is the campus network. The network                                                                                                                                   monitoring and digital representation of
  layer serves as a crucial link between the                                                                                                                                     people, objects, and environments on campus.
  terminal sensing and intelligent platform layers,                                                                                                                              Moreover, open HarmonyOS ensures seamless
  establishing an all-scenario network architecture                                                                                                                              interoperability across devices from various
  characterized by low latency, high bandwidth,                                                                                                                                  brands and using different protocols, effectively
  and broad connectivity. This layer integrates a                                                                                                                                eliminating data silos.
  myriad of core technologies like NearLink, Wi-
  Fi 7, POL, 5G, and Ethernet to address diverse                                                                                                                             • Intelligent campus construction centers on
  network demands across service scenarios. It                                                                                                                                   a digital platform, applying a decouple and
  supports low-power IoT devices via adaptive                                                                                                                                    rebuild strategy to replace traditional mesh
  technologies, while 5G enables high-mobility                                                                                                                                   integration of ELV systems. It is driven by three
  services. Featuring ubiquitous coverage, seamless                                                                                                                              key capabilities: easy integration, servitization,
  handover, and deterministic transmission, it                                                                                                                                   and scenario-based design. The construction
  ensures the efficient uplink of extensive sensing                                                                                                                              phase consists of four stages: preparation,
  data and real-time delivery of scheduling                                                                                                                                      implementation, acceptance, and rollout/
  commands from the intelligent platform.                                                                                                                                        handover (see Figure 42). In the preparation
                                                                                                                                                                                 phase, solution bidding is conducted based on
• Fourth, the device layer, which is the data                                                                                                                                    the top-level design to select contractors with
  source for AI campuses. Using multimodal                                                                                                                                       proven integration and verification expertise.
  sensing and an open HarmonyOS IoT system,                                                                                                                                      During implementation, a joint project team is
  this layer bridges the information gap between                                                                                                                                 set up, the functional requirement specification
  physical and digital spaces. The device layer covers                                                                                                                           (FRS) is clarified, and ELV engineering,
  five sensing technologies (including 5.5G wireless,                                                                                                                            platform construction, and service integration
  Wi-Fi, passive IoT, optical, and visual sensing) and                                                                                                                           are executed. The acceptance phase verifies
  supports diverse terminals, such as video security                                                                                                                             outcomes against the FRS and defined service
  devices, smart screens, access control systems,                                                                                                                                benefits. Finally, the rollout/handover phase
  PV and energy storage systems, VR/AR devices,                                                                                                                                  completes training and promotion to ensure full
  robots, and drones. This enables comprehensive                                                                                                                                 system operability.


         Planning                                                                                                                                                      Construction                                                                       Operations

                                1.Preparation                                                                                2.Implementation                                         3.Acceptance                             4.Rollout/Handover

                                                                           Project management, change management, milestone review, and supervision
              Import




  Campus
                                                                                                                               conﬁrmation
                                                                                                                               Requirement




   owner
                       selection


                                              selection




                                                                                                                                                         Cooperative                                                                        Transfer to
                       Solution


                                              Supplier




                                                                                                                                                                                   Official                     Internal        Internal
                                                                                                                                                       implementation                                                                          O&M/
                                                                                                                                                                                 acceptance                     training       promotion
                                                                                               Set up a joint project team




                                                                                                                                                        and system test                                                                     operations
                                                                                                                                                                                                                                                               Handover
                                                                            Contract signing




                                                                                                                                                                  Scenario
                                                Capability demonstration




                                                                                                                                                              integration test                                                  Customer training
                                                                                                                                 Requirement survey




                                                                                                                                                                                            System acceptance
                         Solution provision




                                                                                                                                                               Application
                                                                                                                                                           development and test
                                                                                                                                                                                                                Trial run




  Campus                                                                                                                                                                                                                        Rollout assurance
 contractor                                                                                                                                           Infrastructure    Digital platform
                                                                                                                                                       construction      construction

                                                                                                                                                                                                                            Transfer to O&M/operations
                                                                                                                                                              ELV engineering


                                                                           Project management, change management, milestone review, and supervision



                                                                            Figure 42 Intelligent campus construction phases



                                                                                                                                                                       116
05 Advancing with AI: Paths and Evaluation for Building AI Campuses



    To ensure successful construction, intelligent                    • Technical assurance: Risks are managed
    campus development must balance risks and                           by choosing reliable suppliers with proven
    resources by establishing a comprehensive                           development skills and integration experience,
    assurance system covering five key areas:                           conducting third-party assessments, and
    organization, funding, technology, project                          ensuring implementation teams closely follow
    duration, and ecosystem resources.                                  the top-level design strategy. For example,
                                                                        third-party professional organizations may be
    • Organizational assurance: Campus construction                     engaged to manage technical risks throughout
       is framed as digital transformation, led by                      key stages like solution review and technical
       a three-in-one team comprising enterprise                        verification. Meanwhile, implementation teams
       decision-makers, IT engineers and project                        must align with the top-level design and
       developers, as well as service and operations                    maintain regular communication to ensure the
       staff. Decision-makers set objectives and manage                 technical architecture stays consistent with the
       resources; IT engineers and project developers                   campus' strategies.
       execute the plan and oversee implementation;
       and service and operations staff contribute to                 • Project duration assurance: Implementation
       operability design and coordinate risk and cost                  s c h e d u l e s a re d e s i g n e d a ro u n d s e r v i c e
       management.                                                      requirements and system dependencies, allowing
                                                                        systems without prerequisite constraints to
    • Funding assurance: A dedicated budget for                         progress in tandem. A thorough full-lifecycle
       intelligent campus construction is allocated (e.g.,              assessment is conducted upfront, allocating ample
       10%–15% for venue campus projects). Returns                      time for intelligent project tendering, solution
       are evaluated over the full lifecycle, balancing                 refinement, and delivery acceptance. Contingency
       investment, operational revenue, and long-                       plans are also in place to handle unforeseen
       term benefits. This includes assessing input-                    issues, ensuring on-time project delivery.
       output ratios during construction and applying
       a value-benefit model to measure the impact of                 • Ecosystem resource assurance: Adopting a
       intelligent campus development on businesses,                    platform + ecosystem approach, the intelligent
       ensuring sustainable financial support for                       campus platform acts as the core carrier to
       ongoing intelligent upgrades.                                    integrate resources like architectural design
                                                                        firms, software and hardware vendors, and
                                                                        O&M suppliers. A collaborative cooperation
                                                                        mechanism is established to complement each
                                                                        other's advantages and build an open and
                                                                        shared ecosystem resource pool. This ensures
                                                                        a stable supply of resources for intelligent
                                                                        campus feature updates and scenario expansion,
                                                                        supporting sustainable long-term growth.


                                                                      These assurance measures effectively cope with
                                                                      risks related to progress, requirements, and
                                                                      technology during construction, ensuring the
                                                                      smooth implementation of intelligent campuses.




                                                                117
                                                              Advancing with AI: Paths and Evaluation for Building AI Campuses 05




  5.1.3 Operations: Value-Oriented, Data-Driven Evolution of
  Intelligent Campuses

Operations are continuous, cyclical management                     Its core value is reflected in three dimensions.
activities built around business processes. In
intelligent campuses, operations represent the                     • Data as capital: Data progresses from a
implementation phase across the entire lifecycle,                    mere resource to a valued asset. With AI and
where services are delivered, value is created, and                  digital platforms, campus operations evolve from
intelligence is embedded. AI is the core engine for                  traditional cost centers into data value centers.
shaping the value of intelligent campus operations.                  A unified digital foundation enables all-domain
                                                                     sensing and real-time interaction, connecting
                                                                     systems for facilities, energy, and services. This
    B y a n a ly z i n g m u l t i - d i m e n s i o n a l           changes decision-making from experience-based
    data, AI not only supports lifecycle                             judgment to intelligent prediction. Data becomes
    management and precise decision-                                 tradable, monetizable assets. This trend has been
    making, but also streamlines service                             recognized by decision-makers in data enterprises.
    processes, reduces energy consumption,
    and enhances both service and
    management efficiency.                                             Campuses are no longer just data
                                                                       collectors, but value creators.
    X u Bo, Manager at Zhuhai Xiangyi                                 By refining data on energy use, foot
    Aviation Technology Co., Ltd.                                      traffic, and industry collaboration,
                                                                       campuses can transform accumulated
                                                                       data into measurable, tradable assets.
                                                                       AI applications in security, inspection,
In practice, these capabilities are realized through                   and customer service enable
intelligent campus solutions which focus on daily                      automatic alerting and intelligent
operations such as security, facility management, and                  scheduling, reducing dependence
energy efficiency. By leveraging big data, campuses                    on manual work while enhancing
achieve continuous improvement and innovation,                         efficiency and monetization.
thereby maximizing the operational value.
                                                                       Xiao Guangrui, Chairman of Beijing
The concept and value of AI campus operations                          Bridata Technology Co., Ltd.
emerge in contrast to traditional practices.
Traditional campus operations, centered on physical
spaces and basic maintenance, face challenges such
as data silos, heavy reliance on manual work, and                  • Digital governance: Governance improves
high O&M costs. Intelligent campus operations,                       from fragmented management to integrated
however, introduce a new model of operation, with                    collaboration, strengthening safety and
data intelligence as the lifeblood, AI agents as the                 resilience. With digital infrastructure and
nervous system, digital platforms as the backbone, and               dynamic data on people, devices, and
application services as the body. This fundamentally                 environments, a digital twin system can be
transforms operations through the deep integration                   established for campuses. AI enables governance
of cloud, IoT, large models, and AI agents.                          systems with real-time monitoring, simulation,




                                                             118
05 Advancing with AI: Paths and Evaluation for Building AI Campuses



       and intelligent response, supporting precise                      implementation.
       management and emergency command in
       complex scenarios such as safety risks, public                 • Technical support: Digital platforms provide
       health, and traffic flows. This not only improves                 unified O&M services. Cloud-edge collaboration
       the timeliness and accuracy of governance                         addresses O&M challenges in application
       responses, but also transforms governance                         integration, lowers skill requirements, and
       from reactive handling to proactive prevention,                   supports end-to-end O&M.
       and from isolated management to systemic
       coordination.                                                  Similarly, the three stages are as follows:


    • Low-carbon development: Operations evolve                       • Scenario adaptation: Select pilot scenarios
       from energy consumption to sustainable                            (e.g., security patrols) around core business
       development. AI and big data allow precise                        needs. Map processes, organizations, and roles
       mapping and dynamic optimization of energy                        to solution functions. Standardize workflows,
       use against human activity and production                         redesign tools, optimize methods, and iterate
       rhythms. By integrating distributed energy,                       based on user feedback.
       storage, and microgrids, campuses build
       intelligent energy management platforms                        • Continuous operations: Establish closed-loop
       for integrated dispatching of generation,                         management of planning, execution, evaluation,
       grid, load, and storage, and real-time carbon                     and improvement. Define goals and strategies,
       emission monitoring. Future-oriented models,                      design processes and performance standards,
       such as green power trading and carbon asset                      expand tools and workflows from pilots,
       management, will further convert energy-saving                    strengthen audits, and evaluate operations by
       behaviors into sustainable economic returns,                      analyzing data and identifying pain points (e.g.,
       aligning ecological and economic benefits.                        function deficiencies). Continuous improvement
                                                                         is driven by incentives and accountability,
    The AI campus operational system rests on two                        ensuring business goals are achieved.
    pillars, and unfolds through three stages. The two
    pillars are as follows:                                           • Model innovation: Traditional campuses rely
                                                                         on specialized, localized fieldwork. Intelligent
    • Strategic collaboration: Corporate executives                      campus operations, powered by digital platforms,
       should define strategic goals, foster a digital                   enable a new model of centralized operations
       culture (e.g., promoting the value of intelligent                 + field services. By building an Agentic IOC,
       scenarios), and set clear responsibilities and                    intelligent campuses transcend specialty and
       performance targets to ensure effective                           regional limitations, achieving integrated
                                                                         operations that optimize workforce utilization
                                                                         and management efficiency.




                                                                119
                                                      Advancing with AI: Paths and Evaluation for Building AI Campuses 05




    5.2 AI Campus Maturity Assessment
As AI technologies are deeply integrated into              Currently, there are no unified standards for
campus scenarios, the original assessment                  assessing AI campuses. Prioritizing holistic balance,
framework can no longer fully capture the new              focused priorities, dynamic adjustment, and
characteristics and value of intelligent campuses.         practical applicability, and drawing on advanced
To bridge this gap, this white paper introduces            practices in the intelligent campus domain, this
an upgraded AI Campus Maturity Assessment                  white paper proposes a new AI Campus Maturity
Model, drawing on prior experience and tailored            Assessment Model as an industry reference.
to future application needs. The model is designed
to better align with the development logic and             The model defines six level-1 dimensions: strategic
value drivers of AI campuses.                              commitment, intelligent services, user experience,
                                                           efficient operations, technological leadership, and
Key questions for campus managers and operators            infrastructure. These six dimensions are mutually
include: How can the implementation phases and             reinforcing and seamlessly integrated. Together,
outcomes of AI campuses be accurately assessed?            they anchor the strategic direction of AI campuses
How can clear indicators guide the integration of          while focusing on the tangible value of AI in
AI capabilities from the planning phase? How can           service implementation, experience enhancement,
assessment uncover gaps in campus intelligence             and efficiency improvement. This model provides
and drive continuous improvement?                          a clear pathway for campuses to advance their
                                                           intelligence maturity.




                                                     120
05 Advancing with AI: Paths and Evaluation for Building AI Campuses




      5.2.1 Maturity Assessment Principles

    Building on Huawei's generational intelligent                     strengthens adaptability to AI capabilities while
    campus practices, the AI Campus Maturity                          retaining proven frameworks.
    Assessment Model is designed around four
    guiding principles: clear objectives, precise                     The model is designed for self-assessment, offering
    positioning, practical orientation, and a well-                   a lightweight and user-friendly approach. It defines
    defined architecture. Its primary purpose is to                   the depth of AI penetration in campuses, adds AI-
    define the level of AI evolution within campuses,                 specific capability dimensions to the traditional
    both to support the establishment of industry                     framework, and organizes indicators into an easy-
    standards in the AI campus domain and to provide                  to-understand, easy-to-use chain. This enables
    customers with clear guidance for self-assessment.                companies to quickly identify their current stage
    As an enhanced version of traditional models, it                  and benchmark progress against the model.




      5.2.2 Maturity Assessment System and Framework

    We have developed an AI campus maturity                           • Efficient operations: Assesses the effectiveness
    assessment system, with AI positioned as the core                    of AI in campus operations, covering automation
    enabler. The system outlines clear pathways for                      coverage in workflows, optimization efficiency
    campuses to advance their intelligence, spanning                     in resource scheduling, and speed of AI-enabled
    strategic, business, and service dimensions.                         risk detection and response.


    The system consists of six level-1 dimensions, each               • Technological leadership: Evaluates the
    defining core capabilities around AI integration.                    application of cutting-edge technologies, focusing
                                                                         on AI's integration with other technologies,
    • Strategic commitment: Focuses on strategy                          autonomous iteration of AI models, and elasticity
       awareness and execution, assessing the clarity of                 of AI-enabled computing infrastructure.
       AI campus planning, readiness of organizational
       assurance, and management of strategy                          • Infrastructure: Examines AI-adapted
       implementation.                                                   infrastructure capabilities, including the
                                                                         proportion of computing resources allocated to
    • Business innovation: Measures AI-driven                            AI, network support for AI data transmission, and
       business transformation, including AI penetration                 coverage of AI-enabled data collection scenarios.
       in new services, process automation, and
       efficiency of cross-domain AI data collaboration.              Framework and scoring rules

    • User experience: Evaluates AI's role in                         T h e s y s t e m a d o p t s a f i ve - l e ve l s c o r i n g
       personalized services and scenario-based                       framework, for which the rules are as follows:
       experience upgrades, with emphasis on accuracy
       of user requirement identification and timeliness              Maturity level: On a scale of one to five points, the
       of service response.                                           overall maturity score is calculated as the weighted




                                                                121
                                                        Advancing with AI: Paths and Evaluation for Building AI Campuses 05




average of all level-1 scores (AI-related indicators         • Score level-3 indicators on a 1–5 scale: Each
may carry an additional 20–30% weight). Levels                 level-3 indicator is rated against the L1–L5
are defined as follows: 1.0–2.0 points (traditional            descriptions in the Table 1 (e.g., L1 = none, L5 =
campus), 2.1–3.0 points (intelligent campus 1.0),              full AI coverage), using detailed rules.
3.1–5.0 points (intelligent campus 2.0/AI campus).
                                                             • Calculate the score of level-2 indicators: For
Each level-3 indicator is scored on a five-point               each level-2 indicator, average the scores of its
scale, with results weighted to calculate the scores           associated level-3 indicators.
of upper-level indicators. The assessment focuses
on three key aspects of AI technology: penetration           • Calculate the score of level-1 indicators: For
(application scope), maturity (technical depth), and           each level-1 indicator, average the scores of its
value contribution (business impact).                          associated level-2 indicators.


Score calculation:                                           • Calculate the total maturity score: Multiply each
                                                               level-1 score by its assigned weight (in decimal
The scoring logic is based on matching campus                  form), then sum all weighted level-1 scores to
types with corresponding weights. The overall                  obtain the final total score (see Figure 43).
maturity score (on a 1–5 scale) is calculated as
the weighted sum of level-1 dimension scores,                • Overall maturity score = Weighted sum of all
following these steps:                                         level-1 scores


                              Intelligent campus maturity assessment framework:
                          Recommended weights of level-1 dimensions by campus type

                                                                                     Campuses Serving
                                      Campuses for Internal Use
                                                                                    External Enterprises
  Level-1
  Maturity Dimension
                                                       Cross-City/                                  Cross-City/
                                  Single Campus                            Single Campus
                                                     Country Campus                               Country Campus

  [Strategic commitment]
  Strategy awareness and                0%                   10%                  0%                     15%
  execution

  [Business innovation]
  AI-driven business                    5%                   10%                  5%                     15%
  transformation

  [User experience]
  Superior experience and              20%                   15%                  25%                    20%
  services

  [Efficient operations]
  Efficient campus operations          20%                   30%                  40%                    20%
  and management

  [Technological leadership]
  Cutting-edge technology              30%                   15%                  20%                    10%
  application

  [Infrastructure]
  Future-proof infrastructure          25%                   20%                  10%                    20%
  deployment


                     Figure 43 Maturity scoring and weighting rules for intelligent campuses




                                                       122
05 Advancing with AI: Paths and Evaluation for Building AI Campuses



    Based on the scoring results, the intelligence                                                               driving business innovation and end-to-end
    level of AI campuses can be classified into                                                                  process automation. The campus demonstrates
    corresponding maturity stages, with core                                                                     autonomous iteration of AI models, fully AI-
    capability requirements defined as follows:                                                                  adapted infrastructure including computing
                                                                                                                 and networks, superior user experience, and a
    • Traditional campus (1.0–2.0 points): The                                                                   mature AI-powered operational system.
        campus is equipped with basic digital capabilities,
        introduces isolated AI applications in selected                                                     This assessment system applies to campuses of all
        business scenarios, and establishes an initial                                                      types, supporting both planning and design as well
        intelligent operations platform. However, AI                                                        as optimization during deployment and operations.
        integration and value contribution remain limited.                                                  As the scope of AI campuses continues to expand,
                                                                                                            the model will be continuously upgraded.
    • Intelligent campus 1.0 (2.1–3.0 points): AI
        achieves greater penetration in core business                                                       To provide a clear view of capability differences
        scenarios, enabling partial workflow automation                                                     across maturity levels, we include the AI campus
        and delivering noticeable improvements in                                                           maturity assessment indicator table (see Appendix).
        operational efficiency and user experience. Basic                                                   This table details the requirements for each
        AI technical support capabilities are established.                                                  level-2 and level-3 indicator under the six level-1
                                                                                                            dimensions, helping campuses quickly identify their
    • AI campus (3.1–5.0 points): AI is fully                                                               current stage and clarify the core directions for
        embedded across the entire business chain,                                                          advancing to higher maturity levels.


     Level-1 Dimension       Level-2 Indicator           Level-3 Indicator              L1: 1 Point             L2: 2 Points    L3: 3 Points        L4: 4 Points           L5: 5 Points

                                                                                                                                                                   AI deeply integrated with
                                                                                   Clear vision and                            Short-term AI                       campus strategies, quantifiable
                                                                                                                    →                                   →
                                                        1.1.1 Intelligent campus
                                                                                   objectives for intelligent                  projects, but not                   long-term goals (e .g., AI-
                                                        vision and objectives
                                                                                   campuses                                    systematic                          driven efficiency improvement
                                                                                                                                                                   by 30%)
                             1.1 Intelligent strategy
                             planning
                                                                                                                                                                   A comprehensive roadmap
                                                        1.1.2 Intelligent                                                                                          covering technologies,
                                                                                                                    →                                   →
                                                                                   No implementation                           Technical pilot
                                                        campus roadmap and                                                                                         data, and talent, with a
                                                                                   plan                                        roadmap
                                                        implementation path                                                                                        well-established dynamic
                                                                                                                                                                   adjustment mechanism

     [Strategic                                                                                                                                                    Establishment of an AI
                                                        1.2.1 Dedicated
                                                                                                                    →                                   →
     commitment]                                                                   No dedicated person in                      Cross-department                    innovation center, efficient
                                                        intelligent campus
     Strategy planning and                                                         charge                                      virtual team                        collaboration with business
                                                        team
     execution                                                                                                                                                     departments
                             1.2 Intelligent
                                                        1.2.2 Intelligent campus   Intelligent campus                          Intelligent campus
                                                                                                                    →                                   →
                             organization and                                                                                                                      Intelligent campus resources >
                                                        investment and             resources < 1% of                           resources: 1%–3%
                             governance                                                                                                                            3% of annual revenue
                                                        resource allocation        annual revenue                              of annual revenue

                                                                                                                                                                   Establishment of an AI ethics
                                                                                                                    →
                                                        1.2.3 AI ethics and risk   No guidelines for AI                        Basic data privacy
                                                                                                                                                                   committee to regularly audit
                                                        management                 ethics                                      protection
                                                                                                                                                                   algorithmic fairness

                             1.3 Monitoring of          1.3.1 Performance          Not included in the                                                             KPIs-based real-time
                                                                                                                    →                                   →
                                                                                                                               Regular check of
                             intelligent strategy       evaluation mechanism       performance evaluation                                                          dashboards, automatic strategy
                                                                                                                               project progress
                             execution                  for intelligent campuses   of any department                                                               optimization

                                                                                   Innovative scenarios

                                                                                                                    →                                   →
                                                        2.1.1 Proportion of        and scenario
                                                                                                                               > 20%                               > 30%
                                                        innovative scenarios       optimization rate <
                             2.1 Continuous
                                                                                   10%
                             innovation in
                             intelligent scenarios
                                                        2.1.2 Measurement of                                                                                       Qualitative and quantitative
                                                                                                                    →                                   →
                                                                                                                               Measurable cost
     [Business innovation]                              scenario innovation        Non-quantifiable                                                                value can be measured by AI
                                                                                                                               reduction
     Business                                           value                                                                                                      independently
     transformation and
     scenario innovation                                2.2.1 Business                                                                                             Complete intelligent campus
                                                                                                                    →                                   →
                                                                                   No business                                 Complete process
                                                        transformation                                                                                             process architecture and
                                                                                   architecture                                architecture
                             2.2 Campus business        architecture                                                                                               operational system
                             transformation and
                             innovation                                                                                        10% to 20%
                                                                                                                    →                                   →
                                                        2.2.2 Business             No quantitative                                                                 Reduced campus-wide
                                                                                                                               higher efficiency
                                                        transformation KPIs        indicators                                                                      operational cost ≥ 25%
                                                                                                                               in some areas




                                                                                                   123
                                                                                                      Advancing with AI: Paths and Evaluation for Building AI Campuses 05




Level-1 Dimension        Level-2 Indicator          Level-3 Indicator               L1: 1 Point            L2: 2 Points    L3: 3 Points        L4: 4 Points           L5: 5 Points

                                                                                                                          50% of high-
                                                                                                               →                                   →
                                                   3.1.1 Intelligent service   Isolated information                                                           All scenarios covered, emotion
                                                                                                                          frequency services
                                                   coverage                    service                                                                        recognition accuracy > 90%
                                                                                                                          covered
                         3.1 Intelligent
                         personalized services
                                                   3.1.2 Personalized                                                                                         AI-based dynamic
                                                                                                               →                                   →
                                                                                                                          Rule-based
                                                   service                     No recommendation                                                              recommendation, user
                                                                                                                          recommendation
                                                   recommendation                                                                                             satisfaction: 4.5–5 points

                                                                                                                          Voice/Image
                                                                                                               →                                   →
                                                   3.2.1 Multimodal                                                                                           Immersive experience with VR/
                                                                               Traditional interface                      interaction
[User experience]                                  interaction                                                                                                AR
                                                                                                                          supported
Superior experience      3.2 Interaction
and services             experience
                                                                                                                                                              Response within seconds,
                                                                                                               →                                   →
                                                   3.2.2 Real-time             Response time > 5
                                                                                                                          1–5 minutes                         self-adaptive optimization
                                                   response                    minutes
                                                                                                                                                              mechanism

                                                   3.3.1 Coverage of                                                      AI-based
                                                                                                               →                                   →
                                                                                                                                                              AI-based demand prediction,
                                                   intelligent space           Basic digitalization                       multi-scenario
                                                                                                                                                              proactive service provision
                         3.3 Intelligent space     scenarios                                                              collaboration
                         innovation

                                                                                                               →                                   →
                                                                                                                                                              4.5–5 points, AI service
                                                   3.3.2 User satisfaction     Satisfaction < 3.5 points                  3.5–4.5 points
                                                                                                                                                              complaint rate < 1%


                                                                                                               →                                   →
                                                   4.1.1 Process               Key process automation                                                         > 90%, AI-based decision
                                                                                                                          50%–70%
                                                   automation rate             rate < 20%                                                                     making and anomaly handling
                         4.1 Intelligent and
                                                                                                                          Autonomous
                         automated operations      4.1.2 Intelligent and
                                                                                                               →                                   →
                                                                               System detection,                          devices,                            AI fault prediction (accuracy >
                                                   automated operations
                                                                               manual execution                           command-based                       95%), autonomous execution
                                                   management
                                                                                                                          execution

                                                                                                                          Data analysis for
                                                                                                               →                                   →
                                                   4.2.1 Proportion of         Dependent on                                                                   Real-time AI decision
[Efficient operations]                                                                                                    partial business
                                                   data-driven decisions       experience                                                                     suggestions, adoption > 80%
Efficient campus                                                                                                          activities
                         4.2 AI decision-
operations and
                         making support
management                                         4.2.2 Resource                                                         Dynamic                             AI-powered collaborative
                                                   optimization and            Static allocation               →          scheduling of            →          optimization of energy, space,
                                                   scheduling                                                             basic resources                     and other resources

                                                                               Human-machine                              AI identification
                                                                                                               →                                   →
                                                   4.3.1 Intelligent campus                                                                                   Campus-wide proactive
                                                                               collaboration in security                  of abnormal
                         4.3 Campus security       security management                                                                                        warning, false alarms < 5%
                                                                               management                                 behavior
                         and emergency
                         management
                                                                                                               →                                   →
                                                   4.3.2 Intelligent           Manual emergency                           AI-assisted                         Automatic trigger, 50% faster
                                                   emergency response          plan                                       simulation                          response

                                                                                                                          Leading
                                                                                                                                                              Deep learning/Large models,
                                                                                                               →                                   →
                                                   5.1.1 Technology            Traditional intelligent                    technologies such
                                                                                                                                                              supporting transfer learning
                                                   maturity                    technologies                               as IoT and big
                         5.1 Depth of                                                                                                                         across technical domains
                                                                                                                          data
                         intelligent
                         technologies
                                                                                                                          Self-development                    Self-development of core
                                                                                                               →                                   →
                                                   5.1.2 Technology            Fully outsourced and
                                                                                                                          and self-O&M of                     algorithms, localization rate >
                                                   autonomy                    purchased
                                                                                                                          core parts                          80%

                                                                                                                                                              Full-stack integration of AI,
                                                                                                               →                                   →
                                                   5.2.1 Technology            No technology                              AI and IoT/big
[Technological                                                                                                                                                edge computing, and digital
                                                   convergence                 convergence                                data convergence
leadership]              5.2 Breadth                                                                                                                          twin
Cutting-edge             of intelligent
technology application   technologies                                                                                                                         Quarterly iteration, automatic
                                                                                                               →                                   →
                                                   5.2.2 Technology                                                       Updated once or
                                                                               No updates                                                                     optimization based on
                                                   update and iteration                                                   twice a year
                                                                                                                                                              feedback

                                                                               Proportion of R&D
                                                                                                               →                                   →
                                                   5.3.1 R&D investment                                                                                       > 10%, focusing on frontier
                                                                               in intelligent campus                      5%–8%
                                                   proportion                                                                                                 exploration
                                                                               investment < 3%
                         5.3 AI innovation
                                                   5.3.2 Innovation
                                                                                                               →                                   →
                                                                                                                          1–2 innovations
                                                   achievement                 No application                                                                 Applied within 6 months
                                                                                                                          applied each year
                                                   application

                                                                                                                                                              AI computing clusters,

                                                                                                               →                                   →
                                                   6.1.1 Computing power       No dedicated                               Basic cloud                         supporting training of models
                                                   scale and elasticity        computing power                            computing power                     with hundreds of billions of
                         6.1 AI computing
                                                                                                                                                              parameters
                         infrastructure

                                                                                                               →                                   →
                                                   6.1.2 Green computing                                                                                      PUE < 1.3, waste heat recovery
                                                                               PUE > 2.0                                  PUE: 1.5–1.8
                                                   level                                                                                                      > 50%


                                                                                                               →                                   →
                                                   6.2.1 Network coverage                                                 5G coverage in                      5G-A/6G full coverage, E2E
[Infrastructure]                                                               Mainly wired network
                                                   and latency                                                            some areas                          latency < 10 ms
Future-proof             6.2 Network
infrastructure           infrastructure
                                                                                                               →                                   →
                                                   6.2.2 IoT sensing                                                                                          > 90%, real-time collection of
deployment                                                                     Sensor coverage < 30%                      50%–70%
                                                   density                                                                                                    multi-dimensional data


                                                                                                               →                                   →
                                                   6.3.1 Data quality and                                                 Partial                             Unified data lake, AI labeling
                                                                               Data silos
                                                   governance                                                             consolidation                       quality > 95%

                         6.3 Data infrastructure                                                                                                              Blockchain-based evidence

                                                                                                               →                                   →
                                                   6.3.2 Data security and     No encryption                                                                  storage, compliant with
                                                                                                                          Basic protection
                                                   compliance                  measures                                                                       international standards such as
                                                                                                                                                              GDPR



                                             Appendix: AI campus maturity assessment indicator table



                                                                                                124
References


1.   State Council of China, Opinions on Deepening the Implementation of the "Artificial Intelligence+"
     Initiative [Z]. 2025

2.   Ministry of Industry and Information Technology, Ministry of Education, Ministry of Culture and Tourism,
     National Radio and Television Administration, and General Administration of Sport of China, Action
     Plan for the Integration and Development of Virtual Reality and Industry Applications (2022–2026) [Z].
     MIIT Joint Electronics [2022] No.148, 2022

3.   Tengchong Scientists Forum Editorial Board (led by Yang Yuliang), Tech Predictions and Future Visions
     2049 [R]. 2025

4.   Huawei, Intelligent World 2035 [R]. 2025

5.   Deloitte, Tech Trends 2025 [R]. 2026

6.   Microsoft, 2025 Work Trend Index Annual Report [R]. 2025

7.   Stanford University, AI Index Report 2025 [R]. USA, 2025

8.   SportsTechX, Global Sports Tech Ecosystem Report 2024 [R]. Germany, 2024

9.   ISO/IEC 30172, Digital Twins—Concepts, Terminology and Use Cases [S]. 2022

10. European Union, Artificial Intelligence Act [Z]. 2024

11. European Union, Apply AI Strategy [Z]. 2025

12. United States of America, Secure A.I. Act of 2024 [Z]. 2024

13. United States of America, Winning the Race: AI Action Plan [Z]. 2025

14. Gartner, The Future of Sales in 2025: A Gartner Trend Insight Report [R]. 2020




                                                    125
Acronyms or Abbreviations
   Acronyms or Abbreviations                               Full Name

              5G               5th Generation of Mobile Communication

             5.5G              5.5th Generation Fixed Network

            5G NR              5G New Radio

              6G               6th Generation of Mobile Communication

             AGV               Automated Guided Vehicle

             AND               Autonomous Driving Network

              AI               Artificial Intelligence

             AIGC              AI-Generated Content

              AP               Access Point

              AR               Augmented Reality

             ASIC              Application-Specific Integrated Circuit

             BIM               Building Information Modeling

             BLE               Bluetooth Low Energy

             BOS               Business Operations Support

             CPU               Central Processing Unit

            DetNet             Deterministic Networking

            DMRS               Demodulation Reference Signal

             DoF               Degrees of Freedom

             EDR               Endpoint Detection and Response

             eMBB              Enhanced Mobile Broadband

             F5.5G             5.5th Generation Fixed Network

            FDMA               Frequency Division Multiple Access

             FlexE             Flexible Ethernet

             FPGA              Field Programmable Gate Array

              FPS              Frames Per Second

             HCS               Harmonized Communication and Sensing

              ICT              Information and Communications Technology

             IMT               International Mobile Telecommunications

             MAC               Media Access Control

             Mbps              Megabits Per Second

            mMTC               Massive Machine-Type Communications

              MR               Mixed Reality

             NPU               Neural Processing Unit

             ONT               Optical Network Terminal

              OT               Operational Technology




                                      126
Acronyms or Abbreviations                                Full Name

          PCS               Power Conversion System

          PHY               Physical Layer

          PLC               Programmable Logic Controller

          PON               Passive Optical Network

          PPD               Pixel Per Degree

          PUE               Power Usage Effectiveness

          QoS               Quality of Service

          RAM               Random Access Memory

          RFID              Radio Frequency Identification

          RLC               Radio Link Control

          SDK               Software Development Kit

          SDN               Software-Defined Network

          SLA               Service Level Agreement

          SLM               Spatial Light Modulator

           SPE              Speech Encoder

          SQL               Structured Query Language

          TDD               Time Division Duplex

          TDM               Time Division Multiplexing

         TDMA               Time Division Multiple Access

          TEE               Trusted Execution Environment

          THZ               THZ

          TIM               Thermal Interface Material

          TSN               Time Sensitive Networking

         URLLC              Ultra-Reliable Low-Latency Communication

          UWB               Ultra-Wideband

          VLEO              Very Low-Earth Orbit

          VPN               Virtual Private Network

           VR               Virtual Reality

          W2W               Wafer-to-Wafer

          WDM               Wavelength Division Multiplexing

         WDMA               Wavelength-Division Multiple Access

         Wi-Fi 7            Wireless Fidelity 7

         WLAN               Wireless Local Area Network

           XR               Extended Reality




                                  127
HUAWEI TECHNOLOGIES CO., LTD.
Huawei Industrial Base
Bantian Longgang
Shenzhen 518129, P. R. China
Tel: +86-755-28780808
www.huawei.com




Trademark Notice
            ,        ,     are trademarks or registered trademarks of Huawei Technologies Co., Ltd.
Other Trademarks, product, service and company names mentioned are the property of their respective owners.


General Disclaimer
The information in this document may contain predictive statement including, without limitation, statements regarding the future financial and
operating results, future product portfolios, new technologies, etc. There are a number of factors that could cause actual results and developments
to differ materially from those expressed or implied in the predictive statements. Therefore, such information is provided for reference purpose only
and constitutes neither an offer nor an acceptance. Huawei may change the information at any time without notice.
Copyright © 2026 HUAWEI TECHNOLOGIES CO., LTD. All Rights Reserved.
No part of this document may be reproduced or transmitted in any form or by any means without prior written consent of Huawei Technologies Co., Ltd.
