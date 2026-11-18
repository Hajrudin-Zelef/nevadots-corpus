(Data Communication Series) Zhenbin Li, Zhibo Hu, Cheng Li - SRv6 Network Programming_ Ushering in a New .pdf
Ouvre dans une nouvelle fenêtreOuvre un site Web externeOuvre un site Web externe dans une nouvelle fenêtre
Ce site Web utilise des technologies telles que les cookies pour activer les fonctionnalités essentielles du site, ainsi que pour analyses, personnalisation et publicité ciblée. Pour en savoir plus, consultez le lien suivant : Politique de confidentialité
Préférences en matière de conservation des données
(Data Communication Series) Zhenbin Li, Zhibo Hu, Cheng Li - SRv6 Network Programming_ Ushering in a New .pdf
Description améliorée par l'IA
This document provides advance praise for the book "SRv6 Network Programming: Ushering in a New Era of IP Networks" from several experts in the field. They praise the book for providing a comprehensive overview of SRv6 including its background, fundamentals, benefits, and applications. They believe the book will help promote the deployment and development of SRv6.
(Data Communication Series) Zhenbin Li, Zhibo Hu, Cheng Li - SRv6 Network Programming_ Ushering in a New .pdf
2.
Advance Praise
“Tis bookprovides a comprehensive overview of SRv6, including its back-
ground, fundamentals, benefts, and applications. I frmly believe that the
publication of this book will further promote the large-scale deployment
and development of IPv6.”
— Hequan Wu, Academician of Chinese Academy of Engineering
“Te book SRv6 Network Programming: Ushering in a New Era of IP
Networks, written by Huawei’s Zhenbin Li and his team, provides a com-
plete description of SRv6 innovations and standards, and systematically
summarizes their experience in SRv6 R&D. It gives a holistic view into
SRv6. I sincerely hope that the publication of this book can positively
promote the development of IPv6 core technologies in China and make a
strong impact on the world.”
— Xing Li, Professor of Department of Electronic Engineering,
Tsinghua University
“Tis book takes a much-needed step-by-step approach to explaining
the SR technologies that apply to IPv6 networks. In doing so, it clarifes
and builds on the standardization work of the IETF’s SPRING working
group that has successfully brought together all of the large networking
equipment vendors with a number of future-looking network operators to
‘make the Internet work better.’ Te future of the Internet is notoriously
hard to predict, but this book will assist readers to embrace its potential.”
— Adrian Farrel, Former IETF Routing Area Director
“In the past 20+ years, MPLS played an important role in the development
of IP networks. As the development of 5G and cloud progresses, SRv6 is
3.
winning much attentionin the IP industry as the promising technology.
In the process, IP experts from Huawei make great contributions to the
innovation and standardization work in IETF. Te book written by the
team details the principles of SRv6 and corresponding applications for 5G
and cloud according to years of extensive experience. I believe that it can
be of much help to readers to master SRv6 and will facilitate the develop-
ment of SRv6 technology and industry.”
— Loa Andersson, MPLS WG Chair of IETF
“In this, Zhenbin Li and his team walk the reader through the history
leading to SRv6 and introduce the basic concepts as well as more advanced
ideas for the technology. Contributions for this book are written by
technologists who have been actively involved in the development and
standardization of SRv6 in the IETF from the beginning and have person-
ally helped to evolve not only the base technology but also many more
advanced features that are critical for successful service creation to meet
the demands placed upon today’s networks. Te reader will gain much
insight into how SRv6 has and continues to evolve, and in addition will
read about many new concepts that are actively being worked on in the
standards bodies.”
— James N. Guichard, SPRING WG Chair of IETF
“Tis book is a comprehensive introduction and tutorial to SRv6. It com-
prehensively covers all aspects from background to SRv6 principles and
basics, and to services built upon SRv6, such as trafc engineering, reac-
tions to network failures, VPN, and multicast. It concludes with a preview
of future evolution directions such as 5G and SRv6 header compression.
In addition to the technology, it includes personal refections on the back-
ground and reasons for the technical choices.”
— Bruno Decraene, Orange expert and senior network architect at
Orange/SPRING WG Co-chair of IETF
“SRv6 is maturing, and the family of standards defning all aspects of
the solution keeps growing — greatly supported by the authors’ endur-
ing eforts in the IETF. Our lab and other organizations have successfully
completed multi-vendor interoperability tests in the past three years, with
a growing number of participants confrming that the technology becomes
adopted. Te authors systematically and comprehensively describe the
4.
evolution and applicationof SRv6 as well as the benefts that it brings to
the industry. I truly believe that this book provides excellent value in guid-
ing SRv6 application and deployment.”
— Carsten Rossenhovel, Managing Director of EANTC (European
Advanced Networking Test Center)
Data Communication Series
CloudData Center Network Architectures and Technologies
Lei Zhang and Le Chen
Campus Network Architectures and Technologies
Ningguo Shen, Bin Yu, Mingxiang Huang, and Hailin Xu
Enterprise Wireless Local Area Network Architectures and Technologies
Rihai Wu, Xun Yang, Xia Zhou, and Yibo Wang
Sofware-Defned Wide Area Network Architectures and Technologies
Cheng Sheng, Jie Bai, and Qi Sun
SRv6 Network Programming
Ushering in a New Era of IP Networks
Zhenbin Li, Zhibo Hu, and Cheng Li
For more information on this series, please visit: https://www.routledge.com/
Data-Communication-Series/book-series/DCSHW
v
Contents
Foreword I, xxi
ForewordII, xxv
Preface, xxix
Teams, xxxv
Acknowledgments, xxxix
Authors, xli
PART I Introduction
CHAPTER 1 ◾ SRV6 Background 3
1.1 OVERVIEW OF INTERNET DEVELOPMENT 3
1.2 START OF ALL IP 1.0: A COMPLETE VICTORY FOR IP 4
1.2.1 Competition between ATM and IP 4
1.2.2 MPLS: Te Key to All IP 1.0 5
1.3 CHALLENGES FACING ALL IP 1.0: IP/MPLS DILEMMA 7
1.3.1 MPLS Dilemma 7
1.3.2 IPv4 Dilemma 9
1.3.3 Challenges for IPv6 10
1.4 OPPORTUNITIES FOR ALL IP 1.0: SDN AND
NETWORK PROGRAMMING 12
1.4.1 OpenFlow 14
1.4.2 POF 16
Contents ◾ xi
5.3.1Principles of L3VPN over SRv6 BE 170
5.3.1.1 Workfow of L3VPN over SRv6 BE
in the Control Plane 170
5.3.1.2 Workfow of L3VPN over SRv6 BE
in the Forwarding Plane 171
5.3.2 Principles of L3VPN over SRv6 TE 172
5.3.2.1 Workfow of L3VPN over SRv6 Policy
in the Control Plane 173
5.3.2.2 Workfow of L3VPN over SRv6 Policy
in the Forwarding Plane 175
5.4 SRv6 EVPN 176
5.4.1 Principles of EVPN E-LAN over SRv6 179
5.4.1.1 MAC Address Learning and
Unicast Forwarding 179
5.4.1.2 Replication List Establishment and
BUM Trafc Forwarding 182
5.4.2 Principles of EVPN E-Line over SRv6 186
5.4.3 Principles of EVPN L3VPN over SRv6 188
5.4.4 SRv6 EVPN Protocol Extensions 189
5.4.4.1 Ethernet A-D Route 189
5.4.4.2 MAC/IP Advertisement Route 191
5.4.4.3 IMET Route 193
5.4.4.4 ES Route 195
5.4.4.5 IP Prefx Route 195
5.5 STORIES BEHIND SRv6 DESIGN 197
REFERENCES 198
CHAPTER 6 ◾ SRv6 Reliability 201
6.1 IP FRR AND E2E PROTECTION 201
6.1.1 TI-LFA Protection 202
6.1.1.1 LFA 202
6.1.1.2 RLFA 204
17.
xii ◾ Contents
6.1.1.3TI-LFA 209
6.1.2 SRv6 Midpoint Protection 213
6.1.3 Egress Protection 217
6.1.3.1 Anycast FRR 218
6.1.3.2 Mirror Protection 219
6.2 MICROLOOP AVOIDANCE 223
6.2.1 Microloop Cause 223
6.2.2 SRv6 Local Microloop Avoidance in a Trafc
Switchover Scenario 225
6.2.3 SRv6 Microloop Avoidance in a Trafc Switchback
Scenario 227
6.2.4 SRv6 Remote Microloop Avoidance in a Trafc
Switchover Scenario 231
6.3 STORIES BEHIND SRv6 DESIGN 233
REFERENCES 234
CHAPTER 7 ◾ SRv6 Network Evolution 237
7.1 CHALLENGES FACED BY SRv6 NETWORK
EVOLUTION 237
7.1.1 Network Upgrade to Support IPv6 237
7.1.2 SRv6 Compatibility with Legacy Devices 238
7.1.3 Security Challenges Faced by SRv6 238
7.2 INCREMENTAL DEPLOYMENT FOR SRv6 NETWORKS 241
7.2.1 SRv6 Evolution Paths 242
7.2.2 SRv6 Deployment Process 243
7.2.3 SRv6 Evolution Practices 245
7.2.3.1 SRv6 Deployment Practice of China Telecom 245
7.2.3.2 SRv6 Deployment Practice of China Unicom 246
7.3 SRv6 COMPATIBILITY WITH LEGACY DEVICES 247
7.3.1 Using Binding SIDs to Reduce the Depth of the
SRv6 SID Stack 247
7.3.2 Applying FlowSpec to SRv6 248
Contents ◾ xv
9.2.4IFIT Encapsulation Mode 341
9.2.5 IFIT for SRv6 342
9.2.5.1 Passport Mode 342
9.2.5.2 Postcard Mode 344
9.2.5.3 SRv6 IFIT Encapsulation 346
9.3 STORIES BEHIND SRv6 DESIGN 349
REFERENCES 352
CHAPTER 10 ◾ SRv6 for 5G 355
10.1 5G NETWORK EVOLUTION 355
10.2 SRv6 FOR NETWORK SLICING 360
10.2.1 5G Network Slicing 360
10.2.2 Transport Network Slicing Architecture 363
10.2.3 SRv6-based Network Slicing 366
10.2.4 Network Slice Scalability 373
10.2.4.1 Control Plane Scalability of Network Slices 373
10.2.4.2 Data Plane Scalability of Network Slices 377
10.3 SRv6 FOR DETNET 379
10.3.1 5G uRLLC 379
10.3.1.1 Smart Grid 379
10.3.1.2 IoV 380
10.3.1.3 Cloud VR 381
10.3.2 DetNet Fundamentals 381
10.3.2.1 Redundancy Protection Fundamentals 383
10.3.2.2 Deterministic Latency Fundamentals 384
10.3.3 SRv6-based DetNet 386
10.3.3.1 SRv6-Based Redundancy Protection 386
10.3.3.2 SRv6-Based Deterministic Latency 389
10.4 SRv6 FOR 5G MOBILE NETWORKS 391
10.4.1 5G Mobile Network Architecture 391
10.4.2 SRv6 Deployment on a 5G Mobile Network 393
21.
xvi ◾ Contents
10.4.3Key SRv6 Functions on a 5G Mobile Network 395
10.4.3.1 Args.Mob.Session 395
10.4.3.2 End.M.GTP6.D/End.M.GTP6.E 396
10.4.3.3 End.M.GTP4.E 397
10.4.3.4 End.Limit SID 397
10.4.4 Control Plane of a 5G Mobile Network 398
10.5 STORIES BEHIND SRv6 DESIGN 399
REFERENCES 402
CHAPTER 11: SRv6 for Cloud Services 405
11.1 SRv6 FOR TELCO CLOUD 405
11.1.1 Telco Cloud Overview 405
11.1.2 Telco Cloud Transport Network Architecture 409
11.1.3 Edge Telco Cloud Architecture 413
11.1.3.1 NAAF Physical Architecture 413
11.1.3.2 NAAF Transmission Protocols and Key
Technologies 414
11.2 SRv6 FOR SFC 416
11.2.1 SFC Overview 416
11.2.1.1 Transport Network Gi-LAN 417
11.2.1.2 Multi-tenant Data Center 418
11.2.2 PBR/NSH-based SFC 418
11.2.3 SRv6-based SFC 423
11.2.3.1 Stateless SRv6 SFC 424
11.2.3.2 Stateful SRv6 SFC 428
11.3 SRv6 FOR SD-WAN 431
11.3.1 SD-WAN Overview 431
11.3.2 SRv6-based SD-WAN 434
11.3.2.1 Basic SRv6 SD-WAN Networking 434
11.3.2.2 Control Plane Processing
Flow onthe SRv6 SD-WAN 436
11.3.2.3 SRv6 SD-WAN Data Forwarding Process 437
22.
Contents ◾ xvii
11.3.2.4Advantages of the SRv6 SD-WAN Solution 440
11.4 STORIES BEHIND SRv6 DESIGN 441
REFERENCES 442
CHAPTER 12 ◾ SRv6 Multicast/BIERv6 445
12.1 OVERVIEW OF MULTICAST TECHNOLOGIES AND
SERVICES 445
12.1.1 PIM 446
12.1.2 MVPN 449
12.1.2.1 MVPN Routes 450
12.1.2.2 MVPN over an mLDP P2MP Tunnel 453
12.1.2.3 MVPN over an RSVP-TE P2MP Tunnel 454
12.1.2.4 Advantages of MVPN 456
12.1.2.5 Disadvantages of MVPN 457
12.1.3 BIER 458
12.1.3.1 BIER Working Group Anecdotes 460
12.2 BIER MULTICAST TECHNOLOGY 460
12.2.1 Basic Concepts of BIER 460
12.2.2 BIER Architecture 462
12.2.2.1 Routing Underlay 462
12.2.2.2 BIER Layer 463
12.2.2.3 Multicast Flow Overlay 463
12.2.3 BIER Forwarding Fundamentals 464
12.2.3.1 BFR-ID Confguration 464
12.2.3.2 BIFT Introduction 464
12.2.3.3 BIFT Establishment 465
12.2.3.4 BIFT Lookup and Packet Forwarding 466
12.2.4 BIER Data Plane 467
12.2.5 BIER Control Plane 470
12.2.6 MVPN over BIER 473
12.2.7 Characteristics of BIER 477
12.3 BIERv6 MULTICAST TECHNOLOGY 478
23.
xviii ◾ Contents
12.3.1Proposal and Design of BIERv6 478
12.3.2 BIERv6 Fundamentals 480
12.3.3 BIERv6 Control Plane 483
12.3.4 MVPN over BIERv6 486
12.3.5 GTM over BIERv6 488
12.3.6 Inter-AS Multicast over BIERv6 490
12.3.7 Characteristics of BIERv6 492
12.4 OTHER SRv6 MULTICAST TECHNOLOGIES 492
12.4.1 P2MP Multicast Forwarding Path Establishment
Using the Tree SID Solution 492
12.4.2 Multicast Packet Forwarding Using the Tree SID
Solution 494
12.4.3 Characteristics of the Tree SID Solution 494
12.5 STORIES BEHIND SRv6 DESIGN 495
REFERENCES 496
PART IV Summary and Future Developments
CHAPTER 13 ◾ SRv6 Industry and Future 501
13.1 SRv6 INDUSTRY PROGRESS 501
13.1.1 SRv6 Standardization Progress 501
13.1.2 SRv6-Related Products 503
13.1.3 SRv6 Interoperability Test 503
13.1.3.1 SRv6 Interoperability Test by the EANTC 503
13.1.3.2 SRv6 Interoperability Test by China’s
Expert Committee of Promoting Large-
Scale IPv6 Deployment 504
13.1.4 Commercial SRv6 Deployment 504
13.1.5 SRv6 Industrial Activities 505
13.2 SRv6 EXTENSION HEADER COMPRESSION 506
13.2.1 Impact of the SRv6 Extension Header Length 506
13.2.2 SRv6 Extension Header Compression Solutions 507
24.
Contents ◾ xix
13.2.2.1G-SRv6 508
13.2.2.2 uSID 515
13.2.2.3 SRm6 520
13.2.3 Comparison between SRv6 Extension Header
Compression Solutions 522
13.2.4 Future of SRv6 Extension Header Compression 526
13.3 APN6 527
13.3.1 Why APN6 527
13.3.1.1 Current Carriers’ Pain Points
and Requirements 527
13.3.1.2 Challenges Faced by Traditional
Diferentiated Service Solutions 528
13.3.1.3 APN6 Solution 529
13.3.2 APN6 Framework 531
13.3.3 APN6 Framework Requirements 533
13.3.3.1 Requirements for Carrying Application
Characteristic Information 533
13.3.3.2 Requirements for Processing
Application-Aware Information 535
13.3.3.3 Security Requirements 537
13.3.4 Future of APN6 537
13.4 FROM SRv6 TO IPv6+ 537
13.5 STORIES BEHIND SRv6 DESIGN 539
13.5.1 Design of SRv6 Compression: From C-SRH to
G-SRH 539
13.5.2 Design Principle: Internet and Limited Domains 540
13.5.3 Design Principle: Te Greatest Truths Are the
Simplest 541
REFERENCES 542
POSTFACE: SRv6 PATH, 545
APPENDIX A: IPv6 BASICS, 557
25.
xx ◾ Contents
APPENDIXB: IS-IS TLVs, 591
APPENDIX C: OSPFv3 TLVs, 595
APPENDIX D: ACRONYMS AND ABBREVIATIONS, 601
26.
xxi
Foreword I
Defining andintroducing a novel technology not only requires the
ability to address and solve use cases or critical problems, but must
also come at the right moment. Innovation always requires the right tim-
ing: you need to come with the right answer at the right moment. Timing
is critical for the success of any innovation. Tere are so many examples
of brilliant and innovative technologies that came too early or too late and
that disappeared without leaving any trace.
It’s now been more than four decades since the Internet was invented.
Te IP underlying technology is a powerful, fexible, and scalable con-
nectionless protocol on top of which we have multiple transport protocols
leveraged by application protocols that allowed the web to become what
we have today. IP demonstrated a very strong scalability, fexibility, and
efciency. A simple datagram service based on a connectionless model
that leverages a shortest-path tree routing paradigm is the foundation of
all communications today.
IPv6 was invented more than 25 years ago. It was the time when we
thought that the only major problem to solve was the exhaustion of IPv4
addresses, and extending the address space to 128 bits would have solved
the problem for a long time. In theory, everything worked well, but in
practice, the industry addressed and did overcome the problem with a
workaround: NAT and later Carrier-Grade NAT. So, for many years, IPv6
remained on the side.
A few years later (around the end of the 20th century), MPLS was
invented and introduced an interesting and powerful mechanism:
separation of control and data planes. Tanks to MPLS, we’ve been able to
override routing decisions based on policies and labels. Tis was the time
we implemented Trafc Engineering, Fast Reroute, and diferent favors
of VPNs.
27.
xxii ◾ ForewordI
Te separation of control and data planes opened a brand-new era.
From there on, the ability to implement specifc policies for diferent ser-
vices and data fows became more and more a necessity for most network
operators. Te era of SDN started. However, the way policies were imple-
mented using MPLS required state in the network and we arrived at a
scalability boundary.
Segment Routing (RFC8402) fnally brought a powerful way to intro-
duce source routing into modern networks running either MPLS or IPv6
data plane. Why suddenly has source routing become so popular? Because
of timing! Now is the time when you want to control data fows with-
out incurring heavy scale state machinery (that would not work anyway).
Source routing brings the state in the packet and leaves the control plane
of the network very light and very easy to manage.
Now the times are mature for the introduction of source routing but
revisited with a modern technology that copes with both MPLS and IPv6
data planes. In addition, Segment Routing applied to IPv6 data plane
(SRv6) allows the leveraging of the two main strong points of IPv6 archi-
tecture. First, SRv6 leverages the IPv6 128 bit address space that allows the
defnition of a node with enough bits so that an augmented semantic can
be defned (e.g., a node identity and a node location). Second, IPv6 defnes
the concept of Extension Headers (EH) which can be added to the packet
for diferent purposes. One of these purposes is source routing, and espe-
cially with the Segment Routing Header (SRH, RFC8754), you can now
specify the path the packet should follow.
Te innovation SRv6 brought to the industry has multiple aspects:
it allows the implementation of a straightforward, highly scalable, back-
ward compatible mechanism for source routing. Tis allows the imple-
mentation of large-scale policies addressing the variety of use cases
network operators have to address these days (5G, IoT, Connected Objects,
Virtualization, etc.). Another advantage is the ability to express not only
a path (that the packet should take) but also the process a packet should
go through during its journey. A typical example is the Service Chain use
case where a packet has to traverse service application elements before
reaching its destination.
SRv6 introduces the ability to combine forwarding decisions with
application/processing decisions, and this is called SRv6 Network
Programming. Finally, and this is the timing factor, we are able to lever-
age a mechanism that was invented many years ago (source routing, IPv6)
28.
Foreword I ◾xxiii
and combine it with modern capabilities, allowing to address current use
cases and requirements and taking into account the scale and perfor-
mance dimensions.
SR and SRv6 began in 2012 and 2013, and at that time, we were a very
small team. Rapidly, the industry understood the potential, and large col-
laboration between vendors and operators took place. Te standardization
of SR and SRv6 took place in IETF, and now we have a large community
participating in the SR and SRv6 efort.
Zhenbin Li and the Huawei team he leads are part of this community,
and their contribution to the evolution and standardization of SR tech-
nologies became substantial over the last years. Huawei SRv6 team has
been instrumental in the way SRv6 has been defned and standardized in
IETF, adopted by the industry and still extended to cope with increasing
and ever-emerging new requirements. Also, at the time of this writing,
Zhenbin Li is also a member of IAB.
Tis book gives an exhaustive view of the SRv6 technology including
the network programming capability, and I’m sure it will help the reader
to understand SRv6 technology, the use cases it applies to, and eventually,
to appreciate SRv6 technology as much as I have appreciated participating
in its invention.
Stefano Previdi
SR-MPLS and SRv6 Pioneer and Original Contributor
30.
xxv
Foreword II
The InternetProtocol version 6 (IPv6) was designed by the IPv6
Task Force within the IETF under the co-chairmanship of Steve
Deering and Robert Hinden and a small group of 40 engineers. Steve is
also the designer of Multicast. Te last draf standard of IPv6 (RFC 2460)
was released in December 1998, winning against IPv7, IPv8, and IPv9 pro-
posals. Tis release was basically the last efort of the IPv6 Task Force as
it had to close down its working group at the February 2–5, 1999, meeting
in Grenoble. In this meeting, I proposed the formation of the IPv6 Forum
to promote IPv6 to industry, ISPs/ MNOs, governments, academia, and
research ecosystem to start large-scale pilots and initial deployments.
Tere was a nice coincidence with the creation of 3GPP in December 1998
within ETSI in Sophia Antipolis. It was quite clear to the core IPv6 team
that 3G is the frst innovative driver that needs plenty of IP addresses. I
contacted Karl-Heinz Rosenbruck, Director General of ETSI and at that
time he was the chairman of 3GPP and proposed to him to adopt IPv6
for 3G instead of WAP. Afer 6 months of discussions with the various
3GPP WGs, 3GPP announced in May 10, 2000, the adoption of IPv6 for
3G. However, the 3G MNOs did not have the capacity building and the
skills to adopt IPv6 but preferred to use IPv4 and especially the Network
Address Translation (NAT) for 3G. Tis was in itself a great achievement
to get the wireless world to adopt the Internet for the telecom world as they
were at that time not that enthusiastic about the open Internet. Te client/
server model became the norm and the wireless Internet especially with
4G boomed in our hands and the rest is history. Te Internet has reached
5 billion users with a kind of economy class service.
Te public IPv4 address space managed by IANA (http://www.iana.org)
was completely depleted back in February 1, 2011. Tis creates by itself a
critical challenge when adding new IoT networks and enabling machine
31.
xxvi ◾ ForewordII
learning services on the Internet. Without publicly routable IP addressing,
the Internet of Tings, and anything that’s part of Machine Learning ser-
vices on the Internet, would be greatly reduced in its capabilities and then
limited in its potential success. Most discussions of IP over everything
have been based on the illusionary assumption that the IP address space is
an unlimited resource or it’s even taken for granted that IP is like oxygen
produced for free by nature.
Te introduction of IPv6 provides enhanced features that were not
tightly designed or scalable in IPv4 like IP mobility, end-to-end (e2e) con-
nectivity, ad hoc services, etc. IPv6 will be addressing the extreme sce-
narios where IP becomes a commodity service. Tis new address platform
will enable lower cost network deployment of large-scale sensor networks,
RFID, IP in the car, to any imaginable scenario where networking adds
value to commodity.
IPv6 deployment is now in full swing with some countries, such as
Belgium, achieving over 50% penetration. India has taken the lead by hav-
ing over 350 million IPv6 users. China has over 200 million IPv6 users
while the US has over 100 million users using IPv6 without the users even
knowing it.
Tere are many infections happening this decade to infuence the
design of the frst tangible IoT, 5G, and Smart Cities. It will take a com-
bination of IoT, SDN-NFV, Cloud Computing, Edge Computing, Big IoT
Data, and 5G, to sif through to realize the paradigm shif from current
research-based work to advanced IoT, 5G, and Smart Cities.
However, the move to NAT has basically killed the end-to-end model
that IPv6 restored so that applications and devices can peer directly with
each other. Te IPv6 deployment has continued its growth and reached
today over 1 billion IPv6 users. Tese users can be qualifed as business
class benefciaries. Te restoration of the end-to-end model has created
new opportunities for designing new end-to-end protocols such as IPv6-
based Segment (SRv6) which uses the prime and clean state design of IPv6
of end-to-end services. SRv6 is the frst big attempt to usher in the era of
IPv6 innovation, and boasts the following two key characteristics:
• SRv6 is an IPv6-native network technology designed to simplify
the transport network. In this era where IPv4, IPv6, MPLS, and
SR-MPLS coexist, SRv6 will achieve optimal simplifcation at the
transport layer under the minimalism and centralization guiding
32.
Foreword II ◾xxvii
principles. Te real benefts of SRv6 include the support of VPN,
FRR, TE, network slicing, and BIERv6, enabling application-driven
path programming, providing diferentiated SLA assurance for
users, and helping carriers transform from “ofering bandwidth” to
“ofering services.”
• SRv6 is a rare project that the IETF has focused on over the past
decade. It provides a programmable network architecture for 5G and
cloud transport, while also heralding the era of IPv6+ network sys-
tem innovation. Datacom industry players, such as Huawei, Cisco,
China Telecom, China Mobile, China Unicom, and SofBank, have
invested a tremendous amount of efort into jointly promoting SRv6
maturity, and the SRv6 transport solution is being applied to an
increasing number of networks.
Huawei is the frst in the industry to launch SRv6-capable data communi-
cation products. In the initial phase of SRv6’s history, the company actively
participated in innovation and standardization, and achieved a plethora
of signifcant milestones. To add to that, Zhenbin Li was elected as an
IETF Internet Architecture Board (IAB) member in early 2019. Tis book
is a summary of the SRv6 research conducted by him and his team, as well
as their corresponding experiences. Te book systematically interprets the
innovative technologies and standards of SRv6, especially with regard to
SRv6 implementation practices, which is not only inspiring and helpful
but also benefcial to the continuous promotion of IPv6 innovation.
Latif Ladid
Founder and President, IPv6 Forum; Member of 3GPP PCG (MRP),
Co-chair, IEEE FNI 5G, World Forum/5G Summits.
Chair, ETSI IPv6 Integration ISG.
34.
xxix
Preface
Segment Routing overIPv6 (SRv6) is an emerging IP technology.
Te development of 5G and cloud services creates many new require-
ments on network service deployment and automated O&M. SRv6 pro-
vides comprehensive network programming capabilities to better meet
the requirements of new network services and is compatible with IPv6 to
simplify network service deployment.
IP transport networks are built around connections. 5G changes the
attributes of connections, and cloud changes their scope. Tese changes
bring big opportunities for SRv6 development. Te development of 5G ser-
vices poses higher requirements on network connections, such as stronger
Service Level Agreement (SLA) guarantee and deterministic latency. As
such, packets need to carry more information. Tese requirements can
be well met through SRv6 extensions. Te development of cloud services
makes service processing locations more fexible. Some cloud services
(such as telco cloud) further break down the boundary between physical
and virtual network devices, integrating services and transport networks.
All of this changes the scope of network connections. Te unifed pro-
gramming capability of SRv6 in service and transport, as well as the native
IP attribute, enables the rapid establishment of connections and meets the
requirements for fexible adjustment of the connection scope. As men-
tioned in this book, SRv6 network programming has ushered in a new
network era, redefning the development of IP technologies signifcantly.
Technical experts from Huawei Data Communication Product Line
have been conducting long-term and in-depth research and development
in IP and SRv6 felds, and contributing to numerous SRv6 standards in
the IETF. In addition, many experts have participated in SRv6 network
deployments, accumulating extensive experience in the feld. We compiled
this book based on comprehensive research, development, and network
35.
xxx ◾ Preface
O&Mexperience to provide a complete overview of SRv6 in the hope of
better understanding its fundamentals and the new network technologies
that derive from it. We also hope it will spark participation in the research,
application, and deployment of these new technologies, promoting the
development of communications networks.
OVERVIEW
Tis book begins with the challenges services face regarding IP technol-
ogy development. It describes the background and mission of SRv6 and
presents a comprehensive overview of the technical principles, service
applications, planning and design, network deployment, and industry
development of next-generation IP networks. Tis book consists of 13
chapters and is divided into four parts:
Part I: Introduction
Chapter 1 focuses on the development of IP technologies and reveals
why SRv6 is developing so quickly.
Part II: SRv6 1.0
Chapters 2 through 8 introduce SRv6 1.0, covering basic SRv6 capa-
bilities and demonstrating how SRv6 supports existing services in a
simple and efcient manner.
Part III: SRv6 2.0
Chapters 9 through 12 introduce SRv6 2.0, covering new SRv6-based
network technologies targeted at 5G and cloud services and dem-
onstrating service model innovation brought by SRv6 network
programming.
Part IV: Summary and Future Developments
Chapter 13 summarizes the development of the SRv6 industry and
forecasts developmental trends from SRv6 to IPv6+, which stretches
people’s imagination about future networks.
CHAPTER 1: SRv6 BACKGROUND
Tis chapter comprehensively describes the development of IP technolo-
gies, introduces SRv6 based on historical experience and requirements for
36.
Preface ◾ xxxi
IPtechnology development, and summarizes the value and signifcance of
SRv6 from a macro perspective.
CHAPTER 2: SRv6 FUNDAMENTALS
Tis chapter describes SRv6 fundamentals to demonstrate how SRv6 is used
to implement network programming, and summarizes the advantages of
SRv6 network programming and its mission from a micro perspective.
CHAPTER 3: BASIC PROTOCOLS FOR SRv6
Tis chapter describes the fundamentals of and extensions to Intermediate
System to Intermediate System (IS-IS) and Open Shortest Path First version 3
(OSPFv3) for SRv6. Protocols such as Resource Reservation Protocol-Trafc
Engineering (RSVP-TE) and Label Distribution Protocol (LDP) are not
required on an SRv6 network, thereby simplifying the network control plane.
CHAPTER 4: SRv6 TE
Tis chapter describes the fundamentals and extensions of SRv6 Trafc
Engineering (TE), which is a basic feature of SRv6. SRv6 Policy is the main
mechanism for implementing SRv6 trafc engineering and draws on the
source routing mechanism of Segment Routing to encapsulate an ordered
list of instructions on the headend, guiding packets through the network.
CHAPTER 5: SRv6 VPN
Tis chapter describes the principles and protocol extensions of Virtual
Private Network (VPN), a basic SRv6 feature. SRv6 supports existing
Layer 2 VPN (L2VPN), Layer 3 VPN (L3VPN), and Ethernet VPN (EVPN)
services, which can be deployed as long as edge nodes are upgraded to
support SRv6, shortening the VPN service provisioning time.
CHAPTER 6: SRv6 RELIABILITY
Tis chapter describes the fundamentals and protocol extensions of SRv6
reliability technologies, including FRR, TI-LFA, midpoint protection,
egress protection, and microloop avoidance. Tese technologies ensure
E2E local protection switching within 50 ms on an SRv6 network.
CHAPTER 7: SRv6 NETWORK EVOLUTION
Tis chapter describes the challenges and technical solutions of SRv6 net-
work evolution, that is, how to evolve an existing IP/MPLS network to
37.
xxxii ◾ Preface
anSRv6 network. SRv6 supports incremental deployment, which protects
existing investments and allows for new services.
CHAPTER 8: SRv6 NETWORK DEPLOYMENT
Tis chapter describes SRv6 network deployment, including SRv6 applica-
tion scenarios and how to design and confgure features such as TE, VPN,
and reliability. Network deployment practices show that SRv6 is advanta-
geous over traditional solutions in terms of inter-AS, scalability, protocol
simplifcation, and incremental deployment.
CHAPTER 9: SRv6 OAM AND ON-PATH
NETWORK TELEMETRY
Tis chapter describes the fundamentals and protocol extensions of SRv6
OAM and on-path network telemetry. SRv6 OAM and on-path network
telemetry provide a solid basis for SRv6 network quality guarantee, allow-
ing for extensive use of SRv6 on carrier networks.
CHAPTER 10: SRv6 FOR 5G
Tis chapter describes new SRv6 technologies applicable to 5G services,
including VPN+ network slicing, Deterministic Networking (DetNet),
and SRv6-to-mobile core solutions and relevant protocol extensions.
CHAPTER 11: SRv6 FOR CLOUD SERVICES
Tis chapter describes the application of SRv6 in cloud services, including
the concept, challenges, and SRv6-based solution for telco cloud as well as
the fundamentals and protocol extensions of SRv6 for SFC and SRv6 for
SD-WAN.
CHAPTER 12: SRv6 MULTICAST/BIERv6
Tis chapter describes SRv6-based multicast technologies, focusing on the
fundamentals and protocol extensions of Bit Index Explicit Replication
(BIER) and Bit Index Explicit Replication IPv6 Encapsulation (BIERv6).
Working with SRv6, BIERv6 allows both unicast and multicast services to
be transmitted on a unifed IPv6 data plane based on an explicit forward-
ing path programmed on the ingress.
CHAPTER 13: SRv6 INDUSTRY AND FUTURE
Tis chapter summarizes the development of the SRv6 industry and
provides forecasts for its future, focusing on SRv6 extension header
38.
Preface ◾ xxxiii
compression,Application-aware IPv6 Networking (APN6), and the three
phases that may exist during its development from SRv6 to IPv6+.
As SRv6 involves a signifcant number of IPv6 basics, this book
expands on them in Appendix A to help better understand SRv6. In addi-
tion, this book describes IS-IS and OSPFv3 TLVs in Appendices B and
C, respectively, as SRv6 is implemented based on these IS-IS and OSPFv3
extensions. Tis will help to demonstrate how SRv6 is implemented as
well as the relationships between IS-IS and SRv6, and between OSPFv3
and SRv6.
In the Postface “SRv6 Path,” Zhenbin Li draws on his personal
experience to summarize the development history of SRv6 and Huawei’s
participation in promoting SRv6 innovation and standardization. In addi-
tion, “Stories behind SRv6 Design” is provided at the end of each chapter,
touching on the experience and philosophy behind the protocol design,
interpreting its technical nature, or describing the experience and lessons
learned during the development of technologies and standards. Trough
this content, it is hoped that readers can obtain a greater understanding of
SRv6 and the reasoning behind its design principles. Some of the content
constitutes the author’s opinion and should be used for reference only.
Te editorial board for this book gathered the technical experts from
Huawei’s data communication research team, standards and patents team,
protocol development team, solution team, technical documentation
team, and translation team. Team members include the developers and
promoters of SRv6 standards, R&D staf on SRv6 design and implemen-
tation, and solution experts who helped customers successfully complete
SRv6 network design and deployment. Teir achievements and experi-
ence are systematically summed up in this book. Information department
staf, including Lanjun Luo, edited the text and worked on the fgures.
Te publication of this book is the collaborative result of team efort and
collective wisdom. Heartfelt thanks go to every member of the editorial
board. Tough the process is hard, we really enjoyed the teamwork in
which everyone learned so much from each other.
40.
xxxv
Teams
TECHNICAL COMMITTEE
Chair
Kewen Hu(Kevin Hu), President of Huawei Data Communication Product
Line
Members
Shaowei Liu, Director of Huawei Data Communication R&D Mgmt Dept
Zhipeng Zhao, Director of Huawei Data Communication Marketing Dept
Chenxi Wang, Director of Huawei Data Communication Strategy &
Business Development Dept
Wei Hu, Director of Huawei Carrier IP Marketing & Solution Sales Dept
Jinzhu Chen, President of Service Router Domain, Huawei Data
Communication Product Line
Meng Zuo, President of Core Router Domain, Huawei Data
Communication Product Line
Banghua Chen, Director of Huawei Data Communication Marketing
Execution Dept
Suning Ye, Director of Router PDU, Huawei Data Communication
Product Line
Xiao Qian, Director of Huawei Data Communication Research Dept
Jianbing Wang, Director of Huawei Data Communication Architecture &
Design Dept
41.
xxxvi ◾ Teams
ZhaokunDing, Director of Huawei Data Communication Protocol
Development Dept
Minwei Jin, Director of IP Technology Research Dept, NW, Huawei Data
Communication Product Line
Dawei Fan, Director of Huawei Data Communication Standard & Patent
Dept
Jianping Sun, Director of Huawei Data Communication Solutions Dept
Mingzhen Xie, Director of Huawei Information Digitalization and
Experience Assurance (IDEA) Dept, DC
EDITORIAL BOARD
Editor-in-Chief: Zhenbin Li (Robin Li)
Deputy Editors-in-Chief: Zhibo Hu, Cheng Li
Members: Lanjun Luo, Ting Liao, Shunwan Zhuang, Haibo Wang, Huizhi
Wen, Yaqun Xiao, Guoyi Chen, Tianran Zhou, Jie Dong, Xuesong Geng,
Shuping Peng, Lei Li, Jingrong Xie, Jianwei Mao
Technical Reviewers: Rui Gu, Gang Yan
Translators: Yanqing Zhao, Wei Li, Xue Zhou, Yufu Yang, Zhaodi Zhang,
Huan Liu, Yadong Deng, Chen Jiang, Ruijuan Li, Junjie Guan, Jun Peng,
Samuel Luke Winfeld-D'Arcy, George Fahy, Rene Okech, Evan Reeves
TECHNICAL REVIEWERS
Rui Gu: Chief Solution Architect of Huawei Data Communication
Product Line. Mr. Gu joined Huawei in 2007 and now leads the Data
Communication Solutions Design Dept. He has extensive experience
working in the Versatile Routing Platform (VRP) department and has
conducted in-depth research on IP/MPLS protocols. He once led the R&D
team of Huawei’s next-generation backbone routers and also has a wealth
of experience in implementing end-to-end data communication products
and solutions. He worked in Europe from 2012 to 2017, led the develop-
ment and innovation of data communication solutions across Europe,
and took the lead in the solution design of numerous data communication
projects for leading European carriers.
42.
Teams ◾ xxxvii
GangYan: Chief IGP expert of Huawei Data Communication Product
Line. Mr. Yan joined Huawei in 2000 and has been working in the VRP
department. From 2000 to 2010, he was responsible for the architecture
design of the VRP IGP sub-system and completed the design and delivery
of fast convergence, Loop-Free Alternate (LFA) Fast Reroute (FRR)/multi-
source FRR, and Non-Stop Routing (NSR). He has extensive experience
in protocol design, and since 2011, he has been responsible for improving
the O&M capabilities of VRP protocols, building the VRP compatibility
management system, and building and delivering the YANG model base-
line. He is currently leading the team to innovate IP technologies, includ-
ing SRv6 slicing, E2E 50 ms protection, and Bit Index Explicit Replication
(BIER)-related protocols.
44.
xxxix
Acknowledgments
While promoting theinnovation of SRv6 standards, numer-
ous people have contributed heavily throughout the process from
the initial concept to actual products and solutions, including strategic
decision-making, technical research, standards promotion, product devel-
opment, commercial deployment, and industry ecosystem construction.
We have received extensive support and help from both inside and outside
Huawei. We would like to take this opportunity to express our heartfelt
thanks to Kewen Hu, Shaowei Liu, Chenxi Wang, Jinzhu Chen, Meng
Zuo, Suning Ye, Xiao Qian, Jianbing Wang, Zhaokun Ding, Minwei Jin,
Dawei Fan, Jianping Sun, Mingzhen Xie, Jiandong Zhang, Nierui Yang,
Yue Liu, Zhiqiang Fan, Yuan Zhang, Xinbing Tang, Shucheng Liu, Juhua
Xu, Xinjun Chen, Lei Bao, Naiwen Wei, Xiaofei Wang, Shuying Liu, Wei
Hu, Banghua Chen, Su Feng, Jianwu Hao, Dahe Zhao, Jianming Cao,
Yahao Zhang, Minhu Zhang, Yi Zeng, Jiandong Jin, Weiwei Jin, Xing
Huang, Xiaohui Li, Ming Yang, Wenjiang Shi, Xiaoxing Xu, Yiguang Cao,
Qingjun Li, Gang Zhao, Xiaoqi Gao, Jialing Li, Peng Zheng, Peng Wu,
Xiaoliang Wang, Fu Miao, Fuyou Miao, Min Liu, Xia Chen, Yunan Gu,
Jie Hou, Yuezhong Song, Ling Xu, Bing Liu, Ning Zong, Qin Wu, Liang
Xia, Bo Wu, Zitao Wang, Yuefeng Qiu, Huaru Yang, Xipeng Xiao, Dhruv
Dhody, Shucheng Liu, Hongjie Yang, Shuanglong Chen, Ruizhao Hu, Xin
Yan, Hong Shen, Wenxia Dong, Guanjun Zhou, Yuanyi Sun, Leyan Wang,
Shuhui Wang, Xiaohui Tong, Mingyan Xi, Xiaoling Wang, Yonghua Mao,
Lu Huang, Kaichun Wang, Chen Li, Jiangshan Chen, Guoyou Sun, Xiuren
Dou, Ruoyu Li, Cheng Yang, Ying Liu, Hongkun Li, Mengling Xu, Taixu
Tian, Jun Gong, Yang Xia, Fenghua Zhao, Pingan Yang, Yongkang Zhang,
Guangying Zheng, Sheng Fang, Chuang Chen, Ka Zhang, Yu Jiang, Hanlin
Li, Ren Tan, Yunxiang Zheng, Yanmiao Wang, Weidong Li, Zhidong Yin,
Yuanbin Yin, Zhong Chen, Chun Liu, Xinzong Zeng, Mingliang Yin,
45.
xl ◾ Acknowledgments
FengqingYu, Weiguo Hao, Shuguang Pan, Haotao Pan, Wei Li, and other
leaders and colleagues from Huawei. We sincerely thank Hui Tian, Feng
Zhao, Yunqing Chen, Huiling Zhao, Chongfeng Xie, Fan Shi, Bo Lei,
Qiong Sun, Aijun Wang, Yongqing Zhu, Huanan Chen, Xiaodong Duan,
Weiqiang Cheng, Fengwei Qin, Zhenqiang Li, Liang Geng, Peng Liu,
Xiongyan Tang, Chang Cao, Ran Pang, Xiaohu Xu, Ying Liu, Zhonghui
Li, Suogang Li, Tao Huang, Jiang Liu, Bin Yang, Fengyu Zhang, Dong
Liu, Dujuan Gu, and other technical experts in China’s IP feld who have
innovated in technology and promoted the formulation of standards for
a long time now. We also sincerely thank Adrian Farrel, Loa Andersson,
James Guichard, Bruno Decraene, Carsten Rossenhovel, Clarence Filsfls,
Keyur Patel, Pablo Camarillo Garvia, Daniel Voyer, Satoru Matsushima,
Alvaro Retana, Martin Vigoureux, Deborah Brungard, Susan Hares,
John Scudder, Joel Halpern, Matthew Bocci, Stephane Litkowski, Lou
Berger, Christian Hopps, Acee Lindem, Gunter Van de Velde, Daniel
King, Nicolai Leymann, Tarek Saad, Sam Aldrin, Stewart Bryant, Andy
Malis, Julien Meuric, Mike McBride, Jef Tantsura, Chris Bowers, Zhaohui
Zhang, Jefrey Haas, Reshad Rahman, Tony Przygienda, Zafar Ali, Darren
Dukes, Francois Clad, Kamran Raza, Ketan Talaulikar, Daniel Bernier,
Daisuke Koshiro, Gaurav Dawra, Jaehwan Jin, Jongyoon Shin, Ahmed
Shawky, Linda Dunbar, Donald Eastlake, Yingzhen Qu, Haoyu Song,
Parviz Yegani, Huaimo Chen, Young Lee, Ifekhar Hussain, Himanshu
Shah, Wim Henderickx, Ali Sajassi, Patrice Brissette, Jorge Rabadan, and
other IP experts in the industry who helped us with work on SRv6. Finally,
we would like to express our special thanks to Stefano Previdi and Latif
Ladid, both of whom wrote Forewords for this book.
We hope that this book can, as comprehensively as possible, present
basic SRv6 technologies and emerging technologies targeted at 5G and
cloud services to help readers comprehensively understand the fundamen-
tals of SRv6, its benefts to the industry, and its far-reaching impact on
communications networks. As SRv6 is an emerging technology and our
capabilities are limited, errors and omissions are inevitable. We will be
grateful for any information that helps to rectify these. Please send any
comments and feedback by email to lizhenbin@huawei.com.
46.
xli
Authors
Zhenbin Li isthe Chief Protocol Expert of Huawei and member of the
IETF IAB, responsible for IP protocol research and standards promotion
at Huawei. Zhenbin Li joined Huawei in 2000. For more than a decade,
he had been responsible for the architecture, design, and development
of Huawei’s IP operating system—Versatile Routing Platform (VRP)—
and MPLS sub-system as an architect and system engineer. From 2015
to 2017, Zhenbin Li worked as an SDN architect and was responsible for
the research, architecture design, and development of network control-
lers. Since 2009, Zhenbin Li has been actively participating in the innova-
tion and standardization work of the IETF, and in the past 6 years, he has
been continuously promoting the innovation and standardization of BGP,
PCEP, and NETCONF/YANG protocols for SDN transition. His research
currently focuses on Segment Routing over IPv6 (SRv6), 5G transport,
telemetry, and network intelligence, and he has led and participated in
more than 100 IETF RFCs/drafs as well as applied for more than 110
patents. In 2019, he was elected a member of the IETF IAB to undertake
Internet architecture management from 2019 to 2021.
Zhibo Hu is a Senior Huawei Expert in SR and IGP, responsible for SR
and IGP planning and innovation. Currently, Zhibo Hu is mainly engaged
in the research of SR-MPLS/SRv6 protocols and 5G network slicing tech-
nologies. Since 2017, Zhibo Hu has been actively participating in the inno-
vation and standardization work of the IETF and leading in standards
related to SRv6 reliability, SRv6 YANG, 5G network slicing, and IGP, and
is committed to leveraging SRv6 innovation to support network evolution
toward 5G and cloudifcation.
47.
xlii ◾ Authors
ChengLi is a Huawei Senior Pre-research Engineer and IP standards rep-
resentative, responsible for Huawei’s SRv6 research and standardization,
involving SRv6 extension header compression, SRv6 OAM/path segments,
SFC, and security. Cheng Li started to participate in IETF meetings from
2018. Up to now, Cheng Li has submitted more than 30 individual drafs,
more than ten of which have been promoted to working group drafs.
C H AP T E R 1
SRv6 Background
In this chapter, we expand on the history of Internet technology
development, from the competition between Asynchronous Transfer
Mode (ATM) and IP to the emergence of Multiprotocol Label Switching
(MPLS), the dawning of the All IP 1.0 era and the consequent challenges,
as well as the rise of Sofware Defned Networking (SDN). We then sum
up by introducing Segment Routing over IPv6 (SRv6), which is a key tech-
nology to enable the All IP 2.0 era.
1.1 OVERVIEW OF INTERNET DEVELOPMENT
Humans develop in tandem, as opposed to individually, through com-
munication and collaboration. Over our extensive history, various com-
munications modes have emerged, ranging from beacon fres to emails,
and from messenger birds to quantum entanglement. Te scope of com-
munications has also expanded from people within a certain vicinity to
people in distant localities, across a whole country, around the globe, and
even in outer space. Tat said, we as a people have never stopped our pur-
suit of communication technologies, which in turn have boosted human
prosperity. It is, therefore, safe to say that we are no longer content with
just human-to-human communication, or put diferently, we now aim at
achieving the connectivity of everything anytime and anywhere using the
Internet. It is within this context that the development of the Internet has
profoundly impacted the path of human society.
Afer years of development, the Internet has become almost as essen-
tial as water and electricity. Although the Internet has made life more
3
51.
4 ◾ SRv6Network Programming
FIGURE 1.1 Internet development milestones.
convenient through the information age, few people know the ins and outs
of its technological development history. With this in mind, we briefy
summarize the history of Internet technology development in Figure 1.1.
In 1969, Advanced Research Projects Agency Network (ARPANET) —
the major predecessor of the Internet — came into existence.
In 1981, IPv4[1] was defned.
In 1986, the Internet Engineering Task Force (IETF), dedicated to
formulating Internet standards, was founded.
In 1995, IPv6,[2] the next generation of IPv4, was standardized.
In 1996, MPLS[3] was proposed.
In 2007, SDN[4] debuted.
In 2008, the OpenFlow[5] protocol was introduced.
In 2013, SR[6] was proposed, including Segment Routing over MPLS
(SR-MPLS)[7] and SRv6.[8]
In 2014, Virtual eXtensible Local Area Network (VXLAN)[9] was
released.
On November 25, 2019, IPv4 addresses were exhausted.[10]
1.2 START OF ALL IP 1.0: A COMPLETE VICTORY FOR IP
1.2.1 Competition between ATM and IP
In the initial stage of network development, multiple types of networks,
such as X.25, Frame Relay (FR), ATM, and IP, coexisted to meet difer-
ent service requirements. Tese networks could not interwork with each
other, and also competed, with mainly ATM and IP networks taking cen-
ter stage.
ATM is a transmission mode that uses fxed-length cell switching. It
establishes paths in connection-oriented mode and can provide bet-
ter Quality of Service (QoS) capabilities than IP. Its design philosophy
involves centering on networks and providing reliable transmission, and
its design concepts refect the reliability and manageability requirements
52.
SRv6 Background ◾5
of telecommunications networks. Tis is the reason why ATM was widely
deployed on early telecommunications networks.
Te design concepts of IP difer greatly from those of ATM. To be more
precise, IP is a connectionless communication mechanism that provides
the best-efort forwarding capability, and the packet length is not fxed.
On top of that, IP networks mainly rely on the transport-layer protocols
(e.g., TCP) to ensure transmission reliability, and the requirement for the
network layer involves ease of use. To add on to this, the design concept of
IP networks embodies the “terminal-centric and best-efort” notion of the
computer network. We can therefore say that IP is widely used on com-
puter networks because it meets the corresponding service requirements.
Te competition between ATM and IP networks can essentially be
represented as a competition between telecommunications and computer
networks. In other words, telecommunications practitioners sought to use
ATM for network interconnection to protect network investments. On the
fip side, computer practitioners aimed at using ATM as only a link-layer
technology to provide QoS guarantee for IP networks, while setting aside
the task of establishing network connections for IP.
Computer networks subsequently evolved toward broadband, intelli-
gence, and integration, with mainly burst services. Despite this, the QoS
requirements that trafc places on computer networks are not as high
as those on telecommunications networks, and the length of packets is
not fxed. As such, the advantages of ATM — fxed-length cell switching
and good QoS capabilities — cannot be brought into full play on com-
puter networks. Not only that, the QoS capabilities of ATM are based
on connection-oriented control with a certain packet header overhead.
Terefore, ATM is inefcient in carrying computer network trafc, and it
yields high transmission and switching costs.
To sum up, as network scale expanded and network services increased
in number, ATM networks became more complex than IP networks, while
also bearing higher management costs. Within the context of costs versus
benefts, ATM networks exited the arena as they were gradually replaced
by IP networks.
1.2.2 MPLS: The Key to All IP 1.0
Although with relation to the development of computer networks, the
IP network is more ftting than the ATM network, a certain level of QoS
guarantee is still required. To compensate for the IP network’s insufcient
53.
6 ◾ SRv6Network Programming
QoS capabilities, numerous technologies integrating IP and ATM, such
as Local Area Network Emulation (LANE), IP over ATM (IPoA),[11] and
tag switching,[12] have been proposed. However, these technologies only
addressed part of the issue, until 1996 when MPLS technology was pro-
posed[3] to provide a better solution to this issue.
MPLS is considered as a Layer 2.5 technology that runs between Layer
2 and Layer 3. It supports multiple network-layer protocols, such as IPv4
and IPv6, and is compatible with multiple link-layer technologies, such
as ATM and Ethernet. Some of its other highlights include the fact that
it incorporates ATM’s Virtual Channel Identifer (VCI) and Virtual Path
Identifer (VPI) switching concepts, combines the fexibility of IP routing
and simplicity of label switching, and adds connection-oriented attributes
to connectionless IP networks. By establishing virtual connections, MPLS
provides better QoS capabilities for IP networks.
However, this is not the only reason why it was initially proposed. Point
in case being that MPLS also forwards data based on the switching of fxed-
length 32-bit labels, and therefore it features a higher forwarding efciency
than IP, which forwards data based on the Longest Prefx Match (LPM).
Tat said, as hardware capabilities have and continue to improve, MPLS no
longer features distinct advantages in forwarding efciency. Nevertheless,
MPLS provides a good QoS guarantee for IP through connection-oriented
label forwarding and also supports Trafc Engineering (TE), Virtual
Private Network (VPN), and Fast Reroute (FRR).[13] Tese advantages play
a key role in the continuous expansion of IP networks, while also catapult-
ing the IP transformation of telecom networks.
In general, the success of MPLS depends mainly on its three important
features: TE, VPN, and FRR.
• TE: Based on Resource Reservation Protocol-Trafc Engineering
(RSVP-TE),[14] MPLS labels can be allocated and distributed along
the MPLS TE path, and TE features (such as resource guarantee and
explicit path forwarding) can be implemented. Tis overcomes IP
networks’ lack of support for TE.
• VPN: MPLS labels can be used to identify VPNs[15] for isolation
of VPN services. As one of the major application scenarios of MPLS,
VPN is a key technology for enterprise interconnection and multi-
service transport as well as an important revenue source for carriers.
54.
SRv6 Background ◾7
• FRR: Te IP network cannot provide complete FRR protection,
which in turn means that it is unable to meet the high-reliability
requirements of carrier-grade services. MPLS improves the FRR
capabilities of IP networks and supports 50 ms carrier-grade protec-
tion switching in most failure scenarios.
Because IP networks are cost-efective and MPLS provides good TE, VPN,
and FRR capabilities, IP/MPLS networks gradually replaced dedicated
networks, such as ATM, FR, and X.25. Ultimately, MPLS was applied to
various networks, including IP backbone, metro, and mobile transport, to
support multiservice transport and implement the Internet’s All IP trans-
formation. In this book, we refer to the IP/MPLS multiservice transport
era as the All IP 1.0 era.
1.3 CHALLENGES FACING ALL IP 1.0: IP/MPLS DILEMMA
Although IP/MPLS drove networks into the All IP 1.0 era, the IPv4 and
MPLS combination has also set forth numerous challenges, which are
becoming more prominent as network scale expands and cloud services
develop, and are thereby hindering the further development of networks.
1.3.1 MPLS Dilemma
From one perspective, MPLS plays an important role in All IP transport,
while from another perspective, it complicates inter-domain network
interconnection by causing isolated network islands.
To put it more precisely, consider the fact that on the one hand, MPLS
is deployed in diferent network domains, such as IP backbone, metro,
and mobile transport networks, forming independent MPLS domains
and creating new network boundaries. However, many services require
E2E deployment, and this means that services need to be deployed across
multiple MPLS domains, which in turn results in complex inter-domain
MPLS solutions. In that regard, multiple inter-Autonomous System (AS)
solutions, such as Option A, Option B, and Option C,[15,16] have been pro-
posed for inter-AS MPLS VPN, and each one involves relatively complex
service deployment.
On the other hand, as the Internet and cloud computing develop, more
and more cloud data centers are built. To meet the requirements of multi-
tenant networking, multiple overlay technologies were proposed, among
which VXLAN is a typical example. At the same time, quite a few attempts
55.
8 ◾ SRv6Network Programming
were made to provide VPN services by introducing MPLS to data cen-
ters. However, these attempts all wound up in failure due to multiple fac-
tors, including numerous network boundaries, complex management, and
insufcient scalability.
In Figure 1.2, trafc from end users to the cloud data center needs to
travel multiple network domains. It passes through the MPLS-based Fixed
Mobile Convergence (FMC) transport network. Te trafc then enters
the MPLS-based IP backbone network through the native IP network,
accesses the data center’s IP network at the edge, and reaches the VXLAN
gateway. From there, it travels along the VXLAN tunnel, arrives at the
Top of Rack (TOR) switch at the egress of the VXLAN tunnel, and fnally
accesses the Virtual Network Function (VNF) device. We can therefore
envision how complex the service access process is due to an excessive
number of network domains.
Other major factors hindering the development of MPLS are scalability
and extensibility, which involves two aspects: scalability of the label space
and extensibility of encapsulation.
In the MPLS label space, as shown in Figure 1.3, there are 20 bits for the
MPLS label, which equates to a 220 label space.
FIGURE 1.2 Isolated MPLS network islands.
FIGURE 1.3 MPLS label encapsulation format.
56.
SRv6 Background ◾9
As the network scale expands, the label space is no longer sufcient.
Moreover, due to the limitation of the RSVP-TE protocol, the control
plane of MPLS networks also faces challenges such as complexity and a
lack of scalability. Tese make it difcult for MPLS to satisfy the require-
ments of network development.
On the other hand, the encoding of felds of MPLS encapsulation is
fxed. Although the MPLS label stack provides certain extensibility, as the
new network services develop and require more fexible encapsulation
in the forwarding plane (e.g., carrying metadata[17] in Service Function
Chaining (SFC) or In-situ Operations, Administration, and Maintenance
(IOAM)[18] packets), the extensibility of MPLS encapsulation faces more
challenges.
1.3.2 IPv4 Dilemma
One of the biggest problems regarding IPv4 is its insufcient address
resources.Sincethe1980s,IPv4addresseswereconsumedatanunexpectedly
fast pace. Put diferently, the Internet Assigned Numbers Authority (IANA)
announced that the last fve IPv4 address blocks were allocated on February
3, 2011. At 15:35 (UTC+1) on November 25, 2019, the fnal/22 IPv4 address
block was allocated in Europe, thereby signifying the depletion of global
IPv4 public addresses. Although technologies such as Network Address
Translation (NAT) help alleviate this issue by reusing private network
address blocks, this is by no means the ultimate solution.
As shown in Figure 1.4, NAT not only requires extra network confgu-
rations but also needs the maintenance of network state mappings, which
further complicates network deployment. On top of that, NAT does not
support source tracing of IPv4 addresses because the actual addresses are
hidden, and this generates management risks.
IPv4 also faces another dilemma: the insufcient extensibility of packet
headers results in inadequate programmability. Given this, it is difcult
for IPv4 networks to support many new services that require more exten-
sions of the header, such as source routing, SFC, and IOAM. Although
IPv4 defnes the Options feld for extension, it is rarely implemented and
used. Tis means that the insufcient extensibility of the IPv4 header will
constrain IPv4 development to a certain extent. Taking this into account,
the Internet Architecture Board (IAB) stated in 2016 that formulating
standards about new features based on IPv4 will not be considered in the
future.
57.
10 ◾ SRv6Network Programming
FIGURE 1.4 NAT.
To fnd a solution against IPv4 address exhaustion and poor extensi-
bility, the industry designed the next-generation upgrade solution for
IPv4 — IPv6.[2]
1.3.3 Challenges for IPv6
As the next-generation IP protocol, IPv6 aims to solve two major prob-
lems of IPv4: limited address space and insufcient extensibility.[19] To this
end, IPv6 ofers certain improvements to IPv4.
One such improvement is the expanded address space, or stated dif-
ferently, the increase from 32 bits for IPv4 addresses to 128 bits for IPv6
addresses. Te magnitude of this expansion can be compared to allocating
an IPv6 address to every grain of sand on the Earth, efectively solving the
problem of insufcient IPv4 addresses.
Te extension header mechanism counts as another noteworthy
enhancement. Requirement For Comments (RFC) 8200[19] defnes the fol-
lowing IPv6 extension headers, which ideally should appear in a packet in
the following order (Appendix A expands on the details of IPv6):
1. IPv6 header
2. Hop-by-Hop Options header
58.
SRv6 Background ◾11
3. Destination Options header
4. Routing header
5. Fragment header
6. Authentication header
7. Encapsulating Security Payload header
8. Destination Options header
9. Upper-Layer header
Figure 1.5 shows the encapsulation format of a common IPv6 extension
header that carries a TCP message.
As far as IPv6 is concerned, extension headers provide good extensibil-
ity and programmability. For example, the Hop-by-Hop Options header
can be used to implement hop-by-hop IPv6 data processing, and the
Routing header to implement source routing.
Over 20years have passed since IPv6 was proposed, yet IPv6 develop-
ment is still quite slow. Only within the last few years have technology
development and policies propelled IPv6 deployment. In retrospect, the
slow IPv6 development can mainly be attributed to the following factors:
1. Incompatibility with IPv4 and high costs for network upgrades:
Although IPv6 ofers an address space of 128 bits compared to
32 bits in IPv4, it is incompatible with IPv4, meaning that hosts
using IPv6 addresses cannot directly communicate with those using
IPv4 addresses. As such, a transition solution is required, resulting
in high network upgrade costs.
2. Insufcient service driving force and low network upgrade ben-
efts: IPv6 advocates have been promoting the 128-bit address space,
FIGURE 1.5 Encapsulation format of an IPv6 extension header carrying a TCP
message.
59.
12 ◾ SRv6Network Programming
which is viewed as a solution to IPv4 address exhaustion. However,
technologies such as NAT can also solve this problem, and NAT does
indeed serve as the main remedy. Te specifc process involves lever-
aging private network addresses and address translation technolo-
gies to temporarily alleviate the problem which may hinder network
service development.
A key advantage of NAT is the fact that it can be deployed at a lower cost
than that needed to upgrade IPv4 networks to IPv6. Considering the
fact that existing services run well on IPv4 networks, there is no point in
upgrading them, especially as this does not yield new revenue and, on the
contrary, would lead to higher costs. Tis is the major reason why carriers
are unwilling to upgrade.
In light of this, the key to solving slow IPv6 deployment lies in fnding
more attractive services supported by IPv6, as opposed to IPv4. Tis way,
business benefts can drive carriers to upgrade their networks to IPv6.
1.4 OPPORTUNITIES FOR ALL IP 1.0: SDN
AND NETWORK PROGRAMMING
In addition to the insufcient extensibility and programmability of
IPv4 and MPLS data planes, the All IP 1.0 era also faces the following
challenges[20]:
• Lack of a global network view and trafc visualization capability.
As a consequence of this, it is difcult to make optimal decisions
from a global perspective of the network, or quickly respond to TE
requirements.
• Lack of a unifed abstract model in the data plane prevents the con-
trol plane from supporting new functions by programming with
data-plane Application Programming Interfaces (APIs).
• Lack of automation tools and a long service rollout period.
• Te data and control planes of a device are tightly coupled and bun-
dled for sale. As such, these two planes rely on each other as far as evo-
lution is concerned, and the control plane on a device provided by one
vendor cannot control the data plane on a device provided by another.
60.
SRv6 Background ◾13
Te traditional network devices in the All IP 1.0 era are similar to the
IBM mainframes of the 1960s. To put it more exactly, the network
device hardware, Operating System (OS), and network applications are
tightly coupled and rely on each other. Because of this, if we aim to
conduct innovation or evolution on one part, we must also upgrade the
other parts correspondingly, and this architecture impedes network
innovation.[20]
In contrast, personal computers have a vastly diferent develop-
ment model, which involves them using a universal processor, based on
which they implement sofware-defned functions. Terefore, the com-
puter has a more fexible programming capability, resulting in explosive
growth in sofware applications. Furthermore, the open-source model
of computer sofware breeds a large amount of open-source sofware,
accelerates the sofware development process, and promotes the rapid
development of the entire computer industry. We can look at the Linux
open-source OS as the best example of this.
Drawing on universal hardware, sofware-defned functions, and the
open-source model in the computer feld, Professor Nick McKeown’s team
proposed a new network architecture — SDN.[4]
In the SDN architecture, the control plane of a network is separated
from its data plane: Te data plane becomes more generalized, similar
to the universal hardware of a computer. It no longer needs to specif-
cally implement the control logics of various network protocols; instead, it
only needs to execute the operation instructions received from the control
plane. Te control logic of a network device is defned by the SDN con-
troller and applications, and as such, network functions are defned by
sofware. With the emergence of open-source SDN controllers and open-
source SDN open interfaces, the network architecture includes three ele-
ments: universal hardware, sofware-defned support, and open-source
model. Figure 1.6 depicts the evolution from the traditional network
architecture to the SDN architecture.
SDN has the following three characteristics[19]:
1. Open network programmability: SDN provides a new network
abstraction model with a complete set of universal APIs for users,
who can then program on the controller to confgure, control, and
manage networks.
61.
14 ◾ SRv6Network Programming
FIGURE 1.6 Evolution from the traditional network architecture to the SDN
architecture.
2. Separation of the control and data planes: Separation refers to the
decoupled control and data planes, which can evolve independently
and communicate using a set of open APIs.
3. Logical centralized control: Tis refers to the centralized control of
distributed network states. Logical centralized control architecture
is the basis for SDN, propelling automated network control into the
realm of possibilities.
A network that has the preceding three characteristics can be called an
SDN network. Among the three characteristics, separation of the control
and data planes creates the necessary conditions for logical centralized
control, which in turn provides the architectural basis for open network
programmability. Note that open network programmability is the core
characteristics of SDN.
Tat being said, it is worth noting that SDN is only a network architec-
ture, and multiple technologies have been proposed to implement it, such
as OpenFlow, Protocol Oblivious Forwarding (POF),[21] Programming
Protocol-independent Packet Processors (P4),[22] and SR.[7]
1.4.1 OpenFlow
On March 14, 2008, Professor Nick McKeown and others proposed
OpenFlow, which is a protocol used between the SDN control plane and
data plane.
62.
SRv6 Background ◾15
FIGURE 1.7 OpenFlow 1.0 architecture.
In the OpenFlow protocol architecture shown in Figure 1.7, an
OpenFlow channel is established between an OpenFlow switch and
OpenFlow controller to exchange information. Te controller can deliver
fow table entries to the OpenFlow switch using the OpenFlow protocol.
Each fow table entry defnes a type of fow and corresponding forwarding
actions. Tat is, if the matching for a specifc type of fow succeeds, cor-
responding actions will be executed to process and forward packets.
Essentially, the forwarding behavior on a device can be abstracted into
“matching and action.” For example, Layer 2 switches and Layer 3 rout-
ers forward packets by searching for a destination Media Access Control
(MAC) address and destination IP address, respectively. Te design prin-
ciple of OpenFlow involves abstracting matching and forwarding actions
into specifc operations and using the controller to deliver fow table
entries to switches to guide packet forwarding. In short, OpenFlow is an
SDN technology that abstracts and generalizes network processing rules
and supports centralized programming.
In OpenFlow, a packet can be forwarded to a specifc interface or dis-
carded by matching protocol felds such as Ethernet, IPv4, or IPv6. Te
controller can program a matching+action rule via a fow table entry to
implement network programming. For example, the controller delivers a
fow table entry to switch A, instructing the switch to forward a packet
whose destination IP address is 192.168.1.20 to outbound interface 1.
OpenFlow’s advantage lies in the fexible programming of forwarding
rules, but that’s not to say it does not have apparent problems.
1. Te fow table entries are stored in the expensive Ternary Content
Addressable Memory (TCAM), meaning that only 1k–10k entries
can generally be supported in OpenFlow switches. Te limited
OpenFlow fow table specifcations result in OpenFlow switches
ofering insufcient scalability. Currently, OpenFlow is mainly
deployed in data centers for simple data switching and cannot be
deployed at a location requiring numerous fow table entries.
63.
16 ◾ SRv6Network Programming
2. Te advantage of an OpenFlow switch (used as a Layer 3 switch or
router) is that it can forward packets according to the fow table entry
generated by the central controller without requiring distributed
routing protocols, such as an Interior Gateway Protocol (IGP).
However, no carrier would like to abandon distributed routing pro-
tocols on their live networks, causing the distributed routing proto-
cols like IGPs to continue running on OpenFlow switches. In this
case, the basic shortest path forwarding can be performed by the IGP
while OpenFlow can only be used to optimize trafc steering, so the
benefts of OpenFlow are limited comparing to the cost brought by
it. Taking this into account, the OpenFlow switch does not simplify
the protocols, but on the contrary, it introduces additional complex-
ity brought by OpenFlow.
3. OpenFlow supports packet forwarding by adding corresponding
fow table entries based on the existing forwarding logic. It cannot
program the forwarding logic of a switch. For this reason, when
new features are added to OpenFlow, the protocol stack implemen-
tation on the controller and switch has to be updated. Sometimes,
even the switch’s chip and other hardware need to be redesigned.
Terefore, the cost of supporting new features is signifcant in
OpenFlow.
4. OpenFlow does not have sufcient capability to support stateful net-
work processing in the data plane that covers a full range of L4–L7
services. Terefore, the limited expressivity impacts the forward-
ing plane programmability. Te network states must be maintained
on the controller and synchronized up with the OpenFlow Switch.
Overdependence on the controller signifcantly burdens it, as well as
brings problems in extensibility and performance.
Due to the preceding limitations, OpenFlow has not been widely deployed.
1.4.2 POF
Huawei proposed POF to provide programmable forwarding logic for
switches, which is missing from OpenFlow.
Similar to OpenFlow, the architecture of POF includes two parts:
the control plane (POF controller) and the data plane (POF Forwarding
Element (FE)).
64.
SRv6 Background ◾17
POF makes the forwarding plane totally protocol-oblivious. Te POF
FE has no need to understand the packet format. All an FE needs to do
is, under the instruction of its controller, extract and assemble the search
keys (located by one or more<ofset, length>tuples) from the packet
header, conduct table lookups, and then execute the associated instruc-
tions. As a result, the FE can easily support new protocols and forwarding
requirements in the future.[21]
In summary, POF is an SDN technology that comprehensively abstracts
network processing into a more interoperable, protocol-oblivious process,
while providing the capabilities of programming forwarding rules and
forwarding logic. Figure 1.8 shows the architecture of the POF hardware
and sofware switches.
BecausePOFsupportsprotocol-obliviousforwarding,itcanbedeployed
on any network, including non-Ethernet networks such as Named Data
Network (NDN) and Content-Centric Network (CCN). In addition, the
POF FE supports adding metadata to packets so that stateful processing
can be supported.
(a) (b)
FIGURE 1.8 POF hardware switch (a) and POF sofware switch (b).
65.
18 ◾ SRv6Network Programming
However, POF is more complex than OpenFlow. Furthermore, a Flow
Instruction Set[21] needs to be defned to implement complex instruction-
based scheduling, which afects forwarding performance to some extent.
Terefore, POF has not made much progress in commercial use.
1.4.3 P4
In order to solve the insufcient programmability of OpenFlow, Professors
Nick McKeown (Stanford) and Jennifer Rexford (Princeton), among oth-
ers, proposed P4.[21] P4 is a high-level language for programming protocol-
independent packet processors. P4 can be used to confgure switches to
express how to parse, process, and forward packets.
P4 supports programmable packet processing on a device by defning a
P4 program containing the following components: headers, parsers, tables,
actions, and control programs.[22] A P4 program can be compiled and run
on the common abstract forwarding model-based devices. Figure 1.9
shows the common abstract forwarding model.
With P4 you can confgure and program how a switch processes
packets even afer the switch is deployed, thereby eliminating the need to
purchase new devices to support new features. Tis innovation solves the
insufcient programmability of OpenFlow. In addition, P4 programma-
bility enables switches to support protocol-independent packet forward-
ing, which means that diferent protocols can be supported by defning
associated P4 programs.
FIGURE 1.9 Common abstract forwarding model.
66.
SRv6 Background ◾19
Although P4 has certain technical advantages, it has not made much
progress in commercial deployment. One reason is that networks are
evolving nowhere fast. Te standardization of new network features is not
solely up to one device vendor. It is usually discussed for several years
among carriers, device vendors, and other relevant parties in the industry.
With such a long process, the beneft of supporting new features in a short
time provided by P4 is no longer signifcant. Another reason is that fully
centralized SDN has insufcient reliability and response speed but high
requirements on the controller. However, carriers do not need to com-
pletely reconstruct their networks by overturning the distributed routing
protocol architecture. Instead, the better solution is to provide the global
trafc optimization based on existing distributed routing protocols. So
far, P4 has not yet been put into large-scale commercial use. On June 11,
2019, Barefoot, a P4 startup, was ultimately acquired by Intel.
1.4.4 SR
OpenFlow, POF, and P4 were originally designed to provide programma-
bility for networks. But is revolutionary innovation necessary for network
programmability? Te answer is no.
In 2013, SR was proposed, which is a transitional extension based
on the existing network and provides network programmability. SR is
a source routing paradigm which can program the forwarding path of
packets by allowing the ingress of the path to insert forwarding instruc-
tions into packets. Te core idea of SR is to combine diferent segments
into a path and insert segment information into packets at the ingress of
the path to guide packet forwarding. A segment is an instruction, which
is executed on a node for packet forwarding or processing. It can refer to
a specifc interface or the shortest path to a node through which a packet
is forwarded. Such a segment is identifed by a Segment Identifer (SID).
Currently, there are two data planes for SR: MPLS and IPv6. When SR
is applied to the MPLS data plane, it is called SR-MPLS, and the SID is
encoded as an MPLS label. When SR is applied to the IPv6 data plane, it is
called SRv6, and the SID is encoded as an IPv6 address.
Te design of SR is easily comparable to many real-life examples, for
instance, traveling by train or plane. Te following uses travel as an exam-
ple to further explain SR.
If fying from Haikou to London requires a stop in Guangzhou and in
Beijing, we need to buy three tickets: Haikou to Guangzhou, Guangzhou
to Beijing, and Beijing to London.
67.
20 ◾ SRv6Network Programming
With three tickets in hand, we know we will fy from Haikou to
Guangzhou on fight HU7009 according to the frst ticket; when arriving
in Guangzhou, we take fight HU7808 to Beijing, according to the second
ticket; when arriving in Beijing, we fy to London by fight CA937, accord-
ing to the last ticket. Ultimately, we fy to London segment by segment,
using the three tickets.
Te packet forwarding on an SR network is similar. As shown in
Figure 1.10, a packet enters the SR network from node A. From the des-
tination address, node A knows that the packet needs to pass through
nodes B and C before reaching node D. Terefore, node A inserts the SIDs
of nodes B, C, and D into the packet header to indicate how to steer the
packet. Te packet will be sent to node B, node C, and fnally node D
according to the SID information in the packet header.
Compared with RSVP-TE MPLS, SR-MPLS has the following
advantages:
• Simplifes the control plane. In SR-MPLS, label distribution proto-
cols such as RSVP-TE are not needed any more. Instead, only IGP
and Border Gateway Protocol (BGP) extensions are required in the
SR-MPLS control plane, thereby reducing the number of control
plane protocols.
• Simplifes network states. When RSVP-TE is used on an MPLS net-
work, nodes need to maintain per-fow states. In contrast, on an
SR-MPLS network, only the ingress node needs to maintain per-fow
states, while the transit and egress nodes do not.
FIGURE 1.10 Packet forwarding process with SR.
68.
SRv6 Background ◾21
SR-MPLS provides network programming capabilities by reusing the
existing MPLS forwarding mechanism. Terefore, it can support smooth
upgrades from existing MPLS networks to SR-MPLS networks. In this
way, SR-MPLS, as the transitional innovation, is much easier to be adopted
by the industry. In addition, SR retains the distributed intelligence of the
network while introducing the global trafc optimization of the SDN con-
troller. Tis makes implementation more practical, and it will go further.
So far, SR has already become the de facto SDN standard.
Although SR-MPLS based on MPLS data plane can provide good pro-
grammability, it cannot satisfy services that need to carry metadata, such
as SFC and IOAM, as MPLS encapsulation has relatively poor extensibil-
ity. Compared with SR-MPLS, SRv6, which is based on IPv6 data plane,
not only inherits all the advantages of SR-MPLS, but also provides better
extensibility.
1.5 KEY TO ALL IP 2.0: SRv6
As mentioned before, the key to speeding up IPv6 deployment lies in fnd-
ing the services supported by IPv6, as opposed to IPv4. Driven by business
benefts, carriers will upgrade their networks to IPv6. But what drives the
need for IPv6 over IPv4? Te answer lies in the network programmability
of IPv6, which allows new services to be deployed quickly and easily to
create revenue for carriers.
Although IPv4 also provides the programmable Options feld, this feld
is not ofen used. IPv6, however, takes the extensibility of packet headers
into account from the very beginning. A number of extension headers,
including Hop-by-Hop Options, Destination Options, and Routing
headers,[19] were designed to support further extension.
However, afer 20-plus years, IPv6 extension headers are still rarely
applied to their full potential. With the rise of new services such as 5G
and cloud, and the development of network programming technologies,
services require the forwarding plane of a network to provide stronger
programming capabilities and a simpler converged network solution.
Tis is where SRv6 comes into play.
SRv6 is an SR network paradigm based on IPv6 data plane by making
use of a new IPv6 Routing Header, called Segment Routing Header (SRH),
allowing the ingress to insert forwarding instructions to guide data packet
forwarding. As shown in Figure 1.11, SRv6 combines the advantages of
69.
22 ◾ SRv6Network Programming
FIGURE 1.11 SR+IPv6=SRv6.
SR-MPLS’s programmability and IPv6 headers’ extensibility, giving IPv6
an edge.
As of now, SRv6 has been commercially deployed by multiple carri-
ers around the world just afer 2years since the draf SRv6 Network
Programming[23] was submitted to the IETF. Such rapid development is
uncommon among IP technologies. During these 2-plus years when we
promoted SRv6 innovation and standardization, we have communicated
extensively with industry experts and made a number of refections on the
experience and lessons learned in the development of Internet technolo-
gies. Tis has given us a further understanding of the value and signif-
cance of SRv6.
To sum up, the MPLS-based All IP 1.0 era has achieved great success
but has also brought some problems and challenges:
1. Isolated IP transport network islands: Although MPLS unifed the
technologies for transport networks, the IP backbone, metro, and
mobile transport networks are separated and need to be intercon-
nected using complex technologies such as inter-AS VPN, making
E2E service deployment difcult.
2. Limited programming space in IPv4 and MPLS encapsulation: Many
new services require more forwarding information to be added to
packets. However, the IETF announced that it has stopped formulat-
ing further standards for IPv4. In addition, the format of the MPLS
Label feld is fxed and lacks extensibility. Tese reasons make it dif-
fcult for IPv4 and MPLS to meet the requirements of new services
for network programming.
70.
SRv6 Background ◾23
3. Decoupling of applications and transport networks: Tis makes it
difcult to optimize networks and improve the value of networks.
Many carriers fnd themselves stuck as a provider of pipes and cannot
beneft from value-added applications. Moreover, the lack of applica-
tion information means that carriers can only implement network
adjustment and optimization in a coarse-granularity way, result-
ing in wasting resources. Attempts have been made over the years
to apply network technologies to user terminals, but all have failed.
An example of such attempts is ATM-to-desktop. Other attempts
have been made to deploy MPLS closer to hosts and applications, for
example, deploying MPLS for the cloud. But the fact is, deploying
MPLS in data centers is very difcult while VXLAN becomes the de
facto standard of data centers.
SRv6 technology is the answer to these problems.
1. SRv6 is compatible with IPv6 forwarding and can implement inter-
connection of diferent network domains easily through IPv6 reach-
ability. Unlike MPLS, SRv6 does not require additional signaling or
networkwide upgrades.
2. SRv6, based on SRHs, supports encapsulation of more information
into packets, meeting diversifed requirements of new services.
3. SRv6’s afnity to IPv6 enables it to seamlessly integrate IP transport
networks with IPv6-capable applications and provide more poten-
tial value-added services for carriers through application-aware
networks.
Te development of IPv6 over the past 20-plus years proves that the
demand for address space alone cannot promote the large-scale deploy-
ment of IPv6. But the rapid development of SRv6 indicates that IPv6
development can be boosted by requirements on new services. As shown
in Figure 1.12, along with the development of services such as 5G, cloud,
and Internet of Tings (IoT), the increasing number of network devices
require more addresses and network programmability. SRv6 can better
meet the requirements of these services, promote the development of
network services, and drive networks into a new All IP era, that is, an
71.
24 ◾ SRv6Network Programming
FIGURE 1.12 IP technology development generations.
intelligent IP era where all things are connected based on IPv6. In this
book, we refer to this era as the All IP 2.0 era.
1.6 STORIES BEHIND SRv6 DESIGN
I. SRv6 and SDN
Since its proposal in 2007, SDN has exerted a lasting impact on
industries. Te concept of SDN is also made more applicable over
diferent scenarios. From the originally revolutionary OpenFlow
and POF that require complete separation of forwarding and con-
trol, SDN has undergone gradual evolution amid a heated debate in
the industry. Te most important driving force behind this is the
IETF. From 2013 to 2016, an important task of the IETF was the
standardization of southbound protocols of SDN controllers, such as
BGP, Path Computation Element Communication Protocol (PCEP),
and Network Confguration Protocol (NETCONF)/Yet Another
Next Generation (YANG). Afer more than 4years of efort, the IETF
completed the main work of SDN transition in the control plane and
started to work on SRv6 Network Programming in 2017. Compared
with SR-MPLS, SRv6 has stronger network programming capabili-
ties. Programming, once implemented based on an OpenFlow- or
POF-capable forwarding plane, is now implemented through SRv6,
which provides better compatibility. Tis refects the SDN transition
in the forwarding plane. In other words, the SDN transition is still
ongoing, and the focus has shifed from the control plane to the for-
warding plane.
72.
SRv6 Background ◾25
II. Rethinking the Value and Signifcance of SR
While exploring this technology and discussing it with others, I
was ofen struck with some excellent thoughts, which were of great
help for me to understand the essence of technologies. Section 1.5 of
this chapter summarizes the problems faced by MPLS and the value
and signifcance of SRv6. Later I discussed with experts in the indus-
try and acquired more enlightening viewpoints which are shared in
the following for your reference:
1. MPLS is also essentially an extension of IP functions. However,
due to the limitations in the past, it was implemented by using
the Shim layer, which requires networkwide upgrades to sup-
port the extended functions. Afer more than 20years of sof-
ware and hardware development, many of these limitations
have been removed. SRv6 uses a new method to integrate IP and
MPLS functions (SRv6 SIDs refect both IP-like and MPLS-like
identifers), which better complies with the trend of technology
development.
2. SRv6 enables SDN on carrier IP networks. VXLAN is an impor-
tant foundation for the development of SDN on data center
networks. However, no VXLAN-like technology was available
to boost the development of SDN on carrier IP networks. SRv6
solves this problem.
During the development of SDN, there was a bias toward the
construction of SDN controller capabilities, but the impact of
network infrastructure was largely ignored. Carrier IP networks
are a lot more complex than data center networks, and this has
much to do with MPLS as a basic transport technology. In turn,
this has resulted in complex functions and difcult deployment
of the SDN controller. Te native IP attribute of SRv6 greatly
simplifes basic transport technologies. Furthermore, SRv6 can
be deployed from the edge to the core, in a staggered manner,
from SRv6 VPN to loose TE, strict TE, and more. Tis also
enables SDN to be gradually rolled out on carrier IP networks in
a simple-to-complex manner.
3. 5G changes the attributes of connections, and cloud changes the
scope of connections. Tese changes bring big opportunities for
SRv6 development.
73.
26 ◾ SRv6Network Programming
Network connections are the core of IP transport networks,
and the development of 5G services poses more requirements on
network connections, such as stronger Service Level Agreement
(SLA) assurance and deterministic latency. 5G also changes or,
more precisely, enhances the attributes of connections, requiring
more information to be carried in packets. In addition, with the
development of cloud services, service processing locations have
become more fexible. Some cloud services (such as telco cloud)
furtherbreaktheboundarybetweenphysicalandvirtualnetwork
devices, integrating services and transport networks. All these
have changed the scope of network connections. SRv6 enables
the programming of services and transport networks based on
a single data plane. Also, thanks to its native IP attribute, SRv6
allows rapid setup of connections and satisfes requirements of
fexible adjustment of the connection scope.
III. Development of IP Generations
We used to believe that IP was developed in an incremental and
compatible manner. Tat is, IP does not have clear defnitions of gen-
erations such as 2G, 3G, 4G, and 5G like wireless. However, when we
look back upon the development of IP over the past few decades, we
fnd some intergenerational characteristics.
First is the rise and decline of network protocols. In the 1990s,
the competition between ATM and IP led to the decline of telecom-
munications. IP transport networks unifed networks by replac-
ing independent networks such as ATM, FR, and Time Division
Multiplexing (TDM). With the rise of SR, traditional MPLS signaling
Label Distribution Protocol (LDP) and RSVP-TE are declining, and
the entire MPLS will gradually decline as its data plane is replaced by
IPv6 extensions due to the increasing popularity of SRv6.
Second, IP has the habit of expanding application scenarios. IP was
frst applied to the Internet. Later, IP-based MPLS was applied to IP
backbone, metro, and mobile transport networks. With the success
of SDN, IP is widely used in data centers to replace the traditional
Layer 2 networking. SRv6 is in the process of continuing this trend,
as it can well meet the requirements of new scenarios such as 5G and
cloud. To give another analogy of this trend, the development of ring
roads in Beijing is similar to the development of IP. As the city keeps
74.
SRv6 Background ◾27
expanding, new ring roads are built around the city, going from two
to three, and even to six rings. Construction of each ring road requires
the development and improvement of a new solution, along with the
reconstruction of areas that have already been encircled within the
ring roads. Just like SRv6 is used for new 5G and cloud scenarios,
MPLS will be replaced by SRv6 on existing IP transport networks.
Tese signifcant changes in network development act as a
reference for defning IP generations. Summarizing the history of
network development and defning IP generations help us better
seize opportunities for the future.
REFERENCES
[1] Postel J. Internet Protocol[EB/OL]. (2013-03-02)[2020-03-25]. RFC 791.
[2] Deering S, Hinden R. Internet Protocol Version 6 (IPv6) Specifcation[EB/
OL]. (2013-03-02)[2020-03-25]. RFC 2460.
[3] Rosen E, Viswanathan A, Callon R. Multiprotocol Label Switching
Architecture[EB/OL]. (2020-01-21)[2020-03-25]. RFC 3031.
[4] Casado M, Freedman M J, Pettit J, Luo J, Mckeown N, Shenker S. Ethane:
Taking Control of the Enterprise[EB/OL]. (2007-08-31)[2020-03-25]. ACM
SIGCOMM Computer Communication Review, 2007.
[5] Mckeown N, Anderson T, Balakrishnan H, Parulkar G, Peterson L,
Rexford J, Shenker S, Turner J. OpenFlow: Enabling Innovation in Campus
Networks[EB/OL]. (2008-03-31)[2020-03-25]. ACM SIGCOMM Computer
Communication Review, 2008.
[6] Filsfls C, Previdi S, Insberg L, Decraene B, Litkowski S, Shakir R. Segment
Routing Architecture[EB/OL]. (2018-12-19)[2020-03-25]. RFC 8402.
[7] Bashandy A, Filsfls C, Previdi S, Decraene B, Litkowski S, Shakir R.
Segment Routing with MPLS Data Plane[EB/OL]. (2019-12-06)[2020-03-
25]. draf-ietf-spring-segment-routing-mpls-22.
[8] Filsfls C, Dukes D, Previdi S, Leddy J, Matsushima S, Voyer D. IPv6 Segment
Routing Header (SRH)[EB/OL]. (2020-03-14)[2020-03-25]. RFC 8754.
[9] MahalingamM,DuttD,DudaK,AgarwalP,KreegerL,SridharT,BursellM,
Wright C. Virtual eXtensible Local Area Network (VXLAN): A Framework
for Overlaying Virtualized Layer 2 Networks over Layer 3 Networks[EB/
OL]. (2020-01-21)[2020-03-25]. RFC 7348.
[10] Réseaux IP Européens Network Coordination Centre. Te RIPE NCC has
run out of IPv4 Addresses[EB/OL]. (2019-11-25)[2020-03-25].
[11] Huang S, Liu J. Computer Network Tutorial Problem Solving and Experiment
Guide[M]. Beijing: Tsinghua University Press, 2006.
[12] RekhterY,DavieB,KatzD,RosenE,SwallowG.CiscoSystems’TagSwitching
Architecture - Overview[EB/OL]. (2013-03-02)[2020-03-25]. RFC 2105.
75.
28 ◾ SRv6Network Programming
[13] Pan P, Swallow G, Atlas A. Fast Reroute Extensions to RSVP-TE for LSP
Tunnels[EB/OL]. (2020-01-21)[2020-03-25]. RFC 4090.
[14] Awduche D, Berger L, Gan D, Li T, Srinivasan V, Swallow G. RSVP-TE:
Extensions to RSVP for LSP Tunnels[EB/OL]. (2020-01-21)[2020-03-25].
RFC 3209.
[15] Rosen E, Rekhter Y. BGP/MPLS IP Virtual Private Networks (VPNs)[EB/
OL]. (2020-01-21)[2020-03-25]. RFC 4364.
[16] Leymann N, Decraene B, Filsfls C, Konstantynowicz M, Steinberg D.
Seamless MPLS Architecture[EB/OL]. (2015-10-14)[2020-03-25]. draf-ietf-
mpls-seamless-mpls-07.
[17] Halpern J, Pignataro C. Service Function Chaining (SFC) Architecture[EB/
OL]. (2020-01-21)[2020-03-25]. RFC 7665.
[18] Brockners F, Bhandari S, Pignataro C, Gredler H, Leddy J, Youell S, Mizrahi
T, Mozes D, Lapukhov P, Chang R, Bernier D. Data Fields for In-situ
OAM[EB/OL]. (2020-03-09)[2020-03-25]. draf-ietf-ippm-ioam-data-09.
[19] Deering S, Hinden R. Internet Protocol Version 6 (IPv6) Specifcation[EB/
OL]. (2020-02-04)[2020-03-25]. RFC 8200.
[20] Yang Z, Li C. Network Reconstruction: SDN Architecture and Implemen-
tation[M]. Beijing: Electronic Industry Press, 2017.
[21] Song H. Protocol-Oblivious Forwarding: Unleash the Power of SDN
through a Future-Proof Forwarding Plane[EB/OL]. (2013-08-16)[2020-03-
25]. Proceedings of the Second ACM SIGCOMM Workshop on Hot Topics in
Sofware Defned networking, 2013.
[22] Bosshart P, Daly D, Gibb G, Izzard M, Mckeown N, Rexford J, Schlesinger C,
Talayco A, Vahdat A, Varghese G. P4: Programming Protocol-Independent
Packet Processors[EB/OL]. (2014-07-28)[2020-03-25]. ACM SIGCOMM
Computer Communication Review, 2014.
[23] Filsfls C, Camarillo P, Leddy J, Voyer D, Matsushima S, Li Z. SRv6 Network
Programming[EB/OL]. (2019-12-05)[2020-03-25]. draf-ietf-spring-srv6-
network-programming-05.
C H AP T E R 2
SRv6 Fundamentals
This chapter describes SRv6 fundamentals, including the basic
concepts, SRv6 extension header, instruction sets, packet forwarding
processes, and technical advantages of SRv6. Inheriting the advantages of
both IPv6 and source routing technology, SRv6 is more suitable for cross-
domain deployment than MPLS as it supports incremental evolution
and ensures better scalability, extensibility, and programmability. Tese
advantages mean that SRv6 is strategically important for future technical
evolution.
2.1 SRv6 OVERVIEW
SRv6 provides outstanding network programming capabilities.[1]
In Chapter 1, we introduced the development of network programming
and the advantages of SRv6 in this regard. Network programming stems
from computer programming, through which we can convert our intent
into a series of instructions that computers can understand and execute to
meet our requirements. Similarly, if service intent can be translated into a
series of device-executable forwarding instructions on a network, network
programming can be achieved, ultimately meeting service customization
requirements. Figure 2.1 illustrates the implementation of network pro-
gramming as compared to computer programming.
SRv6 was introduced to translate network functions into instructions
and encapsulate the instructions into 128-bit IPv6 addresses. As such,
service requirements can be translated into an ordered list of instruc-
tions, which can then be executed by network devices along the service
31
79.
32 ◾ SRv6Network Programming
FIGURE 2.1 Computer programming and network programming.
forwarding path, thereby achieving fexible orchestration and on-demand
customization of services on an SRv6 network.
2.2 NETWORK INSTRUCTIONS: SRv6 SEGMENTS
A computer instruction typically consists of an opcode and an operand. Te
former determines the operation to be performed; the latter determines the
data, memory address, or both to be used in the computation. Similarly, net-
work instructions, which are called SRv6 segments, need to be defned for
SRv6 network programming, and they are identifed using 128-bit SIDs.[2]
Each SRv6 SID usually consists of three felds, as shown in Figure 2.2.
Te three felds are described as follows:
1. Te Locator feld identifes the location of a network node and is
used for other nodes to route and forward packets to this identi-
fed node. A locator has two important characteristics: routable and
aggregatable. “Routable” means that the corresponding locator route
can be advertised by a node to other nodes on the network through
FIGURE 2.2 SRv6 SID.
80.
SRv6 Fundamentals ◾33
an IGP so that those nodes can forward packets to the node through
the locator route. “Aggregatable” means that the locator route can
be aggregated. Te two characteristics help tackle problems such as
high network complexity and large network scale.
Te locator length is variable, so SRv6 SIDs can be used on small
to large networks.
Te following is an example of locator confguration.
<HUAWEI> system-view
[~HUAWEI] segment-routing ipv6
[~HUAWEI-segment-routing-ipv6] locator test1 ipv6-prefix 2001:db8:100:: 64
Afer the preceding confgurations are complete, a locator with the
64-bit prefx 2001:db8:100:: is generated to guide packet forwarding
to the node generating the corresponding instruction, thereby mak-
ing the instruction addressable.
2. Te Function feld specifes the forwarding behavior to be performed
and is similar to the opcode in a computer instruction. In SRv6
network programming, forwarding behaviors are expressed using
diferent functions. You can think of SIDs as similar to computer
instructions, in that each type of SID identifes corresponding for-
warding behaviors, such as forwarding packets to a specifed link or
searching a specifed table for packet forwarding.
Te following is an example of End.X function confguration.
[~HUAWEI-segment-routing-ipv6-locator] opcode ::1 end-x interface
GigabitEthernet3/0/0 next-hop 2001:db8:200::1
Te preceding example defnes an End.X function with an opcode
of ::1. If the Arguments feld is not specifed, the locator pre-
fx 2001:db8:100:: and function opcode ::1 form the SRv6 SID
2001:db8:100::1, which is a manually confgured End.X SID. An
End.X SID identifes one or a group of Layer 3 network adjacencies
connected to a network node. Te forwarding behavior bound to
the SID is to update the next SID into the IPv6 Destination Address
(DA) feld and forward packets to the corresponding neighbor-
ing node through the interface specifed by the End.X SID. In the
preceding example, the End.X function instructs the local node
to forward packets from GigabitEthernet 3/0/0 to the next hop at
2001:db8:200::1.
81.
34 ◾ SRv6Network Programming
3. Te Arguments (Args) feld is optional. It is used to defne param-
eters for instruction execution and can contain fow, service, and any
other related information. For example, the Arg.FE2 parameter can
be specifed for an End.DT2M SID to exclude a specifc or a group
of outbound interfaces when the corresponding Layer 2 forward-
ing table is searched for multicast replication in Ethernet Segment
Identifer (ESI) fltering and Ethernet Virtual Private Network
(EVPN) Ethernet Tree (E-Tree) scenarios.
Much like computers that use limited instruction sets to implement
various computing functions, SRv6 also defnes instruction sets for for-
warding purposes.
Instructions are essential to network programming. Any E2E ser-
vice connection requirement can be expressed using an ordered set of
instructions. In current SR implementation, the source node encapsulates
ordered segment lists into packets to instruct specifed nodes to execute
corresponding instructions, thereby achieving network programmability.
With the increase of SRv6 application scenarios, SRv6 instruction sets will
evolve continuously.
2.3 NETWORK NODES: SRv6 NODES
An SRv6 network may contain nodes of the following roles[3]:
• SRv6 source node: a source node that encapsulates packets with
SRv6 headers
• Transit node: an IPv6 node that forwards SRv6 packets but does not
perform SRv6 processing
• SRv6 segment endpoint node (endpoint node for short): a node that
receives and processes SRv6 packets whose IPv6 DA is a local SID of
the node
A node plays a role based on the task it takes in SRv6 packet forwarding,
and it may play two or more roles. For example, it can be the source node
on one SRv6 path and a transit or endpoint node on another SRv6 path.
82.
SRv6 Fundamentals ◾35
2.3.1 SRv6 Source Node
An SRv6 source node steers a packet using an SRv6 segment list. If the
SRv6 segment list contains only one SID, and no Type Length Value (TLV)
or other information needs to be added to the packet, the DA feld of the
packet is set to the SID, without requiring SRH encapsulation.
An SRv6 source node can be either an SRv6-capable host where IPv6
packets originate or an edge device in an SRv6 domain.
2.3.2 Transit Node
A transit node is an IPv6 node that does not participate in SRv6 process-
ing on the SRv6 packet forwarding path; that is, the transit node just for-
wards IPv6 packets. Afer receiving an SRv6 packet, a transit node parses
the IPv6 DA feld in the packet. If the value of this feld is neither a locally
confgured SRv6 SID nor a local interface address, the transit node consid-
ers the SRv6 packet as an ordinary IPv6 packet. As such, it searches the
corresponding IPv6 routing table according to the longest match rule for
packet processing and forwarding. In this process, processing the DA as
an SRv6 SID or processing SRHs is not required.
A transit node can be either an ordinary IPv6 node or an SRv6-capable
node.
2.3.3 Endpoint Node
An endpoint node is a node that receives a packet destined for itself
(a packet of which the IPv6 DA is a local SID). Unlike transit nodes,
endpoint nodes have to process both SRv6 SIDs and SRHs.
To sum up, an SRv6 source node encapsulates packets with SRv6 head-
ers, a transit node processes and forwards the packets as common IPv6
packets, and an endpoint node processes both SRv6 SIDs and SRHs in the
packets, as shown in Figure 2.3.
Note: As recommended by RFC 3849, to avoid address conficts with live-
network addresses, IPv6 addresses with the prefx 2001:DB8::/32 are usu-
ally used as examples in books. However, 2001:DB8::/32 is too long to ft
neatly into fgures. Terefore, short addresses such as A1::1 and 1::1 are
ofen used in this book. Unless otherwise specifed, all IPv6 addresses in
this book are examples only.
83.
36 ◾ SRv6Network Programming
FIGURE 2.3 SRv6 nodes.
2.4 NETWORK PROGRAM: SRv6 EXTENSION HEADER
2.4.1 SRv6 Extension Header Design
To implement SRv6, the original IPv6 header is extended to defne a new
type of routing header called SRH,[3] which carries segment lists and other
related information to explicitly specify an SRv6 path.
Figure 2.4 shows the format of the SRH.
Table 2.1 describes the felds in an SRH.
Te SRH stores a SID list used for implementing network services,
which is similar to an ordered list of computer instructions. Te segments
represented by Segment List [0] through Segment List [n] are similar to
FIGURE 2.4 IPv6 SRH format.
84.
SRv6 Fundamentals ◾37
TABLE 2.1 Fields in an SRH
Field Length
Next Header 8 bits
Hdr Ext Len 8 bits
Routing 8 bits
Type
Segments 8 bits
Lef (SL)
Last Entry 8 bits
Flags 8 bits
Tag 16 bit
Segment 128 bits × number
List [n] of segments
Optional Variable
TLV
Description
Type of the header following the SRH. Common
header types are as follows:
• 4: IPv4 encapsulation
• 41: IPv6 encapsulation
• 43: IPv6 routing header (IPv6-Route)
• 58: Internet Control Message Protocol version 6
(ICMPv6)
• 59: no Next Header for IPv6
Length of an SRH, excluding the frst 8 bytes.
Type of the extension routing header. SRHs have a
routing type value of 4.
Number of remaining segments.
Index of the last element in a segment list.
Reserved for special processing, such as Operations,
Administration, and Maintenance (OAM).
Whether a packet is part of a group of packets, such
as packets sharing the same set of properties.
Te nth segment in a segment list, expressed using a
128-bit IPv6 address.
Optional TLVs, such as Padding TLVs and
Hash-based Message Authentication Code
(HMAC) TLVs.
the instructions of a computer program, and Segment List [n] indicates
the frst instruction that needs to be executed. Te SL feld, which is simi-
lar to the Program Counter (PC) of a computer program, points to the
instruction that is being executed and can be initially set to n (the index of
the last segment in a segment list). Each time an instruction is executed,
the SL value is decremented by 1 to point to the next instruction to be
executed. From this, we can see that the SRv6 forwarding process can be
easily simulated using a computer program.
To make it easier to explain the SRv6 data forwarding process in this
book, a simplifed version of the SRH shown in Figure 2.5 (a) is used and
can be further simplifed as shown in Figure 2.5 (b).
2.4.2 SRH TLVs
As shown in Figure 2.4, SRHs use optional TLVs to ofer a higher level
of network programmability.[3] Te TLVs provide better extensibility and
85.
38 ◾ SRv6Network Programming
FIGURE 2.5 Abstract SRH.
can carry variable-length data, including encryption, authentication,
and performance monitoring information. Additionally, as we described
in previous sections, the SIDs in a segment list defne instructions to be
executed by nodes. Tis means that each SID can be executed only by the
node advertising it, and other nodes only perform basic table lookup and
forwarding operations. In contrast, the information carried in TLVs can
be processed by any node advertising a SID in the corresponding segment
list. To improve forwarding efciency, a local TLV processing policy that
determines, for example, whether to ignore all TLVs or process only certain
types of TLVs according to interface confgurations, needs to be defned.
SRHs currently support two types of TLVs: Padding TLVs and HMAC
TLVs.[3,4]
2.4.2.1 Padding TLV
Two types of Padding TLVs (Pad1 TLV and PadN TLV) are defned for
SRHs. If the length of subsequent TLVs is not a multiple of 8 bytes, Padding
TLVs can be used for alignment, thereby ensuring that the overall SRH
length is a multiple of 8 bytes. Because Padding TLVs are not instructive,
they are ignored by nodes during SRH processing.
Te Pad1 TLV defnes only the Type feld, which is used for padding a
single byte. If two or more bytes are required, this TLV cannot be used.
Figure 2.6 shows the format of the Pad1 TLV.
Te PadN TLV is used for padding multiple bytes. Figure 2.7 shows the
format of the PadN TLV.
Table 2.2 describes the felds in the PadN TLV.
86.
SRv6 Fundamentals ◾39
FIGURE 2.6 Pad1 TLV.
FIGURE 2.7 PadN TLV.
TABLE 2.2 Fields in the PadN TLV
Field Length Description
Type 8 bits TLV type.
Length 8 bits Length of the Padding feld.
Padding Variable Padding bits, which are used when the packet length is not a
multiple of 8 bytes. As the padding bits are not instructive, they
must be set to 0s before transmission and ignored upon receipt.
2.4.2.2 HMAC TLV
Te HMAC TLV is used to prevent key information in an SRH from being
tampered with. It is optional and requires 8-byte alignment. Figure 2.8
shows the format of the HMAC TLV.
Table 2.3 describes the felds in the HMAC TLV.
FIGURE 2.8 HMAC TLV.
87.
40 ◾ SRv6Network Programming
TABLE 2.3 Fields in the HMAC TLV
Field Length
Type 8 bits
Length 8 bits
D 1 bit
Reserved 15 bits
HMAC 32 bits
Key ID
HMAC Variable (up
to 256 bits)
Description
TLV type.
Length of the HMAC feld.
When this bit is set to 1, DA verifcation is disabled.
Tis feld is mainly used in reduced SRH mode.
Reserved. Tis feld is flled with 0s before transmission
and ignored upon receipt.
Identifes the pre-shared key and algorithm used for
HMAC computation. If this feld is set to 0, the TLV
does not contain the HMAC feld.
HMAC computation result.
2.4.3 SRv6 Instruction Set: Endpoint Node Behaviors
Afer describing SRv6 instructions and SRHs in previous sections, let’s
move onto the details of these instructions. Te IETF draf SRv6 Network
Programming[1] defnes multiple behaviors or instructions. Each SID is
bound to an instruction to specify the action to be taken during SID pro-
cessing. An SRH can encapsulate an ordered list of SIDs, providing packet-
related services including forwarding, encapsulation, and decapsulation.
SRv6 instructions defne the atomic functions of networks. Before
introducing SRv6 instructions, let’s learn the naming rules of these
instructions.
• End: the most basic instruction executed by an endpoint node,
directing the node to terminate the current instruction and start the
next instruction. Te corresponding forwarding behavior is to dec-
rement the SL feld by 1 and copy the SID pointed by the SL feld to
the DA feld in the IPv6 header.
• X: forwards packets through one or a group of Layer 3 outbound
interfaces.
• T: searches a specifed routing table and forwards packets.
• D: decapsulates packets by removing the IPv6 header and related
extension headers.
• V: searches a specifed table for packet forwarding based on Virtual
Local Area Network (VLAN) information.
88.
SRv6 Fundamentals ◾41
• U: searches a specifed table for packet forwarding based on unicast
MAC address information.
• M: searches a Layer 2 forwarding table for multicast forwarding.
• B6: binding to an SRv6 Policy.
• BM: binding to an SR-MPLS Policy.
Table 2.4 describes the functions of common SRv6 instructions, each of
which integrates one or more atomic functions previously listed.
TABLE 2.4 Functions of Common SRv6 Instructions
Instruction Function Description
End Terminates the current
instruction and executes the
next one.
End.X Forwards a packet through a
specifed outbound interface.
End.T Searches a specifed IPv6 routing
table for packet forwarding.
End.DX6 Decapsulates a packet and
forwards it over a specifed IPv6
Layer 3 adjacency.
End.DX4 Decapsulates a packet and
forwards it over a specifed IPv4
Layer 3 adjacency.
End.DT6 Decapsulates a packet and
searches a specifed IPv6 routing
table for packet forwarding.
End.DT4 Decapsulates a packet and
searches a specifed IPv4 routing
table for packet forwarding.
End.DT46 Decapsulates a packet and
searches a specifed IPv4 or IPv6
routing table for packet
forwarding.
Application Scenario
Used for packet forwarding
through a specifed node. An
End SID is similar to an
SR-MPLS node SID.
Used for packet forwarding
through a specifed outbound
interface. An End.X SID is
similar to an SR-MPLS
adjacency SID.
Used in scenarios where
multiple routing tables exist.
Used in L3VPNv6 scenarios
where packets are forwarded to
a Customer Edge (CE) through
a specifed IPv6 adjacency.
Used in L3VPNv4 scenarios
where packets are forwarded to
a CE through a specifed IPv4
adjacency.
Used in L3VPNv6 scenarios.
Used in L3VPNv4 scenarios.
Used in L3VPNv4/L3VPNv6
scenarios.
(Continued)
89.
42 ◾ SRv6Network Programming
TABLE 2.4 (Continued) Functions of Common SRv6 Instructions
Instruction Function Description
End.DX2 Decapsulates a packet and
forwards it through a specifed
Layer 2 outbound interface.
End.DX2V Decapsulates a packet and
searches a specifed Layer 2 table
for packet forwarding based on
inner VLAN information.
End.DT2U Decapsulates a packet, learns the
inner source MAC address in a
specifed Layer 2 table, and
searches the table for packet
forwarding based on the inner
destination MAC address.
End.DT2M Decapsulates a packet, learns the
inner source MAC address in a
specifed Layer 2 table, and
forwards the packet to Layer 2
outbound interfaces excluding
the specifed one.
End.B6.Insert Inserts an SRH and applies a
specifed SRv6 Policy.
End.B6.Insert. Inserts a reduced SRH and
Red applies a specifed SRv6 Policy.
End.B6.Encaps Encapsulates an outer IPv6
header and SRH, and applies a
specifed SRv6 Policy.
End.B6.Encaps. Encapsulates an outer IPv6
Red header and reduced SRH, and
applies a specifed SRv6 Policy.
End.BM Inserts an MPLS label stack and
applies a specifed SR-MPLS
Policy.
Application Scenario
Used in EVPN VPWS scenarios.
Used in EVPN Virtual Private
LAN Service (VPLS) scenarios.
Used in EVPN VPLS unicast
scenarios.
Used in EVPN VPLS multicast
scenarios.
Used in scenarios such as trafc
steering into an SRv6 Policy in
Insert mode, tunnel stitching,
and Sofware Defned Wide
Area Network (SD-WAN)
route selection.
Used in scenarios such as trafc
steering into an SRv6 Policy in
Insert or Reduced mode,
tunnel stitching, and SD-WAN
route selection.
Used in scenarios such as trafc
steering into an SRv6 Policy
in Encaps mode, tunnel
stitching, and SD-WAN route
selection.
Used in scenarios such as trafc
steering into an SRv6 Policy in
Encaps or Reduced mode,
tunnel stitching, and SD-WAN
route selection.
Used in SRv6 and SR-MPLS
interworking scenarios where
trafc needs to be steered into
an SR-MPLS Policy.
90.
SRv6 Fundamentals ◾43
2.4.3.1 End SID
End is the most basic SRv6 instruction. A SID bound to the End instruc-
tion is called an End SID, which identifes a node. An End SID instructs
a network node to forward a packet to the node that advertises the SID.
Afer receiving the packet, the node performs the operations defned by
the SID for packet processing.
Te End SID instruction includes the following operations:
1. Decrements the SL value by 1.
2. Obtains the next SID from the SRH based on the SL value.
3. Updates the DA feld in the IPv6 header with the next SID.
4. Searches the routing table for packet forwarding.
Other parameters, such as Hop Limit, are processed according to the
normal forwarding process.
Te pseudocode of an End SID is as follows:
S01. When an SRH is processed {
S02. If (Segments Left == 0) {
S03. Send an ICMP Parameter Problem message to the Source Address,
code 4 (SR Upper-layer Header Error), pointer set to the
offset of the upper-layer header, interrupt packet processing
and discard the packet
S04. }
S05. If (IPv6 Hop Limit <= 1) {
S06. Send an ICMP Time Exceeded message to the Source Address,
code 0 (Hop limit exceeded in transit), interrupt packet
processing and discard the packet
S07. }
S08. max_LE = (Hdr Ext Len / 2) - 1
S09. If ((Last Entry > max_LE) or (Segments Left > Last Entry+1)) {
S10. Send an ICMP Parameter Problem to the Source Address, code
0 (Erroneous header field encountered), pointer set to the
Segments Left field, interrupt packet processing and discard
the packet
S11. }
S12. Decrement Hop Limit by 1
S13. Decrement Segments Left by 1
S14. Update IPv6 DA with Segment List[Segments Left]
S15. Resubmit the packet to the egress IPv6 FIB lookup and transmission
to the new destination
S16. }
2.4.3.2 End.X SID
End.X (short for Layer-3 cross-connect) supports packet forwarding
to a Layer 3 adjacency over a specifed link. End.X SIDs can be used in
91.
44 ◾ SRv6Network Programming
scenarios including Topology Independent Loop Free Alternate (TI-LFA)
and strict explicit path-based TE.
In essence, End.X SIDs are developed based on End SIDs. End.X can
be disassembled into End+X, where X indicates cross-connect (meaning
that a packet needs to be directly forwarded to a specifed Layer 3 adja-
cency). Terefore, each End.X SID needs to be bound to one or a group of
Layer 3 adjacencies.
Te End.X SID instruction includes the following operations:
1. Decrements the SL value by 1.
2. Obtains the next SID from the SRH based on the SL value.
3. Updates the DA feld in the IPv6 header with the next SID.
4. Forwards the IPv6 packet to the Layer 3 adjacency bound to the
End.X SID.
Te pseudocode of an End.X SID is based on that of an End SID, but with
line S15 being modifed as follows:
S15. Resubmit the packet to the IPv6 module for transmission to the new
IPv6 destination via a member of specific L3 adjacencies
2.4.3.3 End.T SID
End.T (short for specifc IPv6 table lookup) supports packet forwarding
by searching the specifed IPv6 routing table. End.T SIDs can be used in
common IPv6 routing and VPN scenarios.
End.T SIDs are also developed based on End SIDs. End.T can be
disassembled into End+T, where T indicates table lookup for packet
forwarding. Terefore, each End.T SID needs to be bound to an IPv6
routing table.
Te End.T SID instruction includes the following operations:
1. Decrements the SL value by 1.
2. Obtains the next SID from the SRH based on the SL value.
3. Updates the DA feld in the IPv6 header with the next SID.
4. Searches the specifed routing table for IPv6 packet forwarding.
92.
SRv6 Fundamentals ◾45
Te pseudocode of an End.T SID is based on that of an End SID, but with
line S15 being modifed as follows:
S15.1. Set the packet's associated FIB table to the specific IPv6 FIB
S15.2. Resubmit the packet to the egress IPv6 FIB lookup and transmission
to the new IPv6 destination
2.4.3.4 End.DX6 SID
End.DX6(shortfordecapsulationandIPv6cross-connect)supportspacket
decapsulation and forwarding to a specifed IPv6 Layer 3 adjacency. End.
DX6 SIDs are mainly used in L3VPNv6 scenarios as per-CE VPN SIDs.
End.DX6 can be disassembled into End+D+X6, where D indicates
decapsulation, and X6 indicates IPv6 cross-connect (meaning that a
packet needs to be directly forwarded to a specifed IPv6 Layer 3 adja-
cency). Terefore, each End.DX6 SID needs to be bound to one or a group
of IPv6 Layer 3 adjacencies.
Te End.DX6 SID instruction includes the following operations:
1. Removes the outer IPv6 header and SRH.
2. Forwards the inner IPv6 packet to the IPv6 Layer 3 adjacency bound
to the End.DX6 SID.
Te pseudocode of an End.DX6 SID is as follows:
S01. If (Upper-Layer Header type != 41) {
S02. Send an ICMP Parameter Problem message to the Source Address,
code 4 (SR Upper-layer Header Error), pointer set to the offset
of the upper-layer header, interrupt packet processing and discard
the packet
S03. }
S04. Remove the outer IPv6 Header with all its extension headers
S05. Forward the exposed IPv6 packet to a member of
specific L3 adjacencies
2.4.3.5 End.DX4 SID
End.DX4 (short for decapsulation and IPv4 cross-connect) supports packet
decapsulation and forwarding to a specifed IPv4 Layer 3 adjacency. End.
DX4 SIDs are mainly used in L3VPNv4 scenarios as per-CE VPN SIDs.
End.DX4 can be disassembled into End+D+X4, where D indicates
decapsulation, and X4 indicates IPv4 cross-connect (meaning that a packet
needs to be directly forwarded to a specifed IPv4 Layer 3 adjacency).
93.
46 ◾ SRv6Network Programming
Terefore, each End.DX4 SID needs to be bound to one or a group of IPv4
Layer 3 adjacencies.
Te End.DX4 SID instruction includes the following operations:
1. Removes the outer IPv6 header and SRH.
2. Forwards the inner IPv4 packet to the IPv4 Layer 3 adjacency bound
to the End.DX4 SID.
Te pseudocode of an End.DX4 SID is as follows:
S01. If (Upper-Layer Header type != 4) {
S02. Send an ICMP Parameter Problem message to the Source Address,
code 4 (SR Upper-layer Header Error), pointer set to the
offset of the upper-layer header, interrupt packet processing and
discard the packet
S03. }
S04. Remove the outer IPv6 Header with all its extension headers
S05. Forward the exposed IPv4 packet to a member of specific
L3 adjacencies
2.4.3.6 End.DT6 SID
End.DT6 (short for decapsulation and specifc IPv6 table lookup) sup-
ports packet decapsulation and lookup in a specifed IPv6 routing table for
packet forwarding. End.DT6 SIDs are mainly used in L3VPNv6 scenarios
as per-VPN SIDs.
End.DT6 can be disassembled into End+D+T6, where D indi-
cates decapsulation and T6 indicates IPv6 table lookup for forwarding.
Terefore, each End.DT6 SID needs to be bound to an IPv6 routing table,
which can be either the IPv6 routing table of a VPN instance or a public
IPv6 routing table.
Te End.DT6 SID instruction includes the following operations:
1. Removes the outer IPv6 header and SRH.
2. Searches the IPv6 routing table bound to the End.DT6 SID to for-
ward the inner IPv6 packet.
Te pseudocode of an End.DT6 SID is as follows:
S01. If (Upper-Layer Header type != 41) {
S02. Send an ICMP Parameter Problem message to the Source Address,
code 4 (SR Upper-layer Header Error), pointer set to the
offset of the upper-layer header, interrupt packet processing
94.
SRv6 Fundamentals ◾47
and discard the packet
S03. }
S04. Remove the outer IPv6 Header with all its extension headers
S05. Set the packet's associated FIB table to the specific IPv6 FIB
S06. Resubmit the packet to the egress IPv6 FIB lookup and transmission
to the new IPv6 destination
2.4.3.7 End.DT4 SID
End.DT4 (short for decapsulation and specifc IPv4 table lookup) supports
packet decapsulation and lookup in a specifed IPv4 routing table for
packet forwarding. End.DT4 SIDs are mainly used in L3VPNv4 scenarios
as per-VPN SIDs.
End.DT4 can be disassembled into End+D+T4, where D indi-
cates decapsulation and T4 indicates IPv4 table lookup for forwarding.
Terefore, each End.DT4 SID needs to be bound to an IPv4 routing table,
which can be either the IPv4 routing table of a VPN instance or a public
IPv4 routing table.
Te End.DT4 SID instruction includes the following operations:
1. Removes the outer IPv6 header and SRH.
2. Searches the IPv4 routing table bound to the End.DT4 SID to
forward the inner IPv4 packet.
Te pseudocode of an End.DT4 SID is as follows:
S01. If (Upper-layer Header type == 4) {
S02. Remove the outer IPv6 Header with all its extension headers
S03. Set the packet's associated FIB table to the specific IPv4 FIB
S04. Resubmit the packet to the egress IPv4 FIB lookup and transmission
to the new IPv4 destination
S05. } Else if (Upper-layer Header type == 41) {
S06. Remove the outer IPv6 Header with all its extension headers
S07. Set the packet's associated FIB table to the specific IPv6 FIB
S08. Resubmit the packet to the egress IPv6 FIB lookup and transmission
to the new IPv6 destination
S09. } Else {
S10. Send an ICMP Parameter Problem message to the Source Address,
code 4 (SR Upper-layer Header Error), pointer set to the offset
of the upper-layer header, interrupt packet processing and discard
the packet
S11. }
2.4.3.8 End.DT46 SID
End.DT46 (short for decapsulation and specifc IP table lookup) supports
packet decapsulation and lookup in a specifed IPv4 or IPv6 routing table
95.
48 ◾ SRv6Network Programming
for packet forwarding. End.DT46 SIDs are mainly used in L3VPN sce-
narios as per-VPN SIDs.
End.DT46 can be disassembled into End+D+T46, where D indicates
decapsulation and T46 indicates IPv4 or IPv6 table lookup for forward-
ing. Terefore, each End.DT46 SID needs to be bound to an IPv4 or IPv6
routing table, which can be either the IPv4 or IPv6 routing table of a VPN
instance or a public IPv4 or IPv6 routing table.
Te End.DT46 SID instruction includes the following operations:
1. Removes the outer IPv6 header and SRH.
2. According to the Layer 3 protocol type of the inner IP packet,
searches the IPv4 or IPv6 routing table bound to the End.DT46 SID
to forward the inner IP packet.
Te pseudocode of an End.DT46 SID is as follows:
S01. If (Upper-layer Header type == 4) {
S02. Remove the outer IPv6 Header with all its extension headers
S03. Set the packet's associated FIB table to the specific IPv4 FIB
S04. Resubmit the packet to the egress IPv4 FIB lookup and transmission
to the new IPv4 destination
S05. } Else if (Upper-layer Header type == 41) {
S06. Remove the outer IPv6 Header with all its extension headers
S07. Set the packet's associated FIB table to the specific IPv6 FIB
S08. Resubmit the packet to the egress IPv6 FIB lookup and transmission
to the new IPv6 destination
S09. } Else {
S10. Send an ICMP Parameter Problem message to the Source Address,
code 4 (SR Upper-layer Header Error), pointer set to the offset
of the upper-layer header, interrupt packet processing and discard
the packet
S11. }
2.4.3.9 End.DX2 SID
End.DX2 (short for decapsulation and L2 cross-connect) supports packet
decapsulation and forwarding through a specifed Layer 2 outbound inter-
face. End.DX2 SIDs are mainly used in Layer 2 Virtual Private Network
(L2VPN) and EVPN Virtual Private Wire Service (VPWS)[5] scenarios.
End.DX2 can be disassembled into End+D+X2, where D indicates
decapsulation and X2 indicates cross-connect (meaning that a packet
needs to be directly forwarded through a specifed Layer 2 interface).
Terefore, each End.DX2 SID needs to be bound to a Layer 2 outbound
interface.
96.
SRv6 Fundamentals ◾49
Te End.DX2 SID instruction includes the following operations:
1. Removes the outer IPv6 header and SRH.
2. Forwards the inner Ethernet frame to the Layer 2 outbound interface
bound to the End.DX2 SID.
Te pseudocode of an End.DX2 SID is as follows:
S01. If (Upper-Layer Header type != 143) {
S02. Send an ICMP Parameter Problem message to the Source Address,
code 4 (SR Upper-layer Header Error), pointer set to the offset
of the upper-layer header, interrupt packet processing and
discard the packet
S03. }
S04. Remove the outer IPv6 Header with all its extension headers and
forward the Ethernet frame to the specific outgoing L2 interface
2.4.3.10 End.DX2V SID
End.DX2V (short for decapsulation and VLAN L2 table lookup) supports
packet decapsulation and lookup in a specifed Layer 2 table based on the
inner VLAN information of the packet to be forwarded. End.DX2V SIDs
are mainly used in EVPN fexible cross-connect scenarios.[5]
End.DX2V can be disassembled into End+D+X2V, where D indicates
decapsulation and X2V indicates VLAN Layer 2 table lookup. Terefore,
each End.DX2V SID needs to be bound to a Layer 2 table.
Te End.DX2V SID instruction includes the following operations:
1. Removes the outer IPv6 header and SRH.
2. Searches the Layer 2 table bound to the End.DX2V SID based on the
VLAN information in the inner Ethernet frame.
Te pseudocode of an End.DX2V SID is based on that of an End.DX2 SID,
but with line S04 being modifed as follows:
S04. Remove the outer IPv6 Header with all its extension headers, lookup
the exposed inner VLANs in the specific L2 table, and forward via
the matched table entry.
2.4.3.11 End.DT2U SID
End.DT2U (short for decapsulation and unicast MAC L2 table lookup)
supports packet decapsulation, inner source MAC address learning and
97.
50 ◾ SRv6Network Programming
saving to a specifed Layer 2 table, and lookup in the table for packet for-
warding based on the inner destination MAC address. End.DT2U SIDs
are mainly used in EVPN bridging unicast scenarios.[5]
End.DT2U can be disassembled into End+D+T2U, where D indicates
decapsulation and T2U indicates Layer 2 table lookup for unicast forward-
ing. Terefore, each End.DT2U SID needs to be bound to a Layer 2 table.
Te End.DT2U SID instruction includes the following operations:
1. Removes the outer IPv6 header and SRH.
2. Learns the source MAC address of the inner Ethernet frame.
3. Saves the source MAC address to the Layer 2 table bound to the End.
DT2U SID.
4. Searches the Layer 2 table bound to the End.DT2U SID based on the
destination MAC address of the inner Ethernet frame.
5. Forwards the corresponding packet accordingly.
Te pseudocode of an End.DT2U SID is as follows:
S01. If (Upper-Layer Header type != 143) {
S02. Send an ICMP Parameter Problem message to the Source Address,
code 4 (SR Upper-layer Header Error), pointer set to the
offset of the upper-layer header, interrupt packet processing
and discard the packet
S03. }
S04. Remove the IPv6 header and all its extension headers
S05. Learn the exposed inner MAC Source Address in the specific
L2 table (T)
S06. Lookup the exposed inner MAC Destination Address in table T
S07. If (matched entry in T) {
S08. Forward via the matched table T entry
S09. } Else {
S10. Forward via all outgoing L2 interfaces entries in table T
S11. }
2.4.3.12 End.DT2M SID
End.DT2M (short for decapsulation and L2 table fooding) supports
packet decapsulation, inner source MAC address learning and saving
to a specifed Layer 2 table, and packet forwarding to Layer 2 outbound
interfaces excluding the specifed one. End.DT2M SIDs are mainly used
in EVPN bridging Broadcast & Unknown-unicast & Multicast (BUM)[5]
and EVPN E-Tree[6] scenarios.
End.DT2M can be disassembled into End+D+T2M, where D indi-
cates decapsulation and T2M indicates Layer 2 table lookup for multicast
98.
SRv6 Fundamentals ◾51
forwarding. Terefore, each End.DT2M SID needs to be bound to a Layer
2 table and may carry EVPN ESI fltering or EVPN E-Tree parameters in
order to exclude specifed outbound interfaces during fooding.
Te End.DT2M SID instruction includes the following operations:
1. Removes the outer IPv6 header and SRH.
2. Learns the source MAC address of the inner Ethernet frame.
3. Saves the source MAC address to the Layer 2 table bound to the End.
DT2M SID.
4. Forwards the inner Ethernet frame through Layer 2 outbound inter-
faces excluding the interface specifed by the parameters carried in
the End.DT2M SID.
Te pseudocode of an End.DT2M SID is as follows:
S01. If (Upper-Layer Header type != 143) {
S02. Send an ICMP Parameter Problem message to the Source Address,
code 4 (SR Upper-layer Header Error), pointer set to the offset
of the upper-layer header, interrupt packet processing and
discard the packet
S03. }
S04. Remove the IPv6 header and all its extension headers
S05. Learn the exposed inner MAC Source Address in the specific L2 table
S06. Forward via all outgoing L2 interfaces excluding the one specified
in Arg.FE2
2.4.3.13 End.B6.Insert SID
End.B6.Insert (short for endpoint bound to an SRv6 Policy with Insert)
supports the application of specifed SRv6 Policies[7] to packets. End.
B6.Insert SIDs instantiate binding SIDs in SRv6 and are used in scenarios
where TE can be fexibly implemented across SRv6 domains.
End.B6.Insert can be disassembled into End+B6+Insert, where B6
indicates the application of an SRv6 Policy and Insert indicates the inser-
tion of an SRH afer the IPv6 header. Terefore, each End.B6.Insert SID
needs to be bound to an SRv6 Policy.
Te End.B6.Insert SID instruction includes the following operations:
1. Inserts an SRH (including segment lists) afer the IPv6 header.
2. Sets the DA to the frst SID of a specifed SRv6 Policy.
3. Searches the IPv6 routing table.
4. Forwards the new IPv6 packet accordingly.
99.
52 ◾ SRv6Network Programming
Te pseudocode of an End.B6.Insert SID is as follows:
S01. When an SRH is processed {
S02. If (Segments Left == 0) {
S03. Send an ICMP Parameter Problem message to the Source Address,
code 4 (SR Upper-layer Header Error), pointer set to the
offset of the upper-layer header, interrupt packet processing
and discard the packet
S04. }
S04. If (IPv6 Hop Limit <= 1) {
S05. Send an ICMP Time Exceeded message to the Source Address,
code 0 (Hop limit exceeded in transit), interrupt packet
processing and discard the packet
S06. }
S07. max_LE = (Hdr Ext Len / 2) - 1
S08. If ((Last Entry > max_LE) or (Segments Left > (Last Entry+1)){
S09. Send an ICMP Parameter Problem to the Source Address, code 0
(Erroneous header field encountered), pointer set to the
Segments Left field, interrupt packet processing and discard
the packet
S11. }
S12. Decrement Hop Limit by 1
S13. Insert a new SRH after the IPv6 Header and the SRH
contains the list of segments of the specific SRv6 Policy
S14. Set the IPv6 DA to the first segment of the SRv6 Policy
S15. Resubmit the packet to the egress IPv6 FIB lookup and transmission
to the new IPv6 destination
S16. }
Inaddition,theEnd.B6.Insert.RedinstructionisanoptimizationoftheEnd.
B6.Insert instruction, except that the former requires a reduced SRH (i.e.,
the SRH excluding the frst SID of the involved SRv6 Policy) to be added.
2.4.3.14 End.B6.Encaps SID
End.B6.Encaps (short for endpoint bound to an SRv6 Policy with Encaps)
supports the application of specifed SRv6 Policies[7] to packets. End.
B6.Encaps SIDs also instantiate binding SIDs in SRv6 and are used in sce-
narios where TE can be fexibly implemented across SRv6 domains.
End.B6.Encaps can be disassembled into End+B6+Encaps, where
B6 indicates the application of an SRv6 Policy and Encaps indicates the
encapsulation of an outer IPv6 header and SRH. Terefore, each End.
B6.Encaps SID needs to be bound to an SRv6 Policy.
Te End.B6.Encaps SID instruction includes the following operations:
1. Decrements the SL value of the inner SRH by 1.
2. Updates the DA feld of the inner IPv6 header with the SID to which
the SL feld is pointing.
100.
SRv6 Fundamentals ◾53
3. Encapsulates an IPv6 header and SRH (including segment lists).
4. Sets the source address to the address of the current node and the
DA to the frst SID of the involved SRv6 Policy.
5. Sets other felds in the outer IPv6 header.
6. Searches the IPv6 routing table.
7. Forwards the new IPv6 packet accordingly.
Te pseudocode of an End.B6.Encaps SID is as follows:
S01. When an SRH is processed {
S02. If (Segments Left == 0) {
S03. Send an ICMP Parameter Problem message to the Source Address,
code 4 (SR Upper-layer Header Error), pointer set to the offset
of the upper-layer header, interrupt packet processing and
discard the packet
S04. }
S05. If (IPv6 Hop Limit <= 1) {
S06. Send an ICMP Time Exceeded message to the Source Address, code
0 (Hop limit exceeded in transit), interrupt packet processing
and discard the packet
S07. }
S08. max_LE = (Hdr Ext Len / 2) - 1
S09. If ((Last Entry > max_LE) or (Segments Left > (Last Entry+1)) {
S10. Send an ICMP Parameter Problem to the Source Address, code 0
(Erroneous header field encountered), pointer set to the
Segments Left field, interrupt packet processing and discard
the packet
S11. }
S12. Decrement Hop Limit by 1
S13. Decrement Segments Left by 1
S14. Update the inner IPv6 DA with inner Segment List[Segments Left]
S15. Push a new IPv6 header with its own SRH containing the list of
segments of the SRv6 Policy
S16. Set the outer IPv6 SA to itself
S17. Set the outer IPv6 DA to the first SID of the SRv6 Policy
S18. Set the outer Payload Length, Traffic Class, Flow Label and
Next Header fields
S19. Resubmit the packet to the egress IPv6 FIB lookup and transmission
to the new IPv6 destination
S20. }
In addition, the End.B6.Encaps.Red instruction is an optimization of the
End.B6.Encaps instruction, with the diference lying in the fact that the
former requires a reduced SRH to be added.
2.4.3.15 End.BM SID
End.BM (short for endpoint bound to an SR-MPLS Policy) supports the
application of a specifed SR-MPLS Policy[7] to packets. End.BM SIDs