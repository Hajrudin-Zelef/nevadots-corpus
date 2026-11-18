High-Quality 10Gbps AI Campus Technical and
                    Standard White Paper



                        AI




                    1
                                                                                                 Preface



    Rapid developments in the digital economy, combined with breakthroughs in digital technology
    innovation and significant advancements in artificial intelligence (AI), have enabled the implementation of
    comprehensive AI-powered campus networks that seamlessly connect everything in the digital and physical
    environments while providing users with more personalized and warmer proactive services.


    With the surge of Internet of Things (IoT) devices, popularization of HD video streams, and widespread
    use of immersive collaboration (AR/VR), data on campus networks is growing exponentially. Large model
    applications are penetrating deeply into industry scenarios, with both inference and training rapidly
    shifting toward the edge, closer to data sources and business operations. Campuses and enterprises are
    accelerating carbon reduction and efficiency improvement, upgrading campus infrastructure to build zero-
    carbon campuses, and helping achieve the carbon peak and carbon neutrality goals. All of the above poses
    higher requirements on campus network infrastructure.


    Building a high-quality 10 Gbps AI campus network extends beyond mere bandwidth enhancement; it
    reshapes the infrastructure paradigm for the intelligent era, and serves as a pivotal foundation for fostering
    new quality productive forces. In 2024, the industry published the High-Quality 10 Gbps Campus Network
    Technology Development Research Report, which proposed the concept and key technologies for building
    high-quality 10 Gbps campus networks, and reached broad industry consensus. Based on this, the Network
    Innovation and Development Alliance (NIDA), World WLAN Application Alliance (WAA), IEEE UAE Section,
    Internet Association of Kazakhstan, and other organizations jointly compiled the High-Quality 10 Gbps
    AI Campus Technical and Standard White Paper in 2026, which defines the application architecture,
    technical architecture, and physical architecture of high-quality 10 Gbps AI campus networks. The white
    paper specifies six fundamental capabilities of high-quality 10 Gbps AI campus networks: "autonomous
    network", "broadband access 10 Gbps", "connect everything", "deterministic experience", "energy-saving",
    and "full-scope security". It further illustrates the networking architecture and applications of high-quality
    10 Gbps AI campus networks in industries such as education, government, finance, and healthcare. The
    white paper serves as a reference for organizations engaged in campus digital transformation and network
    infrastructure construction, including related agencies, construction and operations enterprises, and
    research institutions, aiming to offer insights into the digital transformation of enterprises.


    The completion of this white paper has been made possible by the support of many parties, to whom we
    extend our sincere appreciation for their contributions.




2                                                          01
Contents                                                                                                                                                                                         3.2 Government Industry ...................................................................................................................................... 53
                                                                                                                                                                                                     3.2.1 Trends and Requirements ........................................................................................................................... 53
01-Trends and Challenges in Campus Services                                                                                                                                            05            3.2.2 Recommended Networking Architecture ............................................................................................... 54
                                                                                                                                                                                                     3.2.3 Government Service Hall ............................................................................................................................ 55
1.1     Wireless ........................................................................................................................................................................ 06
                                                                                                                                                                                                     3.2.4 Mobile Office ................................................................................................................................................... 56
1.2     Digital and Intelligent ............................................................................................................................................ 07
                                                                                                                                                                                                     3.2.5 Cases ................................................................................................................................................................... 57
1.3     Converged .................................................................................................................................................................... 07
                                                                                                                                                                                                 3.3 Finance Industry .................................................................................................................................................. 58
1.4     Energy-Saving ............................................................................................................................................................. 09
                                                                                                                                                                                                         3.3.1 Trends and Requirements ........................................................................................................................... 58
1.5     Secure ............................................................................................................................................................................ 10
                                                                                                                                                                                                         3.3.2 Recommended Networking Architecture ............................................................................................... 59
02-Concept and Connotations of High-Quality 10 Gbps AI Campus                                                                                                                          13                3.3.3 Smart Office .................................................................................................................................................... 60
                                                                                                                                                                                                         3.3.4 Smart Outlet ................................................................................................................................................... 61
2.1 Architecture Definition for High-Quality 10 Gbps AI Campus .............................................. 14                                                                                         3.3.5 Cases ................................................................................................................................................................... 63
        2.1.1 Application Architecture ............................................................................................................................... 14                        3.4 Healthcare Industry ........................................................................................................................................... 64
        2.1.2 Technical Architecture .................................................................................................................................. 15                               3.4.1 Trends and Requirements ........................................................................................................................... 64
        2.1.3 Physical Architecture ..................................................................................................................................... 17                             3.4.2 Recommended Networking Architecture ............................................................................................... 64
2.2 Autonomous Network ...................................................................................................................................... 18                                         3.4.3 Smart Outpatient Service ............................................................................................................................ 65
        2.2.1 Definition .......................................................................................................................................................... 18                   3.4.4 Smart Ward ...................................................................................................................................................... 66
        2.2.2 Key Technologies ............................................................................................................................................ 18                           3.4.5 Smart Operating Room ................................................................................................................................ 67
        2.2.3 Key Metrics ....................................................................................................................................................... 20                     3.4.6 Cases ................................................................................................................................................................... 68
2.3 Broadband Access 10 Gbps ........................................................................................................................... 21                                      3.5 Manufacturing Industry .................................................................................................................................. 69

        2.3.1 Definition .......................................................................................................................................................... 21                   3.5.1 Trends and Requirements ........................................................................................................................... 69
        2.3.2 Key Technologies ............................................................................................................................................ 24                           3.5.2 Recommended Networking Architecture ............................................................................................... 69
        2.3.3 Key Metrics ....................................................................................................................................................... 26                     3.5.3 Highly Reliable Converged Production Network ............................................................................... 70
                                                                                                                                                                                                         3.5.4 Cases .................................................................................................................................................................. 73
2.4 Connect Everything ........................................................................................................................................... 27
                                                                                                                                                                                                 3.6 Retail Industry ..................................................................................................................................................... 74
        2.4.1 Definition .......................................................................................................................................................... 27
                                                                                                                                                                                                     3.6.1 Trends and Requirements ........................................................................................................................... 74
        2.4.2 Key Technologies ............................................................................................................................................ 28
                                                                                                                                                                                                     3.6.2 Recommended Networking Architecture ............................................................................................... 75
        2.4.3 Key Metrics ....................................................................................................................................................... 31
                                                                                                                                                                                                     3.6.3 Smart Retail Store ........................................................................................................................................ 75
2.5 Deterministic Experience ............................................................................................................................... 32
                                                                                                                                                                                                     3.6.4 Cases .................................................................................................................................................................. 77
        2.5.1 Definition .......................................................................................................................................................... 32           3.7 Hotel Industry               ..................................................................................................................................................... 78
        2.5.2 Key Technologies ............................................................................................................................................ 32
                                                                                                                                                                                                         3.7.1 Trends and Requirements ........................................................................................................................... 78
        2.5.3 Key Metrics ....................................................................................................................................................... 33
                                                                                                                                                                                                         3.7.2 Recommended Networking Architecture ............................................................................................... 78
2.6 Energy-Saving ....................................................................................................................................................... 34                             3.7.3 Smart Hotel ..................................................................................................................................................... 79
        2.6.1 Definition .......................................................................................................................................................... 34                   3.7.4 Cases .................................................................................................................................................................. 81
        2.6.2 Key Technologies ............................................................................................................................................ 35                   3.8 Stadium Scenario ................................................................................................................................................ 82
        2.6.3 Key Metrics ....................................................................................................................................................... 37
                                                                                                                                                                                                         3.8.1 Trends and Requirements ........................................................................................................................... 82
2.7 Full-Scope Security ........................................................................................................................................... 38                                   3.8.2 Recommended Networking Architecture ............................................................................................... 82
        2.7.1 Definition .......................................................................................................................................................... 38                   3.8.3 Smart Stadium ................................................................................................................................................ 84
        2.7.2 Key Technologies ............................................................................................................................................ 39                           3.8.4 Cases .................................................................................................................................................................. 86
        2.7.3 Key Metrics ....................................................................................................................................................... 43
                                                                                                                                                                                                 04-Industry Prospects                                                                                                                                                              89
03-Typical Applications of High-Quality 10 Gbps AI Campus                                                                                                                              45
                                                                                                                                                                                                 4.1 AI Campus Service Prospects ....................................................................................................................... 90
3.1 Education Industry ............................................................................................................................................ 46                           4.2 High-Quality 10 Gbps AI Campus Technology Prospects .......................................................... 91
        3.1.1 Trends and Requirements ........................................................................................................................... 46                                     4.2.1 Wi-Fi 8 and NearLink: New Era of Wireless Campus with Ultra-High Reliability ................... 91
        3.1.2 Recommended Networking Architecture .............................................................................................. 47                                                      4.2.2 100GE Access and Simplified All-Optical Ethernet Architecture Dealing with Traffic Surges 92
        3.1.3 Smart Office ..................................................................................................................................................... 48                      4.2.3 E2E IPv6 Enhanced for Industry Empowerment ................................................................................ 92
        3.1.4 Smart Classroom ............................................................................................................................................ 49                            4.2.4 Campus Network Security: From "Passive Defense" to "Proactive Immunity" ......................... 92
        3.1.5 Smart Dormitory ............................................................................................................................................ 51
        3.1.6 Cases .................................................................................................................................................................. 52
                                                                                                                                                                                                 A Acronyms and Abbreviations                                                                                                                                                       93

                                                                                            02                                                                                                                                                                                             03
                                                           01
    Trends and Challenges
       in Campus Services
As the basic unit of a city, a campus is the main place where people live
and carry out production activities, an important carrier to boost digital
economy development, and a key point to realize green and low-carbon
transformation. Emerging technologies, such as 5.5G, artificial intelligence
(AI), and digital twins, are rapidly advancing, driving campuses to evolve
towards full intelligence and creating new scenarios and applications. In
recent years, extensive exploration and practice have been carried out for
developing intelligent campuses, which has entered a critical phase.

Campuses cover most work and production scenarios. More than 80% of
the gross domestic product (GDP) and more than 90% of innovation are
generated in campuses. Campus networks are an important carrier and
necessary tool for campus services. The development trends of campus
services can be used to infer the evolution trends of campus networks.
Campus service development exhibits the following trends.




                     05
1.1 Wireless                                                                                                     1.2 Digital and Intelligent
There are two main types of wireless terminals                                                                   Independent construction of campus infrastructure         support self-learning, self-diagnosis, self-decision-
connected to campus networks. One type includes                                                                  results in isolated systems and data, failing to          making, and self-execution. Innovative capabilities,
smart terminals such as personal computers and                                                                   meet the sustainable development needs of                 such as unattended operations, automatic
smartphones; the other includes Internet of Things                                                               the campus digital economy industry. Future-              control, and adaptive learning are developed
(IoT) terminals such as smart locks and electronic                                                               proof campuses should deploy high-quality and             by integrating digital technologies into robots
shelf labels (ESLs), and is expected to encompass                                                                moderately advanced new IoT sensing networks              and other intelligent devices, fully boosting the
augmented reality (AR) devices, watches, and                                                                     and network infrastructure, leveraging 10 Gbps            campus construction and operations efficiency.
naked-eye 3D displays.                                                                                           connections to genuinely usher in an era of               Thus, campuses can automatically perceive user
                                                                                                                 comprehensive sensing and intelligent connectivity        behavior and preferences and respond in real time
The number of smart terminals and IoT terminals           The usage of IoT terminals in campuses has             of everything.                                            to meet personalized user requirements.
connected to networks has increased by nearly 2.5         become commonplace. According to IoT Analytics,
times in the past decade (2015 to 2025). According        the number of connected devices worldwide has          By 2030, the total number of connections                  In addition, technologies such as AI, big data,
to statistics and forecasts from Gartner and ABI          exceeded 17 billion. Besides the vast number of        worldwide will exceed 200 billion, and 10 Gbps            digital t wins, knowledge graph, and deep
Research, the number of personal terminals has            smart terminals such as smartphones, tablets,          connections will become the norm. The next-               learning are used to build autonomous network
increased by three to five times in the past three        laptops, and landline phones, the number of IoT        generation Net5.5G technology will help campuses          infrastructure for campuses: Network policies
years, with users now accessing campus networks           terminals has reached 7 billion. This data is widely   achieve precise positioning, high-speed network           and their impacts are simulated in real time, and
via multiple terminals simultaneously instead             acknowledged and reflects the rapid development        coverage, and borderless information transmission.        network parameters are continuously optimized
of just one. Higher demands for concurrent                of IoT technologies worldwide. About 80% of IoT        In addition, campuses will deploy ubiquitous IoT          through intelligent decision-making to ensure
connections necessitate the creation of high-             terminals are connected to networks wirelessly,        sensing devices to collect and monitor the running        optimal services. For each fault, campus networks
performance, high-concurrency Wi-Fi networks in           and their number is still growing annually by 17%.     status data of the infrastructure, and will build         can quickly infer and locate the root cause, analyze
campuses.                                                                                                        campus digital twins based on massive data and            the impacts, and recommend the optimal handling
                                                                                                                 AI foundation models. In this way, campuses               solution. Automatic O&M is implemented through
                                                                                                                 will become highly intelligent with capabilities          human-machine collaboration to guarantee
                                                                                                                 of comprehensive sensing, human-machine                   continuous service experience and O&M efficiency,
                    Table 1-1 Key challenges posed by terminals to campus networks                               collaboration, and always-on connectivity, and            laying a solid foundation for digital and intelligent
  Challenge                                                                                                                                                                development of campus networks.
                                  Data Support                               Network Requirement
  Dimension
                                                                        High concurrent access capability
Terminal density        3–5 terminals per person (in 2025)
                                                                        (for example, Wi-Fi 7/8 required)

Traffic pressure
                    Popularization of new terminals such as AR
                        devices and naked-eye 3D displays
                                                                      10 Gbps-level wireless bandwidth and
                                                                                   low latency                   1.3 Converged
Large number of                                                            Multi-protocol compatibility
                         7 billion IoT terminals worldwide
  IoT terminals                                                              (Wi-Fi/BT/ZigBee/RFID)              Tradit ional campuses adopt a fragmented                  Resource fragmentation:
                     80% IoT terminals depending on wireless                                                     subsystem construction mode. As a result ,                The separate network construction for office,
  Network                                                                Automatic O&M and intelligent
                    connections; annual growth rate of 17% in                                                    their security protection, energy efficiency, and
 management                                                                  terminal identification                                                                       production, security protection, IoT, and other
                              wireless IoT terminals
                                                                                                                 conferencing subsystems run independently of              systems leads to repetitive investments, redundant
                                                                                                                 each other, causing data silos, difficult service         cabling, difficult maintenance, and a more than
                                                                                                                 collaboration, low management efficiency, and             40% increase in capital expenditure (CAPEX) and
                                                                                                                 high system maintenance costs:                            operating expenditure (OPEX).

                                                                                                                                                                           Complex O&M:
                                                                                                                                                                           Multiple networks running concurrently cause
                                                                                                                                                                           policy conflicts and difficult fault locating. The
                                                                                                                                                                           mean time to repair (MTTR) exceeds 4 hours.




                                                     06                                                                                                               07
                                                                                                                                     Communication and sensing collaboration for                AI-powered network:
                                                                                                                                     network optimization:                                      Sensing and network resources are automatically
                                                                                                                                     Sensing data such as the personnel distribution            scheduled based on service priorities. This cuts the
                                                                                                                                     heat map dynamically drives the adjustment of              service rollout period from months to hours and
                                                                                                                                     network slice bandwidth.                                   lowers the OPEX by 30%.




                                                                                                                                     ABI Research predicts that architecture                    of reducing the OPEX by 30%, but also drive the
                                                                                                                                     upgrade and converged digital and intelligent              enterprise WLAN market to reach a scale of USD
                                                                                                                                     transformation can not only achieve the core goal          14.5 billion by 2028.

                                                                                                                                       Table 1-2 Comparison of key metrics between the traditional architecture and the upgraded architecture

                                                                                                                                           Metrics          Traditional Architecture Upgraded Architecture                   Improvement
                                                                                                                                        Service rollout
                                                                                                                                                                  3–6 months                      2–8 hours                      98% ↑
                                                                                                                                            period
                                                                                                                                     Resource utilization          30%–40%                        70%–85%                        100% ↑
                                                                                                                                      Fault rectification
                                                                                                                                          duration                 4–12 hours                      < 1 hour                      85% ↑
                                                                                                                                      O&M manpower           High (professional team
                                                                                                                                             input                                         Low (mainly automation)               60% ↑
                                                                                                                                                                    required)




                                                                                                                                     1.4 Energy-Saving
                                                                                                                                     In recent years, climate change has intensified,           management. Through "bits manage watts",
                                                                                                                                     resulting in more extreme weather conditions               campuses must strike a balance between digital
                                                                                                                                     that threaten global sustainability. As a result,          development and the goal of carbon peak/carbon
                                                                                                                                     managing climate change is now a top strategic             neutrality: They must provide sufficient power for
                                                  Figure 1-1 Resource fragmentation                                                  goal for many countries. Ten years after the               IoT and network devices that implement campus
                                                                                                                                     Paris Agreement, more than 160 countries have              intelligence, while using digital technologies to
                                                                                                                                     established targets for carbon neutrality.                 promote clean energy structure, efficient energy
                                                                                                                                                                                                management, and green production and living
Network architecture upgrade integrates vertical                              adjustment, service requirement prediction, and        Campuses generate over 60% of urban carbon                 activities.
service networks to implement "one network                                    instantaneous response, providing ubiquitous           emissions and play a critical role in achieving the
for multiple purposes and multiple capabilities",                             intelligent connections for innovative services.       goal of carbon peak/carbon neutrality. Under the           Zero- carbon transformation in the campus
s i g n i f i c a n t l y i m p ro v i n g n e t w o r k re s o u rc e        A vast number of devices and sensors break             guidance of this goal, green transformation is             production field is a complex systematic project
efficiency and seamless service interconnection                               information silos and build intelligent conference/    now central to campus strategies, shifting from            covering multiple dimensions. It requires the
capabilities. Driven by digital and intelligent                               building/campus ecosystems, which fundamentally        individual energy-saving measures to systematic            collaboration of the campus ecosystem to improve
transformation, campuses are evolving from                                    rely on the infrastructure capabilities of real-time   zero-carbon operations.                                    information infrastructure and management
static spaces to "organic life forms" with self-                              data collection, analysis, and transfer. During this                                                              services, intelligent and high-quality development
evolution capabilities. They leverage deep AI                                 process, the full lifecycle management of data as a    Zero-carbon/Near-zero-carbon campuses are                  of industries, and intelligent integration of
collaboration for sensing of data and network                                 core asset drives two convergence transformations:     important spatial carriers to implement the                campuses and cities. Networks help campuses
resources to implement automatic resource                                                                                            "carbon peak/carbon neutrality" strategy. In               explore a green and low-carbon construction
                                                                                                                                     compliance with the policies for green, clean, and         path that suits their own characteristics, so as to
                                                                                                                                     cyclic development, campuses are encouraged to             scientifically implement the goal of dynamic zero
                                                                                                                                     explore zero-carbon transformation throughout              carbon.
                                                                                                                                     the lifecycle, such as construction, O&M, and




                                                                         08                                                                                                                09
1.5 Secure                                                                                                                     Service
                                                                                                                              Scenario
                                                                                                                                                Specific Scenario/Risk                Typical Risk          Key Feature/Challenge


The IoT trend and multi-network convergence (5G/         and become equally important as the services it                                       Blurred network                   Lateral movement           Physical isolation
Wi-Fi and cloud-edge synergy) have completely            protects. Therefore, campus networks must build a                                     boundaries:                       attack (attacker           failure: 5G/Wi-Fi hybrid
                                                                                                                                               5G and Wi-Fi 6/7 are              moves freely inside        networking, cloud-edge
broken the traditional "castle-and-moat" security        full-chain, all-scenario security protection system
                                                                                                                                               deeply integrated with            the network)               synergy, and multi-cloud
boundary model that implements physical                  covering terminal access, network transmission,                                       wired networks.                   APT stealthy               interconnection make
isolation, resulting in exponential expansion            and service data, to provide multi-dimensional                                        Data is frequently                propagation (using         the separation between
of the attack surface and ubiquitous security            i n - d e p t h d e fe n s e i n a co n ve rg e d n e t wo r k                        exchanged between cloud           complex network            internal and external
                                                                                                                                               data centers (DCs), edge          topologies to hide         networks disappear.
threats. Network security has evolved from a             environment.
                                                                                                                                               nodes, and terminals              traces)                    Surge in east-west traffic:
support system to a core productivity element,                                                                                                 (east-west traffic).              Cross-domain               Traditional border firewalls
                                                                                                                              Network          The microservice                  ransomware                 struggle to effectively
                                                                                                                            transmission       architecture greatly              infection (from the        monitor the high-volume,
                          Table 1-3 Security service scenarios and typical risks                                                               increases the complexity          IT domain to the OT        complex communication
                                                                                                                                               of internal network               domain or different        traffic between servers,
    Service                                                                                                                                    communication.                    cloud environments)        applications, and
                    Specific Scenario/Risk               Typical Risk               Key Feature/Challenge
   Scenario                                                                                                                                                                      Network sniffing           microservices.
                                                                                                                                                                                 and man-in-the-            Traffic encryption: Deep
                    IoT devices:                    Ransomware                      Massive heterogeneous                                                                        middle (MITM)              inspection is harder due to
                    IoT devices deployed in         infection (causing              devices: Numerous types                                                                      attacks (listening to      the significant volume of
                    public places are prone to      device suspension               and unclear ledgers make                                                                     or tampering with          encrypted traffic (such as
                    unauthorized access and         and data encryption)            management difficult.                                                                        data on transmission       TLS traffic).
                    spoofing.                       DDoS botnet                     Long lifecycle/Difficult                                                                     links)
                    Firmware of cameras and         (devices are                    update: Firmware
                    sensors is not updated in       controlled to launch            update is delayed or
                    a timely manner (high-risk      attacks)                        impossible, leaving                                        Increased data mobility           Data theft                 Ubiquitous data: Data is
                    vulnerabilities exist, CVSS     Key infrastructure              numerous vulnerabilities                                   and exposure:                     through MITM               distributed on terminals,
                    ≥ 9.0).                         suspension                      unaddressed.                                               Sensitive data frequently         attacks (sensitive         edges, and clouds, and
                    Industrial systems such         (industrial control             Physical exposure: Devices                                 flows between terminals,          information                the data flow paths are
                    as PLC and SCADA                devices are                     are often deployed in                                      edge nodes, clouds, and           intercepted during         complex.
                    use default or weak             damaged)                        unattended or open                                         different service systems.        transmission)              Multi-environment
                    passwords.                      Vehicle control                 environments and are                                       Data is stored on hybrid          Phishing for               storage: Data is stored
                    The OTA upgrade                 hijacking                       prone to unauthorized                                      clouds, edge nodes, and           credentials (for           in public clouds, private
                    mechanism of the in-            (endangering                    access and spoofing.                                       even terminals.                   unauthorized access        clouds, local DCs, edge
                    vehicle system has              personal safety)                Lack of security baselines:                                Microservice-based                to data)                   devices, and other
    Terminal        vulnerabilities.                                                The default configurations                                 applications complicate           Internal data leakage      environments.
     access                                                                         are insecure, and no                                       data access permission            (caused by malicious       Complex permission
                                                                                    unified security standards              Service data       management.                       internal personnel         management: Fine-
                                                                                    are available.                                                                               or incorrect               grained permission control
                                                                                                                                                                                 configurations)            is difficult to implement
                                                                                                                                                                                 Data encryption            across networks,
                    Smart terminals:                Ransomware spread               BYOD management                                                                              by ransomware              environments, and
                    Remote office terminals         (entering the                   difficulty: Devices are                                                                      (causing service           applications.
                    (laptops and mobile             intranet through                owned by individuals,                                                                        interruption and           Data lifecycle
                    phones) access an               infected terminals)             leading to weak security                                                                     data asset loss)           management: Data
                    enterprise network              Sensitive data                  controls.                                                                                    Not meeting                creation, storage, use,
                    through insecure public         leakage (device                 Complex access                                                                               data compliance            transmission, and
                    Wi-Fi.                          missing or theft)               environment: Enterprise                                                                      requirements (such         destruction need to be
                    BYOD devices access             Pivot for lateral               core resources are                                                                           as GDPR and CCPA)          protected.
                    sensitive services without      movement in                     accessed through
                    installing compliant            internal networks               untrusted networks.
                    security software or being
                    managed by MDM.

                                                                                                                          According to Gartner, by 2026, 60% of enterprises           cornerstone of service continuity. Embedding
                                                                                                                          will be forced to deploy the zero trust architecture        security capabilities into networks is the only way
                                                                                                                          due to service interruptions caused by security             to build a future-proof campus featuring both
                                                                                                                          gaps. The security of converged networks is the             intelligence and reliability in the digital wave.



                                                    10                                                                                                                           11
                     02
              Concept and
     Connotations of High-
        Quality 10 Gbps AI
                   Campus




12       13
2.1 Architecture Definition for High-Quality 10 Gbps                                                                          Campus for AI: network experience upgraded
                                                                                                                              for AI applications
                                                                                                                                                                                            AI for Access: terminal management capability
                                                                                                                                                                                            upgrade using AI technologies
    AI Campus                                                                                                                 As AI agents become widespread, how to use them               Terminals of various types are the main service
                                                                                                                              securely and efficiently becomes a key challenge              recipients but are also a key source of threats.
                                                                                                                              for networks. Based on intelligent AI application             Therefore, achieving visibility, manageability,
Based on the analysis of campus service                                aspects of application architecture, technical         identification and assurance technologies, AI                 and security control at the network access layer
characteristics, key network capabilities, and                         architecture, and physical architecture, aiming to     application sessions are guaranteed millisecond-              is also a key challenge for campus networks. AI
networking characteristics, the high-quality 10                        provide a better campus network solution.              level deterministic latency and 1E-9 packet loss              technologies can be used to identify terminal types
Gbps AI campus architecture is defined from                                                                                   rate. This meets the high-quality and high-                   and control terminal behaviors, and implement
                                                                                                                              standard net work requirements of new AI                      functions such as automatic configuration and
                                                                                                                              terminals, agents, and services (such as embodied             protection policy. Moreover, spatial sensing
                                                                                                                                                                                            extends security from connections to the physical
  2.1.1             Application Architecture                                                                                  AI and AI operating systems). With AI-based traffic
                                                                                                                              collection, identification, and analysis capabilities,        environment.
                                                                                                                              networks can provide differentiated experience
                                                                                                                              for AI applications of different users and services,
                                                                                                                              and provide real-time service capabilities for
                                                                                                                              AI. In addition, campus networks can provide
                                                                                                                              comprehensive security assurance for AI terminals
                                                                                                                              and agents through physical encryption of wireless
                                                                                                                              and wired links.



                                                                                                                               2.1.2           Technical Architecture

                                                                                                                              There are six primary technical focuses for                   scenarios and capabilities.
                                                                                                                              campus networks, determined by service




             Figure 2-1 Logical architecture and key capabilities of a future-oriented AI campus


A I fo r N e t w o r k : n e t w o r k m a n a g e m e n t             collaboration with the building management
upgraded with the AI brain                                             system to control air conditioners and lights.
New technologies such as large-scale models                            On the other hand, large-scale models help
and natural language translation are introduced                        implement automatic check and maintenance,
to help networks evolve towards L4 autonomous                          provide real-time visibility at the device, network,
driving. On one hand, campus networks can                              and application levels, support real-time user and
p ro v i d e m o re i n fo r m a t i o n fo r A I , s u c h a s        application journey playback, and enable minute-
information about terminals, services, traffic,                        level fault demarcation and locating. The network
locations, and environments, to help AI improve                        intelligent agent automatically analyzes the root
its overall capabilities. For instance, sensing                        cause of each fault based on the fault analysis
informat ion can be ut ilized to implement                             model, achieving detection and auto-solving of
intelligent energy saving in buildings, including                      90% wireless faults.

                                                                                                                                                               Figure 2-2 Key technologies of AI-driven campuses


                                                                  14                                                                                                                   15
Autonomous network: According to the YD/T                                    to the centralized solution, Wi-Fi IoT supports co-    can easily propagate through internal campus               at the link layer of the wireless air interface
6764-2025 industry standard for the autonomous                               site deployment with a radius of 10 m to further       devices. Therefore, campuses must prioritize               can fundamentally solve wireless security risks.
network architecture, networks are categorized                               simplify IoT convergence in industry scenarios         enhancing their network security system. An                CSI sensing and other Wi-Fi spatial sensing and
into six levels from L0 to L5 based on their degree                          such as asset management, healthcare, retail, and      example is zero trust network access (ZTNA), or            detection technologies implement detection of
of automation. For each level, the standard defines                          industrial production, helping next-generation         zero trust architecture (ZTA), which leverages             personnel intrusions and unauthorized devices
clear, unambiguous generational characteristics                              production to meet flexible manufacturing and          AI to implement zero trust on user access and              (such as rogue cameras) within seconds. Terminal
of technological maturity and service application                            security requirements.                                 detects more than 95% of abnormal behaviors in             and NE side securit y is ensured through a
effectiveness indicators. The key technologies of                                                                                   real time. Wired and wireless link-layer encryption        systematic device security protection architecture
L4 are generative AI and AI agents. Dialog-based                             Deterministic experience: With the popularization      technologies ensure zero data leakage from end             and situational awareness capability.
O&M is supported through natural language                                    of AI terminals, embodied AI, smart manufacturing,     to end. Especially, anti-eavesdropping technology
interfaces. Intelligent task planning and execution                          and other smart applications, campus networks
enable the system to proactively optimize user                               are required to ensure deterministic network
experience and detect exceptions. Except for a few                           experience. This can be achieved through a series       2.1.3          Physical Architecture
scenarios that must be manually handled, most                                of functions: Intelligent application identification
O&M scenarios can be handled by the system                                   helps identify key applications such as audio,         As shown in the figure, a campus network consists          network access devices. The aggregation/core
automatically or with minor human assistance.                                video, cloud desktop, and collaborative office         of the terminal layer, access/aggregation/core             layer consists of fixed or modular switches. The
That is, L4 supports dialog-based proactive O&M                              based on traffic characteristics. Intelligent all-     layer, WAN layer, and management-control-                  WAN layer consists of routers and firewalls. The
on the basis of L3's visualized O&M, further                                 flow scheduling implements network-wide E2E            analysis layer. The terminal layer consists of             management-control-analysis layer consists of the
reducing manual operations during O&M. The                                   slicing and device-network synergy to achieve a        various smart terminals and IoT terminals.                 software-defined networking (SDN) controller and
benefits include 100% success of remote site-                                freeze rate of less than 1% for AI and XR services.    The access layer consists of wired and wireless            analyzer, and is the brain of network management.
visit-free deployment, 100% auto-solving of                                  Intelligent all-flow poor-QoE analysis provides
faults that do not require site visits, automatic                            insights into the experience by analyzing millions
detection and solving of 80% wireless issues, and                            of flows, enabling application-level journey
AI-based user experience scoring. This achieves                              playback and network-wide visualization. Exclusive
one-stop management of networks, security, and                               service assurance enables VIP users and terminals
users, one-map visibility of terminals, and quality                          to access the network anytime and anywhere with
visualization and locating.                                                  a deterministic latency of less than 50 ms and
                                                                             guaranteed bandwidth. Other capabilities include
Broadband access 10 Gbps: Wi-Fi 7 technology                                 proactive care for VIP users, real-time experience
drives the upgrade of access bandwidth to 2.5GE–                             evaluation, and timely fault warning.
10GE, enables 10-meter high-performance Wi-Fi
coverage, and brings 60%–80% capacity increase                               Energy-saving: Energy saving is now a common
with converged scheduling, spatial reuse, and                                mission in the industry. For campuses, this means
dynamic-zoom smart antennas. High-reliability                                reducing both device power usage and overall
continuous networking ensures zero packet loss.                              campus energy consumption. AI-based tidal traffic
The bearer network is simplified from three                                  prediction, new energy-saving protocol standards,
layers to one layer, achieving "One Device, One                              and intelligent chip sleep modes can reduce 20%
Network".                                                                    energy consumption of network devices. Wireless
                                                                             sensing technologies enable campus networks
C o n n e c t e ve r y t h i n g : U b i q u i t o u s w i re l e s s        to sense the personnel presence status so as to
connections ("WLAN + X") serve as the key                                    control equipment such as air conditioners and
technology basis for varied campus access                                    lights, implementing "bits control watts" and
requirements. Wi-Fi supports long-distance (>                                reducing the campus overall energy consumption
15 m) integrated sensing and communication.                                  by 20%.
Objects are precisely sensed through fine timing
measurement (FTM), channel state information                                 Full-scope security: In campus scenarios where
(CSI) sensing, and mmWave sensing, integrating                               personnel activities are dense, inadequate
sensing into security protection and energy saving                           security infrastructure is a major vulnerability
solutions. From the distributed passive solution                             of the entire system. For example, ransomware
                                                                                                                                                  Figure 2-3 Physical architecture of a high-quality 10 Gbps AI campus network


                                                                        16                                                                                                                17
Terminal layer: includes various access terminals          Core layer: usually has core switches deployed         Multi-dimensional digital twin sensing                                      interconnection and seamless operations and
for campus office and production, such as smart            to connect to firewalls, routers, and aggregation      Multi-dimensional O&M views from networks to                                support unified networking cognition for agents
terminals and IoT terminals.                               switches. The core layer features high                 applications are provided, including the network                            and easy manual intervention. From the technical
                                                           performance, high density, and high scalability,       view, application view, and user view. The network                          perspective, fully simplified networking, capability
                                                           and bears services of the entire campus network.       view focuses on multi-dimensional sensing and                               negotiation, and governance capabilities should
                                                                                                                  visualization of network resources, network                                 be provided, and a unified pan-ICT infrastructure
Access layer: includes network devices that                WAN layer: provides egress routing capabilities        performance, configuration forwarding, and                                  model should be defined.
connect terminals to the network and provides              for the campus network. In the multi-branch            network exceptions. The application view focuses
2.5GE to 10GE access capabilities. The access layer        campus scenario, it provides high-performance          on multi-dimensional sensing and visualization
provides access modes including Wi-Fi 7 wireless           interconnection capabilities for branch campuses.      of the performance, load, and exceptions of
                                                                                                                                                                                              2.2.2.2 AI Agent
access and 2.5GE/10GE wired access.                        The WAN layer also provides network security           application flows.
                                                           protection capabilities to ensure the security of                                                                                  The typical AI agent architecture consists of
                                                           campus network devices and services.                   Predictive digital twin technology                                          four core modules: LLM engine, planning and
                                                                                                                  The surge of AI terminals drives the requirements                           execution system, tool service matrix, and
Aggregation layer: includes network devices                Management-control-analysis layer: functions as        for failure prediction of IoT modules on wireless                           knowledge-memory augmentation unit. Take
located between the access layer and core layer.           the brain in the campus SDN solution. This layer       networks and optical modules on fixed networks.                             the fault AI agent as an example. The input is
Being highly active and providing rich features,           manages and controls campus network devices,           Optical module failures frequently interrupt                                the fault symptom description. The planning and
the aggregation layer supports 10GE access and             and analyzes and ensures campus services, serving      training, which requires networks to predict such                           execution system identifies the intent of the input,
provides two mainstream solutions: Ethernet                as the core of an intelligent campus.                  risks in advance. In terms of hard disks, HDD/                              divides the task into individual steps, plans, and
electrical solution and Ethernet optical solution.                                                                SDD faults will cause data loss. This also requires                         executes each step by invoking the corresponding
                                                                                                                  predictive risk management and disk replacement                             tool. Reflection and optimization are required
                                                                                                                  window planning. The multi-source component                                 when an error or interruption occurs. The key
                                                                                                                  failure data is integrated based on the Transformer                         technologies are as follows:
                                                                                                                  architecture to build a professional prediction
2.2 Autonomous Network                                                                                            algorithm model, which can accurately predict the                           P l a n n i n g a n d re f l e c t i o n : P l a n n i n g i s t h e
                                                                                                                  failure risks of optical modules and hard disks,                            core mechanism of dynamic decision-making
                                                                                                                  ensuring service continuity and optimizing spare                            throughout the AI agent process. Currently, the
                                                                                                                  parts management.                                                           mainstream solutions in the industry include
 2.2.1          Definition                                                                                                                                                                    SOP planning and free planning. For some
                                                                                                                  Digital twin simulation technology                                          known problems, the SOP mode can be used to
                                                                                                                  Digital twin simulation displays key indicators                             preset the handling process. For some unknown
According to the autonomous network                        task execution. In the AI agent era, guided by the
                                                                                                                  that are difficult to obtain and provides what-                             scenarios, the free planning mode will be used in
architecture standard, an autonomous network               objectives of high availability, optimal experience,
                                                                                                                  if analysis. For example, Wi-Fi 3D coverage                                 the future. The core of the reflection mechanism
is a net work consist ing of intelligent and               and simplified operations, enterprises are building
                                                                                                                  simulation can be used to plan the coverage                                 is to iteratively optimize policies through self-
automated network infrastructure, operations and           the new ICT infrastructure from three aspects:
                                                                                                                  of directional antennas in high-density and                                 evaluation and environment feedback . For
management systems, and service systems. With              intelligent-native capability and resilience of the
                                                                                                                  warehouse campus scenarios, discover coverage                               example, the Reflexion framework uses dynamic
the Intelligent World 2035 vision, enterprises are         underlying ICT infrastructure, AI agent autonomy
                                                                                                                  holes in the routine O&M process, and verify and                            memory to store key decision nodes. When an
accelerating the intelligent upgrade of multi-agent        capability of O&M systems, and self-adaptability
                                                                                                                  deliver radio calibration configurations; simulation                        execution exception is triggered, the reflection
collaborative decision-making and autonomous               and self-evolution of swarm intelligence.
                                                                                                                  also applies to Wi-Fi FTM location and CSI sensing                          process is started to generate a correction policy.
                                                                                                                  of personnel presence and quantity, helping
                                                                                                                  engineers determine the deployment locations                                Knowledge and memory: Enhanced prompts
 2.2.2          Key Technologies                                                                                  of sensing APs to ensure the optimal sensing                                are generated by integrating external knowledge
                                                                                                                  performance across the network.                                             bases and context information, building the dual-
2.2.2.1 Digital Twin Network                                                                                                                                                                  memory system. Short-term memory stores
                                                                                                                  Digital twin interconnection protocol                                       interaction information (such as historical user
Enterprise O&M digital twin technologies are               agents are used together with high-precision           O&M systems of multiple vendors and domains                                 instructions) in real time based on the model
classified into four types: multi-dimensional              models to implement closed-loop decision-making        coexist in enterprise campus O&M scenarios,                                 context window to keep conversations coherent
sensing, predictive analytics, simulation and              and verification, for example, to perform the          necessitating a set of digital twin interconnection                         and track task progress. Long-term memory uses
deduction, and interconnection protocols. AI               prediction-simulation-optimization process.            p ro t o c o l s t o i m p l e m e n t l i g h t w e i g h t d a t a        vector databases to retain user profiles, behavior




                                                      18                                                                                                                                 19
patterns, and domain knowledge, implementing                 Conflict resolution: The main scenarios include                                                                     Table 2-1 Key metrics
cross-session knowledge reuse (for example, the              execution objective deviation, cross-domain
                                                                                                                                                                                                                          Good: Conditional
O&M Q&A copilot provides personalized services               diagnosis conflict, and collaboration obstacles                              Category            Metric         Excellent: Highly Autonomous
                                                                                                                                                                                                                            Autonomous
by invoking historical interaction records).                 caused by information inconsistency. Solution
                                                                                                                                                                              CAD drawing and 3D simulation,         Image (PDF, JPG, etc.) and 2D
                                                             technologies include case-based reasoning (CBR)                                             Intelligent WLAN    supporting AI-based identification       simulation; manual obstacle
Foundation model: In terms of the architecture,              and multi-agent debate. In CBR, historical cases                                                 planning         and drawing of obstacles and             drawing and manual AP
both fast thinking and slow thinking models are              are searched and adapted to new scenarios. For                                                                     automatic AP deployment                       deployment
necessary. When to use the slow thinking model               example, the old solution is adjusted to resolve                                                                 Simplified and automatic LAN          Separate automatic deployment
and when to use the fast thinking model can be               similar faults. In multi-agent debate, after each                                              LAN-WAN            & WAN deployment based on           of LANs and WANs; configuration
determined through intelligent routing, which can                                                                                       Planning and       convergence       branch scenarios; one-click batch       and deployment on a per-site
                                                             agent proposes a proposition from the domain
                                                                                                                                        construction                         deployment of multiple branches                     basis
be implemented by training dedicated embedding               perspective, the judge agent comprehensively
models considering the query difficulty and task             evaluates the propositions and makes a decision.                                              Automation        Single-site provisioning: minute-       Single-site provisioning: hour-
                                                                                                                                                            efficiency                      level                            level/day-level
type.                                                        For example, a consensus is reached on the root
                                                             cause of a network fault after a debate between                                                                 Zero Touch Provisioning (ZTP) and
                                                                                                                                                         Device plug-and-
                                                             the protocol-layer and device-layer agents.                                                                      Secure Zero Touch Provisioning                      ZTP
                                                                                                                                                               play
                                                                                                                                                                                          (SZTP)
2.2.2.3 Intelligent Collaboration
                                                             Multi-agent communication protocol: The
                                                                                                                                                           Digital twin:
Challenges of intelligent collaboration include              core challenges of multi-agent interaction                                                  multi-layer map      Accurate sensing of the network
cross-agent objective decomposition and task                 include semantic gap (language/understanding                                                visualization for    status and actual experience of       Multi-layer KPI visualization of
orchestration, conflict resolution, and multi-               deviation), LLM processing exceptions caused                                                networks, users,    users, terminals, and applications,   networks, users, and applications
                                                                                                                                                          terminals, and         with multi-layer visibility
agent interaction. The technical implementation is           by long context, and token processing capability                                              applications
described as follows:                                        differences. A task-oriented protocol system needs
                                                             to be developed, which defines E2E interaction                                              Auto-solving of      Automatic detection of wireless
                                                                                                                                                                                                                    Detection of faults but without
                                                                                                                                        Maintenance      wireless network     faults and auto-solving of 80%
                                                             process standards covering registration, coalition                                                                                                              auto-solving
                                                                                                                                            and                faults                  wireless faults
Objective decomposition and task orchestration:              formation, negotiation, execution, and feedback,
                                                                                                                                        optimization
Decompose a cross- domain task into sub-                     a n d s t a n d a rd i z e s t ra n s m i s s i o n p ro t o c o l s ,                         Automatic        Auto-solving of 85% user access
                                                                                                                                                                                                                   Demarcation of faults but without
objectives that can be understood within a single                                                                                                           handling of       and experience faults; minute-
                                                             interfaces, and information models. The system                                                                                                         auto-solving; hour-level or day-
                                                                                                                                                          user access and      level fault demarcation and
domain based on the LLM and process knowledge.               implements efficient communication across agents                                                                                                             level fault locating
                                                                                                                                                         experience faults                locating
The main technical challenges to be addressed                (for example, resolving semantic conflicts between
include: differentiated understanding capabilities,                                                                                                         Intelligent      Foundation model-based natural
                                                             device agents and policy agents at the protocol                                                                                                       Basic intelligent O&M and limited
                                                                                                                                                          O&M based on          language interaction, status
which may cause inconsistent understanding of                level), ensuring high reliability and security of                                                                                                      intelligent interaction, unable to
                                                                                                                                                          AI foundation       inspection, and intelligent fault
                                                                                                                                                                                                                       automatically resolve faults
objectives; dependency between sub-objectives                autonomous collaboration in the multi-agent                                                      models               auto-solving by agents
of multiple agents; strong autonomy of agents                system.
with their respective high-priority objectives/tasks,
which makes it difficult to forcibly deliver/execute
objectives.
                                                                                                                                      2.3 Broadband Access 10 Gbps

                                                                                                                                       2.3.1           Definition
 2.2.3           Key Metrics
                                                                                                                                      "Broadband access 10 Gbps" aims to achieve 10              networks, campus networks provide high-speed
An autonomous network aims to reduce the                     Networks will gradually evolve from conditional                          Gbps user access and represents the evolution              connections for users and meet the stringent
network complexity and skill demands, changing               autonomous to highly autonomous and full                                 direction of next-generation campus access. With           requirements of new applications on network
O&M and delivery by professional personnel to                autonomous. The following table provides                                 the high-bandwidth and high-concurrency access             bandwidth, latency, and concurrency performance.
automated processes with little or no human                  suggestions on key metrics.                                              capabilities of wireless networks (Wi-Fi 7) and the        Campus network connections are the bridges
involvement. This cannot be achieved overnight.                                                                                       high-performance bearing capabilities of wired             between the physical and digital worlds,




                                                        20                                                                                                                                  21
helping build a converged communications
                                                           2.3.1.1 10 Gbps Wireless                                           2.3.1.2 10 Gbps Wired
network infrastructure featuring wide coverage,
high bandwidth, and high scalability. The key              10 Gbps wireless refers to the high bandwidth,                     10 Gbps wired means that access-side switches                Ethernet solution and all-optical Ethernet solution.
technologies are as follows:                               h i g h c o n c u r re n c y, a n d s e a m l e s s ro a m i n g   support 10GE connection with wired terminals,                The all-optical Ethernet solution is further
                                                           capabilities provided by the Wi-Fi 7 protocol                      or the uplink bandwidth of APs is upgraded to                classified into active Ethernet network and passive
                                                           and collaboration between APs, which meet the                      10GE. It can be implemented through the classic              Ethernet network (PEN) solutions.
. Wireless side: Wi-Fi 7, continuous networking,           requirements of emerging applications on campus
 smart roaming, etc.                                       networks for network bandwidth and latency.
. Wired side: optical and electrical Ethernet                                                                                                                       Table 2-2 10 Gbps wired capabilities

 technologies. All-optical Ethernet includes active        The Wi-Fi 7 standard introduces technologies                                                                Classic All-Optical          All-Optical             All-Optical
                                                                                                                                                 Classic Ethernet
 Ethernet optical and passive Ethernet optical             such as the 6 GHz frequency band, 320 MHz                                                                        Ethernet              Ethernet: Active         Ethernet: PEN
 technologies.                                             frequency bandwidth, 4096-QAM modulation,
                                                                                                                                                  2.5 Gbps to 10      2.5 Gbps to 10 Gbps,
                                                           Multiple Resource Unit (MRU), and Multi-Link                          Bandwidth                                                      2.5 Gbps to 10 Gbps     2.5 Gbps to 10 Gbps
                                                                                                                                                       Gbps           25 Gbps to 400 Gbps
                                                           Operation (MLO) to increase the throughput
                                                           of Wi-Fi networks to 23 Gbps and provide low-                             PoE                 Y                      N                         Y                       N
                                                           latency access assurance for users, helping high-
                                                           quality 10 Gbps AI campus networks improve user
                                                                                                                                   Passive
                                                           experience.                                                                                   N                      N                         N                       Y
                                                                                                                                 architecture




                                                                                                                              1. The key to selecting a wired solution is the cable        solution increases the cable deployment cost by
                                                                                                                                 type. If electrical cables are used, as shown in          about 30%. A more economical solution is to
                                                                                                                                 Figure 2-5, cables of Cat6A or higher must be             reuse the existing Cat5E/Cat6 cables: Through
                                                                                                                                 deployed if 5 Gbps or 10 Gbps bandwidth is                access switch reconstruction, the transmission rate
                                                                                                                                 required, so as to meet the high-performance              can be increased to 2.5 Gbps to meet the basic
                                                                                                                                 access requirements of Wi-Fi 7. However, this             performance requirements of Wi-Fi 7.




                             Figure 2-4 Comparison between Wi-Fi standards                                                                                Figure 2-5 How cables evolve with Wi-Fi generations


                                                      22                                                                                                                              23
2. In all-optical scenarios, hybrid copper-fiber         power supply and high-speed access capabilities.
   cables (hybrid cables for short, as shown in          This integration significantly simplifies network
   Figure 2-6) can provide both long-distance            deployment.




                                                                                                                                        Figure 2-7 Smart antennas and multi-AP coordination



                                                                                                              2.3.2.2 Smart Roaming
                        Figure 2-6 Hybrid cable and optical module composition
                                                                                                              Smart roaming uses the network AI capability to
                               Table 2-3 Key capabilities of hybrid cables                                    profile the roaming behaviors of STAs and steer
                                                                                                              STA roaming in a differentiated manner. This
    Cable                            PoE Power Supply         PoE+ Power Supply      PoE++ Power Supply       solves problems in traditional roaming such as
                   Cable Diameter
 Specifications                      Distance (15.4 W)         Distance (30 W)         Distance (60 W)
                                                                                                              sticky STAs, long channel scanning time, and poor
  Hybrid cable-                                                                                               roaming compatibility, improving the roaming
                      6.2 mm               1280 m                    500 m                  250 m
    17AWG                                                                                                     experience of STAs. Key technologies of smart
                                                                                                              roaming include STA identification, STA profiling,
  Hybrid cable-
                      5.7 mm                500 m                    200 m                  100 m             STA quality data learning, and roaming steering.
    21AWG
                                                                                                              Smart roaming replaces STA-triggered roaming
  Hybrid cable-                                                                                               with network-steered roaming, optimizes roaming
                      4.4 mm                200 m                    100 m                  50 m
    24AWG                                                                                                     handover opportunities, and shortens the roaming
                                                                                                              time. It supports the personalized roaming
* The American Wire Gauge (AWG) is a                     value indicates a thicker wire diameter and higher   parameter settings for each type of STA , and
  standard unit for measuring the diameter of            current carrying capacity.                           minimizes the possible adverse impacts caused by
  an electrically conducting wire. A smaller AWG                                                              STAs' protocol compatibility and implementation
                                                                                                              differences.

                                                                                                                                                                             Figure 2-8 Smart roaming handover
 2.3.2            Key Technologies
                                                                                                              2.3.2.3 10 Gigabit Ethernet
2.3.2.1 Continuous Networking                                                                                 A cost-effective network acceleration solution is
Continuous networking allows a WLAN with                 Continuous networking implements AP grouping         to reuse the existing Cat5E/Cat6 cables to increase
multiple APs to provide seamless signal coverage         and priority division by synchronizing data          the rate to 2.5 Gbps (as shown in Figure 2-9),
and ensure optimal service experience for users.         between AP radio chips at the microsecond (μs)       which meets the basic requirements of Wi-Fi 7.
Key technologies of continuous networking include        level. After an AP preempts a channel, it becomes
smart antennas and multi-AP coordination.                the primary AP through negotiation, and instructs
                                                         secondary APs to adjust the coverage angles
                                                         of their smart antennas and send data packets
                                                         synchronously. In this way, multiple APs can work
                                                         on the same channel, significantly improving
                                                         spectrum efficiency and system capacity.                                                                        Figure 2-9 2.5GE Wi-Fi 7 traditional network


                                                    24                                                                                                              25
When the bandwidth requirement exceeds 2.5               · Passive Ethernet Network (PEN) Solution:                                                     Table 2-4 Key metrics
Gbps, another option is optical cables, which can        uses passive all-optical technology to build a
support future bandwidth requirements of over            simplified 10 Gbps all-optical network with high       Category                       Metric                           Excellent               Good
10 Gbps. The following solutions are available           performance. The solution extends high-speed
based on the evolution of all-optical Ethernet           10GE optical fibers to rooms to meet the future                       (Wireless) Single-user maximum rate            ≥ 3.5 Gbps              ≤ 3.5 Gbps
technologies:                                            service evolution needs, and saves the deployment
                                                         of active ELV rooms, providing security and                              (Wireless) Total throughput of
· Active Ethernet Network Solution: uses hybrid          reliabilit y. Figure 2-11 shows the solution                            three-AP 80 MHz intra-frequency               960 Mbps               700 Mbps
cables to provide long-distance PoE power supply         networking.                                                                 networking with 18 STAs
                                                                                                                Bandwidth
and high-speed access capabilities, as shown in                                                                                (Wired) High bandwidth at the access
                                                                                                                                                                            Exclusive 10GE           Exclusive GE
Figure 2-10.                                                                                                                        side (access device uplink)

                                                                                                                                 (Wired) High bandwidth based on
                                                                                                                                                                           Single module: >
                                                                                                                                 the passive, simplified architecture                           Single module: 10 Gbps
                                                                                                                                                                               100 Gbps
                                                                                                                                (aggregation/core device downlink)

                                                                                                                                 Number of 4K HD video channels                    60                     40

                                                                                                               Concurrency

                                                                                                                                     Passive high-density access                 96/PCS                80/PCS

                                                                                                                                                                           Continuous large-    Single-AP working, no
                                                                                                                                 (Wireless) Distributed architecture       scale networking     continuous networking
                                                                                                                                                                           and SSID sharing        or zero roaming
                                                                                                               Architecture
                                                                                                                                 (Wired) Integrated networking of
                                                                                                                                                                               Unified
                                                                                                                                aggregation and access devices and                                  Not supported
                                                                                                                                                                             management
                                                                                                                                       unified management

                                                                                                              Long-distance
                                                                                                                                  Long-distance PoE++ capability                ≥ 250 m                ≥ 200 m
                                                                                                                 access




  Figure 2-10 Active Ethernet Network Solution                       Figure 2-11 PEN Solution
                                                                                                             2.4 Connect Everything


                                                                                                              2.4.1           Definition
 2.3.3         Key Metrics
                                                                                                             Emerging industry applications in campuses                 band and large-scale antenna array) enables both
                                                                                                             require not only high-quality wireless connections,        high-quality communication (such as Wi-Fi IoT)
According to standards proposed by the World             ultra-broadband campus network should meet the      but also the support for a wide range of access            and environment sensing (such as positioning and
WLAN Application Alliance (WAA), a 10 Gbps               following criteria.                                 scenarios, including high-precision wireless               imaging) to achieve seamless sensing and service
                                                                                                             positioning, wireless integrated sensing and               coverage in physical space, extending beyond
                                                                                                             communication, and IoT convergence. The unified            mere communication.
                                                                                                             wireless infrastructure (such as high frequency




                                                    26                                                                                                             27
                                                                                                                                       and multi-function integration. Campus terminals             which monitors channel utilization in real time
  2.4.2                Key Technologies
                                                                                                                                       are both the sources of data and the execution               and dynamically allocates frequency band
                                                                                                                                       points of intelligent instructions. Their coverage           resources. In this way, Wi-Fi 7 is responsible
2.4.2.1 Overview of Wireless Integrated Sensing and Communication                                                                      range and level of intelligence directly affect the          for large-bandwidth data transmission, and
                                                                                                                                       overall sensing accuracy and response speed.                 NearLink focuses on transmitting control
The Inst itute of Electrical and Electronics                                   collaboration between the WLAN and campus                                                                            instructions with low latency (such as robot arm
Engineers (IEEE) clearly defines the industry                                  digital platforms enables real-time monitoring and      NearLink: a revolutionary Chinese native standard            operations), achieving time division multiplexing
evolution direction of WLAN-based integrated                                   intelligent analysis of campus environments and         for a wide range of application fields such as               (TDM).
sensing and communication in IEEE 802.11bf.                                    devices and ensures quick response. This provides       vehicle, industrial control, and whole-house
That is, wireless signals received by STAs with                                strong technical support for smart campus               intelligence. The alliance has more than 1200
                                                                                                                                                                                                  . NearLink and Wi-Fi can be used together based
WLAN sensing capabilities are used to determine                                construction and development.                           members, and the chip shipment volume reached                on dynamic spectrum sensing, achieving a
target characteristics in specific environments; the                                                                                   85 million in 2024 and is expected to exceed 100             latency of 10 ms in industrial scenarios, 60%
                                                                                                                                       million in 2025, covering over 20 vehicle models             lower than Bluetooth's 25–100 ms.
                                                                                                                                       such as AITO M9 and Stelato S9. In 2025, the
                                                                                                                                       number of product categories will reach 500.
                                                                                                                                                                                                  2.4.2.3 Integrated IoT
                                                                                                                                       Bluetooth: The new channel detection technology
                                                                                                                                       improves the indoor positioning precision to sub-          With high concurrency and low latency, Wi-Fi
                                                                                                                                       meter level. The "NearLink + Bluetooth" dynamic            has become an ideal unified IoT access platform,
                                                                                                                                       switching solution can be used to reduce cross-            effect ively collaborat ing with mainstream
                                                                                                                                       device interference. However, Bluetooth lags               protocols such as Bluetooth and Radio Frequency
                                                   Figure 2-12 IEEE 802.11bf timeline
                                                                                                                                       behind NearLink/UWB in terms of the positioning            Identification (RFID). By implementing unified
                                                                                                                                       precision (1–3 m) and delay.                               access and management on the network side,
W i - F i - i n t e g ra t e d i n t e l l i g e n c e : To b u i l d a        In the process of digital and intelligent
                                                                                                                                                                                                  this solution significantly lowers the architecture
new paradigm of "integrated sensing and                                        transformation in smart campuses, campus
                                                                                                                                       UWB: ultra-wideband, supporting centimeter-                complexity and O&M costs brought by traditional
communication" for campus networks, WLAN                                       infrastructure is evolving from "connection
                                                                                                                                       level high-precision positioning. With nanosecond-         m u l t i - p ro t o c o l I o T d e p l o y m e n t , p ro v i d i n g
works as the ubiquitous sensing carrier with the                               pipes" to "intelligent platforms". With Wi-Fi as
                                                                                                                                       level narrow pulses (bandwidth ≥ 500 MHz), UWB             an efficient and cost-effective path for asset
following commercial advantages:                                               the unified foundation, the "Wi-Fi+X" converged
                                                                                                                                       uses time-of-flight (TOF) ranging and trilateration        digitalization.
                                                                               system, which natively integrates IoT access,
                                                                                                                                       algorithm to achieve a 10-cm-level precision.
. Ubiquitous presence and cost advantages: After                               environment sensing, and precise positioning                                                                       . In smart business scenarios, the solution
                                                                               capabilities, is building an intelligent network that   It is mainly used in industrial automation and
  the COVID-19 pandemic, as the world widely                                                                                                                                                        efficiently supports real-time information
                                                                               covers connections, sensing, and physical space         warehousing logistics scenarios and has the
  accepts mobile distributed office and cloud-                                                                                                                                                      synchronization and centralized management of
                                                                               based on the core concept of "integrated sensing        advantage of being secure against relay attacks.
  based services, WLAN continues to be the                                                                                                                                                          numerous ESLs, enabling dynamic pricing and
                                                                               and communication". This provides a new path for                                                                     inventory visualization.
  information system with the widest coverage                                                                                          NFC: near field communication, indispensable
                                                                               refined operations and service innovation in smart                                                                 . In the asset management field, integrated
  in campuses. A WLAN serving as the sensing                                                                                           for secure near-field interaction within 10 cm. It
                                                                               campuses.                                                                                                            RFID and Bluetooth capabilities enable
  end offers significant advantages in digitizing,                                                                                     prevents eavesdropping and unauthorized access
  processing, and analyzing the physical world                                                                                                                                                      automatic stocktaking of fixed assets and track
                                                                                                                                       and is suitable for financial payment and access
  information.                                                                                                                                                                                      management of mobile assets, improving asset
                                                                               2.4.2.2 Short-Range Communication                       control systems. NFC connects with just one tap,
                                                                                                                                                                                                    utilization efficiency.
                                                                               Technologies                                            which simplifies the Bluetooth pairing process and
                                                                                                                                                                                                  . In industrial interconnection scenarios, stable
. Privacy protection and data security: In the                                                                                         improves the consumer experience.
                                                                               Campus terminals are the sensing end of smart                                                                        and reliable wireless access is provided for
  digital era, privacy protection and data security
                                                                               campuses and play an important role in campus                                                                        numerous devices including production line
  are becoming increasingly important. Integrated                                                                                      As a popular new technology, NearLink faces
                                                                               information and monitoring. Various sensors                                                                          sensors, smart meters, and environment
  sensing implemented based on traditional                                                                                             both opportunities and challenges when being
                                                                               and monitoring terminals, such as detectors and                                                                      monitoring terminals, with the capabilities of
  visual sensing and WLAN technologies                                                                                                 integrated with existing technologies.
                                                                               cameras, are deployed to monitor the temperature,                                                                    real-time backhaul of production and energy
  can provide low-cost coverage and multi-
                                                                               humidity, and other environmental information           . NearLink works on open frequency bands such                consumption data.
  dimensional accurate judgment while effectively
                                                                               of the campus in real time, and detect potential
  guaranteeing privacy security and information                                                                                         as 2.4 GHz, 5 GHz, and 6 GHz, risking spectrum
                                                                               security risks. The key trends at the terminal
  security.                                                                                                                             overlap with Wi-Fi. Co-channel interference
                                                                               layer are intelligence (built-in edge computing
                                                                                                                                        between NearLink and Wi-Fi 7 can be avoided
                                                                               capability), IP connectivity (direct network access),
                                                                                                                                        using the dynamic spectrum sensing technology,



                                                                          28                                                                                                                 29
                                                             . Proactive security protection: The characteristics                                                                                  . FTM-based positioning (with a precision of 1–3
2.4.2.4 Integrated Sensing                                                                                                            2.4.2.5 Integrated Positioning
                                                               of wireless signals are used to detect intrusions                                                                                     meters) is built on international standards and is
Wi-Fi signals have evolved beyond basic                        a n d i m p l e m e n t n o n - i n t r u s i v e e l e c t ro n i c   Based on the unified Wi-Fi infrastructure,                     natively supported by smartphones. It provides
connectivity into a significant sensing medium.                perimeter protection, and can be used to detect                        the network provides positioning capabilities                  seamless personnel/asset positioning, indoor
Combined with dedicated sensors such as mmWave                 hidden cameras in specific solutions, building a                       ranging from region-level statistics collection to             navigation, and geo-fencing management,
radars, Wi-Fi signals enable the transition from               new dimension of proactive security protection                         centimeter-level tracking on demand, accurately                improving the efficiency of guest services and
"connection" to "cognition" and truly implement                covering both physical and network spaces.                             empowering diversified service scenarios and                   security management.
"integrated sensing and communication".                                                                                               maximizing infrastructure investment returns.                . UWB fused positioning ( with centimeter-
                                                               Wi-Fi sensing is an effective and economical
                                                               method for wireless sensing, and can be used                                                                                          level precision) uses Wi-Fi net works for
. Spatial and environmental intelligence: Wi-                  for room-level presence detection (10 m), AI-                          . RSSI-based positioning (with a precision of 5–10             synchronization and backhaul and provides
  Fi channel state information (CSI) can be                    assisted positioning and tracking (2–5 m),                                                                                            reliable support for high-precision scenarios,
                                                                                                                                       meters) provides an ultra-low-cost solution ideal
  intelligently parsed to seamlessly detect space              sub-meter-level body activity identification,                                                                                         such as safety monitoring for high-risk operation
                                                                                                                                       for business insight and operations optimization
  occupancy and identify group behaviors. The                  centimeter-level vital sign detection (breathing/                                                                                     personnel (e.g., based on positioning safety
                                                                                                                                       scenarios, such as heat map analysis of customer
  lighting and air conditioning systems can                    h e a r t b e a t ) , m e d i c a l i m a g i n g , a n d m o re                                                                      lights), precise positioning of valuable assets,
                                                                                                                                       flow in shopping malls and supermarkets and
  then be automatically and accurately adjusted                scenarios. Compared with infrared/mmWave                                                                                              and navigation for AGVs and robots.
                                                                                                                                       macro-level building footfall statistics collection.
  according to the detection results, driving refined          solutions, Wi-Fi sensing leverages the integrated
  energy saving and reducing operating costs.                  sensing and communication architecture that
. Health and safety guarding: Sensing nodes                    does not require cabling and integrates multiple
  equipped with mmWave radars are deployed                     systems, significantly reducing deployment and
  in key areas to continuously monitor breathing
                                                                                                                                       2.4.3            Key Metrics
                                                               O&M costs.
  and heart rate, evaluate sleep quality, and
  provide instant warnings for risks such as falls
  and bed exits, all in a non-contact manner. This                                                                                                                                Table 2-6 Key metrics
  provides round-the-clock, privacy-friendly care
  for scenarios such as elderly care and health                                                                                             Metric                             Excellent                                        Good
  management.                                                                                                                                                                                                     Wi-Fi 6/7, Bluetooth, ZigBee, etc.
                                                                                                                                         IoT capability       Wi-Fi 7, NearLink, Bluetooth, ZigBee, etc.          (NearLink or mmWave radar not
                               Table 2-5 Application scenarios of Wi-Fi sensing                                                                                                                                              supported)
    Application                                                                 KPI or Recommended Frequency
                                     Technical Means                                                                                      Positioning                  UWB, FTM, BLE, and RSSI                              BLE and RSSI
     Scenario                                                                               Band
                      The channel CSI fluctuates in the time domain
                                                                               Maximum coverage range: 10 m to                             Container                    Wi-Fi built-in container                            Not supported
                      corresponding to different human body states,
 Human presence                                                                              15 m
                       based on which human presence or absence
   detection                                                                   Distance resolution: 0.5 m to 2 m
                      can be identified. This method is applicable to
                                                                                  Distance precision: < 0.2 m                            IoT scalability           USB, built-in IoT radio, and PCIe                             USB
                        both movement and stationary scenarios.
                                                                                                                                                                Wi-Fi CSI sensing, mmWave radar, and
                       The Doppler spectrum, target moving speed,                                                                           Sensing                                                                        mmWave radar
      Activity                                                                    Large-scale activity: sub-7 GHz                                                  unauthorized camera detection
                      and CSI amplitude change are extracted based
   identification                                                                  Small-scale activity: 60 GHz
                                       on the CSI.                                                                                      Coverage point                                                           Adding sensor coverage points (one
                                                                                                                                                                         No need to increase
                                                                                                                                           quantity                                                                 sensor added per 10–15 m2)
                        Fingerprinting-based method (commonly
                                         used):                                                                                                                                                                   Adding dedicated cables (such as
                                                                                                                                            Cabling                Reusing existing Ethernet cables
                         Database-based positioning and tracking                                                                                                                                                     RS485 cables) for sensors
       Target                Geometric model-based method:
                                                                                     Distance resolution: 0.5 m                         Communication
  positioning and        Multi-parameter joint estimation is used                                                                                                     O&M through one system                       O&M through multiple systems
                                                                                     Distance precision: < 0.2 m                         system O&M
     tracking            to improve the positioning and tracking
                       performance in multipath channel scenarios.
                       The performance is limited by the number of
                                antennas and bandwidth.

     Vital sign       The channel CSI amplitude and phase changes
                                                                                    Heartbeat detection: 60 GHz
     detection                       are estimated.




                                                        30                                                                                                                                    31
2.5 Deterministic Experience                                                                                     2.5.2.2 Deterministic Bandwidth                             Leveraging high-precision time synchronization,
                                                                                                                                                                             TSN identifies key service traffic based on flow
                                                                                                                 Deterministic bandwidth means that bandwidths               rules and performs deterministic traffic scheduling
                                                                                                                 to be occupied by different services can be isolated        at precisely defined time points. In this way, the
                                                                                                                 from each other, preventing interference during             single-hop forwarding latency of key service traffic
 2.5.1          Definition                                                                                       data transmission. On a network that carries                is controlled within microseconds. The involved
                                                                                                                 numerous services, network slicing can be used to           key technologies include high-precision time
                                                                                                                 guarantee bandwidth for key services to prevent             synchronization (such as IEEE 1588v2) and traffic
As the digital economy continues to evolve and              effort" data transmission, which is inherently
                                                                                                                 packet loss due to insufficient bandwidth during            scheduling mechanism (such as 802.1Qbv gate
industries undergo intelligent transformation,              unreliable and often results in packet loss and
                                                                                                                 service concurrency.                                        control list scheduling).
campuses have emerged as key hubs for economic              transmission jitter. The deterministic experience
activit y and innovation. Consequently, the                 capability improves user experience by delivering
                                                                                                                 Network slicing technology allocates resources of
demands on campus networks have shifted from                deterministic priority, bandwidth, and latency, as                                                               2.5.2.4 High Reliability
                                                                                                                 a physical network to multiple logical networks,
basic connectivity to delivering superior user              well as high reliability, meeting network quality
                                                                                                                 with each logical network serving a specific                High reliability prevents unreliable transmission
experiences. Traditional networks rely on "best-            requirements in different scenarios.
                                                                                                                 type of service or industry. The logical topology,          caused by factors such as packet loss and delay
                                                                                                                 SLA requirements, reliability, and security level           during data forwarding. As technology advances,
                                                                                                                 of each network slice can be flexibly defined,              more and more services (such as payment
                                                                                                                 thereby meeting diverse requirements of services,           service and PLC control information transmission
 2.5.2          Key Technologies                                                                                 industries, and users.                                      for industrial equipment) have become highly
                                                                                                                                                                             sensitive to packet loss and require higher network
                                                                                                                                                                             reliability. A common technology for reducing the
2.5.2.1 Deterministic Priority                              2.5.2.1.2 User Priority                              2.5.2.3 Deterministic Latency
                                                                                                                                                                             packet loss rate is dual fed and selective receiving.
Deterministic priority means that both application          Deterministic user priority means that key users     Deterministic latency refers to a definite time
forwarding and user network resource allocation             receive prioritized access to network services.      required for end-to-end data transmission.                  In dual fed and selective receiving, the ingress
follow predefined, deterministic priority rules.            Key users include important enterprise users and     In addition to the inherent physical latency,               device replicates received data, and sends the
                                                            important terminals on the network, such as          traditional networks have uncertain latency caused          original data and its copy to the egress device
                                                            video conferencing terminals in conference rooms.    by processes such as queue-based transmission               through different paths. The egress device then
2.5.2.1.1 Application Priority
                                                            Technologies such as preferential access, wireless   and table lookup-based forwarding. Moreover, the            forwards the data that arrives first. Dual fed and
Deterministic application packet priority ensures           signal enhancement, and wireless preferential        best-effort communication mechanism used on                 selective receiving mainly solves the problem of
that packets of key applications, such as video             forwarding are used to ensure the wireless           traditional networks lacks determinism and real-            packet loss caused by single points of failure on
conferencing and cloud desktop, are preferentially          network experience of key users.                     time performance — capabilities that are critical           forwarding paths, significantly improving network
forwarded by dynamically adjusting their priorities.                                                             in fields such as industrial manufacturing. To solve        reliability.
This involves the following key technologies:               . Preferential access guarantees the access          the problem of uncertain latency, technologies
                                                              experience of key users by allowing them to        such as time-sensitive networking (TSN) are
. Application identification: On the basis of IP              access the network and disconnecting online        developed.
  header analysis, Layer 4 to Layer 7 content and             non-key users when the number of access users
  protocol fields of packets are extracted and                reaches the threshold.
  analyzed to identify the application name.                . Wireless signal enhancement means to enhance
. Intelligent multimedia scheduling: Technologies             the signal strength at locations of key users to
  such as congestion control and differentiated               resolve poor and unstable connectivity caused       2.5.3          Key Metrics
  quality of service (QoS) are used to accurately             by distance from APs or user mobility.
  suppress bandwidth-hungry service traffic and             . Wireless preferential forwarding allows APs to
                                                                                                                 After the preceding functions are deployed on               can be achieved.
  preferentially schedule audio and video traffic,            reserve air interface resources for key users.
                                                                                                                 a campus network, the following performance
  ensuring the quality of high-priority services and          This function improves air interface experience
  improving bandwidth utilization.                            of key users when APs are congested by high-
                                                              bandwidth services (such as large file download/
                                                              upload and HD video) of some greedy STAs with
                                                              non-standard preemption capabilities.




                                                       32                                                                                                               33
                             Table 2-7 Key metrics of experience assurance
                                                                                                                 2.6.2          Key Technologies
   Category                        Metric                           Excellent                Good

                   Resolution of audio and video services                                                       2.6.2.1 Device-Level Energy Saving                         2.6.2.2 Network-Level Energy Saving
                   without downgrading or video freezing                4K                    720p
                                                                                                                In device-level energy saving, energy saving modes         N e t w o r k- l e v e l d y n a m i c e n e r g y s a v i n g i s
                            upon link congestion
                                                                                                                are defined on single devices based on different           implemented on demand through intelligent SDN
  Application    Time taken to insert a 10 MB image in the                                                      loads. For example, a device enters the sleep/             measures. Specifically, shutdown can be performed
                                                                        6s                     20s
   assurance            collaborative office scenario                                                           standby mode when it is unloaded, and enters               by group at a scheduled time and by role for each
                 Performance for 15-channel video playing                                                       the low power consumption mode when lightly                region, so that the network intelligently shuts
                                                                 1080p, latency <     720p, latency < 1000      loaded.                                                    down some devices by time segment and device
                  and 15-channel PPT slide switching on
                                                                     100 ms                    ms
                             cloud desktops                                                                                                                                role to achieve network-level energy saving.
                                                                                                                APs are the most numerous nodes on a WLAN,
                                                                  M-LAG, 50 ms
                               Hitless upgrade                                           Not supported          and their working status becomes the focus of an           AI analyzes network tidal characteristics and
                                                                service switchover
                                                                                                                energy-saving solution. APs are usually powered            configures energy-saving policies for each region.
                                                                 10 ms to 20 ms                                 by PoE switches. In addition to entering the               The following figure shows the network traffic
   Industrial                                                                         > 50 ms switchover
                 High-performance industrial ring network       switchover upon a                               sleep mode or low power consumption mode,                  statistics of a teaching building in a university over
   assurance                                                                             upon a fault
                                                                      fault
                                                                                                                APs can achieve energy saving through PoE OFF              five days. It can be seen that the network usage
                     Dual fed and selective receiving for                                                       operations.                                                increases significantly at 07:00, reaches the peak
                                                                    Supported            Not supported
                                redundancy                                                                                                                                 in the morning and afternoon, and drops to near
                                                                                                                . PoE OFF mode: The PoE switch disables the port           zero after 00:00.
                   Latency when the air interface channel                                                         from supplying power to the AP. In this case, the
                                                                     < 50 ms                < 200 ms
                       utilization is greater than 80%                                                            AP does not consume any power. When
                                                                                                                  the AP is started, the port provides power to the
                    Less than –68 dBm downlink signal
      User                                                                                                        AP again.
                     strength of a STA (or about 10 m               Bandwidth            No bandwidth
   assurance                                                                                                    . Sleep mode: Key hardware such as the CPU runs
                 horizontal distance between a STA and an       increased by 30%           increase
                           AP, without blocking)                                                                  at extremely low power, and other components
                                                                                                                  are shut down. The sleep mode saves 80% to
                            Fault alarm function                    Supported            Not supported
                                                                                                                  90% energy for a single AP, and the AP can
                                                                                                                  resume operation within one minute.




2.6 Energy-Saving

 2.6.1          Definition

Campus network energy saving includes multiple           collaborates with the building management system
dimensions, including device-level energy saving,        to significantly reduce the energy consumption
network-level energy saving, and smart building          of facilities such as air conditioners and lighting,
energy saving. Smart building energy saving uses         thereby reducing energy waste in campuses and
the CSI sensing capability of WLANs to monitor           achieving the goal of green buildings.
and identify environment changes. CSI sensing


                                                                                                                                Figure 2-13 Network traffic statistics of a teaching building in a university




                                                    34                                                                                                                35
Network energy consumption visualization refers            Energy saving can also be achieved by simplifying
                                                                                                                  2.6.2.3 Intelligent Building Collaboration
to collecting energy consumption information               the network architecture. For example, making the
reported by devices such as APs to monitor                 access layer lightweight and the aggregation layer     Intelligent building collaboration is a solution in
the energy usage of the campus net work ,                  passive enables lightweight operation and reduces      which the WLAN collaborates with the building
buildings, floors, and specific APs in real time.          energy consumption.                                    management system to save energy on building
The information is analyzed and displayed on the                                                                  facilities such as air conditioners and lights. Over
intelligent analysis system — usually a component          . The passive architecture saves the deployment        75% of building energy consumption comes from
of the NMS or SDN controller. As shown in the                of active devices at the aggregation layer, saving   air conditioning and lighting. As air conditioners
following figure, the system can display the                 energy at the aggregation layer.                     and lights are mainly for people's use, they can be
network status at the region level, including              . The access layer uses the simplified architecture,   turned off at night when no one uses them to save
regional overall energy consumption by day or                replacing the original access devices that           energy. As technology advances, in addition to
hour, regional network usage (channel utilization),          consume dozens of watts with lightweight             communication functions, a WLAN now supports
and regional overall network quality by day or               devices that consume only several watts.             spatial and personnel sensing based on channel
hour. This visualized management helps network                                                                    state information (CSI), providing information
administrators easily grasp the overall energy                                                                    input for the building management system to take
usage and implement time- and region-specific                                                                     decisions on energy saving.
energy-saving measures.
                                                                                                                  As the WLAN collaborates with the building
                                                                                                                  management system to save energy, the running
                                                                                                                                                                              Figure 2-16 Intelligent building collaboration
                                                                                                                  time of the air conditioning and lighting systems
                                                                                                                  is significantly reduced, lowering their energy
                                                                                                                  consumption by 15% to 30%. This lowers power
                                                                                                                  costs for building operators and offers substantial
                                                                                                                  economic and environmental benefits.




                                                                                                                   2.6.3           Key Metrics
    Figure 2-14 Campus energy consumption
   monitoring through the WLAN visualization
                    system                                                                                                                          Table 2-8 Key metrics for energy saving

                                                                                                                     Category                        Metric                           Excellent                  Good

                                                                                                                                                                                 Supported, saving
                                                                                                                   Device energy
                                                                                                                                       Chip-level dynamic energy saving          more than 10% in            Not supported
                                                                                                                      saving
                                                                                                                                                                                energy consumption

                                                                                                                                        Energy saving based on NE tidal
                                                                                                                                                                                 AI-based prediction       Manual prediction
                                                                                                                                                   prediction

                                                                                                                                       Shutdown when unoccupied, sleep
                                                                                                                                                                                      Supported              Not supported
                                                                                                                     Network                 mode under low load
                                                                                                                   energy saving
                                                                                                                                                 Wake-up time                          Seconds                  Minutes

                                                                                                                                             Energy-saving benefit                       30%                      10%

                                Figure 2-15 Simplified network architecture                                                           Energy saving based on CSI sensing:
                                                                                                                      Smart           human presence detection through
                                                                                                                     building         Wi-Fi for linked turn-off of building           Supported              Not supported
                                                                                                                   energy saving      equipment such as air conditioners
                                                                                                                                                    and lights



                                                      36                                                                                                                 37
2.7 Full-Scope Security                                                                                                                2.7.2                Key Technologies

                                                                                                                                                                                                                     Terminal identification on the network side is to
                                                                                                                                     2.7.2.1 Access Security
  2.7.1               Definition                                                                                                                                                                                     analyze and extract terminal characteristics based
                                                                                                                                     Asset visualization and management: Terminal                                    on the digest fields of protocol packets to identify
In campus scenarios, full-scope security aims                                  all objects (assets/users/data), and all processes    identification and visualization capabilities,                                  information such as the terminal type, model, and
to build a dynamic, intelligent comprehensive                                  (access/transmission/processing), serving as a key    including EDR asset collection on the terminal side                             vendor. Terminal identification methods include
protection system. The system is centered around                               solution to cope with complex network threats.        and terminal fingerprint analysis on the network                                passive fingerprint collection, proactive scanning,
a unified policy platform, integrates various                                                                                        side, are used to accurately identify terminals                                 and AI clustering.
security capabilities, and provides closed-loop                                                                                      and control network access, implementing secure
                                                                               Zero-trust architecture:
protection for all domains of the campus (including                                                                                  access.
physical and virtual assets, users, service flows,                             As a revolutionary paradigm in security defense,
data, and applications), ensuring service continuity                           the campus security solution based on the concept
and data security. Full-scope security firmly follows                          of zero trust provides comprehensive security
the concepts of zero trust, in-depth defense, and                              protection across terminal security, terminal
continuous self-adaptation. Through the deep                                   compliant access, link security, microsegmentation,
integration and collaboration of technologies, it                              resilience, and trustworthiness.
guarantees security for all space (physical/virtual),




                                        Figure 2-17 Campus network security architecture

Zero-trust network security:

1. Access security: provides overall secure access                             4. Microsegmentation: implements fine-grained                                                      Figure 2-18 Terminal identification process
   capabilities in terms of connection and space                                  isolation policies on the network (especially
   anytime, anywhere, covering scenarios such as                                  east-west traffic) to limit the lateral movement
   terminal security, space security, and remote                                  scope of attacks.                                  With the popularization of AI agents, smart                                     is the EDR hierarchical advanced threat protection
   secure access.                                                              5. Continuous threat detection: uses AI and           t e r m i n a l s p o s e h i g h e r s e c u r i t y p ro t e c t i o n        system, which can defend against various attacks
2. Identity and access management: implements                                     machine learning (ML) technologies to quickly      requirements and challenges, making it essential                                such as ransomware, zero-day, and fileless attacks,
   unified identit y governance, multi-factor                                     detect advanced threats through network traffic    for the industry to provide more powerful                                       and implement integrated cloud-network-edge-
   authentication, and attribute-based dynamic                                    analysis, endpoint detection and response (EDR),   terminal-side protection capabilities. An example                               device protection through network-security
   access control.                                                                and user and entity behavior analytics.                                                                                            collaboration.
3. L i n k s e c u r i t y : e n a b l e s l i n k- l a y e r a n t i -        6. Threat risk response: accurately identifies risk
   eavesdropping for transmission over the wireless                               sources based on continuous threat detection,
   air interface and wired backhaul for APs and                                   and automatically responds to risks near the
   switches (through MACsec).                                                     point of network access to block the network
                                                                                  access of risky terminals.



                                                                          38                                                                                                                                    39
                                                                                                                                                                       used together to implement single sign-on (SSO)
                                                                                                           2.7.2.2 Compliant Access
                                                                                                                                                                       for network access, service access, and O&M
                                                                                                           Compliant access: Only compliant, authorized                operations, eliminating privilege escalation risks.
                                                                                                           terminals are allowed to access the campus
                                                                                                           network, and different network permissions are              Microsegmentat ion: is a net work securit y
                                                                                                           assigned to terminals based on the identity and             architecture approach that focuses on dividing
                                                                                                           compliance status. The terminal compliance                  the network into granular security zones (micro-
                                                                                                           mechanism includes host security check, secure              segments) and enforcing stringent access control
                                                                                                           access authentication (such as 802.1X and Portal            policies between these zones to limit attacker
                                                                                                           authentication), dumb terminal identification and           lateral movement, thereby enhancing overall
                                                                                                           anti-spoofing, and unauthorized access prevention.          security.
                                                                                                           RADIUS and TACACS+ authentication protocols are




                                  Figure 2-19 EDR security protection


The campus physical space is also secured.              intrusion analysis, special event monitoring,
Technologies such as Wi-Fi 7 and mmWave radar           and indoor unauthorized device detection. With
implement personnel and object sensing in areas,        technological maturity and industry development,
which can be used in scenarios such as personnel        more scenario-specific solutions will emerge.                                              Figure 2-21 Security groups


                                                                                                                                                                       Air interface signal scrambling technology draws
                                                                                                           2.7.2.3 Link Security
                                                                                                                                                                       on multi-user multiple-input multiple-output
                                                                                                           Encryption algorithms such as WPA2 and WPA3                 (MU-MIMO) and leverages idle antennas of APs to
                                                                                                           are used to encrypt the transmission over                   send extra electromagnetic wave noise, protecting
                                                                                                           the air interface, ensuring that data captured              the communication path of STAs. Within the
                                                                                                           by unauthorized users is encrypted and can                  location range of a target STA, the actual data is
                                                                                                           be parsed into valid information only after                 not affected by interference signals, ensuring that
                                                                                                           decryption. In wireless scenarios with high security        the data can be correctly demodulated. However,
                                                                                                           requirements, scrambling is performed for air               outside the location range of a target STA, the
                                                                                                           interface transmission, making it impossible for            actual data is affected by the interference signals,
                                                                                                           unauthorized users to obtain any valid information          meaning unauthorized users cannot demodulate
                                                                                                           over the air interface. In terms of wired links,            Wi-Fi signals. As shown in the following figure,
                                                                                                           MACsec is used to implement physical layer                  outside the location range of a target STA ,
                                                                                                           encryption.                                                 only meaningless noises can be captured when
                                                                                                                                                                       someone attempts to eavesdrop or capture
                                                                                                           Wi-Fi Protected Access (WPA), which includes                wireless packets.
                                 Figure 2-20 Spatial security detection                                    WPA , WPA2, and WPA3, is a set of security
                                                                                                           standards developed by the Wi-Fi Alliance to                When the target STA moves, the information can
                                                                                                           safeguard wireless network access, and supports             be quickly updated to ensure the accuracy of air
                                                                                                           authentication modes such as EAP-PEAP and EAP-              interface scrambling.
                                                                                                           TLS.




                                                   40                                                                                                             41
                                                                                                                                   2.7.3            Key Metrics

                                                                                                                                  After the preceding terminal, link, and device                          achieve the following security capabilities.
                                                                                                                                  security functions are deployed, the network can

                                                                                                                                                                             Table 2-9 Key metrics of zero trust

                                                                                                                                        Category                         Metric                                   Excellent                      Good

                                                                                                                                    Wireless security        Data encryption algorithm                              WPA3                         WPA2

                                                                                                                                                                                                       Intrusion detection, personnel
                                                                                                                                                            Wi-Fi CSI sensing for spatial                 anomaly detection, and
                                                                                                                                     Spatial security                                                                                     No spatial security
                                                                                                                                                                      security                           scanning of unauthorized
                                                                                                                                                                                                          devices such as cameras
                                                                                                                                                                                                         Detecting unauthorized
                                                                                                                                                                Unauthorized camera                      cameras only using the
                                                                                                                                     Privacy security                                                                                        Not supported
                                                                                                                                                                     detection                        network, without the need to
                                                                                                                                                                                                      deploy an independent system
                     Figure 2-22 Signals received by different users after air interface scrambling                                    Connection            Anti-eavesdropping on air                Scrambling air interface signals
                                                                                                                                                                                                                                             Not supported
                                                                                                                                        security                  interface signals                      to prevent eavesdropping
MACsec is a Layer 2 encryption technology                                   on protecting key objects such as software, data,                                                                                                               Supported only
                                                                                                                                                                                                          Supported for all wireless
that ensures hop-by-hop link-level secure data                              and hardware on terminals and servers, and the                                                                                                                  by switches; SM
                                                                                                                                       Connection          AP + switch link-layer MACsec                  and wired links; SM series
                                                                                                                                                                                                                                          series cryptographic
transmission. With a series of security functions                           capabilities extend to management and control               security                    encryption                            cryptographic algorithms
                                                                                                                                                                                                                                             algorithms not
including data encryption, integrity check, and                             platforms, routers, firewalls, switches, and WLAN                                                                               supported by switches
                                                                                                                                                                                                                                               supported
replay protection, MACsec applies to scenarios                              products.                                                                                                                     End-to-end SLA quality
                                                                                                                                       Connection              Secure service isolation,                                                   QoS-based service
that require high data confidentiality, such as                                                                                                                                                       assurance (such as slicing) and
                                                                                                                                        security                ensuring service SLA                                                          assurance
government and finance.                                                     Through analysis of the entire lifecycle, the                                                                                  one-click deployment
                                                                            following key capabilities are required: factory-                                                                                                                Passive traffic
                                                                                                                                                                                                         Passive traffic analysis and
                                                                                                                                                                                                                                              analysis and
2.7.2.4 Resilience and Trustworthiness                                      level trustworthiness, protocol security, O&M          Full visualization of
                                                                                                                                                                Terminal identification
                                                                                                                                                                                                        proactive scanning for asset
                                                                                                                                                                                                                                               proactive
                                                                                                                                         terminals                                                     identification AI clustering for
                                                                            trustworthiness, and system trustworthiness.                                                                                                                   scanning for asset
Resilient systems refer to the securit y and                                                                                                                                                            unknown asset identification
                                                                                                                                                                                                                                             identification
trustworthiness of network devices. That is,
                                                                            In terms of device and supply chain security, it is                                                                                                              Not supporting
n e t w o r k d e v i c e s n o t o n l y e n s u re s e c u r i t y                                                                                                                                                                      unauthorized access
                                                                            necessary to start from the manufacturing process,                                                                            Supporting 30 ms fast
(confidentiality, integrity, and availability) but                                                                                                         Secure and compliant terminal                                                  detection (or relying
                                                                            such as establishing a device firmware whitelist                                                                          unauthorized access detection
                                                                                                                                    Terminal security      access based on the zero trust                                                    on an off-path
also enable users to trust their privacy protection,                                                                                                                                                   (built in switches) and EDR
                                                                            mechanism and formulating admission standards                                             concept                                                               device to support
reliability, and stability. From the perspective of                                                                                                                                                     security posture detection
                                                                            for imported chips to prevent backdoor risks.                                                                                                                 the function) or EDR
campus network threats, it is essential to focus                                                                                                                                                                                           security detection
                                                                                                                                                               Service-based security                      VXLAN tunnels and
                                                                                                                                                                                                                                           Not supported; or
                                                                                                                                                           isolation for access terminals,            independent NMS deployment
                                                                                                                                   Microsegmentation                                                                                        supporting only
                                                                                                                                                               preventing the lateral                  (compatible with third-party
                                                                                                                                                                                                                                            VXLAN tunnels
                                                                                                                                                                 movement of risks                               NMSs)
                                                                                                                                                                                                                                           Supporting threat
                                                                                                                                                                                                                                            blocking through
                                                                                                                                                            Near-source threat blocking                    Near-end threat blocking        collaboration with
                                                                                                                                    Network-security
                                                                                                                                                             through network-security                     through collaboration with        security devices;
                                                                                                                                     collaboration
                                                                                                                                                                   collaboration                                   switches                  not supporting
                                                                                                                                                                                                                                               continuous
                                                                                                                                                                                                                                                detection
                                                                                                                                                           Unknown advanced threat detection
                                                                                                                                                            enhanced by graph engine and AI
                                                                                                                                                             technologies to detect abnormal            Device security capabilities
                                                                                                                                                           behaviors such as advanced remote          such as secure communication,
                                                                                                                                      Host security                                                                                          Not supported
                                                                                                                                                           control, phishing, ransomware, zero-       secure storage of sensitive data,
                                                                                                                                                              day, crypto-mining, information                 and secure boot
                                                                                                                                                           theft, and fileless attacks in a timely
                                               Figure 2-23 Campus network threats                                                                                          manner



                                                                       42                                                                                                                            43
                   03
     Typical Applications
      of High-Quality 10
         Gbps AI Campus




44     45
3.1 Education Industry                                                                                                 3.1.2         Recommended Networking Architecture

                                                                                                                      A typical school campus network covers scenarios         O&M, and high energy efficiency. These features
                                                                                                                      such as offices, classrooms, and dormitories. The        allow the network to support diverse services and
 3.1.1           Trends and Requirements                                                                              high-quality 10 Gbps campus network offers high          future evolution of schools.
                                                                                                                      bandwidth, deterministic experience, intelligent
The global education system is accelerating its              D igital transformation in higher education
digital and intelligent transformation, using                extends beyond teaching and research, also
smart technologies to reinvent experience in all             encompassing the intelligent upgrade of campus
scenarios, including teaching, research, services,           services and operations management, which is a
and operations.                                              critical component of smart campus development.
                                                             By establishing a one-stop information service
The proliferation of smart classrooms is reshaping           platform that integrates teaching and student
traditional teaching modes. AI-driven personalized           affairs, as well as the library and logistics systems,
learning systems, AR/VR immersive classrooms,                the smart campus delivers more convenient and
and similar innovations are making teaching                  personalized service experience. Beyond that,
methods more intelligent, personalized, and                  IoT and intelligent technologies are renovating
interactive, greatly improving the teaching                  campus infrastructure management and operating
effectiveness and learning experience. At the                models, transforming operations from manual
same time, the expansion of online teaching and              to automated and intelligent. The large-scale
virtual classrooms allows high-quality educational           access of mobile devices and IoT terminals poses
resources to be shared anytime, anywhere. This               new requirements on campus network coverage,
helps to close the education divide between                  concurrency, and reliability.
regions and people, promoting education equity.
These modern teaching methods require a high-                Universities are home to tens of thousands
bandwidth, low-latency campus network, as                    of students and teachers, and provide a wide
network stability and real-time performance                  range of activities such as teaching, research,
directly impact teaching quality and experience.             services, and administration, which require a
                                                             rapidly growing number of terminals. Traditional
In scientific research, AI technologies plays a vital        O&M — which is carried out manually, relies on
role in areas such as literature data acquisition,           experience, and is reactive — is inefficient and
experiment prediction, and result analysis. For              results in a long fault handling time. There is a
example, when simulating complex experimental                need for an intelligent network O&M platform
scenarios, AI can predict outcomes in advance,               with global visibility. Such a platform can lower
reducing the number of physical trials and                   the O&M threshold, enable rapid fault detection,
improving research efficiency. But to meet the               and automatically handle typical faults, thereby
needs of AI model training and complex data                  supporting campus operations management.
processing, campus networks need to provide
robust computing power support. In addition,
high network transmission speed is essential to
enable rapid upload and download of research
data, especially during collaboration between
                                                                                                                                                 Figure 3-1 Recommended networking architecture
teams where efficient data exchange is critical.
Moreover, network security is vital for preventing
research data leakage and protecting research
achievements.




                                                        46                                                                                                                47
                                                                                                                   The following table lists the recommended                    office network.
 3.1.3          Smart Office
                                                                                                                   indicators for a high-quality 10 Gbps campus

                                                                                                                              Table 3-2 Recommended indicators for a high-quality 10 Gbps campus office network
An increasing number of universities are adopting           and cross-campus collaboration among teachers,
cloud office platforms (such as Microsoft 365               improving productivity and reducing O&M costs.
                                                                                                                                                  Item                                            Recommended Indicator
and Google Workspace) to enable remote
collaboration, online document editing, task                In addition to demanding high network                             Bandwidth of access devices deployed indoors                               ≥ 10 Gbps
tracking, and data sharing. Moreover, recent                bandwidth, cloud office platforms and HD video
years have witnessed the explosive growth in                conferencing also require lower latency, as shown                              WLAN AP standard                                                Wi-Fi 7
online meetings on applications such as Teams               in the following table.
                                                                                                                     Total throughput of 18 STAs connected to three APs in 80 MHz
and Zoom. All of these support cross-department                                                                                                                                                         ≥ 960 Mbps
                                                                                                                                              networking

                    Table 3-1 Bandwidth and latency requirements of different services                                      Concurrent channels of 4K HD video conferencing                            ≥ 60 channels

                                                                                                                         Definition and latency of key applications such as video
                                                                                                                                                                                                   1080p, latency < 100 ms
                         Service                                    Bandwidth                  Latency                               conferencing and cloud desktop

                                                                                                                                       Service latency of key users                                       ≤ 50 ms
                4K UHD video streaming                             15–30 Mbps                   20 ms

                8K UHD video streaming                             40–100 Mbps                  20 ms
                                                                                                                    3.1.4           Smart Classroom
                 HD video conferencing                              5–7.2 Mbps                  20 ms

            Real-time document collaboration                        5–10 Mbps                  < 100 ms            Modern education is undergoing unprecedented                 The introduction of VR technology has made the
                                                                                                                   changes, with traditional classroom models being             higher education experience more immersive
                      Cloud desktop                                 8–20 Mbps                  ≤ 50 ms             replaced by advanced teaching methods such                   than ever. Students can use VR devices to enter
                                                                                                                   as smart classrooms, online teaching, and VR                 simulation labs, remotely operate high-precision
                   Sources: Huawei, Cisco, Microsoft, Frontier Communications, HealthIT.gov
                                                                                                                   immersive classrooms. By integrating multimedia              research equipment, and even conduct simulated
                                                                                                                   devices, sensors, and intelligent interaction                experiments in fields such as medicine and
Burst traffic, such as uploads and downloads,               based, the quality of carrier networks outside the     systems, smart classrooms create a more dynamic              engineering. These modern teaching methods
consumes a large amount of network resources,               campus egress is also a key factor affecting user      and efficient learning environment for students.             require high-bandwidth, low-latency campus
which may cause poor experience such as freezing            experience. In the current landscape, ensuring         In addition, the widespread adoption of online               networks.
in video conferences and collaborative office               optimal experience for key office applications and     and virtual classrooms enables higher education
services. In addition, as these platforms are cloud-        enabling rapid fault locating are urgent priorities.   students to learn anytime, anywhere over the
                                                                                                                   Internet. This significantly enhances the flexibility
                                                                                                                   and accessibility of teaching.




                Figure 3-2 Cross-region collaboration via video conferencing in universities                                                               Figure 3-3 VR smart teaching


                                                       48                                                                                                                  49
Network stability and real-time performance               Therefore, the 10 Gbps AI campus network can          The following table lists the recommended                             classroom network.
directly impact teaching quality and experience.          provide high-speed, stable connectivity to support    indicators for a high-quality 10 Gbps campus
For example, insufficient bandwidth or high               new teaching modes on smart campuses. This
latency can cause lag during class interactions           ensures smooth information exchange between                     Table 3-4 Recommended indicators for a high-quality 10 Gbps campus classroom network
and delayed loading of virtual scenarios, which           teachers and students, making the teaching
ultimately affects overall teaching effectiveness.        process more efficient and interactive.                                                    Item                                              Recommended Indicator

                                                                                                                             Bandwidth of access devices deployed indoors                                       ≥ 10 Gbps
                        Table 3-3 Bandwidth and latency requirements of services
                                                                                                                                            WLAN AP standard                                                     Wi-Fi 7
                        Service                                  Bandwidth                  Latency
                                                                                                                   Total throughput of 18 STAs connected to three APs in 80 MHz
                                                                                                                                                                                                               ≥ 960 Mbps
                                                                                                                                            networking
                      Online class                               10–20 Mbps                 ≤ 100 ms
                                                                                                                           Definition and latency of online class applications                           1080p, latency ≤ 100 ms
           Standard AR & VR (entertainment)                       ≥ 30 Mbps                  ≤ 20 ms
                                                                                                                   Sensor-free WLAN APs with CSI for human presence detection
              Advanced AR & VR (training)                         > 80 Mbps                  ≤ 15 ms               and interconnection with the building management system to
                                                                                                                                                                                                                Supported
                                                                                                                  control facilities such as air conditioning and lighting for energy
                                                                                                                                                 savings

                                                                                                                                       Colored optical port density                                          ≥ 96 per device
Schools are big energy consumers. Numerous                and high OPEX. Traditional energy management
spaces such as classrooms and laboratories                methods require manual inspection of each               Automatic optimization and resolution of coverage, interference,
                                                                                                                   high channel utilization, air interface congestion, and roaming                              Supported
consume a large amount of electricity every day.          classroom, which is time-consuming, labor-                         Wi-Fi issues without manual intervention
In particular, network devices, air conditioning,         intensive, and prone to oversight. To build a green
and lighting often continue operating even when           campus and reduce OPEX, the school urgently              Natural language interaction, fast feedback on network status,
                                                                                                                                                                                                                Supported
                                                                                                                                 faults, and handling suggestions
not in use, leading to significant energy waste           needs an intelligent energy-saving system.


                                                                                                                 3.1.5              Smart Dormitory

                                                                                                                To m e e t t h e e v o l v i n g d e m a n d s o f h i g h e r        studying, entertainment, and social interaction.
                                                                                                                education and student life, university dormitories                    These dormitories must provide high-speed
                                                                                                                need to support diversified and intelligent network                   Internet access to support online learning, remote
                                                                                                                services. Modern university dormitories are no                        classes, video conferences, and other academic
                                                                                                                longer merely places for accommodation — they                         activities, while also providing a stable network for
                                                                                                                have evolved into integrated environments for                         streaming, gaming, and social applications.




                             Figure 3-4 Intelligent light control in classrooms                                                                      Figure 3-5 Diverse dormitory network services


                                                     50                                                                                                                          51
To meet these demands, university dormitories                 reliability and stability. Furthermore, traditional
require high-bandwidth, low-latency network                   manual O&M models are inadequate for large-
infrastructure, which is essential to ensure smooth           scale, high-concurrency networks. Universities
performance when multiple devices are online at               need to adopt more intelligent and automated
the same time. In addition, with the advancement              O&M solutions.
of IoT technologies, smart dormitories are
becoming a trend. Facilities such as access control,          The following table lists the recommended
lighting, air conditioning, and temperature control           indicators for a high-quality 10 Gbps campus
require intelligent management throughout the                 dormitory network.
network, which places higher demands on network


        Table 3-5 Recommended indicators for a high-quality 10 Gbps campus dormitory network

                                Item                                          Recommended Indicator

           Bandwidth of access devices deployed indoors                                ≥ 10 Gbps

                         WLAN AP standard                                               Wi-Fi 7

      Per-STA bandwidth when eight STAs access the network
                                                                                      ≥ 50 Mbps
                         concurrently

           Latency of online classes and real-time games                               < 100 ms

   Sensor-free WLAN APs with CSI for human presence detection
   and interconnection with the building management system to                                                                                            Figure 3-6 Networking diagram
                                                                                      Supported
  control facilities such as air conditioning and lighting for energy
                                 savings                                                                            Technology Advantages and Benefits
  Automatic optimization and resolution of coverage, interference,                                                  In most scenarios tested, the speed at least doubled. Impressively, the download speed reached more than
   high channel utilization, air interface congestion, and roaming                    Supported                     1 Gbps over a distance of more than 10 m.
             Wi-Fi issues without manual intervention
  Natural language interaction, fast feedback on network status,
                faults, and handling suggestions
                                                                                      Supported
                                                                                                                    3.2 Government Industry

 3.1.6           Cases                                                                                               3.2.1          Trends and Requirements

3.1.6.1 A University in Germany                                                                                     According to UN E-Government Survey 2024,                 service collaboration, as well as providing one-
                                                                                                                    81% of countries worldwide have released                  stop efficient government services, thereby making
Key Challenges and Requirements                               Key Technologies                                      digital government strategies to explore how              government services truly inclusive. According to
The university is one of Germany's leading                    Broadband access 10 Gbps: The network uses Wi-        to build a digital government. This facilitates           UN E-Government Survey 2022, 138 countries
universities and also one of the country's oldest             Fi 7 APs that support the 320 MHz bandwidth           scientific government decision-making, efficient          around the world provide multiple government
technical institutions, renowned for its excellence           on the 6 GHz frequency band, where 160 MHz            administrative office, convenient public services,        services online. Among them, 115 countries
in engineering and technology. In the student                 continuous networking can be implemented on           and precise social governance.                            provide cross-departmental and cross-level
dormitory scenario, more than 10 residential areas            abundant channels. Exclusive dynamic-zoom smart                                                                 integrated government services. The government
cover thousands of students, with over 2,000                  antennas can be freely switched between high-         Governments around the world have accelerated             network needs to provide the public with multiple
concurrent access users on average. Adequate                  density and omnidirectional coverage modes to         the construction of an integrated government              services featuring wide coverage, and break cross-
network bandwidth is crucial.                                 increase the coverage by 20%. After this solution     service platform and the creation of one network          departmental barriers to enable smooth sharing of
                                                              is deployed, the Wi-Fi device throughput can reach    for all government services. This helps implement         data across departments, levels, and regions, so as
                                                              18.67 Gbps, effectively ensuring high-concurrency     cross-region, cross-departmental, and cross-level         to provide inclusive one-stop government services.
                                                              service scenarios.


                                                         52                                                                                                              53
The annual growth rate of access to mobile                 As government services become more mobile
                                                                                                                                   3.2.3           Government Service Hall
government applications worldwide exceeds                  and service systems become more complex,
30%. After the pandemic, the frequency of                  security risks increase. In addition, network attack
video conferencing in policy delivery and cross-           technologies are emerging one after another.                           As an important window for interaction between               consultation, and services for people with special
departmental collaboration has increased by 3 to 5         This poses more security threats on the critical                       the government and the public, government                    needs. In addition, intelligence brings many service
times. To support efficient government operations,         information infrastructure of different nations.                       service halls are accelerating their transformation          upgrades. For example, AI-powered customer
it is important to improve the quality of mobile           According to the annual report on network                              to "online- offline integrat ion", as well as                services, AI-powered material pre-review, and AI-
access, video assurance capability, and O&M                security released by the US government, each US                        promoting "one network for all services" and                 powered guide robots are rapidly popularized,
efficiency. Moreover, it is also necessary to build        government department experiences an average                           "service handling across regions". These facilitate          reducing the pressure on employees at windows.
efficient office and video conferencing systems            of 32,000 network security events every year. As                       data sharing and intelligent services. Nowadays,             Furthermore, applications such as VR-/AR-based
that allow government services to be handled               such, building a security system that integrates                       government service handling is gradually shifted             virtual hall navigation and remote video assistance
anywhere and conferences to be held anytime.               network transmission (terminal access, Wi-Fi,                          to online operations. In this way, government                help improve user experience.
Furthermore, improving document, office, and               and wired links), spatial protection, and privacy                      service halls can focus on complex services,
conference handling efficiency is also a must.             protection is an important guarantee for digital
                                                           government construction.



 3.2.2          Recommended Networking Architecture

Typical government campus networks include                 provides a 10GE ultra-broadband network that
various scenarios, such as government service              features all-domain security and experience
hall and mobile office scenarios. The high-quality         a s s u ra n c e t o s u p p o r t d i g i t a l g o v e r n m e n t
10 Gbps government campus network solution                 transformation.




                                                                                                                                                                    Figure 3-8 Smart government service hall

                                                                                                                                  The intelligent upgrade of government service                data transmission, and have low latency, thereby
                                                                                                                                  halls also poses new requirements on network                 ensuring smooth service handling and quick
                                                                                                                                  infrastructure. First of all, an ultra-high-speed Wi-        system response. In addition, government services
                                                                                                                                  Fi network with blind-spot-free wide coverage                involve a large amount of personal privacy and
                                                                                                                                  is required to support mobile service handling               enterprise confidential information. Considering
                                                                                                                                  anytime, anywhere for the public and employees.              this, the network must offer strong security
                                                                                                                                  Next, intelligent applications, such as intelligent          protection.
                                                                                                                                  customer services and intelligent guide, need
                                                                                                                                  to respond in real time to reduce the waiting                The following table lists the recommended
                                                                                                                                  time of the public. This requires the network to             indicators for a high-quality 10 Gbps government
                                                                                                                                  provide high bandwidth, support heavy-traffic                service hall network.


                            Figure 3-7 Recommended networking architecture


                                                      54                                                                                                                                  55
       Table 3-6 Recommended indicators for a high-quality 10 Gbps government service hall network                                                    Table 3-7 Recommended indicators for a high-quality 10 Gbps government mobile office network

                                        Item                                                      Recommended Indicator                                                          Item                                                Recommended Indicator

              Bandwidth of access devices deployed indoors                                                  ≥ 10 Gbps                                        Bandwidth of access devices deployed indoors                                        ≥ 10 Gbps

                               WLAN AP standard                                                               Wi-Fi 7                                                     WLAN AP standard                                                         Wi-Fi 7

    Zero service interruptions upon movement in the government                                                                                        Wi-Fi access: zero service interruptions and zero impacts on
                                                                                                            Supported                                                                                                                            Supported
                              service hall                                                                                                                    services when users move in the office area
                                                                                         WPA3 + physical noise–based interference
                           Wi-Fi anti-eavesdropping                                                                                                         Number of concurrent 4K HD video conferences                                      ≥ 60 channels
                                                                                                       protection
                                                                                                                                                                                                                            WPA3 + physical noise–based interference
      Unauthorized terminal access prevention and anti-spoofing                                             Supported                                                  Wi-Fi anti-eavesdropping
                                                                                                                                                                                                                                          protection

                                                                                                                                                                             Wired security                                           AP-switch full-link MACsec

                                                                                                                                                      Unauthorized terminal access prevention and anti-spoofing                                  Supported

                                                                                                                                                                                                                             VXLAN tunnels and independent NMS
                                                                                                                                                    Service-based security isolation for access terminals, preventing
                                                                                                                                                                                                                            deployment (compatible with third-party
                                                                                                                                                                     the lateral movement of risks
                                                                                                                                                                                                                                           NMSs)

  3.2.4               Mobile Office                                                                                                                  Network-security collaboration for near-source threat blocking
                                                                                                                                                                                                                              Collaboration with switches for threat
                                                                                                                                                                                                                                            blocking


As digital transformation accelerates, government                           T h e a p p l i c a t i o n o f v i d e o c o n fe re n c i n g b y
agencies around the world are promoting wireless                            governments worldwide has been growing
office to improve administrative efficiency,                                exponentially. According to statistics, 92% of                         3.2.5           Cases
enhance flexibility, and optimize public services.                          nat ional government agencies around the
Currently, mobile devices, such as laptops, tablets,                        world have deployed cloud video conferencing
and smartphones, are widely used by government                              systems in 2023, and the number of conferences                        3.2.5.1 A Municipal Government in South Africa
e m p l o y e e s fo r s e r v i c e h a n d l i n g a n y t i m e ,        has increased by 340% compared with that in
anywhere. Wi-Fi networks bring convenience but                              2020. By 2026, the market size of government                          Key Challenges and Requirements                             Key Technologies
also increase the risk of information leakage. Wi-                          video conferencing is expected to exceed USD8                         The city is one of the most urbanized and                   Broadband access 10 Gbps: Wi-Fi 7 APs provide
Fi vulnerabilities (such as the Wi-Fi WPA2 Key                              billion, with a compound annual growth rate                           industrialized economic regions in South Africa.            high-speed wireless access and expand wireless
Reinstallation Attack in 2017 and FragAttacks                               (CAGR) of over 18%. However, the ever-growing                         As part of a major metropolitan area, the city              network coverage. 10GE-capable multi-GE wired
in 2021) around the world and Wi-Fi-targeted                                video conferencing applications pose higher                           is home to leading education and medical                    access switches provide high bandwidth, ensuring
network attacks in various regions have intensified                         requirements on network bandwidth, latency, and                       resources and a highly skilled workforce. The city          fast file download and higher office efficiency.
government customers' concerns about Wi-                                    security protection.                                                  is accelerating digital transformation through
Fi security. Due to these, some government                                                                                                        smart city construction, while network bandwidth,           F u l l - s c o p e s e c u r i t y : T h e l i n k- l a y e r a n t i -
departments deploy Wi-Fi only in a few areas,                               The following table lists the recommended                             coverage, and data security of electricity payment          e a ve s d ro p p i n g t e c h n o l o g y o n w i re l e s s a i r
hindering the development of wireless office.                               indicators for a high-quality 10 Gbps government                      stations and office campuses serve as the pillar of         interfaces eliminates wireless security risks.
                                                                            mobile office network.                                                its transformation.                                         Unauthorized users cannot capture wireless
                                                                                                                                                                                                              data of office personnel, ensuring secure data
                                                                                                                                                                                                              transmission over air interfaces.

                                                                                                                                                                                                              Au t o n o m o u s n e t wo r k : V i s u a l i ze d n e t wo r k
                                                                                                                                                                                                              management, unified management of network-
                                                                                                                                                                                                              wide devices, and minute-level fault locating
                                                                                                                                                                                                              improve network O&M efficiency.




                                                                       56                                                                                                                                57
                                                                                                                   In terms of intelligent operations, AI, big data,          Bank 5.0 poses higher requirements on the bank
                                                                                                                   and process automation are used to improve                 network infrastructure, which is expected to ensure
                                                                                                                   the intelligence level of risk control, operations         stable running of service systems, support high-
                                                                                                                   scheduling, and IT system management, reduce               density access of massive terminals, guarantee
                                                                                                                   operational costs, and enhance agility. In terms of        low-latency transmission of remote video and data
                                                                                                                   scenario-based experience, smart terminals, remote         traffic, implement security isolation, and provide
                                                                                                                   tellers, queue-free experience, and online and             intelligent O&M capabilities, to build an agile,
                                                                                                                   offline integrated services are becoming standard,         secure, and sustainable digital foundation.
                                                                                                                   providing customers with more convenient,
                                                                                                                   efficient, and personalized service experience.




                                                                                                                    3.3.2          Recommended Networking Architecture

                                                                                                                   Typical financial campus networks include smart            provides a 10GE ultra-broadband network that
                                                                                                                   office and smart outlet scenarios. The high-               features all-domain security and deterministic
                                                                                                                   quality 10 Gbps financial campus network solution          experience to support Bank 5.0 evolution.




                                     Figure 3-9 Networking diagram

Technology Advantages and Benefits                        as electricity user information and payment data
10 Gbps high bandwidth allows full coverage               and safeguarding the office network. Intelligent
and network access anytime, anywhere. It also             O&M with automatic locating, inspection, and
enables zero-lag transmission for large data and          patrol capabilities provides real-time visibility into
file transfers, meeting all daily office demands.         network quality, and supports one-click network
The solution also prevents data eavesdropping             fault rectification, improving O&M efficiency by
and unauthorized storage, eliminating risks of            90%.
capturing or leaking sensitive information such



3.3 Finance Industry


 3.3.1          Trends and Requirements

Digital transformation of financial institutions          In the Bank 5.0 era, banks are accelerating
has gone through four eras: Bank 1.0 era where            their intelligent and customer-centric upgrade
physical branches provide services, Bank 2.0 era          to achieve efficient communication, intelligent
where self-services such as ATM services are              operations, and scenario-based experience. In
provided, Bank 3.0 era where services are provided        terms of efficient communication, banks widely
anytime and anywhere through mobile portals               deploy video conferencing, intelligent customer
such as apps, and Bank 4.0 era where FinTech              service, and enterprise collaboration platforms to
redefines banking services. Powered by AI, banking        implement real-time communication and efficient
services are evolving towards Bank 5.0.                   decision-making between HQs and branches.                                           Figure 3-10 Recommended networking architecture



                                                     58                                                                                                                  59
                                                                                                                 The experience of core tools such as video                  The following table lists the recommended
 3.3.3          Smart Office
                                                                                                                 conferencing, cloud desktops, and collaborative             indicators for a high-quality smart office network.
                                                                                                                 office applications directly affects employees' work
With the wide application of remote office and             After the COVID-19 pandemic, hybrid office that       efficiency. The finance industry is in urgent need
mobile devices, the number and types of IT                 becomes commonplace and the development of            of a high-quality network that can guarantee
terminals in the finance industry are increasing           global operations have made video conferencing a      deterministic experience of key applications.
rapidly. These terminals are purchased by different        core tool for cross-regional communication in the
departments through different sources, and are             finance industry. According to Gartner's forecast,
often managed by multiple siloed management                video conferencing usage will grow at a CAGR of
                                                                                                                         Table 3-8 Recommended indicators for a high-quality 10 Gbps smart finance office network
platforms. As a result, the network cannot                 15% to 20% in the next three years. In addition,
dynamically identify online terminals in real              the flexibility of employees' access to the working
                                                                                                                                                Item                                         Recommended Indicator
time or detect abnormal terminal behaviors, and            environment anytime and anywhere, as well as
unauthorized terminal access and spoofing occur            the high requirements of the banking industry on                 Bandwidth of access devices deployed indoors                              ≥ 10 Gbps
frequently, bringing security risks to banking             data security and compliance, have also promoted
services. A network that can "see, distinguish, and        the rapid popularization of cloud desktops in                                 WLAN AP standard                                              Wi-Fi 7
manage" terminals, as well as provide wired and            banks. Moreover, with the diversification of
                                                                                                                   Total throughput of 18 STAs connected to three APs in 80 MHz
wireless security is urgently needed.                      banking services and the increasing demand for                                                                                            ≥ 960 Mbps
                                                                                                                                            networking
                                                           cross-departmental collaboration, collaborative
                                                                                                                          Concurrent channels of 4K HD video conferencing                           ≥ 60 channels
                                                           office applications such as Microsoft 365 have
                                                           become important tools for improving efficiency,            Definition and latency of key applications such as video
                                                                                                                                                                                               1080p, latency < 100 ms
                                                                                                                                   conferencing and cloud desktop
                                                           optimizing processes, and promoting digital
                                                           transformation.                                                           Service latency of key users                                      ≤ 50 ms

                                                                                                                                                                                       WPA3 + physical noise–based interference
                                                                                                                                      Wi-Fi anti-eavesdropping
                                                                                                                                                                                                     protection

                                                                                                                                            Wired security                                    AP-switch full-link MACsec

                                                                                                                     Unauthorized terminal access prevention and anti-spoofing                        Supported

                                                                                                                                                                                        VXLAN tunnels and independent NMS
                                                                                                                   Service-based security isolation for access terminals, preventing
                                                                                                                                                                                       deployment (compatible with third-party
                                                                                                                                    the lateral movement of risks
                                                                                                                                                                                                      NMSs)
                                                                                                                                                                                        Collaboration with switches for threat
                                                                                                                    Network-security collaboration for near-source threat blocking
                                                                                                                                                                                                      blocking




                                                                                                                  3.3.4           Smart Outlet

                                                                                                                 Bank outlet is rapidly evolving from traditional            service options and cuts down customers' waiting
                                                                                                                 service windows to intelligent, digital, and                time. Virtual financial consultants, metaverse
                                                                                                                 self-service integrated service platforms. The              bank experience areas, and AR-based product
                                                                                                                 introduction of intelligent customer service,               display have enhanced interactivity and improved
                                                                                                                 intelligent virtual teller machines (VTMs), and             customer experience and operations efficiency.
                                                                                                                 intelligent greeting robots provides efficient self-




                      Figure 3-11 Bank employee working remotely on cloud desktop




                                                      60                                                                                                                61
                                                                                                                       3.3.5          Cases


                                                                                                                      3.3.5.1 A Bank in Kenya
                                                                                                                      Key Challenges and Requirements                            Key Technologies
                                                                                                                      The bank has significant influence in Kenya's              Broadband access 10 Gbps: Devices support
                                                                                                                      financial services sector. The bank improves               flexible switching between triple-radio and dual-
                                                                                                                      its service capabilities through technological             radio modes and provide a rate of up to 5.95 Gbps.
                                                                                                                      innovation and digital transformation, providing           The built-in smart antennas can automatically
                                                                                                                      real-time financial services to more than 30               adjust directions to ensure always-on signals for
                                                                                                                      million users. Facing the development trend of             users. This significantly enhances users' wireless
                                                                                                                      global fintech, the bank is upgrading its core             experience and enables the network to respond
                                                                                                                      system to cope with the traffic burst of mobile            quickly during peak hours or when a large number
                                                                                                                      services, and enhancing the technical adaptability         of users use services simultaneously.
                                                                                                                      of the system to new services and features to
                                                                                                                      meet the increasing demands of users. This digital         Autonomous network: The solution provides full-
                                         Figure 3-12 Smart bank outlet                                                transformation not only improves service efficiency        lifecycle campus network automation, intelligent
                                                                                                                      and quality, but also helps the bank maintain its          fault auto-solving based on big data and AI,
Such business upgrades require a high-quality,                 The following table lists the recommended
                                                                                                                      leading position in the industry and provide better        automatic discovery of network devices, fault
highly integrated network that features fast                   indicators for a high-quality smart outlet network.
                                                                                                                      and more convenient financial services for users in        information collection, and rectification solution
deployment, high concurrency, high security,
                                                                                                                      Kenya and beyond.                                          recommendation.
unified LAN/WAN management, and intelligent
assurance for high-priority applications.

       Table 3-9 Recommended indicators for a high-quality 10 Gbps smart finance outlet network

                         Item                                             Recommended Indicator

                  WLAN AP standard                                                  Wi-Fi 7

  No service interruption when smart terminals move
                                                                                   Supported
                      in the outlet

  Concurrent channels of 4K HD video conferencing                                ≥ 60 channels

   Definition and latency of key applications such as
                                                                            1080p, latency < 100 ms
         video conferencing and cloud desktop

              Service latency of key users                                          ≤ 50 ms

                                                              Intent-driven deployment through natural language
                  Service provisioning
                                                                  interaction and template-based deployment

                                                              Unified management of multiple outlets, and three-
               O&M and management                             dimensional visualization of applications, terminals,
                                                                                 and networks
                                                              Hyper-converged gateway, integrating functions such
                    Egress gateway
                                                                  as 5G, WAC, routing, security, and switching

               Wi-Fi anti-eavesdropping                       WPA3 + physical noise–based interference protection

                    Wired security                                         AP-switch full-link MACsec
  Unauthorized terminal access prevention and anti-
                                                                                   Supported
                     spoofing
 Network-security collaboration for near-source threat
                                                                 Collaboration with switches for threat blocking
                       blocking                                                                                                                            Figure 3-13 Networking diagram


                                                         62                                                                                                                 63
Technology Advantages and Benefits                                     of bank staff. Video conferences are free from
Full Wi-Fi coverage improves the work efficiency                       disconnection and freezing, and data transmission
of bank staff and meets the network access                             experiences zero failures. The financial network is
requirements of customers, implement ing                               visualized with just one click, and network faults
wireless service handling. Files such as banking                       can be located in minutes, significantly improving
service materials are uploaded and downloaded                          O&M efficiency and reducing the network fault
at a high speed, improving the service efficiency                      locating time by 95%.




3.4 Healthcare Industry


 3.4.1              Trends and Requirements

Currently, hospitals are in the transition period                      In 2024, the number of outpatient and emergency
from full integration to intelligence. In the past,                    visits in Chinese hospitals exceeded 2.38 billion,
siloed applications and data silos cannot support                      the number of inpatient visits exceeded 200
healthcare services well. How to implement                             million, and the number of surgeries exceeded 80
data interconnection and sharing across the                            million. To meet the huge demand for healthcare
hospital and maximize the value of healthcare                          services, improving outpatient service quality,
data in clinical diagnosis and treatment has                           ward management efficiency, and critically ill
                                                                                                                                                        Figure 3-14 Recommended Networking Architecture
always been an urgent issue for hospitals. Smart                       patient surgery is an important means to ensure
hospital construction implements hospital-wide                         healthcare service quality and accelerate the
data integration, strict data security control, and                    construction of smart hospitals.                       3.4.3          Smart Outpatient Service
medical data invoking and intelligent warning
system that complies with international healthcare
                                                                                                                             Outpatient services are the window of hospital            million. The extremely low doctor-patient ratio
standards. This ensures the integrity, validity, and
                                                                                                                             services, and the quality of outpatient services          results in long registration, payment, and waiting
security of patients' healthcare data, effectively
                                                                                                                             directly affects the image of a hospital. In 2024,        time, as well as short consultation time (less than
optimizes hospital resources, and improves the
                                                                                                                             the number of outpatient and emergency visits             10 minutes), which is a long-standing problem. In
overall operation efficiency and competitiveness of
                                                                                                                             in China exceeded 2.38 billion, but the number of         contrast, the non-consultation time can be as long
hospitals.
                                                                                                                             doctors and nurses nationwide was less than 10            as 40 to 80 minutes.




 3.4.2              Recommended Networking Architecture

A typical healthcare campus network covers                             broadband, all-domain security, and ubiquitous
outpatient , inpatient , and operating room                            connectivity to meet the requirements of smart
scenarios. The high-quality 10 Gbps healthcare                         healthcare construction.
c a m p u s n e t w o r k p ro v i d e s 1 0 G b p s u l t ra -




                                                                                                                                                        Figure 3-15 Patients queuing for medical treatment


                                                                  64                                                                                                              65
Outpatients use the IoT to implement remote                 signals, repeated cabling, long construction period,   The following table lists the recommended indicators for a high-quality smart ward network.
device monitoring, intelligent management, and              high costs, multi-protocol incompatibility, and
data integration, reducing manual operations                difficult data integration.                                     Table 3-11 Recommended indicators for a high-quality 10 Gbps smart outpatient network
and improving diagnosis and treatment precision.
However, the IoT is constructed by phase based              The following table lists the recommended                                              Item                                         Recommended Indicator
on requirements, which may lead to conflicts and            indicators for a high-quality smart outpatient
                                                                                                                    Zero roaming of PDAs and zero service interruption during mobile
interference between the wireless network and IoT           network.                                                                                                                                    Supported
                                                                                                                                            ward rounds

        Table 3-10 Recommended indicators for a high-quality 10 Gbps smart outpatient network                                 Bandwidth of access devices deployed indoors                              ≥ 2.5 Gbps


                               Item                                          Recommended Indicator                                          WLAN AP standard                                             Wi-Fi 7


          Bandwidth of access devices deployed indoors                               ≥ 10 Gbps                                 Integration of the intranet, extranet, and IoT                           Supported


                        WLAN AP standard                                              Wi-Fi 7                         Cuffless heart rate, respiratory frequency, and fall monitoring                   Supported

  Wi-Fi access of STAs, ensuring uninterrupted services when STAs
                                                                                     Supported
                move in the outpatient department
                                                                                                                    3.4.5           Smart Operating Room
                                                                        WLAN APs are integrated with IoT to
                      IoT construction mode
                                                                            form a unified network.

              GB-level medical image retrieval speed                                 In seconds                    The ICU provides systematic, high-quality, and               More and more advanced devices are used in
                                                                                                                   centralized healthcare monitoring and treatment              ICUs and operating rooms, greatly improving the
      Total throughput of concurrent Wi-Fi access of 30 users                       ≥ 340 Mbps                     for patients with severe or critical illnesses. There        timeliness and success rate of treatment. However,
                                                                                                                   are dozens of types of bedside devices in ICUs.              due to various types of devices, vendors, and
                                                                                                                   In addition to basic devices such as the monitor,            standards, different devices work independently
 3.4.4          Smart Ward                                                                                         ventilator, infusion pump, and micro-injection               and are not interoperable, and a large amount
                                                                                                                   pump, other devices such as the defibrillator, ECG           of clinical data generated in ICUs and operating
After being consulted and examined by an                    Currently, wireless PDAs are mainly used for           machine, pacemaker, and blood gas analyzer may               rooms needs to be manually recorded and
outpatient doctor, if a patient needs further               mobile ward rounds to view patients' medical           also be deployed as required. The anesthesiologist           analyzed by healthcare personnel, which brings a
treatment, the patient needs to be hospitalized.            records, record patients' diagnosis and treatment      in an operating room is also one of the most                 heavy burden to healthcare personnel. In addition,
In 2024, the patient-bed ratio in Chinese hospitals         data, and deliver doctors' medical advice. However,    critical roles in protecting patients. During an             the value of clinical data cannot be maximized
with more than 500 beds was 1.2 to 1.5, while that          doctors need to shuttle between different wards        operation, the anesthesiologist needs to pay close           due to discontinuous and non-standard data
of the Mayo Clinic in the United States was 28.2,           during ward rounds, and packet loss may occur          attention to the patient's vital signs displayed on          manually recorded.
a difference of nearly 20 times. The extremely low          when PDAs roam between APs. Consequently,              various medical devices and intervene in a timely
patient-bed ratio results in heavy workloads for            doctors need to log in to PDAs repeatedly, severely    manner.
healthcare personnel. 50% of healthcare personnel           affecting the ward round efficiency.
work overtime for more than 60 hours per month.




                            Figure 3-16 Mobile ward round with a wireless PDA                                                                             Figure 3-17 Smart operating room


                                                       66                                                                                                                  67
The following table lists the recommended indicators for a high-quality smart operating room network.             Technology Advantages and Benefits                                         provides strong support for future network service
                                                                                                                  The solution meets the ultra-high bandwidth                                expansion in the next 5 to 10 years. Fast service
      Table 3-12 Recommended indicators for a high-quality 10 Gbps smart operating room network
                                                                                                                  requirements of access devices and offers excellent                        deployment and simplified network management
                                                                                                                  office experience. Full Wi-Fi 7 network coverage                           slash the network OPEX by 63%.
                               Item                                         Recommended Indicator
                                                                                                                  with ultra-high bandwidth and ultra-low latency
           Bandwidth of access devices deployed indoors                             ≥ 10 Gbps

                        WLAN AP standard                                              Wi-Fi 7                     3.5 Manufacturing Industry
           Integration of the intranet, extranet, and IoT                           Supported

         Core switch upgrade without service interruption                           Supported
                                                                                                                   3.5.1               Trends and Requirements

 3.4.6          Cases
                                                                                                                  Intelligent manufacturing is becoming more                                 The network is a foundation for the evolution
                                                                                                                  digitalized, intelligent, and flexible. With the                           toward intelligent manufacturing. It connects
3.4.6.1 A Hospital in the UAE                                                                                     converged application of technologies such as                              various factors of production such as people,
                                                                                                                  the industrial Internet of Things (IIoT), artificial                       machines, materials, methods, and environments,
Key Challenges and Requirements                             Key Technologies                                      intelligence (AI), 5G, and edge computing,                                 and provides support for data generation,
The hospital in the United Arab Emirates (UAE)              Broadband access 10 Gbps: 10GE-capable access         manufacturing enterprises are gradually achieving                          collection, transfer, processing, and monetization.
was established in the 20th century. It develops            switches provide ultra-high access bandwidth. The     device interconnection, data-driven operations,                            To build a network base, various factors of
strategies for the health system, ensuring                  core switches support 100GE ports. Wi-Fi 7 APs        process collaboration, and autonomous decision-                            production need to be connected to the network,
the delivery of high-quality medical services               and smart roaming ensure uninterrupted mobile         m a k i n g , a n d e v o l v i n g t o w a rd s p re d i c t i v e        and the connection requirements of factors
through continuous assessment of population                 services of terminals.                                maintenance, flexible production lines, tailored                           of production and upper-layer applications
health. Amidst the digital transformation surge,            Autonomous network: M × N modular deployment          customization, and sustainable operations. In the                          need to be met. Centering on the value chain,
the hospital upgraded its network with full                 can quickly complete device configuration and         future, intelligent manufacturing will focus on                            manufacturing enterprises demand for stable
consideration of advancement and innovation by              deliver services in minutes. Centered on user         digital twins and industrial foundation models                             production, flexible manufacturing, fast R&D,
leveraging cutting-edge emerging technologies,              experience, the solution provides refined network     to promote intelligent collaboration and full-                             and safe operations during transformation and
ensuring it became mobile, digital, and intelligent.        O&M. With big data and AI models, network issues      lifecycle optimization of industry chains, driving                         upgrade towards intelligent manufacturing in
                                                            can be predicted in advance, making campus            high-quality and sustainable transformation and                            terms of R&D, production, and operations.
                                                            network O&M visualized, intelligent, and efficient.   upgrade of the manufacturing industry.




                                                                                                                   3.5.2               Recommended Networking Architecture

                                                                                                                  A typical manufacturing network achieves IT                                maintenance, production line optimization, and
                                                                                                                  and OT convergence, eliminates data silos, and                             flexible production, and also enables real-time
                                                                                                                  integrates systems such as the MES, ERP, SCADA,                            service collaboration, precise decision-making, and
                                                                                                                  and PLC into a unified network, achieving real-                            continuous innovation, laying a foundation for
                                                                                                                  time data collection, analysis, and feedback. The                          intelligent manufacturing.
                                                                                                                  network supports applications such as predictive




                                       Figure 3-18 Networking diagram


                                                       68                                                                                                                               69
                                                                                                               Customized and diversified products are becoming                          workshop become a major concern of enterprises.
                                                                                                               a new normal in the manufacturing industry.                               As such, enterprises need to adopt automation,
                                                                                                               In addition, enterprises are actively promoting                           robotics, and smart logistics technologies to build
                                                                                                               flexible manufacturing to accelerate delivery,                            flexible manufacturing systems. The network also
                                                                                                               reduce production costs, and improve equipment                            needs to adapt to scenario-specific production
                                                                                                               utilization. According to a market research, the                          line adjustment requirements and provide flexible
                                                                                                               compound annual growth rate (CAGR) of the                                 access to related terminals. The traditional wired
                                                                                                               global flexible manufacturing market in the next                          network access mode involves access point
                                                                                                               five years will reach 20%. Since manufacturing                            planning and cabling, and cannot meet the flexible
                                                                                                               enterprises need to adjust production processes                           manufacturing requirements. Wireless networks
                                                                                                               a c c o rd i n g t o o rd e r s , t h e p ro d u c t i o n l i n e        enable production equipment to go wireless, and
                                                                                                               adjustment efficiency as well as the flexibility                          therefore are being gradually deployed at scale in
                                                                                                               of production equipment and even the entire                               manufacturing enterprises.




                          Figure 3-19 Recommended networking architecture




 3.5.3         Highly Reliable Converged Production Network

For manufacturing enterprises, each production          the production plan, preventing economic losses.                                            Figure 3-20 Smart flexible manufacturing factory
unit has its own production capacity indicators         To achieve stable mass production, reliability
and production cycle time requirements. For             management is required in all aspects such
example, for SAIC Motor's factory in Ningde, the        as people, machines, materials, methods, and
production cycle time is 60 jobs per hour (JPH).        environment, such as raw materials, energy
This means one car is produced every minute. Any                                                               Industrial quality inspection is becoming more                            data surges, quality inspection imposes higher
                                                        meters, and production equipment. As the data
service interruptions may affect the production.                                                               intelligent and automated. AI visualization, multi-                       network requirements. The network is required
                                                        connection foundation, the network connects the
For instance, 14 factories of a famous Japanese                                                                modal sensors, and edge computing are widely                              to provide high bandwidth, low latency, strong
                                                        production management and execution systems
automaker stopped production for 1.5 days due                                                                  used to implement high-precision and efficient                            isolation, and high reliability to ensure stable
                                                        (such as the ERP, MES, SCADA, PLC, and I/O). It
to IT system faults, which directly affected the                                                               defect identification and real-time quality control.                      backhaul of HD videos, real-time inference and
                                                        needs to provide all-time online and highly reliable
production and delivery of 14,000 cars. Stable                                                                 As the number of inspection devices and image                             linkage, and closed-loop data analysis.
                                                        connections to ensure the production continuity of
mass production is to ensure the stability and          enterprises.
continuity of the production process and execute




                                                   70                                                                                                                               71
                                                                                                                                     3.5.4           Cases

                                                                                                                                    3.5.4.1 RiaStone Factory (Visabeira Group)
                                                                                                                                    Key Challenges and Requirements                                making timely alerts ineffective.
                                                                                                                                    The RiaStone Factory, part of the Visabeira Group,            . AGV: The existence of AGVs at the factory
                                                                                                                                    manufactures ceramic tableware. The factory                     provided a testing opportunity for both improved
                                                                                                                                    provided a live industrial environment used to                  communicat ions and seamless handover
                                                                                                                                    assess communication technologies for Industry 4.0              performance.
                                                                                                                                    use cases, validating the usage of Wi-Fi 7 and TSN              Pressing machines impacted by network
                                                                                                                                    technologies to deploy newly envisioned use cases.              traffic: The factory's pressing machines were
                                                                                                                                                                                                  . halted because their network-enabled latency-
                                                                                                                                    The factory manifested different challenges,                    sensitive Manufacturing Executing System (MES,
                                                                                                                                    highlighted in the following:                                   which handles, e.g., monitoring and control)
                                                                                                                                                                                                    features were being disrupted by the underlying
                                                                                                                                    . Quality Control Point (QCP): This entity                    . network traffic.
                                                                                                                                      required real-time monitoring and fault detection
                                 Figure 3-21 AI-based HD quality inspection                                                           by transferring high-resolution images (over 15             Key Technologies
                                                                                                                                      to 120 MB, compressed to 15 to 18.5 MB) to an               The Wi-Fi 7 network was coupled with Multi-
Manufacturing enterprises need to protect their             promotes seamless flow of high-value data and                             AI server (YOLOv5) for defect detection. This               Link Operation (MLO) capabilities, with the tests
fixed assets and information assets from internal           blurs the original security boundaries. In this case,                     demands high throughput and high reliability.               utilizing frequency bands including 2.4 GHz, 5 GHz,
and external attacks. As digital transformation             ensuring network security is particularly important                     . Security video streaming: In the factory, there are         and 6 GHz. Wi-Fi 7 leverages such bands, along
continues to sweep across the manufacturing                 for secure operations of enterprises.                                     multiple high-quality video streams from cameras,           with 320 MHz bandwidth, 4096-QAM modulation,
industry, manufacturing enterprises typically have                                                                                    requiring the network to handle their transport             Multiple Resource Unit (MRU), and MLO to increase
tens of thousands of production devices that are            The following table lists the recommended                                 over a fast and reliable wireless medium.                   throughput to 23 Gbps. TSN was also deployed by
connected to or about to connect to the network             i n d i c a t o r s o f a h i g h l y re l i a b l e c o n v e rg e d   . Smartwatches: Due to the large size and high                implementing traffic prioritization. Internal press
in a single factory. The convergence of IT and OT           production network.                                                       noise levels of the factory, its operators struggled        communication was placed in queues 5, 6, and 7
                                                                                                                                      to constantly monitor all equipment. The factory's          based on criticality, while factory network traffic
          Table 3-13 Recommended indicators of a highly reliable converged production network                                         existing Wi-Fi network provided unreliable                  used queues 0, 1, 2, and 3.
                                                                                                                                      connectivity for small smartwatch antennas,
                               Item                                               Recommended Indicator

                        WLAN AP standard                                                       Wi-Fi 7

                 Maximum single-user Wi-Fi rate                                              ≥ 3.5 Gbps

                 Packet loss rate in Wi-Fi roaming                                             ≤ 0.1%

                 Switchover time upon a link fault                          ≤ 50 ms, without service interruption

      Switchover time upon an industrial ring network fault                                    ≤ 20 ms

                                                                         Dual fed and selective receiving, ensuring
                  Going-wireless of workstations
                                                                            service continuity during roaming

                 1588v2 clock precision @ 64 hops                                                1 µs

   Video definition and latency of key applications such as video
                                                                                    1080p: latency < 100 ms
                  conferencing and cloud desktop

                    Service latency of key users                                               ≤ 50 ms

                 Unified O&M for IT/OT networks                                              Supported
                                                                                                                                                                           Figure 3-22 Networking diagram


                                                       72                                                                                                                                    73
Technology Advantages and Benefits                                           where Wi-Fi 7's more powerful PoE-enabled
                                                                                                                                      3.6.2          Recommended Networking Architecture
W i - F i 7 d e m o n s t ra t e d co n s i s t e n t ly h i g h e r         hardware was able to significantly improve
throughput and lower latency compared to Wi-                                 coverage and communications, despite the end
Fi 6. The performance advantage was particularly                             user devices only supporting up to Wi-Fi 5.             A typical smart store network integrates IoT, AI,         This provides customers with personalized,
evident when utilizing wider bandwidths (160                                                                                         big data analytics, and automation technologies           seamless, and convenient shopping experience
MHz and 320 MHz) and MLO.                                                    Deterministic performance was achieved in the           to enable real-time intelligent interconnection           while enabling efficient and intelligent operations
                                                                             press use case, with TSN guaranteeing real-time         of all elements — people, goods, and spaces.              management.
The tri-band MLO (2.4 GHz, 5 GHz, and 6 GHz)                                 communication with low latency, low jitter, and
achieved the highest spectral efficiency at 12.65                            zero packet loss, even under constrained network
Mbps/MHz and supported 228 concurrent streams                                operations.
in downlink tests.
                                                                             TSN solved a critical operational challenge
It provided enhanced mobility for mobile devices,                            by enabling the integration of isolated press
achieving lower packet loss and reduced RTTs,                                machines into the factory's MES, allowing them to
compared to Wi-Fi 6 in multi-band configurations.                            be monitored, controlled, and automated.
In high-demand media cases, such as the QCP, Wi-                             Simple configuration is a feature achieved by
Fi 7 significantly improved performance, ensuring                            only having to define the TSN flow on the first
the reliable transfer of large, high-resolution                              switch along the communication path, to ensure
images with no losses throughout testing. All                                prioritization throughout the network. This
factory operational and timing requirements were                             drastically reduces the complexity of configuration,
satisfied using the Wi-Fi 7 network.                                         management, and maintenance in operational
                                                                             environments. This finding supports the potential
The simple upgrade of the existing factory's Wi-Fi                           for adopting an interface-based TSN approach to
to Wi-Fi 7, not only enabled previously unfeasible                           simplify deployment for industrial IT staff.
use cases (e.g., the Smartwatch usage in factory),




3.6 Retail Industry


  3.6.1               Trends and Requirements                                                                                                                   Figure 3-23 Recommended networking architecture


The retail industry is rapidly integrating digital                           Amid growing consumer demand for personalized,
                                                                                                                                      3.6.3          Smart Retail Store
transformation with omnichannel retail. For                                  convenient, and instant shopping experience,
example, retailers are using multiple channels,                              retailers are increasingly investing in smart stores.
i n c l u d i n g e - c o m m e rc e p l a t fo r m s , o f f l i n e        As the core of digital retail transformation, these     Traditional stores use paper labels to mark               updates. Additionally, ESLs provide clear displays
stores, mobile apps, social media, and smart                                 stores leverage various innovative technologies,        product prices, which is inefficient, error-prone,        and dynamic content, enhancing both shopping
devices, to connect with consumers and deliver                               such as IoT, AI, AR/VR, and smart shopping              and inflexible. Due to this, an increasing number         convenience and interactive experience for
seamless shopping experience. Driven by various                              guide, to achieve intelligent upgrades for people,      of stores are now adopting electronic shelf               customers. They are also environmentally friendly,
technologies, such as AI, IoT, cloud computing, and                          goods, and spaces. These upgrades deliver more          labels (ESLs). ESLs can automatically synchronize         reducing paper waste while offering retailers a
big data analytics, applications like personalized                           convenient and seamless shopping experience.            prices, promotional details, and inventory status,        more flexible and precise method of operations
recommendations, automated warehousing and                                                                                           avoiding errors and delays caused by manual label         and marketing.
distribution, and intelligent customer service are
gradually enhancing customer experience and
operational efficiency, thereby accelerating the
transition toward smart retail.



                                                                        74                                                                                                                75
                                                                                                                   3.6.4          Cases

                                                                                                                  3.6.4.1 A Chain Retailer in Germany
                                                                                                                  Key Challenges and Requirements                            streamlined within stores of the same scale, M ×
                                                                                                                  The large chain retailer in Germany has numerous           N template-based configuration can be used for
                                                                                                                  stores. Driven by continuous business expansion,           building block–like construction: Deployment tasks
                                                                                                                  the retailer opens more than 10 new stores each            can be customized in drag-and-drop mode on the
                                                                                                                  year, needing to deploy over 300 new network               GUI, and differentiated modifications can be made.
                                                                                                                  devices. Network configuration before the opening          A single template can be copied to other sites in
                                                                                                                  of new stores, commodity price adjustment, and             batches.
                                                                                                                  asset stocktaking in stores all require the network
                                                                                                                  to provide high bandwidth and intelligence.                Broadband access 10 Gbps: Smart antennas provide
                                                                                                                                                                             full wireless coverage for stores.
                                                                                                                  Key Technologies
                                                                                                                  Autonomous network: After the network                      Connect everything: The solution innovatively
                                                                                                                  administrator selects the service scenario and             integrates IoT cards that support multiple
                                                                                                                  requirements on the GUI, service planning and              communication modes, such as Bluetooth, ZigBee,
                                              Figure 3-24 ESLs
                                                                                                                  configuration generation are automatically                 and RFID, helping the enterprise quickly build an
                                                                                                                  completed. Since the network configurations are            IoT network for ESLs and asset stocktaking.


In response to rapidly changing market                      dimensional visibility and rapid fault locating
competition and consumer demands, retailers                 across the store network, thereby reducing OPEX
require standardized, automated, digital, and               and improving operational efficiency.
flexible operations to ensure agile service rollout
in stores in a short time. Additionally, centralized        The following table lists the recommended
management is required to provide multi-                    indicators for a high-quality smart store network.


                Table 3-14 Recommended indicators for a high-quality smart store network

                   Item                                          Recommended Indicator

            WLAN AP standard                                              Wi-Fi 7

                                             PCIe cards, containers, and built-in protocols (such as Bluetooth
              ESL deployment
                                                          and ZigBee) integrated into WLAN APs
                                              Intent-driven deployment through natural language interaction
            Service provisioning
                                                             and template-based deployment
                                              Unified management of LANs and WANs for multiple stores,
          O&M and management                 and three-dimensional visibility into applications, terminals, and
                                                                        networks
                                               Hyper-converged gateway, integrating functions such as 5G,
              Egress gateway
                                                         WAC, routing, security, and switching
                                                                                                                                                       Figure 3-25 Networking diagram

                                                                                                                  Technology Advantages and Benefits                         and automatic asset stocktaking are implemented
                                                                                                                  Wireless coverage without blind spots improves             on one network, improving efficiency by 90%.
                                                                                                                  customers' shopping experience. Fast store network         Local cloud-based deployment of the intelligent
                                                                                                                  configuration and service provisioning shortens the        O&M system enables visualized management and
                                                                                                                  deployment time of a single site to 30 minutes. ESL        improves O&M efficiency by 50%.




                                                       76                                                                                                               77
3.7 Hotel Industry                                                                                                 3.7.3         Smart Hotel

                                                                                                                  Smart hotels require a high-performance, full-          Hotel chains around the world bring new
                                                                                                                  coverage, and intelligent wireless net work             opportunities for hotel network deployment.
 3.7.1          Trends and Requirements                                                                           to provide stable and high-speed wireless               To quickly respond to market changes, shorten
                                                                                                                  connections in all areas, such as guest rooms,          the opening time, reduce costs, and improve
The hotel industry is transforming towards                  Network infrastructure is the cornerstone of          public areas, conference rooms, and restaurants.        operational flexibility, hotels hope to complete
digitalization, personalization, and sustainable            digital hotel transformation. It not only supports    Also, the network must provide high bandwidth           the opening and operations of new branches in a
development. As guests increasingly demand                  the stable running of all smart devices and           and low latency to support multiple heavy-              short time to adapt to rapid market changes and
personalized services, hotels are using smart               applications, but also increases operational          traffic applications, such as smart devices, IoT        fierce competition.
devices, mobile apps, AI-driven personalized                efficiency, improves guest experience, enhances       applications, video streams, mobile payment, and
recommendations, virtual assistants, and many               securit y, and facilitates data analy sis and         smart control systems. Moreover, the network            Hotels have increasing requirements for energy
other technologies to improve guest experience.             marketing. With the continuous development            must support high concurrent connections to             saving. They gradually adopt technologies such as
At the same time, the hotel industry is focusing            of technologies and the change of guest               ensure smooth running during peak hours.                energy-saving building design, renewable energy
on environmental protection and sustainable                 requirements, the reliability, scalability, and       This improves guest experience and increases            (such as solar energy and wind energy), dynamic
development, with green hotels and energy-saving            security of the hotel network infrastructure          operational efficiency.                                 network energy saving, intelligent lighting/air
buildings gradually becoming industry standards.            determine the competitiveness of hotels in the                                                                conditioning system optimization, and efficient
                                                            future market.                                                                                                water resource management to reduce energy
                                                                                                                                                                          consumption and operating costs.


 3.7.2          Recommended Networking Architecture

A typical hotel network uses various technologies           smooth and personalized experience when they
(such as IoT convergence and AI) to ensure                  use various devices and services. This supports the
high network speed and stability. Also, solutions           digital transformation of modern hotels, increases
featuring high security, flexibility, automation,           operational efficiency, and improves guest
and scalability are available to guests for enjoying        satisfaction.




                                                                                                                  Figure 3-27 Wi-Fi CSI-based precise personnel sensing, linked with building control systems to save energy




                            Figure 3-26 Recommended networking architecture


                                                       78                                                                                                            79
In the hotel industry, unauthorized filming tends         guest trust. Therefore, unauthorized filming has
                                                                                                               3.7.4          Cases
to be covert, miniaturized, and remote, which             become a security risk that the industry urgently
severely infringes on guest privacy. Worse yet, it        needs to address.
damages the hotel brand image and diminishes                                                                  3.7.4.1 A Hotel Chain in Italy
                                                                                                              Key Challenges and Requirements                             Key Technologies
                                                                                                              The hotel in Italy has recently renovated its rooms         Broadband access 10 Gbps: Smart roaming ensures
                                                                                                              and suites to provide a unique, excellent, and              hotel guests are always online. High-performance
                                                                                                              home-like experience for guests from all over the           APs are applicable to different scenarios and
                                                                                                              world. High-quality Wi-Fi is now essential for hotel        support concurrent access of hundreds of devices.
                                                                                                              guests, especially in luxury hotels where a high-
                                                                                                              performance, zero-blind-spot wireless network               Full-scope security: End-to-end security protection
                                                                                                              is a must. On one hand, the network provides                safeguards data assets of users and carriers.
                                                                                                              unparalleled Wi-Fi experience for hotel guests. They
                                                                                                              can easily get online for work and entertainment,           Autonomous network: The solution supports unified
                                                                                                              and stay connected with loved ones. Such seamless           LAN/WAN management, plug-and-play, template-
                                                                                                              connections improve the guests' satisfaction. On            based fast deployment, and unified O&M.
                                                                                                              the other hand, hotel staff can provide high-
                                                                                                              quality services for guests through this network,
                                                                                                              making everything from check-in to room service
                                                                                                              faster and more efficient. The network also boosts
                                                                                                              communication efficiency and strengthens overall
                                                                                                              hotel operations.




                                     Figure 3-28 Pervasive spy cameras




The following table lists the recommended indicators for a high-quality smart hotel network.


               Table 3-15 Recommended indicators for a high-quality smart hotel network

                   Item                                        Recommended Indicator

            WLAN AP standard                                             Wi-Fi 7

     Single-user maximum Wi-Fi rate                                   ≥ 3.5 Gbps

   Packet loss rate during Wi-Fi roaming                                 ≤ 0.1%                                                                     Figure 3-29 Networking diagram
                                            Dynamic energy saving of APs and energy saving based on Wi-Fi
              Energy saving
                                                                     CSI sensing                              Technology Advantages and Benefits                          10 Gbps hotel network reshaped the connectivity
                                             Intent-driven deployment through natural language interaction    The NMS manages network-wide devices in a                   experience, enabling guests and staff to adapt
           Service provisioning
                                                            and template-based deployment                     unified manner, ensuring network health. With               seamlessly to today's fast-paced, intelligent world.
                                             Unified management of multiple stores, and three-dimensional     software-defined networking (SDN), a traditional            It empowers the hotel to deliver an enhanced stay
         O&M and management
                                                   visibility into applications, terminals, and networks
                                                                                                              network can be upgraded to a future-oriented                experience and establish itself as a benchmark for
             Privacy security                   Unauthorized camera detection integrated in WLAN APs          network with just a few clicks. The high-quality            smart hotels in Europe.



                                                     80                                                                                                              81
3.8 Stadium Scenario


 3.8.1          Trends and Requirements

Digital intelligence integration: By deploying            core cornerstone for ensuring event operation,
IoT devices and sensors to build an omnipresent           spectator experience, and smart services. Targeting
interconnected net work , smart venues are                the extreme scenario of 100,000-level concurrent
achieving full digitalization through cross-              terminal access during large-scale events, the
system data collection and fusion. Meanwhile,             network must be equipped with millisecond-
the synergistic application of large-scale pre-           level fault self-healing capability to guarantee
trained models (large models) and customized              zero interruption of core services such as high-
vertical domain models (small models) drives the          definition event broadcasting and IoT device joint
transition toward "comprehensive intelligence."           control.

Convergence of physical and virtual realms:               Green and low carbon: Based on the highly
Smart stadiums transcend physical boundaries by           fluctuating crowd flow in the stadium, the campus
leveraging augmented reality (AR), virtual reality        network must possess tidal prediction and sensing
(VR), and digital twin technologies to create             capabilities. During non-event periods or low-
hybrid virtual-physical experiences. For example,         load hours, the system automatically switches to
AR navigation provides real-time seat information         deep sleep or shutdown mode through intelligent
and event updates, while VR enables immersive             algorithms. During high-concurrency event
virtual spectatorship.                                    periods, the system supports wake-up within
                                                          seconds, enabling refined energy consumption                                      Figure 3-30 Recommended networking architecture
Resilience- driven reliabilit y : As a scenario           control for the stadium network, and building an
characterized by high-density personnel gathering         energy-efficient, environmentally friendly "low-      Public network                                              Security network
and high- concurrenc y service bearing, the               carbon" smart stadium foundation.                     The public network provides access services for             The security network mainly provides access for
network reliability of the stadium serves as the                                                                spectators' mobile terminals. It primarily uses Wi-         cameras of the security management system. Since
                                                                                                                Fi and adopts a three-layer architecture of core,           the system generates large amounts of traffic and
                                                                                                                aggregation, and access layers.                             requires constant transmission, it is necessary to
                                                                                                                                                                            ensure sufficient bandwidth and reliability for the
                                                                                                                Office network                                              traffic.
 3.8.2          Recommended Networking Architecture                                                             The office network primarily carries the office data
                                                                                                                for the normal operation of the stadium, as well            Device network
Based on the actual service needs of the stadium,         and functions: public network, office network,        as unified work orders, stadium scheduling, smart           The device network primarily provides access
the entire network is planned and designed into           competition network, security network, and device     operations, and ordinary office scenarios, providing        services for systems such as the access control
five separate networks based on service types             network.                                              Internet access and communication functions                 system, alarm system, patrol system, parking
                                                                                                                for the stadium staff, while also offering access           management system, and some IoT devices, such
                                                                                                                services for remote branch offices of the stadium           as information display and building equipment
                                                                                                                operators.                                                  monitoring.

                                                                                                                Competition network
                                                                                                                The competition network mainly provides office
                                                                                                                access services for competition personnel and
                                                                                                                judges, and provides distribution services for
                                                                                                                competition videos.




                                                     82                                                                                                                83
 3.8.3           Smart Stadium

With the rapid development of technologies such              high-value fans, the network can be planned to
as multimedia live streaming, 4K video, IoT, and             classify these users as VIPs.
AI, there is a significant increase in service volume
and data traffic. A high-speed network can handle            As the number of terminal access devices
massive data traffic, thereby enabling the stadium           continues to grow and the network carries more
to effectively support these new technologies to             operations, managing the network becomes
ensure the advancement of information systems.               increasingly complex. Manageability is the key
The foundational network of the stadium supports             requirement for network operations. The smart
various operations, including live broadcasting of           stadium network solution provides efficient
matches, Internet access for spectators, security            tools and methods, such as network digital map,
monitoring, and daily office tasks for stadium staff.        protocol trace, access analysis, and network health
Reliability is the cornerstone of sustainable smart          monitoring, to improve network maintenance
stadium operations. The smart stadium network                efficiency and reduce daily maintenance costs for
architecture provides a three-level redundancy               the stadium network.
design to ensure the reliability of daily matches.
The first-level network redundancy design ensures            Meanwhile, as more network devices are deployed,
network reliability when network devices or links            especially APs for wireless coverage, the energy                                                  Figure 3-32 VIP box
fail. The second-level application experience                consumption cost accounts for an increasing
assurance design ensures experience protection               proportion of the total operating expenditure. The
for critical services, real-time monitoring of key           smart stadium network solution provides AI-based      The following table lists the recommended indicators for a high-quality smart stadium network.
application traffic and network quality, and rapid           tidal prediction and CSI sensing capabilities. By
fault detection and isolation. The third-level VIP           coordinating with systems such as lighting and air                  Table 3-16 Recommended indicators for a high-quality smart stadium network
user experience assurance design ensures that the            conditioning, and AP shutting down or AP sleep
VIP user experience is guaranteed in high-density            mode, the solution achieves energy saving and                             Item                                          Recommended Indicator
access scenarios through preferential access and             reduces energy consumption costs for the stadium          Bandwidth of access devices deployed
                                                                                                                                                                                             ≥ 10 Gbps
VIP bandwidth reservation technologies. For                  network.                                                                indoors
critical users, such as executives, celebrities, and
                                                                                                                                WLAN AP standard                                              Wi-Fi 7

                                                                                                                        Concurrent channels of 4K HD video
                                                                                                                                                                                           ≥ 60 channels
                                                                                                                                   conferencing
                                                                                                                      Definition and latency of key applications
                                                                                                                                                                                      1080p, latency < 100 ms
                                                                                                                    such as video conferencing and cloud desktop
                                                                                                                                                                                          Latency ≤ 50 ms
                                                                                                                           Service assurance for key users                           Bandwidth increase by 30%
                                                                                                                                                                                        Fault alarm function
                                                                                                                                   Wired security                                    AP-switch full-link MACsec

                                                                                                                                                                                           Tidal prediction
                                                                                                                                                                               Wake-up within seconds (WLAN AP)
                                                                                                                                   Energy saving
                                                                                                                                                                     CSI sensing for intelligent control of air conditioner and
                                                                                                                                                                                          lighting systems

                                                                                                                        Wi-Fi anti-eavesdropping (VIP room)           WPA3 + physical noise–based interference protection
                                                                                                                                                                     Unified management of multiple networks, and three-
                                                                                                                              O&M and management                     dimensional visibility into applications, terminals, and
                                                                                                                                                                                             networks




                                             Figure 3-31 Stadium


                                                        84                                                                                                             85
                                                                                                                   Technology Advantages and Benefits                        customers through differentiated assurance. The
 3.8.4          Cases
                                                                                                                   The 10 Gbps ultra-broadband network architecture          autonomous network O&M technology provides
                                                                                                                   with wired-wireless collaboration fully unleashes         three-layer full-dimension monitoring, fault
3.8.4.1 A Stadium in a Middle Eastern Country                                                                      the potential of high-bandwidth transmission,             detection within seconds, and fault locating within
                                                                                                                   meeting the requirements of the stadium for high-         minutes, significantly reducing manual O&M
Key Challenges and Requirements                            Deterministic experience: For core services such        density concurrent terminal access and ultra-             costs and improving network emergency response
The Middle Eastern country built a large stadium           as sports event broadcasting that require high          high-speed transmission of services such as ultra-        efficiency. The three approaches together build
for hosting major international sports events.             reliability, highly reliable end-to-end transmission    HD sports event broadcasting and VR immersive             a high-performance, high-reliability, and high-
The main stadium accommodates over 80,000                  channels are built based on determinist ic              watching. The deterministic experience assurance          intelligence technical foundation for the smart
spectators, complemented by additional facilities          application priority, bandwidth, and latency,           system for users and applications ensures zero            stadium, comprehensively supporting the upgrade
such as training centers for athletes. With service        ensuring ultra-HD video quality and low-latency         interruption of core sports event services and            of event operations and viewing experience.
requirements such as CCTV video backhaul, live             synchronization for broadcasting world-class            delivers dedicated, highly stable services to VIP
event broadcasting, and global fan Internet access,        international sports events and achieving zero
the stadium imposes stringent demands on both              freezing and zero packet loss. For high-value
wired and wireless network coverage. During major          customers in VIP boxes, differentiated services
events, high-density user access and concurrent            ensure deterministic user priorities and bandwidths
Internet usage create significant challenges for           to provide stable, smooth dedicated network access
network bandwidth, concurrency handling, and               experience, meeting communications requirements
overall network O&M. Broadcasting of international         in premium match watching scenarios.
sports events requires the network to provide real-
time performance, application assurance, and               Autonomous network: A full-dimension traffic
deterministic user experience to ensure reliability        and quality monitoring system is built to cover
for services and access of key customers.                  networks, service applications, and terminals.
                                                           The system supports automatic O&M capable of
Key Technologies                                           intelligent fault identification within seconds and
Broadband access 10 Gbps: On the wireless side,            precise fault locating within minutes, implementing
Wi-Fi 7 is used to provide high bandwidth, high            proactive awareness and fast handling of network
concurrency, and seamless roaming capabilities             exceptions. This provides solid technical support for
through collaboration among APs. On the wired              the continuous and stable broadcasting of large-
side, access switches support uplink bandwidth             scale sports events.
upgrade to 10GE for wired terminals and wireless
APs.




                                     Figure 3-33 Networking diagram


                                                      86                                                                                                                87
                 04
     Industry Prospects




88    89
4.1 AI Campus Service Prospects                                                                                     4.2 High-Quality 10 Gbps AI Campus
                                                                                                                        Technology Prospects
AI is profoundly reshaping the operation modes              In the government office field, AI reconstructs
of key fields such as office, education, healthcare,        service and decision-making modes. Intelligent          The future AI campus services will shape the campus network infrastructure with the following key
government, and manufacturing. These changes                approval and all-in-one network for public              technical focuses and beyond.
not only give rise to new application scenarios, but        services are supported, with AI replacing manual
also pose higher requirements on the underlying             review and cross-departmental data streamlining,
network infrastructure.                                     improving government office efficiency. In city
                                                            governance and emergency response, AI analyzes           4.2.1          Wi-Fi 8 and NearLink: New Era of Wireless Campus
In the enterprise office field, AI applications             traffic cameras to optimize traffic lights, monitors                    with Ultra-High Reliability
improve office efficiency. AI-powered document              vehicle congestion points, and automatically
processing automatically identifies the content             performs scheduling. AI also provides policy
of documents such as contracts and invoices,                simulation and precise services, including policy       With Wi-Fi 7 gradually put into commercial                  The application of NearLink technology in campus
enabling digital archiving and data analysis.               effect prediction based on public services data and     use, the next-generation standard Wi-Fi 8 (IEEE             LANs will open up a new paradigm of wireless
Automated meeting management automatically                  personalized service push based on AI profiles.         802.11bn) has entered the early development                 and intelligent networks. Its ultra-low latency,
schedules meet ings, generates agendas                                                                              phase and is expected to be standardized and                ultra-high reliability, and multi-node concurrency
and meeting minutes, and reduces manual                     In the manufacturing industry, AI drives flexible       productized in 2028. The standard shifts its focus          capabilities can support high-precision scenarios
coordination costs. Cross-language collaboration            production and full-chain collaboration. AI-based       from merely improving rates to providing ultra-             such as real-time control of industrial robotic
translates emails and conference content in real            dynamic production scheduling adapts to small-          high reliability (UHR), targeting problems with             arms and AGV cluster collaboration, replacing
time to support cross-country team collaboration.           batch customization, and the visual inspection          stability, latency, and spectral efficiency in high-        traditional industrial buses to achieve flexible
                                                            system identifies micron-level defects of products.     density access environments and laying a technical          deployment. In office scenarios, NearLink enables
In the education field, AI promotes personalized            In terms of predictive maintenance and supply           foundation for future immersive applications.               millisecond-level synchronous transmission for
and immersive learning. It can dynamically adjust           chain optimization, AI uses sensor data to warn of      Key technical directions of Wi-Fi 8 include multi-          8K holographic conference terminals and zero-
the content difficulty by analyzing learning                device faults and predicts material requirements to     device collaboration, mmWave application,                   freezing interconnection of smart terminals such
data and provide customized learning paths for              optimize the global supply chain. Digital twins are     spectrum optimization, low latency, enhanced                as AR glasses and sensors. In terms of technical
learners. AI can also quickly generate courseware,          used to build virtual models of production lines to     reliability, and AI-driven intelligent management.          features, NearLink provides an anti-interference
exercises, and multi-language teaching materials,           simulate process parameters, and AR glasses are         Based on Wi-Fi 7/8 mmWave technology and AI                 capability 10 times higher than Wi-Fi, and works
and create game-based, immersive learning                   used to provide remote maintenance guidance.            channel modeling, centimeter-level positioning              with the dynamic resource scheduling algorithm
scenarios with VR/AR. Another AI application in                                                                     and real-time perception of environmental status            to improve the campus network energy efficiency
the education field is intelligent assessment and           In the finance, retail, and hotel industries, AI will   (temperature, humidity, and crowd density) can              by more than 30%. Its native security architecture
learning management, which help assess learning             also be deeply integrated into industry applications    be achieved to realize human presence sensing,              supports E2E encryption and trusted access,
patterns in real time and generate risk profiles.           to improve efficiency or provide more high-quality      intelligent security protection, asset management,          providing a basis for wireless reconstruction in
                                                            and efficient capabilities, promoting the digital       and spatial management capabilities.                        high-security scenarios such as finance. With the
In the healthcare field, AI empowers precise                and intelligent development. In these fields, AI                                                                    in-depth integration of NearLink and AI, campus
diagnosis and treatment and resource                        campuses will pose new technical requirements on                                                                    networks will evolve towards touch-free intelligent
optimization. In medical imaging diagnosis and              the underlying network infrastructure.                                                                              connections, promoting the digital transformation
genetic analysis, AI identifies tiny lesions in CT/                                                                                                                             of industries such as manufacturing and
MRI images, accelerates genomic data matching,                                                                                                                                  healthcare.
and tailors cancer treatment solutions. AI-assisted
diagnosis and treatment provide clinical decision-
making support, and surgical robots implement
minimally invasive and precise operations.
AI is also used for public health monitoring
and resource scheduling, and can analyze
the pandemic data in real time to predict the
transmission path and optimize the allocation of
beds and medicines.




                                                       90                                                                                                                  91
                100GE Access and Simplified All-Optical Ethernet
                                                                                                                                                                                                      A
 4.2.2
                Architecture Dealing with Traffic Surges


                                                                                                                                                                       Acronyms and
Campus network bandwidth is continuously                   In the future, all-optical Ethernet will become the
upgraded. 2.5GE/10GE access will evolve to 100GE           norm, and APL/SPE technology will be adopted in
access in the future to meet the requirements              process industries such as the chemical industry.
of ultra-HD real-time services such as XR and
industrial vision.
                                                           The all-optical Ethernet solution "simplifies and
                                                           greens" the network architecture, saving 90%                                                                Abbreviations
                                                           of device space in ELV rooms and cutting per-bit
                                                           energy consumption by 70%.                                                        Table A-1 Acronyms and abbreviations

                                                                                                                   Acronym or Abbreviation                                Full Spelling
 4.2.3          E2E IPv6 Enhanced for Industry Empowerment
                                                                                                                            GDP                                       Gross Domestic Product

In the future, IPv6 Enhanced technology will               soft and hard pipes to implement hierarchical                     PC                                         Personal Computer
implement in-depth E2E convergence from LANs,              service assurance, and in-band flow measurement
                                                                                                                            SLA                                       Service Level Agreement
WANs, and to data center networks (DCNs). Based            technology helps locate cross-domain faults in
on Segment Routing over IPv6 (SRv6), network               real time, providing E2E slicing services with                   QAM                                   Quadrature Amplitude Modulation
slicing, in-band flow measurement and other core           low latency, high reliability, and data encryption
protocols and capabilities, IPv6 Enhanced will             for industries such as finance, government, and                   AP                                             Access Point
build a unified cross-domain intelligent IP network        healthcare. On the DCN side, IPv6 Enhanced
                                                                                                                             VR                                            Virtual Reality
architecture to support integrated services of             application identifiers can be used to ensure data
devices, networks, and clouds.                             quality, isolate data, and encrypt data.                          AR                                          Augmented Reality


On the LAN side, IPv6 Enhanced identifiers on              In addition, with carrier's specific business model               AI                                         Artificial Intelligence

application endpoints can be used to ensure                for enterprise private line services, this may foster
                                                                                                                             VIP                                       Very Important Person
application experience on campus networks. On              a win-win scenario for users, carriers, and cloud
the WAN side, carrier's SRv6 and network slicing           service providers.                                               SDN                                    Software-Defined Networking
technologies achieve the combined usage of
                                                                                                                            ETH                                     Extremely High Throughput

                                                                                                                            PPDU                                  Physical Layer Protocol Data Unit
 4.2.4          Campus Network Security: From "Passive Defense" to
                "Proactive Immunity"                                                                                         RU                                            Resource Unit

                                                                                                                            TCO                                       Total Cost of Operations

Campus network security will evolve towards                The proactive securit y brain shares threat
                                                                                                                            PHY                                         Port Physical Layer
"intelligent intrinsic securit y and dynamic               intelligence across the entire network and uses
immunity". The AI-based digital twin defense               self-healing AI algorithms to improve the defense                PoE                                         Power over Ethernet
system can implement real-time modeling and                response speed to nanosecond-level. Privacy-
                                                                                                                            WLAN                                    Wireless Local Area Network
prediction of network attacks. Quantum key                 preserving federated learning ensures that data
distribution technology will build a physically            in the campus is available but invisible. Mimic                  HTTP                                    Hypertext Transfer Protocol
unbreakable transmission encryption system. The            defense technology significantly reduces the risk
zero trust architecture combined with blockchain           of unknown vulnerabilities through the dynamic                   FTP                                        File Transfer Protocol
technology can dynamically verify device identities        heterogeneous redundancy architecture. The
                                                                                                                            QoS                                          Quality of Service
and trace behavior. The intelligent sensing                network autonomy system implements automatic
network supported by AI and NearLink can                   policy orchestration and elastic capacity expansion,             TSN                                      Time-Sensitive Networking
identify abnormal traffic in milliseconds.                 building a resilient security base with "breath-like
                                                           elasticity".                                                     RTT                                           Round Trip Time




                                                      92                                                                                                     93
Acronym or Abbreviation                  Full Spelling

         VoIP                      Voice over Internet Protocol

         SaaS                         Software as a Service

         POS                               Point of Sale

         AGV                        Automated Guided Vehicle

         EDCA              Enhanced Distributed Channel Access

         SRv6                      Segment Routing over IPv6

        VXLAN                  Virtual Extensible Local Area Network

         VLAN                       Virtual Local Area Network

         ZTP                         Zero Touch Provisioning

         DHCP                  Dynamic Host Configuration Protocol

         LAN                           Local Area Network

         WAN                           Wide Area Network

          API                   Application Programming Interface

          OUI                    Organizationally Unique Identifier

         LLDP                     Link Layer Discovery Protocol

         ONVIF                 Open Network Video Interface Forum

         WPA                         Wi-Fi Protected Access

         MIMO                     Multiple-input Multiple-output

         SZTP                    Secure Zero Touch Provisioning

          CSI                       Channel State Information

         ALS                        Automatic Laser Shutdown

         EEE                         Energy Efficient Ethernet

         RFID                     Radio Frequency IDentification

         MTTR                         Mean Time To Repair

         ZTNA                      Zero Trust Network Access




                          94                                           95
