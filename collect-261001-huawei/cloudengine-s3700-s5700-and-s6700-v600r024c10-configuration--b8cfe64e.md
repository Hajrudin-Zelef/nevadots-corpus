QoS Configuration


QoS Configuration

Issue    01
Date     2025-03-03




HUAWEI TECHNOLOGIES CO., LTD.
Copyright © Huawei Technologies Co., Ltd. 2025. All rights reserved.
No part of this document may be reproduced or transmitted in any form or by any means without prior
written consent of Huawei Technologies Co., Ltd.

Trademarks and Permissions

      and other Huawei trademarks are trademarks of Huawei Technologies Co., Ltd.
All other trademarks and trade names mentioned in this document are the property of their respective
holders.

Notice
The purchased products, services and features are stipulated by the contract made between Huawei and
the customer. All or part of the products, services and features described in this document may not be
within the purchase scope or the usage scope. Unless otherwise specified in the contract, all statements,
information, and recommendations in this document are provided "AS IS" without warranties, guarantees
or representations of any kind, either express or implied.

The information in this document is subject to change without notice. Every effort has been made in the
preparation of this document to ensure accuracy of the contents, but all statements, information, and
recommendations in this document do not constitute a warranty of any kind, express or implied.




Huawei Technologies Co., Ltd.
Address:       Huawei Industrial Base
               Bantian, Longgang
               Shenzhen 518129
               People's Republic of China

Website:       https://e.huawei.com




Issue 01 (2025-03-03)          Copyright © Huawei Technologies Co., Ltd.                                  i
QoS Configuration
QoS Configuration                                                                                                                                                             Contents




                                                                                                                                                        Contents

1 About This Document.............................................................................................................1
2 Overview of QoS...................................................................................................................... 5
3 MQC Configuration................................................................................................................. 9
3.1 Overview of MQC.................................................................................................................................................................... 9
3.2 Understanding MQC............................................................................................................................................................ 10
3.3 Configuration Precautions for MQC............................................................................................................................... 12
3.4 Configuring a Traffic Classifier......................................................................................................................................... 12
3.5 Configuring a Traffic Behavior..........................................................................................................................................17
3.6 Configuring a Traffic Policy............................................................................................................................................... 19
3.7 Applying a Traffic Policy..................................................................................................................................................... 19
3.8 Verifying the Configuration............................................................................................................................................... 21
3.9 Maintaining MQC................................................................................................................................................................. 22

4 Packet Filtering Configuration........................................................................................... 23
4.1 Overview of Packet Filtering............................................................................................................................................. 23
4.2 Configuration Precautions for Packet Filtering...........................................................................................................23
4.3 Configuring MQC-based Packet Filtering..................................................................................................................... 23
4.4 Example for Configuring MQC-based Packet Filtering............................................................................................ 27
4.5 Example for Configuring Access Control Based on Source MAC Addresses..................................................... 29

5 Traffic Statistics Collection Configuration...................................................................... 33
5.1 Overview of Traffic Statistics Collection........................................................................................................................33
5.2 Configuration Precautions for Traffic Statistics Collection..................................................................................... 34
5.3 Configuring MQC-based Traffic Statistics Collection................................................................................................ 34
5.4 Example for Configuring MQC-based Traffic Statistics Collection.......................................................................37

6 Re-marking Configuration.................................................................................................. 40
6.1 Overview of Re-marking.................................................................................................................................................... 40
6.2 Re-marking..............................................................................................................................................................................40
6.3 Configuration Precautions for Re-marking.................................................................................................................. 42
6.4 Configuring MQC-based Priority Re-marking............................................................................................................. 42
6.5 Example for Configuring Re-marking to Distinguish Users................................................................................... 45
6.6 Example for Configuring Re-marking to Distinguish Services.............................................................................. 49

7 Redirection Configuration................................................................................................... 53

Issue 01 (2025-03-03)                                   Copyright © Huawei Technologies Co., Ltd.                                                                                          ii
QoS Configuration
QoS Configuration                                                                                                                                                           Contents

7.1 Overview of Redirection..................................................................................................................................................... 53
7.2 Configuration Precautions for Redirection................................................................................................................... 54
7.3 Configuring MQC-based Redirection............................................................................................................................. 54
7.4 Example for Configuring Redirection to an Interface.............................................................................................. 57
7.5 Example for Configuring Redirection to a Next-Hop Address...............................................................................61
7.6 Example for Configuring Association Between Redirection to a Next-Hop Address and NQA................. 65
7.7 Example for Configuring Redirection to Implement Route Selection.................................................................70

8 Priority Mapping Configuration.........................................................................................75
8.1 Overview of Priority Mapping.......................................................................................................................................... 75
8.2 Understanding Priority Mapping..................................................................................................................................... 76
8.3 Configuration Precautions for Priority Mapping........................................................................................................ 79
8.4 Default Settings for Priority Mapping........................................................................................................................... 79
8.5 Configuring DiffServ Domain-based Priority Mapping............................................................................................85
8.5.1 Specifying the Packet Priority Trusted on an Interface........................................................................................ 85
8.5.2 Configuring a DiffServ Domain.................................................................................................................................... 86
8.5.3 Applying the DiffServ Domain...................................................................................................................................... 86
8.5.4 Verifying the Configuration............................................................................................................................................87
8.5.5 Example for Configuring Priority Mapping...............................................................................................................87
8.6 Configuring an Interface Priority..................................................................................................................................... 90
8.7 Configuring the Internal Priority of Protocol Packets Sent by the Local Device.............................................91
8.8 Configuring the Mapping Between Internal Priorities and Queues.................................................................... 91
8.9 Configuring a Packet Priority............................................................................................................................................ 92
8.10 Troubleshooting Priority Mapping................................................................................................................................ 92
8.10.1 Packets Enter Incorrect Queues................................................................................................................................. 92
8.10.2 Priority Mapping Results Are Incorrect....................................................................................................................94

9 Traffic Policing, Traffic Shaping, and Interface-based Rate Limiting Configuration
.......................................................................................................................................................96
9.1 Overview of Traffic Policing, Traffic Shaping, and Interface-based Rate Limiting......................................... 96
9.2 Understanding Traffic Policing, Traffic Shaping, and Interface-based Rate Limiting.................................... 97
9.2.1 Traffic Metering and Token Bucket Mechanism..................................................................................................... 97
9.2.2 Traffic Policing.................................................................................................................................................................. 107
9.2.3 Traffic Shaping................................................................................................................................................................. 107
9.2.4 Interface-based Rate Limiting.................................................................................................................................... 108
9.3 Configuration Precautions for Traffic Policing, Traffic Shaping, and Interface-based Rate Limiting.....109
9.4 Default Settings for Traffic Policing, Traffic Shaping, and Interface-based Rate Limiting........................ 109
9.5 Configuring Traffic Policing............................................................................................................................................. 110
9.5.1 Configuring MQC-based Traffic Policing (Level-1 CAR).................................................................................... 110
9.5.2 Configuring Traffic Policing (Level-2 CAR)............................................................................................................. 112
9.5.3 Verifying the Configuration......................................................................................................................................... 113
9.5.4 Example for Configuring MQC-based Traffic Policing (Level-1 CAR)........................................................... 114
9.5.5 Example for Configuring Traffic Policing (Level-2 CAR)....................................................................................118


Issue 01 (2025-03-03)                                   Copyright © Huawei Technologies Co., Ltd.                                                                                        iii
QoS Configuration
QoS Configuration                                                                                                                                                                          Contents

9.5.6 Example for Configuring Traffic Policing to Limit the Rate of Each IP Address on a Network Segment
......................................................................................................................................................................................................... 123
9.6 Configuring Traffic Shaping............................................................................................................................................ 126
9.6.1 Configuring Traffic Shaping for a Queue................................................................................................................ 126
9.6.2 Verifying the Configuration......................................................................................................................................... 127
9.6.3 Example for Configuring Traffic Shaping to Limit the Rate of Different Services.................................... 127
9.6.4 Example for Configuring Traffic Shaping Based on Priority Mapping in a DiffServ Domain............... 131
9.6.5 Example for Configuring Traffic Shaping Based on Trusted 802.1p Priorities........................................... 133
9.7 Configuring Interface-based Rate Limiting............................................................................................................... 136
9.7.1 Configuring Traffic Policing to Implement Interface-based Rate Limiting................................................. 136
9.7.2 Configuring Traffic Shaping to Implement Interface-based Rate Limiting................................................. 137
9.7.3 Configuring Rate Limiting on the Management Interface............................................................................... 138
9.7.4 Example for Configuring Traffic Policing to Limit the Rate on an Interface.............................................. 139
9.8 Configuring a QoS Profile................................................................................................................................................ 140
9.9 Maintaining Traffic Policing, Traffic Shaping, and Interface-based Rate Limiting.......................................141

10 Congestion Avoidance Configuration.......................................................................... 142
10.1 Overview of Congestion Avoidance........................................................................................................................... 142
10.2 Understanding Congestion Avoidance......................................................................................................................142
10.2.1 WRED Fundamentals...................................................................................................................................................143
10.2.2 Fundamentals of CFI (Used as the Internal Drop Priority)............................................................................ 145
10.3 Configuration Precautions for Congestion Avoidance.........................................................................................145
10.4 Default Settings for Congestion Avoidance............................................................................................................ 145
10.5 Configuring WRED........................................................................................................................................................... 146
10.6 Configuring the CFI as the Internal Drop Priority.................................................................................................147
10.7 Verifying the Configuration.......................................................................................................................................... 147
10.8 Maintaining Congestion Avoidance........................................................................................................................... 148
10.9 Example for Configuring WRED.................................................................................................................................. 148

11 Congestion Management Configuration.....................................................................153
11.1 Overview of Congestion Management.....................................................................................................................153
11.2 Understanding Congestion Management............................................................................................................... 154
11.3 Configuration Precautions for Congestion Management.................................................................................. 158
11.4 Default Settings for Congestion Management...................................................................................................... 158
11.5 Configuring Congestion Management..................................................................................................................... 158
11.6 (Optional) Configuring Buffer Management......................................................................................................... 160
11.6.1 Understanding Buffer Management...................................................................................................................... 160
11.6.2 Configuring a Burst Traffic Buffering Mode........................................................................................................ 160
11.6.3 Configuring the Buffer Size....................................................................................................................................... 162
11.6.4 Configuring the Device to Generate an Alarm When the Queue Buffer Usage Exceeds the
Threshold...................................................................................................................................................................................... 162
11.6.5 Verifying the Configuration....................................................................................................................................... 163
11.7 (Optional) Configuring Congestion Monitoring....................................................................................................163
11.7.1 Understanding Congestion Monitoring................................................................................................................ 163

Issue 01 (2025-03-03)                                        Copyright © Huawei Technologies Co., Ltd.                                                                                                   iv
QoS Configuration
QoS Configuration                                                                                                                                                  Contents

11.7.2 Configuring Congestion Monitoring...................................................................................................................... 163
11.8 Maintaining Congestion Management.....................................................................................................................164
11.9 Example for Configuring Congestion Management............................................................................................ 164
11.10 Example for Configuring Congestion Avoidance and Congestion Management (PQ+WDRR
Scheduling and WRED Profile)..............................................................................................................................................168
11.11 Example for Configuring Congestion Monitoring.............................................................................................. 172
11.12 Microburst Detection.................................................................................................................................................... 174
11.12.1 Configuring Microburst Detection........................................................................................................................ 174
11.12.2 Verifying the Configuration.................................................................................................................................... 175

12 MPLS QoS Configuration.................................................................................................177
12.1 Overview of MPLS QoS.................................................................................................................................................. 177
12.2 Understanding MPLS QoS.............................................................................................................................................178
12.3 Configuration Precautions for MPLS QoS................................................................................................................184
12.4 Default Settings for MPLS QoS................................................................................................................................... 184
12.5 Configuring a DiffServ Mode....................................................................................................................................... 186
12.6 Configuring Priority Mapping...................................................................................................................................... 188
12.6.1 Configuring a DiffServ Domain............................................................................................................................... 188
12.6.2 Applying a DiffServ Domain..................................................................................................................................... 189
12.7 Checking the MPLS QoS Configuration.................................................................................................................... 190
12.8 Example for Configuring MPLS QoS..........................................................................................................................190

13 Experience Assurance Configuration............................................................................200
13.1 Overview of Experience Assurance............................................................................................................................ 200
13.2 Understanding Experience Assurance....................................................................................................................... 200
13.3 Configuration Precautions for Experience Assurance.......................................................................................... 203
13.4 Default Settings for Experience Assurance..............................................................................................................276
13.5 Configuring Experience Assurance............................................................................................................................. 276
13.5.1 Updating a Service Awareness Signature Database......................................................................................... 276
13.5.2 Configuring MQC-based Experience Assurance................................................................................................. 279
13.5.3 (Optional) Configuring Global Parameters for Experience Assurance.......................................................280
13.5.4 Verifying the Configuration....................................................................................................................................... 283
13.5.5 Example for Configuring MQC-based Experience Assurance........................................................................ 284
13.5.6 Example for Configuring Experience Assurance for Traffic Statistics Collection.....................................287




Issue 01 (2025-03-03)                                Copyright © Huawei Technologies Co., Ltd.                                                                                  v
QoS Configuration
QoS Configuration                                                                1 About This Document




                                            1         About This Document

Intended Audience
                    This document is intended for network engineers responsible for switch
                    management and maintenance. You should be familiar with basic Ethernet
                    knowledge and have extensive network management experience. In addition, you
                    should understand your network well, including the network topology and
                    deployed network services.

Symbol Conventions
                    The symbols used in this document are described in the following table. They are
                    defined as follows.

                     Symbol                                   Description

                                                              Indicates a hazard with a high level of
                                                              risk which, if not avoided, will result in
                                                              death or serious injury.

                                                              Indicates a hazard with a medium
                                                              level of risk which, if not avoided,
                                                              could result in death or serious injury.

                                                              Indicates a hazard with a low level of
                                                              risk which, if not avoided, could result
                                                              in minor or moderate injury.

                                                              Indicates a potentially hazardous
                                                              situation which, if not avoided, could
                                                              result in equipment damage, data loss,
                                                              performance deterioration, or
                                                              unanticipated results.
                                                              NOTICE is used to address practices
                                                              not related to personal injury.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                  1
QoS Configuration
QoS Configuration                                                                     1 About This Document


                     Symbol                                          Description

                                                                     Supplements the important
                                                                     information in the main text.
                                                                     NOTE is used to address information
                                                                     not related to personal injury,
                                                                     equipment damage, and environment
                                                                     deterioration.




Command Conventions
                     Convention                        Description

                     Boldface                          The keywords of a command line are in boldfaces.

                     Italic                            Command arguments are in italic.

                     []                                Items (keywords or arguments) in square brackets
                                                       [ ] are optional.

                     { x | y | ... }                   Alternative items are grouped in braces and
                                                       separated by vertical bars. One is selected.

                     [ x | y | ... ]                   Optional alternative items are grouped in square
                                                       brackets and separated by vertical bars. One or none
                                                       is selected.

                     { x | y | ... } *                 Alternative items are grouped in braces and
                                                       separated by vertical bars. A minimum of one or a
                                                       maximum of all can be selected.

                     [ x | y | ... ] *                 Optional alternative items are grouped in square
                                                       brackets and separated by vertical bars. Many or
                                                       none can be selected.

                     &<1-n>                            This parameter before the & sign can be repeated 1
                                                       to n times.

                     #                                 This parameter before the # sign can be repeated 1
                                                       to n times.




Interface Numbering Conventions
                    Interface numbers used in this manual are examples. In device configuration, use
                    the existing interface numbers on devices.

Security Conventions
                    ●     Password setting
                          –     Configuring a ciphertext password is recommended. For security
                                purposes, do not disable password complexity check, and change the
                                password periodically.

Issue 01 (2025-03-03)                  Copyright © Huawei Technologies Co., Ltd.                              2
QoS Configuration
QoS Configuration                                                               1 About This Document


                        –   When configuring a cleartext password, do not start and end the
                            password with %+%# or %@%# because this will allow the password to
                            be considered as a valid ciphertext that can be decrypted by the device
                            and make it visible in the configuration file.
                        –   Multiple features cannot use the same ciphertext password. For example,
                            the ciphertext password set for the AAA feature cannot be used for other
                            features.
                    ●   Encryption algorithms
                        Currently, the device supports the following encryption algorithms: DES, 3DES,
                        AES, DSA, RSA, DH, ECDH, HMAC, SHA1, SHA2, and MD5. Select an
                        encryption algorithm according to the application scenario. Use the
                        recommended encryption algorithm; otherwise, security protection
                        requirements may not be met.
                        –   Recommended symmetric encryption algorithm: AES (with a 128-bit or
                            longer key).
                        –   Recommended asymmetric encryption algorithm: RSA (with a 3072-bit or
                            longer key). Use different key pairs for encryption and signature.
                        –   Recommended encryption algorithm for the digital signature: RSA (with a
                            3072-bit or longer key).
                        –   Recommended encryption algorithm for key negotiation: DH (with a
                            3072-bit or longer key) or ECDH (with a 256-bit or longer key).
                        –   Recommended hash algorithm: SHA2 (256-bit or higher).
                        –   Recommended hash-based message authentication code (HMAC)
                            algorithm: HMAC-SHA2.
                        –   The SHA1, SHA2, and MD5 encryption algorithms are irreversible, and the
                            DES, 3DES, RSA, and AES encryption algorithms are reversible.
                        –   In SSH2.0, when the symmetric encryption algorithm in CBC mode is
                            used, data may be subject to a plaintext-recovery attack, causing
                            disclosure of encrypted data. Therefore, you are not advised to use the
                            CBC mode for data encryption in SSH2.0.
                        –   SSL provides a handshake mechanism that allows a client and a server to
                            establish a session, authenticate each other's identity, and negotiate the
                            key and cipher suite. It is recommended that a cipher suite of TLS 1.2 or a
                            later version be used during communication. In TLS versions, when the
                            symmetric encryption algorithm in CBC mode is used, data may be
                            subject to a plaintext-recovery attack, causing disclosure of encrypted
                            data. Therefore, you are not advised to use the CBC mode for data
                            encryption in TLS versions.
                    ●   Personal data
                        Some personal data (such as MAC or IP addresses of terminals) may be
                        obtained or used during operation or fault locating of your purchased
                        products, services, or features, so you have an obligation to make privacy
                        policies and take proper measures according to applicable laws of the country
                        to fully protect personal data.
                    ●   The terms mirrored port, port mirroring, flow mirroring, and mirroring in this
                        document are mentioned only to describe the purpose of detecting faults and
                        errors in communication transmission. They do not involve collection or
                        processing of any personal information or communication data of users.

Issue 01 (2025-03-03)          Copyright © Huawei Technologies Co., Ltd.                              3
QoS Configuration
QoS Configuration                                                               1 About This Document


                    ●   Reliability design declaration
                        Network planning and site design must comply with reliability design
                        principles and provide device- and solution-level protection. Device-level
                        protection includes planning principles of dual-network and inter-card dual-
                        link to avoid single point or single link of failure. Solution-level protection
                        refers to fast convergence protection mechanisms such as FRR and VRRP. If
                        solution-level protection is used, ensure that the primary and backup paths do
                        not share links or transmission devices. Otherwise, solution-level protection
                        may fail to take effect.

Reference Standards and Protocols
                    To obtain reference standards and protocols, log in to Huawei official website,
                    search for "standard and protocol compliance list", and download the Huawei S-
                    Series Switch Standard and Protocol Compliance List. If you have not obtained the
                    access permission of the document, see Help on the website to find out how to
                    obtain it.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                            4
QoS Configuration
QoS Configuration                                                                     2 Overview of QoS




                                                        2         Overview of QoS

Introduction to QoS
                    As networks grow in both scope and service diversity, the resulting sharp increase
                    in traffic exacerbates network congestion and increases the forwarding delay,
                    while in some cases even causing packet loss. Any one of these situations will
                    cause services to experience deterioration in quality or even total interruption. To
                    support increasingly diversified services such as gaming, video, and live streaming,
                    it is critical that network congestion be resolved. One answer is to increase
                    network bandwidth, but this comes with increased costs. A better, more cost-
                    effective solution is to use an "assured" policy to manage traffic.

                    Quality of service (QoS) is able to offer such a solution. It does this by providing
                    end-to-end (E2E) service quality guarantee to meet diversified service
                    requirements. It is important to note that QoS does not increase the network
                    bandwidth. Instead, it improves network resource utilization and allows different
                    types of traffic to compete for network resources based on their priorities, so that
                    voice, video, and important data applications are processed preferentially on
                    network devices. QoS is now widely used on, and vital to, Internet applications.


DiffServ
                    QoS is an overall solution, instead of being merely a single function. When two
                    hosts on a network communicate with each other, traffic between them may
                    traverse a large number of devices. QoS can guarantee E2E service quality only
                    when all devices on the network use a unified QoS service model, of which
                    Differentiated Services (DiffServ) is the most commonly used.

                    DiffServ classifies packets on a network into multiple classes and provides
                    differentiated processing for each class. In this way, when congestion occurs,
                    classes with a higher priority are given preference. Packets of the same class are
                    aggregated and sent as a whole to ensure the same delay, jitter, and packet loss
                    rate.

                    In the DiffServ model, a device can flexibly classify packets based on a
                    combination of conditions and re-mark packets with different priorities. The other
                    devices simply need to identify the priorities carried in packets to allocate
                    resources and control traffic.


Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                  5
QoS Configuration
QoS Configuration                                                                         2 Overview of QoS


                    QoS technologies supported by the device are based on the DiffServ service
                    model. In this chapter, we describe QoS technologies in terms of packet
                    classification methods and QoS service technologies.

QoS Packet Classification Methods
                    DiffServ classifies packets on a network into multiple classes and provides
                    differentiated processing for each class. The device supports behavior aggregate
                    (BA) classification and multi-field (MF) classification.
                    MF classification: MQC-based traffic classification
                    MF classification refers to the process of classifying packets in a refined manner
                    based on complex rules, such as 5-tuple (source IP address, source port number,
                    protocol number, destination IP address, and destination port number). Using MF
                    classification, the device classifies packets with the same characteristics into the
                    same type, and assigns them the same QoS level. MF classification can be
                    implemented through traffic classifiers in the Modular QoS Command-Line
                    Interface (MQC), which involves three elements: traffic classifier, traffic behavior,
                    and traffic policy.
                    1.   Traffic classifier: Defines matching rules.
                    2.   Traffic behavior: Specifies actions to be taken for packets. Different QoS
                         functions can be implemented based on traffic behaviors. This document
                         provides guidance for configuring MQC-based traffic statistics collection and
                         packet filtering.
                    3.   Traffic policy: Binds the configured traffic classifier and traffic behavior into a
                         traffic policy, which is then applied in a specified view.
                    BA classification: QoS priority-based classification
                    BA classification refers to the process of classifying packets based on simple rules
                    — a certain priority field in packets, to identify traffic with different priorities.
                    Priorities are described as follows:
                    ●    External priority
                         The external priority is also known as the packet priority or QoS priority. QoS
                         information is recorded by using certain fields in packets, such as the 802.1p
                         value of VLAN packets and the Differentiated Services Code Point (DSCP)
                         value of IP packets. A device can process received packets only based on
                         internal priorities to provide differentiated QoS levels for different services. As
                         such, external priorities are mapped to internal priorities after packets enter
                         the device.
                    ●    Internal priority
                         The internal priority is also known as the class of service (CoS), per-hop
                         behavior (PHB), or local priority. Internal priority values are CS7, CS6, EF, AF4,
                         AF3, AF2, AF1, and BE (in descending order of priority), and correspond to
                         queues 7, 6, 5, 4, 3, 2, 1, and 0, respectively. The internal priority determines
                         the queue into which packets are placed. When QoS services are configured
                         for a queue, the same QoS level is configured for all the packets forwarded
                         through that queue.
                    ●    Drop priority
                         The drop priority is also known as a color. It determines the sequence in
                         which packets are dropped when congestion occurs in a queue, without

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                     6
QoS Configuration
QoS Configuration                                                                       2 Overview of QoS


                        affecting the mapping between internal priorities and queues. The drop
                        priorities defined by Institute of Electrical and Electronics Engineers (IEEE) are
                        green, yellow, and red in ascending order. By default, the device first discards
                        packets with a higher drop priority when congestion occurs in a queue.
                        Whether packets are discarded first is determined by QoS parameter settings.
                        For example, in a weighted random early detection (WRED) drop profile, if
                        green packets can use a maximum of 50% of the buffer and red packets can
                        use the entire buffer, the device first discards green packets when congestion
                        occurs in a queue.

                    When processing QoS services, in the inbound direction, the device maps external
                    priorities of packets to internal and drop priorities, while in the outbound
                    direction, it maps internal and drop priorities to external priorities. To centrally
                    schedule packets, you can use the following methods to adjust the mapping
                    between external priorities and internal priorities:

                    ●   Priority mapping: In the DiffServ model, the device defines DiffServ domains
                        to manage and record the mapping between external priorities and internal
                        or drop priorities, which can vary between multiple DiffServ domains. In this
                        way, differentiated QoS services are provided.

                    Relationship between MF classification and BA classification

                    MF classification can also be used to identify packets with specific QoS priorities,
                    and this is implemented through MQC. In summary, MF classification classifies
                    packets based on MQC traffic classifiers; BA classification classifies packets based
                    on the internal and drop priorities that the external priorities of packets are
                    mapped to, without using MQC traffic classifiers.

QoS Service Technologies
                    After packets are classified, differentiated QoS services can be provided for
                    different types of packets. Here, we describe the following QoS service
                    technologies:

                    Traffic policing, traffic shaping, and interface-based rate limiting

                    In order to ensure services remain stable even in scenarios where network
                    resources are limited, user traffic rates must be limited. This can be achieved by
                    using traffic policing, traffic shaping, and interface-based rate limiting, which
                    define basic bandwidth (rate limiting) of different services passing through
                    network devices and monitor the rate of services entering network devices (speed
                    testing). When the volume of service traffic exceeds the basic bandwidth
                    (speeding), excess traffic is discarded or buffered (punishment). In this way, traffic
                    is limited and resource utilization is improved, resulting in more stable services for
                    users.
                    ●   Traffic policing controls the traffic rate within a bandwidth limit. It does this
                        by discarding excess traffic when the service traffic exceeds the rate limit. This
                        prevents individual services or users from consuming excessive bandwidth
                        resources.
                    ●   Traffic shaping adjusts the rate of outgoing traffic to stabilize the rate at
                        which it is transmitted, thereby avoiding unnecessary packet loss and
                        congestion. Unlike traffic policing, which discards packets exceeding the rate
                        limit, traffic shaping buffers such packets and sends them out at an even rate.

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                  7
QoS Configuration
QoS Configuration                                                                      2 Overview of QoS


                    ●   Interface-based rate limiting limits the total rate of packets sent or received
                        on an interface and can be implemented through either traffic policing or
                        traffic shaping.
                    Congestion avoidance
                    Congestion avoidance is a congestion control mechanism that monitors network
                    resources such as queues and memory buffers, as well as discarding packets when
                    congestion occurs or worsens.
                    Congestion management
                    Congestion management is a queue-based technology that works by buffering
                    packets in queues upon network congestion occurs. Congestion management
                    schedules the packets based on a scheduling algorithm, ensuring that QoS-
                    demanding services, such as latency-sensitive services, are preferentially scheduled.

QoS Service Process
                    To sum up, traffic classification is the basis of differentiated services. Traffic
                    policing, traffic shaping, interface-based rate limiting, congestion avoidance, and
                    congestion management are techniques for controlling network traffic and
                    resource allocation, and ultimately implementing differentiated services.
                    Figure 2-1 shows the QoS service process on network devices.

                    Figure 2-1 QoS service process




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                 8
QoS Configuration
QoS Configuration                                                                  3 MQC Configuration




                                                    3       MQC Configuration


                    3.1 Overview of MQC
                    3.2 Understanding MQC
                    3.3 Configuration Precautions for MQC
                    3.4 Configuring a Traffic Classifier
                    3.5 Configuring a Traffic Behavior
                    3.6 Configuring a Traffic Policy
                    3.7 Applying a Traffic Policy
                    3.8 Verifying the Configuration
                    3.9 Maintaining MQC


3.1 Overview of MQC
                    Modular QoS Command-Line Interface (MQC) allows the device to classify
                    packets based on their characteristics and provide the same QoS level for packets
                    of the same type. This enables the device to provide differentiated services for
                    packets of different types. As more diversified services are deployed on a network,
                    service deployment becomes increasingly complex, if differentiated services need
                    to be provided for traffic of different services or users. Leveraging MQC, you can
                    implement fine-grained classification of network traffic and specify the QoS levels
                    for traffic of different types according to your requirements, enhancing
                    serviceability of your network.

                    To implement MQC, you need to configure traffic classifiers, traffic behaviors, and
                    traffic policies, and apply the traffic policies. MQC is commonly used in the
                    following scenarios:
                    ●   4.3 Configuring MQC-based Packet Filtering
                    ●   Configuring MQC-based Traffic Statistics Collection
                    ●   6.4 Configuring MQC-based Priority Re-marking
                    ●   Configuring MQC-based Redirection


Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                9
QoS Configuration
QoS Configuration                                                                       3 MQC Configuration




3.2 Understanding MQC
MQC Entities
                    MQC involves three entities: traffic classifier, traffic behavior, and traffic policy.
                    Traffic classifier
                    A traffic classifier defines a group of matching rules for classifying packets. To
                    configure a traffic classifier, specify the following:
                    ●   Traffic classifier name
                    ●   Traffic classification rules: The device supports a broad range of traffic
                        classification rules, including: link-layer rules (Layer 2 rules), network-layer
                        rules (Layer 3 rules), transport-layer rules (Layer 4 rules), access control list
                        (ACL) rules, and other rules.
                    ●   Relationship between rules in a traffic classifier
                        –    AND: If a traffic classifier contains ACL rules, a packet matches the traffic
                             classifier only when it matches one ACL rule and all the non-ACL rules. If
                             a traffic classifier does not contain any ACL rules, a packet matches the
                             traffic classifier only when it matches all the rules in the traffic classifier.
                        –    OR: A packet matches a traffic classifier if it matches one or more rules.
                        The traffic classifier c1 is used as an example, which defines the following
                        rules:
                        –    ACL rules: ACL 3000 and ACL 3001
                        –    Non-ACL rules: The VLAN ID is 10, and the 802.1p value in the outer tag
                             of a packet is 3.
                        OR: A packet matches traffic classifier c1 if its VLAN ID is 10 or the 802.1p
                        value in the outer tag is 3, the packet matches ACL 3000, or the packet
                        matches ACL 3001.
                        AND: A packet matches traffic classifier c1 only when its VLAN ID is 10 and
                        the 802.1p value in the outer tag is 3, and the packet matches ACL 3000 or
                        3001.
                    Traffic behavior
                    A traffic behavior defines an action to be taken on packets of a specified type. To
                    configure a traffic behavior, specify the following:
                    ●   Traffic behavior name
                    ●   Actions: The device supports actions such as packet filtering and traffic
                        statistics collection. If a traffic behavior defines multiple non-conflicting
                        actions, all these actions are successfully configured and take effect. If a
                        traffic behavior defines conflicting actions, the outcome is one of the
                        following:
                        –    When conflicting actions are defined in the traffic behavior view, the
                             system displays an error message and the command fails to be executed.
                        –    When a traffic policy that contains a traffic behavior defining conflicting
                             actions is applied, the system displays an error message and the traffic
                             policy fails to be applied.

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                   10
QoS Configuration
QoS Configuration                                                                      3 MQC Configuration


                    Traffic policy
                    A traffic policy binds traffic classifiers and traffic behaviors, and then actions
                    defined in traffic behaviors are taken on packets of specific types. In Figure 3-1,
                    multiple traffic classifiers and traffic behaviors can be bound to one traffic policy.

                    Figure 3-1 Multiple pairs of traffic classifiers and traffic behaviors bound to a
                    traffic policy




MQC Configuration Process
                    Figure 3-2 shows the MQC configuration process in the following steps:
                    1.   Configure a traffic classifier. The traffic classifier defines a group of matching
                         rules to classify traffic and is the basis for providing differentiated services.
                    2.   Configure a traffic behavior. The traffic behavior defines actions for controlling
                         packets that match rules.
                    3.   Create a traffic policy, and bind the traffic classifier and traffic behavior to the
                         traffic policy.
                    4.   Apply the traffic policy in the required view.




Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                 11
QoS Configuration
QoS Configuration                                                                              3 MQC Configuration


                    Figure 3-2 MQC configuration process




3.3 Configuration Precautions for MQC

3.4 Configuring a Traffic Classifier
Prerequisites
                    Before configuring a traffic classifier, complete the following task:
                    ●   Configure an ACL if it is required to classify packets.

Context
                    Multiple rules can be configured in a traffic classifier as long as they do not
                    conflict with each other. You can configure rules based on your requirements.

                         NOTE

                        ● A traffic classifier in which matching rules are ANDed cannot define duplicate matching
                          rules. For example, a traffic classifier cannot define both if-match source-mac and an
                          ACL rule that matches the same source MAC address.
                        ● To match multiple fields of packets of the same type (such as Layer 2/IPv6/IPv4
                          packets) in a view, apply one traffic policy in the view, and specify multiple traffic
                          classifiers and associated traffic behaviors in the traffic policy. If both IPv4 and IPv6
                          packets need to be matched, create one traffic policy for each type of packet.


Procedure
         Step 1 Enter the system view.
                    system-view


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                           12
QoS Configuration
QoS Configuration                                                                             3 MQC Configuration


         Step 2 Create a traffic classifier and enter the traffic classifier view, or enter the view of
                an existing traffic classifier.
                    traffic classifier classifier-name [ type { and | or } ]

                    and is the logical operator between the rules in a traffic classifier, which means
                    that:
                    ●     If the traffic classifier contains ACL rules, packets must match one ACL rule
                          and all the non-ACL rules in order to match the traffic classifier.
                    ●     If the traffic classifier does not contain any ACL rules, packets must match all
                          rules in order to match the traffic classifier.
                    The logical operator or means that packets match a traffic classifier if they match
                    one or more rules in the traffic classifier.

                    By default, the relationship between rules in a traffic classifier is OR.

                    For S5735-L-V2, S5735-S-V2, S5735I-L-V2, S5735I-S-V2, S5735I-H-V2, S5735R-L-V2,
                    S3710-H, S5735R-S-V2, S5735E-L-V2, S5735E-S-V2 series, the device supports a
                    maximum of 1024 traffic classifiers.

                    For the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S, S6730E-
                    H-V2, S5755E-H, S5755-S and S5755-H series, the device supports a maximum of
                    2048 traffic classifiers.

         Step 3 Configure matching rules according to the following table.
                           NOTE

                         For details about precautions for each matching rule, see the corresponding commands in
                         the Command Reference.
                         For S5735-L-V2, S5735-S-V2, S5735I-L-V2, S5735I-S-V2, S5735I-H-V2, S5735R-L-V2, S3710-
                         H, S5735R-S-V2, S5735E-L-V2, S5735E-S-V2 series, a maximum of 1024 if-match
                         commands can be configured in a traffic classifier.
                         For the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S, S6730E-H-V2,
                         S5755E-H, S5755-S and S5755-H series, a maximum of 2048 if-match commands can be
                         configured in a traffic classifier.
                    ●     Link-layer rule (Layer 2 rule)

                           Matching Rule                    Command

                           Destination MAC                  if-match destination-mac mac-address [ mac-
                           address                          address-mask ]
                           Source MAC address               if-match source-mac mac-address [ mac-address-
                                                            mask ]
                           Protocol type in the             if-match l2-protocol { arp | ip | profinet | goose |
                           Ethernet frame                   rarp | protocol-value }
                           header

                           VLAN ID in the                   if-match vlan start-vlan-value [ inner-vlan start-
                           outer tag of VLAN                inner-vlan-value [ to end-inner-vlan-value ] ]
                           packets or VLAN ID               if-match vlan start-vlan-value [ to end-vlan-value ]
                           in the inner and                 [ inner-vlan start-inner-vlan-value ]
                           outer tags of QinQ
                           packets


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                     13
QoS Configuration
QoS Configuration                                                                   3 MQC Configuration


                         Matching Rule           Command

                         802.1p value in the     if-match 8021p 8021p-value &<1-8>
                         outer tag of VLAN
                         packets

                         Inner VLAN ID in        if-match inner-vlan start-inner-vlan-value [ to end-
                         QinQ packets            inner-vlan-value ]
                         802.1p value in the     if-match inner-8021p inner-8021p-value &<1-8>
                         inner tag of QinQ
                         packets

                         Inner and outer         if-match double-tag
                         VLAN IDs in QinQ
                         packets


                    ●   Network-layer rule (Layer 3 rule)
                         Matching Rule           Command

                         DSCP value in IP        if-match dscp dscp-value &<1-8>
                         packets

                         DSCP value in IPv6      if-match ipv6 dscp dscp-value &<1-8>
                         packets

                         IP precedence in IP     if-match ip-precedence ip-precedence &<1-8>
                         packets

                         ECN marking in IP       if-match ecn ecn-value
                         packets

                         ECN marking in IPv6     if-match ipv6 ecn ecn-value
                         packets


                    ●   Transport-layer rule (Layer 4 rule)
                         Matching Rule           Command

                         TCP flag in the TCP     if-match tcp-flag { tcp-flag-value | { ack | fin | psh |
                         packet header           rst | syn | urg } * }


                    ●   ACL rule
                         Matching Rule           Command

                         ACL rule                if-match acl { acl-number | acl-name }




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                              14
QoS Configuration
QoS Configuration                                                                 3 MQC Configuration


                         Matching Rule          Command

                         ACL6 rule              if-match ipv6 acl { acl-number | acl6-name }
                                                [ loose-mode | strict-mode ]
                                                Only the S6780-H, S6750-S, S6730-H-V2, S6750-H,
                                                S5732-H-V2, S6750E-S, S6730E-H-V2, S5755E-H,
                                                S5755-S and S5755-H series support the loose-
                                                mode and strict-mode parameters.


                    ●   Forwarding rule
                         Matching Rule          Command

                         Dropped packets        if-match discard

                         All packets            if-match any

                         Inbound interface      if-match inbound-interface { { interface-type
                                                interface-number1 | interface-name1 } [ to
                                                { interface-type interface-number2 | interface-
                                                name2 } ] } &<1-8>
                         Interface              if-match outbound-interface { { interface-type
                                                interface-number1 | interface-name1 } [ to
                                                { interface-type interface-number2 | interface-
                                                name2 } ] } &<1-8>
                         Known unicast          if-match unicast
                         packets

                         Unknown unicast        if-match unknown-unicast
                         packets


                    ●   VXLAN packet rule
                         Matching Rule          Command

                         Inner information in   if-match vxlan [ transit ] acl { acl-number | acl-
                         VXLAN packets to be    name }
                         matched against
                         ACL rules

                         Inner information in   if-match vxlan [ transit ] ipv6 acl { acl-number |
                         VXLAN packets to be    acl-name } [ loose-mode | strict-mode ]
                         matched against        Only the S6730E-H-V2, S6730-H-V2, S6750E-S,
                         ACL6 rules             S6750-S, S5755E-H, S5755-H, and S5732-H-V2 series
                                                support the loose-mode and strict-mode
                                                parameters.

                         Inner information in   if-match vxlan [ transit ] vni vni-id
                         VXLAN packets




Issue 01 (2025-03-03)          Copyright © Huawei Technologies Co., Ltd.                             15
QoS Configuration
QoS Configuration                                                                    3 MQC Configuration


                        Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S,
                        S6730E-H-V2, S5755E-H, S5755-S and S5755-H series support the commands
                        for configuring VXLAN packet matching rules.
                    ●   SRv6 packet rule
                         Matching Rule          Command

                         Inner information in   if-match srv6 transit acl { acl-number | acl-name }
                         SRv6 packets to be
                         matched against
                         ACL rules

                         Inner information in   if-match srv6 transit ipv6 acl { acl-number | acl-
                         SRv6 packets to be     name } [ loose-mode | strict-mode ]
                         matched against
                         ACL6 rules




                        Only the S6730E-H-V2, S6730-H-V2, S6750E-S, S6750-S, S5755-S, S5755E-H,
                        S5755-H, and S5732-H-V2 series support the commands for configuring SRv6
                        packet matching rules.
                    ●   Application/Application group rule
                         Matching Rule          Command

                         Application            if-match application application-name
                                                NOTE
                                                 application-name specifies the name of an application. You
                                                 can run the display application command to view
                                                 application names.




                        Only the S6730E-H-V2, S6730-H-V2, S5755E-H, S5755-H, S5732-H-V2,
                        S6750E-S, S6750-S, and S5755-S series support the command for configuring
                        an application matching rule.
                    ●   Other rules
                         Matching Rule          Command

                         Inner information in   if-match gre [ inner-source-ip source-ip-address
                         GRE packets            [ mask source-masklen ] | inner-destination-ip
                                                dest-ip-address [ mask dest-masklen ] | inner-
                                                protocol protocol | inner-source-port source-port-
                                                begin | inner-destination-port dest-port-begin ] *
                                                This function is supported only on the S6750-H and
                                                S6780-H.

                         QoS local ID           if-match qos-local-id qos-local-id




Issue 01 (2025-03-03)          Copyright © Huawei Technologies Co., Ltd.                                  16
QoS Configuration
QoS Configuration                                                                            3 MQC Configuration


                            Matching Rule               Command

                            EXP value of MPLS           if-match mpls-exp mpls-exp-value &<1-8>
                            packets                     Only the S6780-H, S6750-S, S6730-H-V2, S6750-H,
                                                        S5732-H-V2, S6750E-S, S6730E-H-V2, S5755E-H,
                                                        S5755-S and S5755-H series support this function.


         Step 4 Exit the traffic classifier view.
                    quit

         Step 5 (Optional) Enable the device to provide nonstop services upon modification of
                MQC-based traffic classification rules.
                    traffic-policy atomic-update-mode

                    If a traffic classification rule is modified after this command is run, the system
                    delivers the new rule and then deletes the old rule to ensure that traffic is not
                    interrupted.
                    With this function enabled, if a traffic policy that has been successfully applied
                    fails to be applied again due to its configuration change, the original traffic policy
                    still takes effect.
                    By default, this function is disabled.

                    ----End


3.5 Configuring a Traffic Behavior
Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Create a traffic behavior and enter the traffic behavior view, or enter the view of
                an existing traffic behavior.
                    traffic behavior behavior-name

         Step 3 Define actions in the traffic behavior. You can configure multiple non-conflicting
                actions in a traffic behavior.
                            NOTE

                           For details about precautions for each action, see the corresponding commands in the
                           Command Reference.
                           For S5735-L-V2, S5735-S-V2, S5735I-L-V2, S5735I-S-V2, S5735I-H-V2, S5735R-L-V2, S3710-
                           H, S5735R-S-V2, S5735E-L-V2, S5735E-S-V2 series, the device supports a maximum of 1024
                           traffic behaviors.
                           For the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S, S6730E-H-V2,
                           S5755E-H, S5755-S and S5755-H series, the device supports a maximum of 2048 traffic
                           behaviors.

                     Action                  Command

                     Packet filtering        See 4 Packet Filtering Configuration.


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                     17
QoS Configuration
QoS Configuration                                                                  3 MQC Configuration


                    Action               Command

                    Traffic statistics   See 5 Traffic Statistics Collection Configuration.
                    collection

                    MQC-based re-        See 6 Re-marking Configuration.
                    marking

                    MQC-based            See 7 Redirection Configuration.
                    redirection

                    MQC-based            See 9.5.1 Configuring MQC-based Traffic Policing (Level-1
                    traffic policing     CAR) in Traffic Policing, Traffic Shaping, and Interface-
                                         based Rate Limiting Configuration.

                    Hierarchical         car car-name share
                    traffic policing     See 9.5.2 Configuring Traffic Policing (Level-2 CAR) in
                                         Traffic Policing, Traffic Shaping, and Interface-based Rate
                                         Limiting Configuration.
                                         Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-
                                         V2, S6750E-S, S6730E-H-V2, S5755E-H, S5755-S and S5755-H
                                         series support this function.

                    MQC-based            mirroring observe-port observe-port-index
                    flow mirroring       mirroring observe-port group group-id
                                         For details, see "Configuring Flow Mirroring" in CLI
                                         Configuration Guide > System Monitoring Configuration >
                                         Mirroring Configuration.

                    Disabling MAC        mac-address learning disable
                    address learning     For details, see "Disabling MAC Address Learning" in CLI
                                         Configuration Guide > Ethernet Switching Configuration >
                                         MAC Configuration.

                    MQC-based            vlan-mapping { vlan vlan-id | inner-vlan inner-vlan-id }
                    VLAN mapping         For details, see "Configuring VLAN Mapping" in CLI
                                         Configuration Guide > Ethernet Switching Configuration >
                                         VLAN Configuration.

                    MQC-based            vlan-stacking vlan vlan-id
                    selective QinQ       For details, see "Configuring QinQ" in CLI Configuration
                                         Guide > Ethernet Switching Configuration > VLAN
                                         Configuration.

                    Disabling URPF       ip urpf disable
                    check                For details, see "URPF Configuration" in CLI Configuration
                                         Guide > Security Configuration.
                                         Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-
                                         V2, S6750E-S, S6730E-H-V2, S5755E-H, S5755-S and S5755-H
                                         series support this function.




Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                             18
QoS Configuration
QoS Configuration                                                                                  3 MQC Configuration


                     Action                  Command

                     Specifying a            network-slice-instance netsliceinstid
                     network slice           For details, see "Network Slicing Configuration" in CLI
                     instance                Configuration Guide > Network Slicing Configuration.



                    ----End


3.6 Configuring a Traffic Policy
Prerequisites
                    Before configuring a traffic policy, complete the following tasks:
                    ●    Configure a traffic classifier.
                    ●    Configure a traffic behavior.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Create a traffic policy and enter the traffic policy view, or enter the view of an
                existing traffic policy.
                    traffic policy policy-name

                    For S5735-L-V2, S5735-S-V2, S5735I-L-V2, S5735I-S-V2, S5735I-H-V2, S5735R-L-V2,
                    S3710-H, S5735R-S-V2, S5735E-L-V2, S5735E-S-V2 series, the device supports a
                    maximum of 1024 traffic policies.
                    For the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S, S6730E-
                    H-V2, S5755E-H, S5755-S and S5755-H series, the device supports a maximum of
                    2048 traffic policies.
         Step 3 Bind a traffic behavior and a traffic classifier to the traffic policy.
                    classifier classifier-name behavior behavior-name [ precedence precedence-value ]

                    For S5735-L-V2, S5735-S-V2, S5735I-L-V2, S5735I-S-V2, S5735I-H-V2, S5735R-L-V2,
                    S3710-H, S5735R-S-V2, S5735E-L-V2, S5735E-S-V2 series, a maximum of 1024
                    traffic classifiers can be bound to a traffic policy.
                    For the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S, S6730E-
                    H-V2, S5755E-H, S5755-S and S5755-H series, a maximum of 2048 traffic
                    classifiers can be bound to a traffic policy.

                    ----End


3.7 Applying a Traffic Policy
Prerequisites
                    Before applying a traffic policy, configure the traffic policy.

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                      19
QoS Configuration
QoS Configuration                                                                                   3 MQC Configuration


                         NOTE

                        If a traffic policy fails to be applied due to insufficient ACL resources on the device, you are
                        advised to delete the traffic policy configuration that fails to be applied. Otherwise, after
                        configurations are saved and the device is restarted, the configuration of other services that
                        are running properly cannot be restored.


Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Apply a traffic policy.
                    ●   Apply a traffic policy to the system.
                        a.    Apply a traffic policy to the system.
                              traffic-policy policy-name global [ slot slot-id ] { inbound | outbound }
                    ●   Apply a traffic policy to an interface.
                        a.    Enter the interface view.
                              interface interface-type interface-number
                        b.    Apply a traffic policy to the interface.
                              traffic-policy policy-name { inbound | outbound }
                        c.    Exit the interface view.
                              quit
                    ●   Apply a traffic policy to a VLAN.
                        a.    Create a VLAN and enter the VLAN view.
                              vlan vlan-id
                        b.    Apply a traffic policy to the VLAN.
                              traffic-policy policy-name { inbound | outbound }
                        c.    Exit the VLAN view.
                              quit
                    ●   Apply a traffic policy to a VPN instance.
                        a.    Create a VPN instance and enter the VPN instance view.
                              ip vpn-instance vpn-instance-name
                        b.    Apply a traffic policy to the VPN instance.
                              traffic-policy policy-name inbound

                                      NOTE

                                     A traffic policy can be applied to a VPN instance only in the inbound direction.
                                     Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S,
                                     S6730E-H-V2, S5755E-H, S5755-S and S5755-H series support this function.
                        c.    Exit the VPN instance view.
                              quit
                    ●   Apply a traffic policy to a QoS group.
                        If the same traffic policy needs to be applied to multiple VLANs or interfaces,
                        you are advised to add the VLANs or interfaces to the same QoS group and
                        then apply the traffic policy to the QoS group.
                        a.    Create a QoS group and enter the QoS group view.
                              qos group group-name
                        b.    Add a specified interface or VLAN to the QoS group.
                              group-member { interface { interface-type interface-num | interface-name [ to interface-type
                              interface-num | interface-name ] } &<1-8> | vlan { vlanid [ to vlanid ] } &<1-8> }


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                           20
QoS Configuration
QoS Configuration                                                                            3 MQC Configuration


                              Only one type of member can be specified.
                        c.    Apply a traffic policy to the QoS group.
                              traffic-policy policy-name { inbound | outbound }

                        d.    Exit the QoS group view.
                              quit

                    ----End


3.8 Verifying the Configuration
Procedure

                    Operation                                          Command

                    Check the configured traffic classifiers.          display traffic classifier [ classifier-
                                                                       name ]

                    Check the configured traffic behaviors.            display traffic behavior [ behavior-
                                                                       name ]

                    Check the configured traffic policy.               display traffic policy [ policy-name
                                                                       [ classifier classifier-name ] ]

                    Check the traffic policy application               display traffic-policy applied-record
                    records.

                    Check TCAM delivery failures.                      display system tcam fail-record [ slot
                                                                       slot-id ]

                    Check the group indexes and rule                   display system tcam service brief
                    counts occupied by different services.             [ slot slot-id ]
                                                                       display system tcam service { cpcar
                                                                       slot slot-id | service-name slot slot-id
                                                                       [ chip chip-id ] }

                    Check the traffic policy application               display system tcam service traffic-
                    records.                                           policy

                    Check information about matched                    display system tcam match-rules slot
                    rules.                                             slot-id

                    Check statistics on packets that match             display traffic-policy statistics
                    a traffic policy.




Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                    21
QoS Configuration
QoS Configuration                                                                                       3 MQC Configuration


                    Operation                                               Command

                    Check matching fields and actions                       display system tcam acl group-
                    supported by a traffic policy in each                   information
                    view.

                    Check information about the resources display traffic-policy pre-state
                    occupied by the traffic policy to be
                    applied to determine whether the
                    traffic policy can be successfully applied
                    after the configuration is committed.


                    For details about the display commands, see the Command Reference.


3.9 Maintaining MQC
Context
                    Before re-collecting statistics on packets matching a traffic policy, clear all existing
                    statistics.


                        NOTICE

                    Traffic statistics cannot be restored after being cleared. Exercise caution when you
                    use this command.


Procedure
                    ●   Clear statistics on packets matching a traffic policy.
                        reset traffic-policy statistics { global [ slot slot-id ] | interface { interface-type interface-number |
                        interface-name } | vlan vlan-id | qos group group-id | vpn-instance vpn-instance-name } [ policy-
                        name ] [ inbound | outbound ] [ classifier-base classifier-name ] [ history ]

                               NOTE

                              Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S, S6730E-H-
                              V2, S5755E-H, S5755-S and S5755-H series support the vpn-instance parameter.

                    ----End




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                                         22
QoS Configuration
QoS Configuration                                                          4 Packet Filtering Configuration




                          4         Packet Filtering Configuration


                    4.1 Overview of Packet Filtering
                    4.2 Configuration Precautions for Packet Filtering
                    4.3 Configuring MQC-based Packet Filtering
                    4.4 Example for Configuring MQC-based Packet Filtering
                    4.5 Example for Configuring Access Control Based on Source MAC Addresses


4.1 Overview of Packet Filtering
                    Untrusted packets are those that present potential security risks or that users do
                    not wish to receive, and are quite common on modern networks. The packet
                    filtering function allows a device to immediately discard such untrusted packets in
                    order to improve overall network security.
                    With Modular QoS Command-Line Interface (MQC), a device is configured to
                    identify untrusted packets and discard them, as well as identify trusted packets
                    and permit them to pass through.
                    MQC-based packet filtering classifies packets more precisely and is more flexible
                    to deploy.


4.2 Configuration Precautions for Packet Filtering

4.3 Configuring MQC-based Packet Filtering
Context
                    A packet filtering-enabled device filters packets that match traffic classification
                    rules.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                 23
QoS Configuration
QoS Configuration                                                                     4 Packet Filtering Configuration


                          NOTE

                         ● In a traffic behavior, if the permit action is configured in combination with other traffic
                           actions, these actions are taken on packets in the order in which they were configured; if
                           the deny action is configured in combination with other traffic actions (excluding traffic
                           statistics collection and flow mirroring), only the deny action is taken on packets.
                         ● If you specify a packet filtering action for packets matching an ACL rule, the system first
                           checks the action defined in the ACL rule. If the ACL rule defines permit, the action
                           specified in the traffic behavior is taken on the packets. If the ACL rule defines deny, the
                           packets are discarded regardless of the action specified in the traffic behavior.
                         ● For details about MQC-related configuration precautions, see "Configuration Precautions
                           for MQC" in MQC Configuration.


Procedure
         Step 1 Configure a traffic classifier.

                    For details about how to configure a traffic classifier, see 3.4 Configuring a Traffic
                    Classifier in "MQC Configuration".

         Step 2 Configure a traffic behavior.
                    1.   Create a traffic behavior and enter the traffic behavior view, or enter the view
                         of an existing traffic behavior.
                         traffic behavior behavior-name

                    2.   Configure a traffic behavior as needed.
                         –      Configure a traffic behavior so that packets matching the associated
                                traffic classifier are forwarded based on the original policy.
                                permit

                                If both permit and other actions are configured in a traffic behavior, these
                                actions are performed in sequence.
                         –      Configure a traffic behavior to deny packets matching the associated
                                traffic classifier.
                                deny

                                In a traffic behavior, the deny action can be used with only traffic
                                statistics collection and flow mirroring. Even if other actions except traffic
                                statistics collection and flow mirroring are configured, they do not take
                                effect.
                    3.   Exit the traffic behavior view.
                         quit


         Step 3 Configure a traffic policy.
                    1.   Create a traffic policy and enter the traffic policy view, or enter the view of an
                         existing traffic policy.
                         traffic policy policy-name

                    2.   Bind a traffic behavior and a traffic classifier to the traffic policy.
                         classifier classifier-name behavior behavior-name [ precedence precedence-value ]

                    3.   Exit the traffic policy view.
                         quit

         Step 4 Apply a traffic policy.
                    ●    Apply a traffic policy to the system.

Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                        24
QoS Configuration
QoS Configuration                                                                      4 Packet Filtering Configuration


                        a.    Apply a traffic policy to the system.
                              traffic-policy policy-name global [ slot slot-id ] { inbound | outbound }

                    ●   Apply a traffic policy to an interface.
                        a.    Enter the interface view.
                              interface interface-type interface-number

                        b.    Apply a traffic policy to the interface.
                              traffic-policy policy-name { inbound | outbound }

                        c.    Exit the interface view.
                              quit

                    ●   Apply a traffic policy to a VLAN.
                        a.    Create a VLAN and enter the VLAN view.
                              vlan vlan-id

                        b.    Apply a traffic policy to the VLAN.
                              traffic-policy policy-name { inbound | outbound }

                        c.    Exit the VLAN view.
                              quit

                    ●   Apply a traffic policy to a VPN instance.
                        a.    Create a VPN instance and enter the VPN instance view.
                              ip vpn-instance vpn-instance-name

                        b.    Apply a traffic policy to the VPN instance.
                              traffic-policy policy-name inbound

                                      NOTE

                                     A traffic policy can be applied to a VPN instance only in the inbound direction.
                                     Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S,
                                     S6730E-H-V2, S5755E-H, S5755-S and S5755-H series support this function.
                        c.    Exit the VPN instance view.
                              quit

                    ●   Apply a traffic policy to a QoS group.
                        If the same traffic policy needs to be applied to multiple VLANs or interfaces,
                        you are advised to add the VLANs or interfaces to the same QoS group and
                        then apply the traffic policy to the QoS group.
                        a.    Create a QoS group and enter the QoS group view.
                              qos group group-name

                        b.    Add a specified interface or VLAN to the QoS group.
                              group-member { interface { interface-type interface-num | interface-name [ to interface-type
                              interface-num | interface-name ] } &<1-8> | vlan { vlanid [ to vlanid ] } &<1-8> }
                              Only one type of member can be specified.
                        c.    Apply a traffic policy to the QoS group.
                              traffic-policy policy-name { inbound | outbound }

                        d.    Exit the QoS group view.
                              quit

                    ----End




Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                           25
QoS Configuration
QoS Configuration                                                          4 Packet Filtering Configuration


Verifying the Configuration

                    Operation                                   Command

                    Check the configured traffic classifiers.   display traffic classifier [ classifier-
                                                                name ]

                    Check the configured traffic behaviors.     display traffic behavior [ behavior-
                                                                name ]

                    Check the configured traffic policy.        display traffic policy [ policy-name
                                                                [ classifier classifier-name ] ]

                    Check the traffic policy application        display traffic-policy applied-record
                    records.

                    Check TCAM delivery failures.               display system tcam fail-record [ slot
                                                                slot-id ]

                    Check the group indexes and rule            display system tcam service brief
                    counts occupied by different services.      [ slot slot-id ]
                                                                display system tcam service { cpcar
                                                                slot slot-id | service-name slot slot-id
                                                                [ chip chip-id ] }

                    Check the traffic policy application        display system tcam service traffic-
                    records.                                    policy

                    Check information about matched             display system tcam match-rules slot
                    rules.                                      slot-id

                    Check statistics on packets that match      display traffic-policy statistics
                    a traffic policy.

                    Check matching fields and actions           display system tcam acl group-
                    supported by a traffic policy in each       information
                    view.

                    Check information about the resources display traffic-policy pre-state
                    occupied by the traffic policy to be
                    applied to determine whether the
                    traffic policy can be successfully applied
                    after the configuration is committed.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                  26
QoS Configuration
QoS Configuration                                                                    4 Packet Filtering Configuration




4.4 Example for Configuring MQC-based Packet
Filtering
Networking Requirements
                    In Figure 4-1, Host1, Host2, and Host3 communicate with each other through
                    DeviceA. For specific reasons, Host1 is allowed to receive traffic from Host2
                    through DeviceA but is not allowed to receive traffic from Host3.

                    Figure 4-1 Network diagram of packet filtering
                          NOTE

                         In this example, interface 1, interface 2, and interface 3 represent 10GE 1/0/1, 10GE 1/0/2,
                         and 10GE 1/0/3, respectively.




Procedure
         Step 1 Configure an ACL rule.
                    # On DeviceA, create ACL 3001 to match the traffic with source IP address
                    192.168.3.1 and destination IP address 192.168.1.1 (traffic from Host3 to Host1).
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceA
                    [DeviceA] acl 3001
                    [DeviceA-acl4-advance-3001] rule permit ip source 192.168.3.1 0 destination 192.168.1.1 0
                    [DeviceA-acl4-advance-3001] quit

         Step 2 Configure a traffic classifier.
                    # On DeviceA, create traffic classifier c1 and reference ACL 3001 in the traffic
                    classifier.
                    [DeviceA] traffic classifier c1
                    [DeviceA-classifier-c1] if-match acl 3001
                    [DeviceA-classifier-c1] quit


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                       27
QoS Configuration
QoS Configuration                                                                         4 Packet Filtering Configuration


         Step 3 Configure a traffic behavior.
                    # On DeviceA, create traffic behavior b1 and define the deny action in the traffic
                    behavior.
                    [DeviceA] traffic behavior b1
                    [DeviceA-behavior-b1] deny
                    [DeviceA-behavior-b1] quit

         Step 4 Configure a traffic policy and apply it to the outbound direction of 10GE 1/0/1.
                    # On DeviceA, create a traffic policy p1 and bind a traffic classifier to a traffic
                    behavior in the traffic policy.
                    [DeviceA] traffic policy p1
                    [DeviceA-trafficpolicy-p1] classifier c1 behavior b1
                    [DeviceA-trafficpolicy-p1] quit

                    # Apply traffic policy p1 to the outbound direction of 10GE 1/0/1.
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] traffic-policy p1 outbound
                    [DeviceA-10GE1/0/1] quit

                    ----End

Verifying the Configuration
                    # Check the ACL configuration.
                    <DeviceA> display acl 3001
                    Advanced ACL 3001, 1 rule
                    ACL's step is 5
                     rule 5 permit ip source 192.168.3.0 0 destination 192.168.1.0 0
                     (0 times matched)

                    # Check the traffic classifier configuration.
                    <DeviceA> display traffic classifier c1
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match acl 3001

                    # Check the traffic policy configuration.
                    <DeviceA> display traffic policy p1
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: OR
                        Behavior: b1
                          Deny


Configuration Scripts
                    DeviceA
                    #
                    sysname DeviceA
                    #
                    acl number 3001
                     rule 5 permit ip source 192.168.3.0 0.0.0.255 destination 192.168.1.0 0.0.0.255
                    #
                    traffic classifier c1 type or


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                          28
QoS Configuration
QoS Configuration                                                              4 Packet Filtering Configuration

                     if-match acl 3001
                    #
                    traffic behavior b1
                     deny
                    #
                    traffic policy p1
                     classifier c1 behavior b1 precedence 5
                    #
                    interface 10GE1/0/1
                     traffic-policy p1 outbound
                    #
                    return



4.5 Example for Configuring Access Control Based on
Source MAC Addresses
Networking Requirements
                    In Figure 4-2, users of an enterprise access the Internet through DeviceA. The
                    enterprise does not allow some hosts on the LAN to access the Internet. However,
                    users can still access the Internet from these hosts by changing host IP addresses,
                    and firewalls cannot prevent such unauthorized access based on IP addresses.
                    Access control based on source MAC addresses can be configured to solve this
                    problem. In this example, some hosts can be prevented from accessing the
                    Internet but can access DeviceA.

                    Figure 4-2 Network diagram
                          NOTE

                         In this example, interface 1 represents 10GE 1/0/1.




Procedure
         Step 1 Create a VLAN and configure interfaces.
                    # On DeviceA, create VLAN 10, configure VLANIF 10, and add 10GE 1/0/1 to the
                    VLAN.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceA
                    [DeviceA] vlan 10
                    [DeviceA-vlan10] quit
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] port link-type trunk
                    [DeviceA-10GE1/0/1] port trunk allow-pass vlan 10


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                               29
QoS Configuration
QoS Configuration                                                                        4 Packet Filtering Configuration

                    [DeviceA-10GE1/0/1] quit
                    [DeviceA] interface vlanif 10
                    [DeviceA-Vlanif10] ip address 10.1.1.1 255.255.255.0

         Step 2 Configure an ACL rule.

                    # On DeviceA, create ACL 3001 to match the traffic with the destination IP
                    address 10.1.1.1/24.
                    [DeviceA] acl 3001
                    [DeviceA-acl4-advance-3001] rule 1 permit ip destination 10.1.1.0 0.0.0.255
                    [DeviceA-acl4-advance-3001] quit

         Step 3 Configure traffic classifiers.

                    # On DeviceA, create a traffic classifier c1 and reference ACL 3001 in the traffic
                    classifier.
                    [DeviceA] traffic classifier c1 type and
                    [DeviceA-classifier-c1] if-match acl 3001
                    [DeviceA-classifier-c1] quit

                    # On DeviceA, create traffic classifiers c2 to c4 to match MAC addresses of user
                    hosts.
                    [DeviceA] traffic classifier c2 type and
                    [DeviceA-classifier-c2] if-match source-mac 00e0-fc0d-0001
                    [DeviceA-classifier-c2] quit
                    [DeviceA] traffic classifier c3 type and
                    [DeviceA-classifier-c3] if-match source-mac 00e0-fc0d-0002
                    [DeviceA-classifier-c3] quit
                    [DeviceA] traffic classifier c4 type and
                    [DeviceA-classifier-c4] if-match source-mac 00e0-fc0d-0003
                    [DeviceA-classifier-c4] quit

         Step 4 Configure traffic behaviors.

                    # On DeviceA, create a traffic behavior b1 and configure the permit action in the
                    traffic behavior.
                    [DeviceA] traffic behavior b1
                    [DeviceA-behavior-b1] permit
                    [DeviceA-behavior-b1] quit

                    # On DeviceA, create a traffic behavior b2 and configure the deny action in the
                    traffic behavior.
                    [DeviceA] traffic behavior b2
                    [DeviceA-behavior-b2] deny
                    [DeviceA-behavior-b2] quit

         Step 5 Configure a traffic policy and apply it to the inbound direction of an interface.

                    # On DeviceA, create a traffic policy p1 and bind traffic classifiers to traffic
                    behaviors in the traffic policy.
                    [DeviceA] traffic policy p1
                    [DeviceA-trafficpolicy-p1] classifier c1 behavior b1 precedence 5
                    [DeviceA-trafficpolicy-p1] classifier c2 behavior b2 precedence 10
                    [DeviceA-trafficpolicy-p1] classifier c3 behavior b2 precedence 15
                    [DeviceA-trafficpolicy-p1] classifier c4 behavior b2 precedence 20
                    [DeviceA-trafficpolicy-p1] quit

                    # Apply the traffic policy p1 to the inbound direction of an interface.

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                         30
QoS Configuration
QoS Configuration                                                             4 Packet Filtering Configuration

                    [DeviceA] interface vlanif 10
                    [DeviceA-Vlanif10] traffic-policy p1 inbound
                    [DeviceA-Vlanif10] quit

                    ----End

Verifying the Configuration
                    # Check the ACL rule configuration.
                    <DeviceA> display acl 3001
                    Advanced ACL 3001, 1 rule
                    ACL's step is 5
                     rule 1 permit ip destination 10.1.1.0 0.0.0.255
                     (0 times matched)

                    # Check the traffic classifier configuration.
                    <DeviceA> display traffic classifier c1
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: AND
                        Rule(s):
                          if-match acl 3001
                    <DeviceA> display traffic classifier c2
                     Traffic Classifier Information:
                      Classifier: c2
                        Type: AND
                        Rule(s):
                          if-match source-mac 00e0-fc0d-0001 ffff-ffff-ffff

                    # Check the traffic policy configuration.
                    <DeviceA> display traffic policy p1
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: AND
                        Behavior: b1

                        Classifier: c2
                         Type: AND
                        Behavior: b2
                         Deny

                        Classifier: c3
                         Type: AND
                        Behavior: b2
                         Deny

                        Classifier: c4
                         Type: AND
                        Behavior: b2
                         Deny


Configuration Scripts
                    DeviceA
                    #
                    sysname DeviceA
                    #
                    vlan batch 10
                    #
                    interface 10GE1/0/1
                     port link-type trunk
                     port trunk allow-pass vlan 10


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                             31
QoS Configuration
QoS Configuration                                                           4 Packet Filtering Configuration

                    #
                    acl 3001
                     rule 1 permit ip destination 10.1.1.0 0.0.0.255
                    #
                    traffic classifier c1 type and
                     if-match acl 3001
                    #
                    traffic classifier c2 type and
                     if-match source-mac 00e0-fc0d-0001
                    #
                    traffic classifier c3 type and
                     if-match source-mac 00e0-fc0d-0002
                    #
                    traffic classifier c4 type and
                     if-match source-mac 00e0-fc0d-0003
                    #
                    traffic behavior b1
                     permit
                    #
                    traffic behavior b2
                     deny
                    #
                    traffic policy p1
                     classifier c1 behavior b1 precedence 5
                     classifier c2 behavior b2 precedence 10
                     classifier c3 behavior b2 precedence 15
                     classifier c4 behavior b2 precedence 20
                    #
                    interface Vlanif10
                     ip address 10.1.1.1 255.255.255.0
                     traffic-policy p1 inbound
                    #
                    return




Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                            32
QoS Configuration
QoS Configuration                                               5 Traffic Statistics Collection Configuration




                                   5         Traffic Statistics Collection
                                                           Configuration

                    5.1 Overview of Traffic Statistics Collection
                    5.2 Configuration Precautions for Traffic Statistics Collection
                    5.3 Configuring MQC-based Traffic Statistics Collection
                    5.4 Example for Configuring MQC-based Traffic Statistics Collection


5.1 Overview of Traffic Statistics Collection
                    Traffic statistics collection allows the device to collect statistics on packets that
                    match traffic classification rules. With this function enabled, the device can collect
                    statistics on forwarded and discarded packets matching traffic policies, helping
                    administrators to locate faults and determine whether traffic policies are correctly
                    applied. You can run the display traffic-policy statistics command to view
                    statistics on forwarded and discarded packets matching traffic policies only when
                    MQC is used to implement traffic statistics collection.
                    Table 5-1 describes the differences between traffic statistics collection and
                    interface statistics collection.

                    Table 5-1 Differences between traffic statistics collection and interface statistics
                    collection
                     Statistics            Command                  Scope                  Remarks
                     Collection Mode

                     Traffic statistics    display traffic-         Packets matching       Packets sent to
                     collection            policy statistics        traffic                the CPU are
                                                                    classification rules   excluded.
                                                                    after a traffic
                                                                    policy is applied




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                  33
QoS Configuration
QoS Configuration                                                          5 Traffic Statistics Collection Configuration


                     Statistics                 Command                    Scope                  Remarks
                     Collection Mode

                     Interface statistics       display interface          All packets on an      Packets sent to
                     collection                                            interface              the CPU are
                                                                                                  included.




5.2 Configuration Precautions for Traffic Statistics
Collection

5.3 Configuring MQC-based Traffic Statistics Collection
Context
                    Traffic statistics collection allows the device to collect statistics on packets that
                    match traffic classification rules. Statistics on forwarded and discarded packets
                    that match traffic policies help to locate faults and check whether traffic policies
                    are correctly applied.

                          NOTE

                         ● If a traffic policy contains a lot of rules and traffic statistics have been cleared, wait for a
                           period before querying traffic statistics. Otherwise, only some or no statistics are
                           displayed.
                         ● If traffic classifiers bound to a traffic policy define many matching rules and the traffic
                           statistics collection action is defined, the device may respond slowly when querying
                           statistics based on MQC instances or traffic classifiers.
                         ● For details about MQC-related configuration precautions, see "Configuration Precautions
                           for MQC" in MQC Configuration.


Procedure
         Step 1 Configure a traffic classifier.
                    For details about how to configure a traffic classifier, see 3.4 Configuring a Traffic
                    Classifier in "MQC Configuration".
         Step 2 Configure a traffic behavior.
                    1.   (Optional) Configure the device to count the inter-frame gap and preamble of
                         Ethernet frames when the device collects traffic statistics.
                         qos statistics ifg enable
                    2.   Create a traffic behavior and enter the traffic behavior view, or enter the view
                         of an existing traffic behavior.
                         traffic behavior behavior-name
                    3.   Enable the traffic statistics collection function.
                         statistics enable [ history-record interval interval-value ]
                    4.   Exit the traffic behavior view.
                         quit


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                            34
QoS Configuration
QoS Configuration                                                           5 Traffic Statistics Collection Configuration


         Step 3 Configure a traffic policy.
                    1.   Create a traffic policy and enter the traffic policy view, or enter the view of an
                         existing traffic policy.
                         traffic policy policy-name
                    2.   Bind a traffic behavior and a traffic classifier to the traffic policy.
                         classifier classifier-name behavior behavior-name [ precedence precedence-value ]
                    3.   Exit the traffic policy view.
                         quit

         Step 4 Apply a traffic policy.
                    ●    Apply a traffic policy to the system.
                         a.     Apply a traffic policy to the system.
                                traffic-policy policy-name global [ slot slot-id ] { inbound | outbound }
                    ●    Apply a traffic policy to an interface.
                         a.     Enter the interface view.
                                interface interface-type interface-number
                         b.     Apply a traffic policy to the interface.
                                traffic-policy policy-name { inbound | outbound }
                         c.     Exit the interface view.
                                quit
                    ●    Apply a traffic policy to a VLAN.
                         a.     Create a VLAN and enter the VLAN view.
                                vlan vlan-id
                         b.     Apply a traffic policy to the VLAN.
                                traffic-policy policy-name { inbound | outbound }
                         c.     Exit the VLAN view.
                                quit
                    ●    Apply a traffic policy to a VPN instance.
                         a.     Create a VPN instance and enter the VPN instance view.
                                ip vpn-instance vpn-instance-name
                         b.     Apply a traffic policy to the VPN instance.
                                traffic-policy policy-name inbound

                                        NOTE

                                       A traffic policy can be applied to a VPN instance only in the inbound direction.
                                       Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S,
                                       S6730E-H-V2, S5755E-H, S5755-S and S5755-H series support this function.
                         c.     Exit the VPN instance view.
                                quit
                    ●    Apply a traffic policy to a QoS group.
                         If the same traffic policy needs to be applied to multiple VLANs or interfaces,
                         you are advised to add the VLANs or interfaces to the same QoS group and
                         then apply the traffic policy to the QoS group.
                         a.     Create a QoS group and enter the QoS group view.
                                qos group group-name
                         b.     Add a specified interface or VLAN to the QoS group.
                                group-member { interface { interface-type interface-num | interface-name [ to interface-type
                                interface-num | interface-name ] } &<1-8> | vlan { vlanid [ to vlanid ] } &<1-8> }
                                Only one type of member can be specified.

Issue 01 (2025-03-03)                  Copyright © Huawei Technologies Co., Ltd.                                           35
QoS Configuration
QoS Configuration                                                       5 Traffic Statistics Collection Configuration


                        c.    Apply a traffic policy to the QoS group.
                              traffic-policy policy-name { inbound | outbound }

                        d.    Exit the QoS group view.
                              quit

                    ----End

Verifying the Configuration

                    Operation                                          Command

                    Check the configured traffic classifiers.          display traffic classifier [ classifier-
                                                                       name ]

                    Check the configured traffic behaviors.            display traffic behavior [ behavior-
                                                                       name ]

                    Check the configured traffic policy.               display traffic policy [ policy-name
                                                                       [ classifier classifier-name ] ]

                    Check the traffic policy application               display traffic-policy applied-record
                    records.

                    Check TCAM delivery failures.                      display system tcam fail-record [ slot
                                                                       slot-id ]

                    Check the group indexes and rule                   display system tcam service brief
                    counts occupied by different services.             [ slot slot-id ]
                                                                       display system tcam service { cpcar
                                                                       slot slot-id | service-name slot slot-id
                                                                       [ chip chip-id ] }

                    Check the traffic policy application               display system tcam service traffic-
                    records.                                           policy

                    Check information about matched                    display system tcam match-rules slot
                    rules.                                             slot-id

                    Check statistics on packets that match             display traffic-policy statistics
                    a traffic policy.

                    Check matching fields and actions                  display system tcam acl group-
                    supported by a traffic policy in each              information
                    view.




Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                    36
QoS Configuration
QoS Configuration                                                     5 Traffic Statistics Collection Configuration


                    Operation                                         Command

                    Check information about the resources display traffic-policy pre-state
                    occupied by the traffic policy to be
                    applied to determine whether the
                    traffic policy can be successfully applied
                    after the configuration is committed.




5.4 Example for Configuring MQC-based Traffic
Statistics Collection
Networking Requirements
                    In Figure 5-1, Host1 sends packets with the 802.1p value of 6 to DeviceA.
                    Statistics on service packets need to be collected to properly allocate bandwidth
                    resources.

                    Figure 5-1 Network diagram of traffic statistics collection
                          NOTE

                         In this example, interface 1 represents 10GE 1/0/1.




Procedure
         Step 1 Set the host name of the device to DeviceA.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceA

         Step 2 Configure an ACL rule.

                    # On DeviceA, create Layer 2 ACL 4000 to match packets with the 802.1p value of
                    6.
                    [DeviceA] acl 4000
                    [DeviceA-acl-L2-4000] rule permit 8021p 6
                    [DeviceA-acl-L2-4000] quit

         Step 3 Configure a traffic classifier.

                    # On DeviceA, create traffic classifier c1 and match ACL 4000.
                    [DeviceA] traffic classifier c1
                    [DeviceA-classifier-c1] if-match acl 4000
                    [DeviceA-classifier-c1] quit


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                   37
QoS Configuration
QoS Configuration                                                               5 Traffic Statistics Collection Configuration


         Step 4 Configure a traffic behavior.
                    # On DeviceA, create traffic behavior b1 and define the traffic statistics collection
                    action in the traffic behavior.
                    [DeviceA] traffic behavior b1
                    [DeviceA-behavior-b1] statistics enable
                    [DeviceA-behavior-b1] quit

         Step 5 Configure a traffic policy and apply it to the interface.
                    # On DeviceA, create traffic policy p1, in which traffic classifier c1 is associated
                    with traffic behavior b1.
                    [DeviceA] traffic policy p1
                    [DeviceA-trafficpolicy-p1] classifier c1 behavior b1
                    [DeviceA-trafficpolicy-p1] quit

                    # Apply traffic policy p1 to the inbound direction of 10GE 1/0/1.
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] traffic-policy p1 inbound
                    [DeviceA-10GE1/0/1] quit
                    [DeviceA] quit

                    ----End

Verifying the Configuration
                    # Check the ACL configuration.
                    <DeviceA> display acl 4000
                    L2 ACL 4000, 1 rule
                    ACL's step is 5
                     rule 5 permit 8021p 6 (0 times matched)

                    # Check the traffic classifier configuration.
                    <DeviceA> display traffic classifier c1
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match acl 4000

                    # Check the traffic policy configuration.
                    <DeviceA> display traffic policy p1
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: OR
                        Behavior: b1
                          Statistics: enable

                    # Check traffic statistics.
                    <DeviceA> display traffic-policy statistics interface 10ge 1/0/1 inbound
                    Traffic policy: p1, inbound
                    --------------------------------------------------------------------------------
                     Slot: 1
                     Item                Packets              Bytes          pps          bps
                     -------------------------------------------------------------------------------
                     Matched                212185             22067448           1600       1379215
                      Passed              212185             22067448           1600        1379215
                      Dropped                   0                0           0          0
                       Filter               0                 0          0           0


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                            38
QoS Configuration
QoS Configuration                                                                  5 Traffic Statistics Collection Configuration

                      CAR                    0                0           0          0
                    -------------------------------------------------------------------------------

                    You can view the statistics on service packets on 10GE 1/0/1.

Configuration Scripts
                    DeviceA
                    #
                    sysname DeviceA
                    #
                    acl number 4000
                     rule 5 permit 8021p 6
                    #
                    traffic classifier c1 type or
                     if-match acl 4000
                    #
                    traffic behavior b1
                     statistics enable
                    #
                    traffic policy p1
                     classifier c1 behavior b1 precedence 5
                    #
                    interface 10GE1/0/1
                     traffic-policy p1 inbound
                    #
                    return




Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                               39
QoS Configuration
QoS Configuration                                                              6 Re-marking Configuration




                                   6          Re-marking Configuration


                    6.1 Overview of Re-marking
                    6.2 Re-marking
                    6.3 Configuration Precautions for Re-marking
                    6.4 Configuring MQC-based Priority Re-marking
                    6.5 Example for Configuring Re-marking to Distinguish Users
                    6.6 Example for Configuring Re-marking to Distinguish Services


6.1 Overview of Re-marking
                    Re-marking technology allows the device to re-mark a field (such as the priority)
                    of packets so that the packets can be scheduled or forwarded based on the re-
                    marked information. For example, the device re-marks the priority of packets
                    matching traffic classification rules. Packets of services that are sensitive to delay
                    and service quality can be re-marked with a high priority so that they can be
                    preferentially scheduled or forwarded. Similarly, priorities of services with no
                    special requirements on delay or service quality can be reduced to save network
                    resources for high-priority packets. For example, re-marking the priority of VLAN
                    packets is to re-mark the 802.1p value of the packets. The packets then can be
                    scheduled and forwarded based on the new 802.1p value.


6.2 Re-marking
Precedence Fields
                    In order for devices to provide differentiated services, certain fields in the packet
                    header or frame header are used to record QoS information. These fields include:
                    ●   802.1p value in an Ethernet frame header
                        Layer 2 devices communicate by exchanging Ethernet frames. In the Ethernet
                        frame header, the PRI field (802.1p value, also called CoS) is used to identify
                        the QoS requirements, as defined in IEEE 802.1Q. Figure 6-1 shows the
                        format of the 802.1p value in an Ethernet frame header.

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                               40
QoS Configuration
QoS Configuration                                                                6 Re-marking Configuration


                        Figure 6-1 802.1p value in an Ethernet frame header




                        As shown in the figure, the 802.1Q header contains a 3-bit PRI field and a 1-
                        bit CFI field. The PRI field defines eight service priority values 7, 6, 5, 4, 3, 2, 1,
                        and 0, listed in descending order of priority. The CFI field defines the drop
                        priority of packets.
                    ●   Precedence field in an IP packet
                        As defined in RFC 791, the 8-bit Type of Service (ToS) field in an IP packet
                        header contains a 3-bit IP precedence field. Figure 6-2 shows the format of
                        the precedence field in an IP packet.

                        Figure 6-2 IP precedence in the DSCP field




                        Bits 0 to 2 represent the precedence field, representing values 7, 6, 5, 4, 3, 2, 1
                        and 0, listed in descending order of priority. The highest-priority values (7 and
                        6) are reserved for routing and network control communication updates.
                        User-level applications can use only priority values 0 to 5.
                        In addition to the precedence field, a ToS field contains the following sub-
                        fields:
                        –   Bit D indicates the delay. The value 0 represents a normal delay and the
                            value 1 represents a short delay.
                        –   Bit T indicates the throughput. The value 0 represents a normal
                            throughput and the value 1 represents a high throughput.
                        –   Bit R indicates the reliability. The value 0 represents normal reliability and
                            the value 1 represents high reliability.
                    ●   DSCP field in an IP packet
                        The ToS field in IP packets was defined in RFC 1349, with bit C (monetary
                        cost) added. Following that, the IETF DiffServ Working Group redefined bits 0

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                   41
QoS Configuration
QoS Configuration                                                                   6 Re-marking Configuration


                         to 5 of a ToS field as the DS CodePoint (DSCP) field in RFC 2474. In RFC 2474,
                         the field name is changed from ToS to differentiated service (DS). Figure 6-2
                         shows the DSCP field in an IP packet.
                         In the DS field, the first six bits (bits 0 to 5) are the DSCP and the last two
                         bits (bits 6 and 7) are reserved. The first three bits (0 to 2) are the Class
                         Selector CodePoint (CSCP), which represents the DSCP type. A DS node selects
                         a Per-Hop Behavior (PHB) based on the DSCP value.

Packet Re-marking
                    Packet re-marking involves setting the preceding QoS-related packet fields and
                    redefining the scheduling and transmission modes of packets. Currently, the device
                    supports the following packet re-marking modes:
                    ●    Re-marking 802.1p values of VLAN packets
                    ●    Re-marking DSCP values of IP packets
                    ●    Re-marking internal priorities of packets
                    ●    Re-marking local IDs


6.3 Configuration Precautions for Re-marking

6.4 Configuring MQC-based Priority Re-marking
Context
                    After re-marking is configured, the device re-marks the packets matching traffic
                    classification rules so that the packets can be scheduled or forwarded based on re-
                    marked priorities.

                          NOTE

                         ● Both remark 8021p and remark dscp can be bound to the same traffic policy and take
                           effect simultaneously.
                         ● For details about MQC-related configuration precautions, see "Configuration Precautions
                           for MQC" in MQC Configuration.


Procedure
         Step 1 Configure a traffic classifier.

                    For details about how to configure a traffic classifier, see 3.4 Configuring a Traffic
                    Classifier in "MQC Configuration".

         Step 2 Configure a traffic behavior.
                    1.   Create a traffic behavior and enter the traffic behavior view, or enter the view
                         of an existing traffic behavior.
                         traffic behavior behavior-name

                    2.   Configure a traffic behavior as needed.
                         –    Re-marking 802.1p values of VLAN packets

Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                    42
QoS Configuration
QoS Configuration                                                                             6 Re-marking Configuration


                                For the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S,
                                S6730E-H-V2, S5755E-H, S5755-S and S5755-H series:
                                remark 8021p { 8021p-value | inner-8021p }

                                For S5735-L-V2, S5735-S-V2, S5735I-L-V2, S5735I-S-V2, S5735I-H-V2,
                                S5735R-L-V2, S3710-H, S5735R-S-V2, S5735E-L-V2, S5735E-S-V2 series:
                                remark 8021p 8021p-value

                         –      Re-marking DSCP values of IP packets
                                remark dscp { dscp-name | dscp-value }

                         –      Re-marking internal priorities
                                remark local-precedence { local-precedence-name | local-precedence-value } [ color ]

                         –      Re-marking local IDs
                                For the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S,
                                S6730E-H-V2, S5755E-H, S5755-S and S5755-H series:
                                remark qos-local-id qos-local-id [ inbound-match ]

                                For S5735-L-V2, S5735-S-V2, S5735I-L-V2, S5735I-S-V2, S5735I-H-V2,
                                S5735R-L-V2, S3710-H, S5735R-S-V2, S5735E-L-V2, S5735E-S-V2 series:
                                remark qos-local-id qos-local-id inbound-match

                    3.   Exit the traffic behavior view.
                         quit


         Step 3 Configure a traffic policy.
                    1.   Create a traffic policy and enter the traffic policy view, or enter the view of an
                         existing traffic policy.
                         traffic policy policy-name

                    2.   Bind a traffic behavior and a traffic classifier to the traffic policy.
                         classifier classifier-name behavior behavior-name [ precedence precedence-value ]

                    3.   Exit the traffic policy view.
                         quit

         Step 4 Apply the traffic policy.
                    ●    Apply a traffic policy to the system.
                         a.     Apply a traffic policy to the system.
                                traffic-policy policy-name global [ slot slot-id ] { inbound | outbound }

                    ●    Apply a traffic policy to an interface.
                         a.     Enter the interface view.
                                interface interface-type interface-number

                         b.     Apply a traffic policy to the interface.
                                traffic-policy policy-name { inbound | outbound }

                         c.     Exit the interface view.
                                quit

                    ●    Apply a traffic policy to a VLAN.
                         a.     Create a VLAN and enter the VLAN view.
                                vlan vlan-id

                         b.     Apply a traffic policy to the VLAN.
                                traffic-policy policy-name { inbound | outbound }

                         c.     Exit the VLAN view.
                                quit


Issue 01 (2025-03-03)                  Copyright © Huawei Technologies Co., Ltd.                                       43
QoS Configuration
QoS Configuration                                                                         6 Re-marking Configuration


                    ●   Apply a traffic policy to a QoS group.
                        If the same traffic policy needs to be applied to multiple VLANs or interfaces,
                        you are advised to add the VLANs or interfaces to the same QoS group and
                        then apply the traffic policy to the QoS group.
                        a.    Create a QoS group and enter the QoS group view.
                              qos group group-name

                        b.    Add a specified interface or VLAN to the QoS group.
                              group-member { interface { interface-type interface-num | interface-name [ to interface-type
                              interface-num | interface-name ] } &<1-8> | vlan { vlanid [ to vlanid ] } &<1-8> }

                              Only one type of member can be specified.
                        c.    Apply a traffic policy to the QoS group.
                              traffic-policy policy-name { inbound | outbound }

                        d.    Exit the QoS group view.
                              quit

                    ----End


Verifying the Configuration

                    Operation                                           Command

                    Check the configured traffic classifiers.           display traffic classifier [ classifier-
                                                                        name ]

                    Check the configured traffic behaviors.             display traffic behavior [ behavior-
                                                                        name ]

                    Check the configured traffic policy.                display traffic policy [ policy-name
                                                                        [ classifier classifier-name ] ]

                    Check the traffic policy application                display traffic-policy applied-record
                    records.

                    Check TCAM delivery failures.                       display system tcam fail-record [ slot
                                                                        slot-id ]

                    Check the group indexes and rule                    display system tcam service brief
                    counts occupied by different services.              [ slot slot-id ]
                                                                        display system tcam service { cpcar
                                                                        slot slot-id | service-name slot slot-id
                                                                        [ chip chip-id ] }

                    Check the traffic policy application                display system tcam service traffic-
                    records.                                            policy

                    Check information about matched                     display system tcam match-rules slot
                    rules.                                              slot-id


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                           44
QoS Configuration
QoS Configuration                                                                     6 Re-marking Configuration


                    Operation                                        Command

                    Check statistics on packets that match           display traffic-policy statistics
                    a traffic policy.

                    Check matching fields and actions                display system tcam acl group-
                    supported by a traffic policy in each            information
                    view.

                    Check information about the resources display traffic-policy pre-state
                    occupied by the traffic policy to be
                    applied to determine whether the
                    traffic policy can be successfully applied
                    after the configuration is committed.




6.5 Example for Configuring Re-marking to Distinguish
Users
Networking Requirements
                    On the network shown in Figure 6-3, packets sent from Host1 and Host2 to
                    DeviceB are identified by different VLAN IDs (10 and 20, respectively). DeviceB re-
                    marks the VLAN packets received from Host1 and Host2 so that the internal
                    priority of the packets sent by Host1 is higher than that of the packets sent by
                    Host2 on DeviceA. This ensures the experience of services on Host1.

                    Figure 6-3 Network diagram for configuring re-marking
                         NOTE

                        In this example, interface 1, interface 2, and interface 3 represent 10GE 1/0/1, 10GE 1/0/2,
                        and 10GE 1/0/3, respectively.




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                        45
QoS Configuration
QoS Configuration                                                              6 Re-marking Configuration




Procedure
         Step 1 Create VLANs and configure interfaces so that DeviceB can communicate with
                Host1, Host2, and DeviceA.
                    # Create VLAN 10, VLAN 20, and VLAN 30 on DeviceB.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 10 20 30

                    # Configure 10GE 1/0/1 as a trunk interface and add it to VLAN 30. Configure
                    10GE 1/0/2 and 10GE 1/0/3 as access interfaces and add them to VLAN 10 and
                    VLAN 20, respectively.
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type trunk
                    [DeviceB-10GE1/0/1] port trunk allow-pass vlan 30
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type access
                    [DeviceB-10GE1/0/2] port default vlan 10
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] portswitch
                    [DeviceB-10GE1/0/3] port link-type access
                    [DeviceB-10GE1/0/3] port default vlan 20
                    [DeviceB-10GE1/0/3] quit

                    # Create VLANIF 10, VLANIF 20, and VLANIF 30, and configure IP addresses for
                    them.
                    [DeviceB] interface vlanif 10
                    [DeviceB-Vlanif10] ip address 192.168.10.1 24
                    [DeviceB-Vlanif10] quit
                    [DeviceB] interface vlanif 20
                    [DeviceB-Vlanif20] ip address 192.168.20.1 24
                    [DeviceB-Vlanif20] quit
                    [DeviceB] interface vlanif 30
                    [DeviceB-Vlanif30] ip address 192.168.100.1 24
                    [DeviceB-Vlanif30] quit


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                          46
QoS Configuration
QoS Configuration                                                               6 Re-marking Configuration


         Step 2 Configure traffic classifiers.

                    # On DeviceB, create and configure traffic classifiers c1 and c2 to classify packets
                    based on VLAN IDs.
                    [DeviceB] traffic classifier c1
                    [DeviceB-classifier-c1] if-match vlan 10
                    [DeviceB-classifier-c1] quit
                    [DeviceB] traffic classifier c2
                    [DeviceB-classifier-c2] if-match vlan 20
                    [DeviceB-classifier-c2] quit

         Step 3 Configure traffic behaviors.

                    # Create and configure traffic behaviors b1 and b2 on DeviceB to re-mark the
                    802.1p value of VLAN packets sent from Host1 into 4 and the 802.1p value of
                    VLAN packets sent from Host2 into 2. In this manner, the priority of packets from
                    Host1 is higher than that of packets from Host2.
                    [DeviceB] traffic behavior b1
                    [DeviceB-behavior-b1] remark 8021p 4
                    [DeviceB-behavior-b1] quit
                    [DeviceB] traffic behavior b2
                    [DeviceB-behavior-b2] remark 8021p 2
                    [DeviceB-behavior-b2] quit

         Step 4 Configure traffic policies and apply them to interfaces.

                    # Create traffic policies p1 and p2 on DeviceB, bind the traffic classifiers and
                    traffic behaviors to the traffic policies, and apply the traffic policy p1 to 10GE
                    1/0/2 in the inbound direction and the traffic policy p2 to 10GE 1/0/3 in the
                    inbound direction to re-mark packet priorities.
                    [DeviceB] traffic policy p1
                    [DeviceB-trafficpolicy-p1] classifier c1 behavior b1
                    [DeviceB-trafficpolicy-p1] quit
                    [DeviceB] traffic policy p2
                    [DeviceB-trafficpolicy-p2] classifier c2 behavior b2
                    [DeviceB-trafficpolicy-p2] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] traffic-policy p1 inbound
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] traffic-policy p2 inbound
                    [DeviceB-10GE1/0/3] quit

                    ----End

Verifying the Configuration
                    # Check the traffic classifier configuration.
                    <DeviceB> display traffic classifier
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match vlan 10

                      Classifier: c2
                       Type: OR
                       Rule(s):
                         if-match vlan 20

                    Total classifier number is 2


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                            47
QoS Configuration
QoS Configuration                                                                                      6 Re-marking Configuration


                    # Check the traffic policy configuration.
                    <DeviceB> display traffic policy
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: OR
                        Behavior: b1
                          Remark:
                           Remark 8021p 4

                       Policy: p2
                        Classifier: c2
                         Type: OR
                        Behavior: b2
                         Remark:
                           Remark 8021p 2

                    Total policy number is 2

                    # Check the traffic policy application records.
                    <DeviceB> display traffic-policy applied-record
                    Total records : 2
                    --------------------------------------------------------------------------------
                    Policy Type/Name                     Apply Parameter               Slot State
                    --------------------------------------------------------------------------------
                    p1                             10GE1/0/2(IN)                  1 success
                    --------------------------------------------------------------------------------
                    p2                             10GE1/0/3(IN)                  1 success
                    --------------------------------------------------------------------------------


Configuration Scripts
                    DeviceB
                    #
                    sysname DeviceB
                    #
                    vlan batch 10 20 30
                    #
                    traffic classifier c1 type or
                     if-match vlan 10
                    #
                    traffic classifier c2 type or
                     if-match vlan 20
                    #
                    traffic behavior b1
                     remark 8021p 4
                    #
                    traffic behavior b2
                     remark 8021p 2
                    #
                    traffic policy p1
                     classifier c1 behavior b1 precedence 5
                    #
                    traffic policy p2
                     classifier c2 behavior b2 precedence 5
                    #
                    interface Vlanif10
                     ip address 192.168.10.1 255.255.255.0
                    #
                    interface Vlanif20
                     ip address 192.168.20.1 255.255.255.0
                    #
                    interface Vlanif30
                     ip address 192.168.100.1 255.255.255.0
                    #
                    interface 10GE1/0/1


Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                               48
QoS Configuration
QoS Configuration                                                                      6 Re-marking Configuration

                     port link-type trunk
                     port trunk allow-pass vlan 30
                    #
                    interface 10GE1/0/2
                     port link-type access
                     port default vlan 10
                     traffic-policy p1 inbound
                    #
                    interface 10GE1/0/3
                     port link-type access
                     port default vlan 20
                     traffic-policy p2 inbound
                    #
                    return



6.6 Example for Configuring Re-marking to Distinguish
Services

Networking Requirements
                    In Figure 6-4, voice, video, and data terminals on the LAN of an enterprise are
                    connected to interface 1 and interface 2 of DeviceC through DeviceA and DeviceB,
                    and are connected to the WAN through DeviceC and DeviceD.

                    Packets of different services are identified by 802.1p priorities on the LAN side.
                    When packets reach the WAN side through interface 3, it is required that
                    differentiated services be provided based on DSCP priorities of packets.

                    Figure 6-4 Network diagram for configuring re-marking
                          NOTE

                         In this example, interface 1, interface 2, and interface 3 represent 10GE 1/0/1, 10GE 1/0/2,
                         and 10GE 1/0/3, respectively.




Procedure
         Step 1 Set the device name to DeviceC.

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                       49
QoS Configuration
QoS Configuration                                                                  6 Re-marking Configuration

                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceC

         Step 2 Configure traffic classifiers.
                    # On DeviceC, create and configure traffic classifiers c1, c2, and c3 to classify
                    packets based on 802.1p priorities.
                    [DeviceC] traffic classifier c1
                    [DeviceC-classifier-c1] if-match 8021p 2
                    [DeviceC-classifier-c1] quit
                    [DeviceC] traffic classifier c2
                    [DeviceC-classifier-c2] if-match 8021p 5
                    [DeviceC-classifier-c2] quit
                    [DeviceC] traffic classifier c3
                    [DeviceC-classifier-c3] if-match 8021p 6
                    [DeviceC-classifier-c3] quit

         Step 3 Configure traffic behaviors.
                    # On DeviceC, create and configure traffic behaviors b1, b2, and b3 to re-mark
                    priorities of user packets.
                    [DeviceC] traffic behavior b1
                    [DeviceC-behavior-b1] remark dscp 15
                    [DeviceC-behavior-b1] quit
                    [DeviceC] traffic behavior b2
                    [DeviceC-behavior-b2] remark dscp 40
                    [DeviceC-behavior-b2] quit
                    [DeviceC] traffic behavior b3
                    [DeviceC-behavior-b3] remark dscp 50
                    [DeviceC-behavior-b3] quit

         Step 4 Configure a traffic policy and apply it to interfaces.
                    # On DeviceC, create a traffic policy p1, bind traffic classifiers to traffic behaviors
                    in the traffic policy, and apply the traffic policy to the inbound direction of
                    interface 1 and interface 2 to re-mark packets.
                    [DeviceC] traffic policy p1
                    [DeviceC-trafficpolicy-p1] classifier c1 behavior b1
                    [DeviceC-trafficpolicy-p1] classifier c2 behavior b2
                    [DeviceC-trafficpolicy-p1] classifier c3 behavior b3
                    [DeviceC-trafficpolicy-p1] quit
                    [DeviceC] interface 10ge 1/0/1
                    [DeviceC-10GE1/0/1] traffic-policy p1 inbound
                    [DeviceC-10GE1/0/1] quit
                    [DeviceC] interface 10ge 1/0/2
                    [DeviceC-10GE1/0/2] traffic-policy p1 inbound
                    [DeviceC-10GE1/0/2] quit

                    ----End

Verifying the Configuration
                    # Check the traffic classifier configuration.
                    <DeviceC> display traffic classifier
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match 8021p 2

                      Classifier: c2
                       Type: OR
                       Rule(s):


Issue 01 (2025-03-03)                  Copyright © Huawei Technologies Co., Ltd.                          50
QoS Configuration
QoS Configuration                                                                                      6 Re-marking Configuration

                          if-match 8021p 5

                       Classifier: c3
                        Type: OR
                        Rule(s):
                          if-match 8021p 6

                    Total classifier number is 3

                    # Check the traffic policy configuration.
                    <DeviceC> display traffic policy
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: OR
                        Behavior: b1
                          Remark:
                           Remark dscp 15

                        Classifier: c2
                         Type: OR
                        Behavior: b2
                         Remark:
                           Remark dscp cs5

                        Classifier: c3
                         Type: OR
                        Behavior: b3
                         Remark:
                           Remark dscp 50

                    Total policy number is 3

                    # Check the traffic policy application records.
                    <DeviceC> display traffic-policy applied-record
                    Total records : 2
                    --------------------------------------------------------------------------------
                    Policy Type/Name                  Apply Parameter                 Slot State
                    --------------------------------------------------------------------------------
                    p1                         10GE1/0/1(IN)                   slot_1     success

                                              10GE1/0/2(IN)                   slot_1    success
                    --------------------------------------------------------------------------------


Configuration Scripts
                    DeviceC
                    #
                    sysname DeviceC
                    #
                    traffic classifier c1 type or
                     if-match 8021p 2
                    #
                    traffic classifier c2 type or
                     if-match 8021p 5
                    #
                    traffic classifier c3 type or
                     if-match 8021p 6
                    #
                    traffic behavior b1
                     remark dscp 15
                    #
                    traffic behavior b2
                     remark dscp cs5
                    #
                    traffic behavior b3


Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                               51
QoS Configuration
QoS Configuration                                                               6 Re-marking Configuration

                     remark dscp 50
                    #
                    traffic policy p1
                     classifier c1 behavior b1 precedence 5
                     classifier c2 behavior b2 precedence 10
                     classifier c3 behavior b3 precedence 15
                    #
                    interface 10GE1/0/1
                     traffic-policy p1 inbound
                    #
                    interface 10GE1/0/2
                     traffic-policy p1 inbound
                    #
                    return




Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                          52
QoS Configuration
QoS Configuration                                                               7 Redirection Configuration




                                   7          Redirection Configuration


                    7.1 Overview of Redirection
                    7.2 Configuration Precautions for Redirection
                    7.3 Configuring MQC-based Redirection
                    7.4 Example for Configuring Redirection to an Interface
                    7.5 Example for Configuring Redirection to a Next-Hop Address
                    7.6 Example for Configuring Association Between Redirection to a Next-Hop
                    Address and NQA
                    7.7 Example for Configuring Redirection to Implement Route Selection


7.1 Overview of Redirection
                    MQC-based redirection redirects packets that match traffic classification rules to a
                    specific location for processing.
                    The device supports redirection to the following objects:
                    ●   Redirection to an interface: Packets are redirected to a specified interface for
                        processing or further forwarding.
                    ●   Redirection to a next-hop address: When the received packets need to be
                        processed by a downstream device, packets can be redirected to a next-hop
                        address. This type of redirection is also called policy-based routing (PBR). PBR
                        takes precedence over direct routes, static routes, and routes generated using
                        dynamic routing protocols. Redirection to a next-hop address is also called
                        policy-based routing (PBR).
                        This operation has no detection mechanism. As such, if the corresponding link
                        fails, the next-hop address to which packets are redirected does not change
                        automatically; rather, a network administrator needs to change it manually or
                        the device waits for the ARP entry corresponding to the next-hop address to
                        age out. As a result, services cannot be switched immediately and may be
                        interrupted for a long period of time. You can associate Network Quality
                        Analysis (NQA) with redirection to a next-hop address to detect the link
                        status of the next hop. If the link of the next hop fails, NQA detection will
                        also fail and this type of redirection immediately becomes unavailable,

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                               53
QoS Configuration
QoS Configuration                                                                           7 Redirection Configuration


                         reducing the communication interruption time and improving service quality.
                         When associating NQA with redirection to a next-hop address, take into
                         account the following:
                         –    If an NQA test instance does not exist or it is not the Internet Control
                              Message Protocol (ICMP) type, link detection fails and redirection to a
                              next-hop address becomes unavailable.
                         –    If an NQA test instance is successfully associated with redirection to a
                              next-hop address and the link is detected to be normal, packets can be
                              redirected to a next-hop address.
                         –    If an NQA test instance is successfully associated but link detection
                              continuously fails and the number of detection times exceeds the preset
                              value, redirection to a next-hop address automatically becomes
                              unavailable.
                         –    If an NQA test instance is successfully associated with redirection to a
                              next-hop address and the link is detected to be normal, redirection to a
                              next-hop address automatically becomes available.


7.2 Configuration Precautions for Redirection

7.3 Configuring MQC-based Redirection
Context
                    After redirection is configured, the device can redirect packets matching traffic
                    classification rules to a specified interface and a specified next-hop address.

                          NOTE

                         ● For Layer 2 data traffic, you are advised to configure redirection to an interface. For
                           Layer 3 data traffic, you are advised to configure redirection to a next-hop address.
                         ● For details about MQC-related configuration precautions, see "Configuration Precautions
                           for MQC" in MQC Configuration.


Procedure
         Step 1 Configure a traffic classifier.
                    For details about how to configure a traffic classifier, see 3.4 Configuring a Traffic
                    Classifier in "MQC Configuration".
         Step 2 Configure a traffic behavior.
                    1.   Enter the system view.
                         system-view
                    2.   Create a traffic behavior and enter the traffic behavior view, or enter the view
                         of an existing traffic behavior.
                         traffic behavior behavior-name
                    3.   Configure a redirection action.
                         –    Redirection to an interface
                              redirect interface interface-type interface-number [ fail-action forward ]


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                         54
QoS Configuration
QoS Configuration                                                                             7 Redirection Configuration


                                      NOTE

                                     Generally, if a packet needs to be redirected to the outbound interface, the
                                     outbound interface needs to be added to the VLAN corresponding to the packet.
                         –      Redirection to a single next-hop IP address
                                redirect [ vpn-instance vpn-instance-name ] nexthop { ip-address [ track nqa admin-name
                                test-name [ reaction probe-failtimes fail-times ] ] } &<1-16> [ fail-action discard ] [ low-
                                precedence ]
                                redirect ipv6 [ vpn-instance vpn-instance-name ] nexthop { ipv6-address [ track nqa admin-
                                name test-name [ reaction probe-failtimes fail-times ] ] } &<1-16> [ fail-action discard ]
                         –      Redirection to multiple next-hop IP addresses
                                redirect load-balance [ vpn-instance vpn-instance-name ] nexthop { ip-address [ track nqa
                                admin-name test-name [ reaction probe-failtimes fail-times ] ] } &<1-16> [ fail-action
                                discard ] [ low-precedence ]
                                redirect ipv6 load-balance [ vpn-instance vpn-instance-name ] nexthop { ipv6-address [ track
                                nqa admin-name test-name [ reaction probe-failtimes fail-times ] ] } &<1-16> [ fail-action
                                discard ]

                                Only the S5732-H-V2, S6730E-H-V2, S6730-H-V2, S6780-H, S6750-H,
                                S6750E-S, S6750-S, S5755-S, S5755E-H, S5755-H, S5735R-S-V2, S5735E-
                                S-V2, S5735-S-V2, S5735I-S-V2, and S5735I-H-V2 support redirection to
                                multiple next-hop IP addresses.
                                The S3710-H does not support redirection to multiple next-hop IP
                                addresses.
                         –      Redirection to a remote next-hop address
                                redirect remote [ vpn-instance vpn-instance-name ] { ip-address [ track nqa admin-name test-
                                name [ reaction probe-failtimes fail-times ] ] } &<1-16> [ exact ] [ low-precedence ]
                                redirect remote [ vpn-instance vpn-instance-name ] ipv6-address &<1-16> [ exact ]

                                      NOTE

                                     If low-precedence is specified, the device does not support redirection of IPv6
                                     packets or IP packets encapsulated in tunnel mode.
                    4.   Exit the traffic behavior view.
                         quit

         Step 3 Configure a traffic policy.
                    1.   Create a traffic policy and enter the traffic policy view, or enter the view of an
                         existing traffic policy.
                         traffic policy policy-name

                    2.   Bind a traffic behavior and a traffic classifier to the traffic policy.
                         classifier classifier-name behavior behavior-name [ precedence precedence-value ]

                    3.   Exit the traffic policy view.
                         quit

         Step 4 Apply the traffic policy.
                    ●    Apply a traffic policy to the system.
                         a.     Apply a traffic policy to the system.
                                traffic-policy policy-name global [ slot slot-id ] { inbound | outbound }

                    ●    Apply a traffic policy to an interface.
                         a.     Enter the interface view.
                                interface interface-type interface-number

                         b.     Apply a traffic policy to the interface.
                                traffic-policy policy-name { inbound | outbound }


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                              55
QoS Configuration
QoS Configuration                                                                          7 Redirection Configuration


                        c.    Exit the interface view.
                              quit

                    ●   Apply a traffic policy to a VLAN.
                        a.    Create a VLAN and enter the VLAN view.
                              vlan vlan-id

                        b.    Apply a traffic policy to the VLAN.
                              traffic-policy policy-name { inbound | outbound }

                        c.    Exit the VLAN view.
                              quit

                    ●   Apply a traffic policy to a QoS group.
                        If the same traffic policy needs to be applied to multiple VLANs or interfaces,
                        you are advised to add the VLANs or interfaces to the same QoS group and
                        then apply the traffic policy to the QoS group.
                        a.    Create a QoS group and enter the QoS group view.
                              qos group group-name

                        b.    Add a specified interface or VLAN to the QoS group.
                              group-member { interface { interface-type interface-num | interface-name [ to interface-type
                              interface-num | interface-name ] } &<1-8> | vlan { vlanid [ to vlanid ] } &<1-8> }
                              Only one type of member can be specified.
                        c.    Apply a traffic policy to the QoS group.
                              traffic-policy policy-name { inbound | outbound }

                        d.    Exit the QoS group view.
                              quit

                    ----End

Verifying the Configuration

                    Operation                                           Command

                    Check the configured traffic classifiers.           display traffic classifier [ classifier-
                                                                        name ]

                    Check the configured traffic behaviors.             display traffic behavior [ behavior-
                                                                        name ]

                    Check the configured traffic policy.                display traffic policy [ policy-name
                                                                        [ classifier classifier-name ] ]

                    Check the traffic policy application                display traffic-policy applied-record
                    records.

                    Check TCAM delivery failures.                       display system tcam fail-record [ slot
                                                                        slot-id ]




Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                           56
QoS Configuration
QoS Configuration                                                                     7 Redirection Configuration


                    Operation                                        Command

                    Check the group indexes and rule                 display system tcam service brief
                    counts occupied by different services.           [ slot slot-id ]
                                                                     display system tcam service { cpcar
                                                                     slot slot-id | service-name slot slot-id
                                                                     [ chip chip-id ] }

                    Check the traffic policy application             display system tcam service traffic-
                    records.                                         policy

                    Check information about matched                  display system tcam match-rules slot
                    rules.                                           slot-id

                    Check statistics on packets that match           display traffic-policy statistics
                    a traffic policy.

                    Check matching fields and actions                display system tcam acl group-
                    supported by a traffic policy in each            information
                    view.

                    Check information about the resources display traffic-policy pre-state
                    occupied by the traffic policy to be
                    applied to determine whether the
                    traffic policy can be successfully applied
                    after the configuration is committed.




7.4 Example for Configuring Redirection to an Interface
Networking Requirements
                    In Figure 7-1, the server connects to the Internet through DeviceA, DeviceB, and
                    DeviceD. All traffic from the Internet needs to be redirected to DeviceC for filtering
                    to ensure the security of traffic to the server.

Figure 7-1 Network diagram of redirecting packets to an interface
     NOTE

   In this example, interface 1, interface 2, interface 3, and interface 4 represent 10GE 1/0/1, 10GE 1/0/2, 10GE
   1/0/3, and 10GE 1/0/4, respectively.




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                         57
QoS Configuration
QoS Configuration                                                             7 Redirection Configuration




Procedure
         Step 1 Create VLANs and configure interfaces to ensure Layer 2 connectivity.

                    # Create VLAN 200 and VLAN 300 on DeviceB.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 200 300

                    # Configure 10GE 1/0/1 on DeviceB as a trunk interface and add it to VLAN 200
                    and VLAN 300. Configure 10GE 1/0/2 and 10GE 1/0/3 on DeviceB as access
                    interfaces, and add 10GE 1/0/2 to VLAN 200 and 10GE 1/0/3 to VLAN 300.
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type trunk
                    [DeviceB-10GE1/0/1] port trunk allow-pass vlan 200 300
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type access
                    [DeviceB-10GE1/0/2] port default vlan 200
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] portswitch
                    [DeviceB-10GE1/0/3] port link-type access
                    [DeviceB-10GE1/0/3] port default vlan 300
                    [DeviceB-10GE1/0/3] quit

                    # Create VLAN 200 and VLAN 300 on DeviceA.

Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                           58
QoS Configuration
QoS Configuration                                                               7 Redirection Configuration

                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceA
                    [DeviceA] vlan batch 200 300

                    # Configure 10GE 1/0/1, 10GE 1/0/2, 10GE 1/0/3, and 10GE 1/0/4 on DeviceA as
                    trunk interfaces and add them to VLAN 200 and VLAN 300. To prevent loops, add
                    10GE 1/0/3 and 10GE 1/0/4 to the same port isolation group and disable MAC
                    address learning on 10GE 1/0/4 to prevent MAC address flapping.
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] portswitch
                    [DeviceA-10GE1/0/1] port link-type trunk
                    [DeviceA-10GE1/0/1] port trunk allow-pass vlan 200 300
                    [DeviceA-10GE1/0/1] quit
                    [DeviceA] interface 10ge 1/0/2
                    [DeviceA-10GE1/0/2] portswitch
                    [DeviceA-10GE1/0/2] port link-type trunk
                    [DeviceA-10GE1/0/2] port trunk allow-pass vlan 200 300
                    [DeviceA-10GE1/0/2] quit
                    [DeviceA] interface 10ge 1/0/3
                    [DeviceA-10GE1/0/3] portswitch
                    [DeviceA-10GE1/0/3] port link-type trunk
                    [DeviceA-10GE1/0/3] port trunk allow-pass vlan 200 300
                    [DeviceA-10GE1/0/3] port-isolate enable group 1
                    [DeviceA-10GE1/0/3] quit
                    [DeviceA] interface 10ge 1/0/4
                    [DeviceA-10GE1/0/4] portswitch
                    [DeviceA-10GE1/0/4] port link-type trunk
                    [DeviceA-10GE1/0/4] port trunk allow-pass vlan 200 300
                    [DeviceA-10GE1/0/4] port-isolate enable group 1
                    [DeviceA-10GE1/0/4] mac-address learning disable
                    [DeviceA-10GE1/0/4] quit

         Step 2 Configure redirection to an interface on DeviceA.
                    # Configure a traffic classifier. Configure a matching rule based on all data packets
                    in the traffic classifier c1.
                    [DeviceA] traffic classifier c1
                    [DeviceA-classifier-c1] if-match any
                    [DeviceA-classifier-c1] quit

                    # Configure a traffic behavior. Define redirection to a specified interface in the
                    traffic behavior b1.
                    [DeviceA] traffic behavior b1
                    [DeviceA-behavior-b1] redirect interface 10ge 1/0/3
                    [DeviceA-behavior-b1] quit

                    # Create a traffic policy p1, and bind the traffic classifier c1 and traffic behavior
                    b1 to the traffic policy.
                    [DeviceA] traffic policy p1
                    [DeviceA-trafficpolicy-p1] classifier c1 behavior b1
                    [DeviceA-trafficpolicy-p1] quit

                    # Apply the traffic policy to the inbound direction of 10GE 1/0/1.
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] traffic-policy p1 inbound
                    [DeviceA-10GE1/0/1] quit

                    ----End

Verifying the Configuration
                    # Check the traffic classifier configuration.

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                               59
QoS Configuration
QoS Configuration                                                                                      7 Redirection Configuration

                    <DeviceA> display traffic classifier
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match any

                    Total classifier number is 1

                    # Check the traffic behavior configuration.
                    <DeviceA> display traffic behavior
                     Traffic Behavior Information:
                      Behavior: b1
                        Redirect:
                          Redirect interface 10GE1/0/3

                    Total behavior number is 1

                    # Check the traffic policy configuration.
                    <DeviceA> display traffic policy
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: OR
                        Behavior: b1
                          Redirect:
                           Redirect interface 10GE1/0/3

                    Total policy number is 1

                    # Check the traffic policy application records.
                    <DeviceA> display traffic-policy applied-record
                    Total records : 1
                    --------------------------------------------------------------------------------
                    Policy Type/Name                     Apply Parameter              Slot State
                    --------------------------------------------------------------------------------
                    p1                             10GE1/0/1(IN)              1    success
                    --------------------------------------------------------------------------------


Configuration Scripts
                    ●     DeviceA
                          #
                          sysname DeviceA
                          #
                          vlan batch 200 300
                          #
                          traffic classifier c1 type or
                           if-match any
                          #
                          traffic behavior b1
                           redirect interface 10GE 1/0/3
                          #
                          traffic policy p1
                           classifier c1 behavior b1 precedence 5
                          #
                          interface 10GE1/0/1
                           port link-type trunk
                           port trunk allow-pass vlan 200 300
                           traffic-policy p1 inbound
                          #
                          interface 10GE1/0/2
                           port link-type trunk
                           port trunk allow-pass vlan 200 300
                          #
                          interface 10GE1/0/3


Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                                60
QoS Configuration
QoS Configuration                                                                     7 Redirection Configuration

                         port link-type trunk
                         port trunk allow-pass vlan 200 300
                         port-isolate enable group 1
                        #
                        interface 10GE1/0/4
                         port link-type trunk
                         port trunk allow-pass vlan 200 300
                         port-isolate enable group 1
                         mac-address learning disable
                        #
                        return

                    ●   DeviceB
                        #
                        sysname DeviceB
                        #
                        vlan batch 200 300
                        #
                        interface 10GE1/0/1
                         port link-type trunk
                         port trunk allow-pass vlan 200 300
                        #
                        interface 10GE1/0/2
                         port link-type access
                         port default vlan 200
                        #
                        interface 10GE1/0/3
                         port link-type access
                         port default vlan 300
                        #
                        return



7.5 Example for Configuring Redirection to a Next-Hop
Address
Networking Requirements
                    In Figure 7-2, DeviceA functioning as a Layer 3 forwarding device is routable to
                    NetworkA and is connected to the Internet through two links. One uplink is a
                    high-speed link with the gateway at 10.1.20.1/24, and the other is a low-speed
                    link with the gateway at 10.1.30.1/24. The user requires that DeviceA forward
                    packets from network segments 192.168.100.0/24 and 192.168.101.0/24 to the
                    Internet through the high-speed link and low-speed link, respectively. This
                    requirement can be met by configuring redirection to a next-hop address, which is
                    sometimes also called policy-based routing (PBR).

                    Figure 7-2 Network diagram of redirection to a next-hop address
                         NOTE

                        In this example, interface 1, interface 2, and interface 3 represent 10GE 1/0/1, 10GE 1/0/2,
                        and 10GE 1/0/3, respectively.




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                        61
QoS Configuration
QoS Configuration                                                              7 Redirection Configuration




Procedure
         Step 1 Create VLANs and configure interfaces.
                    # Create VLAN 10, VLAN 20, and VLAN 30 on DeviceA.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceA
                    [DeviceA] vlan batch 10 20 30

                    # Configure 10GE 1/0/1, 10GE 1/0/2, and 10GE 1/0/3 on DeviceA as trunk
                    interfaces and add them to corresponding VLANs.
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] portswitch
                    [DeviceA-10GE1/0/1] port link-type trunk
                    [DeviceA-10GE1/0/1] port trunk allow-pass vlan 10
                    [DeviceA-10GE1/0/1] quit
                    [DeviceA] interface 10ge 1/0/2
                    [DeviceA-10GE1/0/2] portswitch
                    [DeviceA-10GE1/0/2] port link-type trunk
                    [DeviceA-10GE1/0/2] port trunk allow-pass vlan 20
                    [DeviceA-10GE1/0/2] quit
                    [DeviceA] interface 10ge 1/0/3
                    [DeviceA-10GE1/0/3] portswitch
                    [DeviceA-10GE1/0/3] port link-type trunk
                    [DeviceA-10GE1/0/3] port trunk allow-pass vlan 30
                    [DeviceA-10GE1/0/3] quit

                    # Create VLANIF 10, VLANIF 20, and VLANIF 30 and configure IP addresses for
                    them.
                    [DeviceA] interface vlanif 10
                    [DeviceA-Vlanif10] ip address 10.1.10.2 24
                    [DeviceA-Vlanif10] quit
                    [DeviceA] interface vlanif 20
                    [DeviceA-Vlanif20] ip address 10.1.20.2 24
                    [DeviceA-Vlanif20] quit
                    [DeviceA] interface vlanif 30
                    [DeviceA-Vlanif30] ip address 10.1.30.2 24
                    [DeviceA-Vlanif30] quit


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                           62
QoS Configuration
QoS Configuration                                                                         7 Redirection Configuration


         Step 2 Configure ACL rules.
                    # Create advanced ACLs 3001 and 3002 on DeviceA to allow packets on network
                    segments 192.168.100.0/24 and 192.168.101.0/24 to pass through.
                    [DeviceA] acl 3001
                    [DeviceA-acl4-advance-3001] rule permit ip source 192.168.100.0 0.0.0.255
                    [DeviceA-acl4-advance-3001] quit
                    [DeviceA] acl 3002
                    [DeviceA-acl4-advance-3002] rule permit ip source 192.168.101.0 0.0.0.255
                    [DeviceA-acl4-advance-3002] quit

         Step 3 Configure traffic classifiers.
                    # Create traffic classifiers c1 and c2 on DeviceA, and bind c1 to ACL 3001 and c2
                    to ACL 3002.
                    [DeviceA] traffic classifier c1
                    [DeviceA-classifier-c1] if-match acl 3001
                    [DeviceA-classifier-c1] quit
                    [DeviceA] traffic classifier c2
                    [DeviceA-classifier-c2] if-match acl 3002
                    [DeviceA-classifier-c2] quit

         Step 4 Configure traffic behaviors.
                    # Create traffic behaviors b1 and b2 on DeviceA and configure actions that
                    redirect packets to IP addresses 10.1.20.1 and 10.1.30.1.
                    [DeviceA] traffic behavior b1
                    [DeviceA-behavior-b1] redirect nexthop 10.1.20.1
                    [DeviceA-behavior-b1] quit
                    [DeviceA] traffic behavior b2
                    [DeviceA-behavior-b2] redirect nexthop 10.1.30.1
                    [DeviceA-behavior-b2] quit

         Step 5 Configure a traffic policy and apply it to an interface.
                    # Create a traffic policy p1 on DeviceA, and bind the traffic classifier c1 to the
                    traffic behavior b1 and the traffic classifier c2 to the traffic behavior b2.
                    [DeviceA] traffic policy p1
                    [DeviceA-trafficpolicy-p1] classifier c1 behavior b1
                    [DeviceA-trafficpolicy-p1] classifier c2 behavior b2
                    [DeviceA-trafficpolicy-p1] quit

                    # Apply traffic policy p1 to the inbound direction of 10GE 1/0/1.
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] traffic-policy p1 inbound
                    [DeviceA-10GE1/0/1] quit

                    ----End

Verifying the Configuration
                    # Check the traffic classifier configuration.
                    <DeviceA> display traffic classifier
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match acl 3001

                      Classifier: c2
                       Type: OR


Issue 01 (2025-03-03)                  Copyright © Huawei Technologies Co., Ltd.                                  63
QoS Configuration
QoS Configuration                                                                                      7 Redirection Configuration

                        Rule(s):
                         if-match acl 3002

                    Total classifier number is 2

                    # Check the traffic policy configuration.
                    <DeviceA> display traffic policy
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: OR
                        Behavior: b1
                          Redirect:
                           Redirect nexthop
                           10.1.20.1

                        Classifier: c2
                         Type: OR
                        Behavior: b2
                         Redirect:
                           Redirect nexthop
                           10.1.30.1

                    Total policy number is 2

                    # Check the traffic policy application records.
                    <DeviceA> display traffic-policy applied-record
                    Total records : 1
                    --------------------------------------------------------------------------------
                    Policy Type/Name                     Apply Parameter              Slot State
                    --------------------------------------------------------------------------------
                    p1                             10GE1/0/1(IN)               1 success
                    --------------------------------------------------------------------------------


Configuration Scripts
                    DeviceA
                    #
                    sysname DeviceA
                    #
                    vlan batch 10 20 30
                    #
                    acl number 3001
                     rule 5 permit ip source 192.168.100.0 0.0.0.255
                    #
                    acl number 3002
                     rule 5 permit ip source 192.168.101.0 0.0.0.255
                    #
                    traffic classifier c1 type or
                     if-match acl 3001
                    #
                    traffic classifier c2 type or
                     if-match acl 3002
                    #
                    traffic behavior b1
                     redirect nexthop 10.1.20.1
                    #
                    traffic behavior b2
                     redirect nexthop 10.1.30.1
                    #
                    traffic policy p1
                     classifier c1 behavior b1 precedence 5
                     classifier c2 behavior b2 precedence 10
                    #
                    interface Vlanif10
                     ip address 10.1.10.2 255.255.255.0


Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                                64
QoS Configuration
QoS Configuration                                                                      7 Redirection Configuration

                    #
                    interface Vlanif20
                     ip address 10.1.20.2 255.255.255.0
                    #
                    interface Vlanif30
                     ip address 10.1.30.2 255.255.255.0
                    #
                    interface 10GE1/0/1
                     port link-type trunk
                     port trunk allow-pass vlan 10
                     traffic-policy p1 inbound
                    #
                    interface 10GE1/0/2
                     port link-type trunk
                     port trunk allow-pass vlan 20
                    #
                    interface 10GE1/0/3
                     port link-type trunk
                     port trunk allow-pass vlan 30
                    #
                    return



7.6 Example for Configuring Association Between
Redirection to a Next-Hop Address and NQA
Networking Requirements
                    In Figure 7-3, DeviceA is the upper-layer device of DeviceB and DeviceB is the user
                    gateway. There are reachable routes between DeviceA and DeviceB. DeviceA is
                    connected to the Internet through two links: high-speed link with the gateway at
                    10.1.20.1/24 and low-speed link with the gateway at 10.1.30.1/24. A default route
                    has been configured on DeviceA to ensure that traffic is transmitted through the
                    high-speed link by default. The customer requirements are as follows:
                    ●    Packets from the network segment 192.168.101.0/24 are redirected to the
                         low-speed link for transmission, alleviating the bandwidth pressure of the
                         high-speed link.
                    ●    If the low-speed link fails, packets from the network segment
                         192.168.101.0/24 can be rapidly switched back to the high-speed link to
                         minimize communication interruption caused by the link fault.

                    Figure 7-3 Network diagram of association between redirection to a next-hop
                    address and NQA
                          NOTE

                         In this example, interface 1, interface 2, and interface 3 represent 10GE 1/0/1, 10GE 1/0/2,
                         and 10GE 1/0/3, respectively.




Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                       65
QoS Configuration
QoS Configuration                                                             7 Redirection Configuration




Procedure
         Step 1 Create VLANs and configure interfaces.
                    # Configure DeviceA.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceA
                    [DeviceA] vlan batch 10 20 30
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] portswitch
                    [DeviceA-10GE1/0/1] port link-type trunk
                    [DeviceA-10GE1/0/1] port trunk allow-pass vlan 10
                    [DeviceA-10GE1/0/1] quit
                    [DeviceA] interface 10ge 1/0/2
                    [DeviceA-10GE1/0/2] portswitch
                    [DeviceA-10GE1/0/2] port link-type trunk
                    [DeviceA-10GE1/0/2] port trunk allow-pass vlan 20
                    [DeviceA-10GE1/0/2] quit
                    [DeviceA] interface 10ge1/0/3
                    [DeviceA-10GE1/0/3] portswitch
                    [DeviceA-10GE1/0/3] port link-type trunk
                    [DeviceA-10GE1/0/3] port trunk allow-pass vlan 30
                    [DeviceA-10GE1/0/3] quit
                    [DeviceA] interface vlanif 10
                    [DeviceA-Vlanif10] ip address 10.1.20.2 24
                    [DeviceA-Vlanif10] quit
                    [DeviceA] interface vlanif 20
                    [DeviceA-Vlanif20] ip address 10.1.30.2 24
                    [DeviceA-Vlanif20] quit
                    [DeviceA] interface vlanif 30
                    [DeviceA-Vlanif30] ip address 10.1.10.2 24
                    [DeviceA-Vlanif30] quit

                    # Configure DeviceC.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceC
                    [DeviceC] vlan batch 10
                    [DeviceC] interface 10ge 1/0/1
                    [DeviceC-10GE1/0/1] portswitch


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                           66
QoS Configuration
QoS Configuration                                                                          7 Redirection Configuration

                    [DeviceC-10GE1/0/1] port link-type trunk
                    [DeviceC-10GE1/0/1] port trunk allow-pass vlan 10
                    [DeviceC-10GE1/0/1] quit
                    [DeviceC] interface vlanif 10
                    [DeviceC-Vlanif10] ip address 10.1.20.1 24
                    [DeviceC-Vlanif10] quit

                    # Configure DeviceD.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceD
                    [DeviceD] vlan batch 20
                    [DeviceD] interface 10ge 1/0/1
                    [DeviceD-10GE1/0/1] portswitch
                    [DeviceD-10GE1/0/1] port link-type trunk
                    [DeviceD-10GE1/0/1] port trunk allow-pass vlan 20
                    [DeviceD-10GE1/0/1] quit
                    [DeviceD] interface vlanif 20
                    [DeviceD-Vlanif20] ip address 10.1.30.1 24
                    [DeviceD-Vlanif20] quit

         Step 2 On DeviceA, configure an NQA test instance.
                    [DeviceA] nqa test-instance user test
                    [DeviceA-nqa-user-test] test-type icmp
                    [DeviceA-nqa-user-test] destination-address ipv4 10.1.30.1
                    [DeviceA-nqa-user-test] frequency 11
                    [DeviceA-nqa-user-test] probe-count 2
                    [DeviceA-nqa-user-test] interval seconds 5
                    [DeviceA-nqa-user-test] timeout 4
                    [DeviceA-nqa-user-test] start now
                    [DeviceA-nqa-user-test] quit

         Step 3 Configure an ACL rule.
                    # Create advanced ACL 3001 on DeviceA to allow packets from the network
                    segment 192.168.101.0/24 to pass through.
                    [DeviceA] acl 3001
                    [DeviceA-acl4-advance-3001] rule permit ip source 192.168.101.0 0.0.0.255
                    [DeviceA-acl4-advance-3001] quit

         Step 4 Configure a traffic classifier.
                    # Create a traffic classifier c1 on DeviceA and reference ACL 3001.
                    [DeviceA] traffic classifier c1
                    [DeviceA-classifier-c1] if-match acl 3001
                    [DeviceA-classifier-c1] quit

         Step 5 Configure a traffic behavior.
                    # Create a traffic behavior b1 on DeviceA to redirect packets to the IP address
                    10.1.30.1, and associate NQA with redirection to a next-hop address.
                    [DeviceA] traffic behavior b1
                    [DeviceA-behavior-b1] redirect nexthop 10.1.30.1 track nqa user test
                    [DeviceA-behavior-b1] quit

         Step 6 Configure a traffic policy and apply it to an interface.
                    # Create a traffic policy p1 on DeviceA, and bind the traffic classifier c1 and traffic
                    behavior b1 to the traffic policy.
                    [DeviceA] traffic policy p1
                    [DeviceA-trafficpolicy-p1] classifier c1 behavior b1
                    [DeviceA-trafficpolicy-p1] quit

                    # Apply traffic policy p1 to the inbound direction of 10GE 1/0/3.

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                      67
QoS Configuration
QoS Configuration                                                                                      7 Redirection Configuration

                    [DeviceA] interface 10ge 1/0/3
                    [DeviceA-10GE1/0/3] traffic-policy p1 inbound
                    [DeviceA-10GE1/0/3] quit
                    [DeviceA] quit

                    ----End

Verifying the Configuration
                    # Check the ACL configuration.
                    <DeviceA> display acl 3001
                    Advanced ACL 3001, 1 rule
                    ACL's step is 5
                     rule 5 permit ip source 192.168.101.0 0.0.0.255 (0 times matched)

                    # Check the traffic classifier configuration.
                    <DeviceA> display traffic classifier
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match acl 3001

                    Total classifier number is 1

                    # Check the traffic policy configuration.
                    <DeviceA> display traffic policy
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: OR
                        Behavior: b1
                          Redirect:
                           Redirect nexthop
                           10.1.30.1 track nqa user test

                    Total policy number is 1

                    # Check the traffic policy application records.
                    <DeviceA> display traffic-policy applied-record
                    Total records : 1
                    --------------------------------------------------------------------------------
                    Policy Type/Name                     Apply Parameter               Slot State
                    --------------------------------------------------------------------------------
                    p1                             10GE1/0/3(IN)                1 success
                    --------------------------------------------------------------------------------

                    # Check the NQA test result. If "Completion:success" and "Lost packet ratio: 0 %"
                    are displayed, the NQA test succeeds and the link is normal.
                    <DeviceA> display nqa results test-instance user test

                    NQA entry(user, test) :test flag is active ,test type is ICMP
                    1 . Test 1 result The test is finished
                     Send operation times: 2              Receive response times: 2
                     Completion:success                  RTD over thresholds number: 0
                     Attempts number:1                    Drop operation number:0
                     Disconnect operation number:0           Operation timeout number:0
                     System busy operation number:0           Connection fail number:0
                     Operation sequence errors number:0 RTT Status errors number:0
                     Destination IP address:10.1.30.1
                     Min/Max/Average completion time: 3/4/3
                     Sum/Square-Sum completion time: 7/25
                     Last response packet receiving tim: 2020-04-09 09:55:38.2
                     Lost packet ratio: 0 %


Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                                68
QoS Configuration
QoS Configuration                                                             7 Redirection Configuration


Configuration Scripts
                    ●   DeviceA
                        #
                        sysname DeviceA
                        #
                        vlan batch 10 20 30
                        #
                        acl number 3001
                         rule 5 permit ip source 192.168.101.0 0.0.0.255
                        #
                        traffic classifier c1 type or
                         if-match acl 3001
                        #
                        traffic behavior b1
                         redirect nexthop 10.1.30.1 track nqa user test
                        #
                        traffic policy p1
                         classifier c1 behavior b1 precedence 5
                        #
                        interface Vlanif10
                         ip address 10.1.20.2 255.255.255.0
                        #
                        interface Vlanif20
                         ip address 10.1.30.2 255.255.255.0
                        #
                        interface Vlanif30
                         ip address 10.1.10.2 255.255.255.0
                        #
                        interface 10GE1/0/1
                         port link-type trunk
                         port trunk allow-pass vlan 10
                        #
                        interface 10GE1/0/2
                         port link-type trunk
                         port trunk allow-pass vlan 20
                        #
                        interface 10GE1/0/3
                         port link-type trunk
                         port trunk allow-pass vlan 30
                         traffic-policy p1 inbound
                        #
                        nqa test-instance user test
                         test-type icmp
                         destination-address ipv4 10.1.30.1
                         interval seconds 5
                         timeout 4
                         probe-count 2
                         frequency 11
                         start now
                        #
                        return

                    ●   DeviceC
                        #
                        sysname DeviceC
                        #
                        vlan batch 10
                        #
                        interface Vlanif10
                         ip address 10.1.20.1 255.255.255.0
                        #
                        interface 10GE1/0/1
                         port link-type trunk
                         port trunk allow-pass vlan 10
                        #
                        return

                    ●   DeviceD


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                           69
QoS Configuration
QoS Configuration                                                                     7 Redirection Configuration

                        #
                        sysname DeviceD
                        #
                        vlan batch 20
                        #
                        interface Vlanif20
                         ip address 10.1.30.1 255.255.255.0
                        #
                        interface 10GE1/0/1
                         port link-type trunk
                         port trunk allow-pass vlan 20
                        #
                        return



7.7 Example for Configuring Redirection to Implement
Route Selection
Networking Requirements
                    In Figure 7-4, the PC needs to access the server through link A, link B, and link C
                    (is used by default) in descending order of priority.

                    Figure 7-4 Network diagram for configuring redirection to implement route
                    selection
                         NOTE

                        In this example, interface 1, interface 2, and interface 3 represent 10GE 1/0/1, 10GE 1/0/2,
                        and 10GE 1/0/3, respectively.




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                        70
QoS Configuration
QoS Configuration                                                              7 Redirection Configuration




Configuration Roadmap
                    The configuration roadmap is as follows:
                    1.   Configure a dynamic routing protocol to generate a route or configure a static
                         route destined for the server and specify 10.1.1.2/24 as the next-hop address
                         so that packets pass through link A.
                    2.   Redirect the packets that match the network segment where the server
                         resides to the next-hop address 10.1.2.2/24 and allow the packets to pass
                         through link B; specify low-preference for redirection to a next-hop address;
                         configure a high priority for the traffic classifier when binding the traffic
                         classifier and the traffic behavior to the traffic policy. In this way, packets
                         preferentially pass through link A.
                    3.   Configure a default route and specify 10.1.3.2/24 as the next-hop address so
                         that packets pass through link C.
                    4.   After the configuration is complete, DeviceA handles packets in one of the
                         following ways (listed in descending order of priority): forwards packets
                         according to the route generated by a routing protocol or static route >
                         redirects packets matching the network segment where the server resides to a
                         next-hop address > forwards packets to a next-hop address according to the
                         default route if no matching route is found.




Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                             71
QoS Configuration
QoS Configuration                                                                          7 Redirection Configuration


Procedure
         Step 1 Create VLANs and configure interfaces.

                    # Create VLANs 10, 20, and 30 on DeviceA.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceA
                    [DeviceA] vlan batch 10 20 30

                    # Configure 10GE 1/0/1, 10GE 1/0/2, and 10GE 1/0/3 on DeviceA as trunk
                    interfaces and add them to VLAN 10, VLAN 20, and VLAN 30, respectively.
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] portswitch
                    [DeviceA-10GE1/0/1] port link-type trunk
                    [DeviceA-10GE1/0/1] port trunk allow-pass vlan 10
                    [DeviceA-10GE1/0/1] quit
                    [DeviceA] interface 10ge 1/0/2
                    [DeviceA-10GE1/0/2] portswitch
                    [DeviceA-10GE1/0/2] port link-type trunk
                    [DeviceA-10GE1/0/2] port trunk allow-pass vlan 20
                    [DeviceA-10GE1/0/2] quit
                    [DeviceA] interface 10ge 1/0/3
                    [DeviceA-10GE1/0/3] portswitch
                    [DeviceA-10GE1/0/3] port link-type trunk
                    [DeviceA-10GE1/0/3] port trunk allow-pass vlan 30
                    [DeviceA-10GE1/0/3] quit

                    # Create VLANIF 10, VLANIF 20, and VLANIF 30 and configure IP addresses for
                    them.
                    [DeviceA] interface vlanif 10
                    [DeviceA-Vlanif10] ip address 10.1.1.1 24
                    [DeviceA-Vlanif10] quit
                    [DeviceA] interface vlanif 20
                    [DeviceA-Vlanif20] ip address 10.1.2.1 24
                    [DeviceA-Vlanif20] quit
                    [DeviceA] interface vlanif 30
                    [DeviceA-Vlanif30] ip address 10.1.3.1 24
                    [DeviceA-Vlanif30] quit

         Step 2 Configure a static route and a default route to the server.

                    # Configure a static route to the server and specify 10.1.1.2 as the next-hop
                    address.
                    [DeviceA] ip route-static 10.1.4.0 24 10.1.1.2

                    # Configure a default route to the server and specify 10.1.3.2 as the next-hop
                    address.
                    [DeviceA] ip route-static 0.0.0.0 0 10.1.3.2

         Step 3 Configure a traffic classifier.

                    # On DeviceA, create ACL 3001 to allow packets destined for the network
                    segment 10.1.4.0/24 to pass through.
                    [DeviceA] acl 3001
                    [DeviceA-acl4-advance-3001] rule permit ip destination 10.1.4.0 0.0.0.255
                    [DeviceA-acl4-advance-3001] quit

                    # On DeviceA, create a traffic classifier c1 and configure a matching rule based on
                    ACL 3001.

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                      72
QoS Configuration
QoS Configuration                                                                        7 Redirection Configuration

                    [DeviceA] traffic classifier c1
                    [DeviceA-classifier-c1] if-match acl 3001
                    [DeviceA-classifier-c1] quit

         Step 4 Configure a traffic behavior.
                    # On DeviceA, create a traffic behavior b1, specify 10.1.2.2 as the next-hop
                    address to which packets are redirected, and specify low-preference so that the
                    priority of redirection to the next-hop address is lower than that of the route
                    generated by a dynamic routing protocol or a static route.
                    [DeviceA] traffic behavior b1
                    [DeviceA-behavior-b1] redirect nexthop 10.1.2.2 low-precedence
                    [DeviceA-behavior-b1] quit

         Step 5 Configure a traffic policy.
                    # On DeviceA, create a traffic policy p1 and bind the traffic classifier to the traffic
                    behavior in the traffic policy.
                    [DeviceA] traffic policy p1
                    [DeviceA-trafficpolicy-p1] classifier c1 behavior b1 precedence 5
                    [DeviceA-trafficpolicy-p1] quit

                    # Apply the traffic policy p1 to the system. After the configuration is complete, all
                    packets received by DeviceA match the traffic policy p1.
                    [DeviceA] traffic-policy p1 global inbound
                    [DeviceA] quit

                    ----End

Verifying the Configuration
                    # Check the ACL configuration.
                    <DeviceA> display acl 3001
                    Advanced ACL 3001, 1 rule
                    ACL's step is 5
                     rule 5 permit ip destination 10.1.4.0 0.0.0.255 (0 times matched)

                    # Check the traffic classifier configuration.
                    <DeviceA> display traffic classifier
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match acl 3001

                    Total classifier number is 1

                    # Check the traffic policy configuration.
                    <DeviceA> display traffic policy
                     Traffic Policy Information:
                       Policy: p1
                        Classifier: c1
                          Type: OR
                        Behavior: b1
                          Redirect:
                           Redirect nexthop
                           10.1.2.2 low-
                    precedence
                    Total policy number is 1

                    # Check the traffic policy application records.

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                    73
QoS Configuration
QoS Configuration                                                                                      7 Redirection Configuration

                    <DeviceA> display traffic-policy applied-record
                    Total records : 1
                    --------------------------------------------------------------------------------
                    Policy Type/Name                     Apply Parameter              Slot State
                    --------------------------------------------------------------------------------
                    p1                             Global(IN)               1     success
                    --------------------------------------------------------------------------------


Configuration Scripts
                    DeviceA
                    #
                    sysname DeviceA
                    #
                    vlan batch 10 20 30
                    #
                    traffic-policy p1 global inbound
                    #
                    acl number 3001
                     rule 5 permit ip destination 10.1.4.0 0.0.0.255
                    #
                    traffic classifier c1 type or
                     if-match acl 3001
                    #
                    traffic behavior b1
                     redirect nexthop 10.1.2.2 low-precedence
                    #
                    traffic policy p1
                     classifier c1 behavior b1 precedence 5
                    #
                    interface Vlanif10
                     ip address 10.1.1.1 255.255.255.0
                    #
                    interface Vlanif20
                     ip address 10.1.2.1 255.255.255.0
                    #
                    interface Vlanif30
                     ip address 10.1.3.1 255.255.255.0
                    #
                    interface 10GE1/0/1
                     port link-type trunk
                     port trunk allow-pass vlan 10
                    #
                    interface 10GE1/0/2
                     port link-type trunk
                     port trunk allow-pass vlan 20
                    #
                    interface 10GE1/0/3
                     port link-type trunk
                     port trunk allow-pass vlan 30
                    #
                    ip route-static 10.1.4.0 255.255.255.0 10.1.1.2
                    ip route-static 0.0.0.0 0 10.1.3.2
                    #
                    return




Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                                74
QoS Configuration
QoS Configuration                                                         8 Priority Mapping Configuration




                        8        Priority Mapping Configuration


                    8.1 Overview of Priority Mapping
                    8.2 Understanding Priority Mapping
                    8.3 Configuration Precautions for Priority Mapping
                    8.4 Default Settings for Priority Mapping
                    8.5 Configuring DiffServ Domain-based Priority Mapping
                    8.6 Configuring an Interface Priority
                    8.7 Configuring the Internal Priority of Protocol Packets Sent by the Local Device
                    8.8 Configuring the Mapping Between Internal Priorities and Queues
                    8.9 Configuring a Packet Priority
                    8.10 Troubleshooting Priority Mapping


8.1 Overview of Priority Mapping
Definition
                    Priority mapping maps external priorities carried in packets into internal priorities
                    on a device, and manages and records the mapping between external and internal
                    priorities using DiffServ domains. In this way, the device provides differentiated
                    services for packets based on internal priorities.

                    Common concepts of priorities are described as follows:

                    ●   External priority
                        The external priority is also known as the packet priority or QoS priority. QoS
                        information is recorded by using certain fields in packets, such as the 802.1p
                        value of VLAN packets and the Differentiated Services Code Point (DSCP)
                        value of IP packets. A device can process received packets only based on
                        internal priorities to provide differentiated QoS levels for different services. As
                        such, external priorities are mapped to internal priorities after packets enter
                        the device.

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                75
QoS Configuration
QoS Configuration                                                        8 Priority Mapping Configuration


                    ●   Internal priority
                        The internal priority is also known as the class of service (CoS), per-hop
                        behavior (PHB), or local priority. Internal priority values are CS7, CS6, EF, AF4,
                        AF3, AF2, AF1, and BE (in descending order of priority), and correspond to
                        queues 7, 6, 5, 4, 3, 2, 1, and 0, respectively. The internal priority determines
                        the queue into which packets are placed. When QoS services are configured
                        for a queue, the same QoS level is configured for all the packets forwarded
                        through that queue.
                    ●   Drop priority
                        The drop priority is also known as a color. It determines the sequence in
                        which packets are dropped when congestion occurs in a queue, without
                        affecting the mapping between internal priorities and queues. The drop
                        priorities defined by Institute of Electrical and Electronics Engineers (IEEE) are
                        green, yellow, and red in ascending order. By default, the device first discards
                        packets with a higher drop priority when congestion occurs in a queue.
                        Whether packets are discarded first is determined by QoS parameter settings.
                        For example, in a weighted random early detection (WRED) drop profile, if
                        green packets can use a maximum of 50% of the buffer and red packets can
                        use the entire buffer, the device first discards green packets when congestion
                        occurs in a queue.

Purpose
                    Packets transmitted over different networks carry different external priority fields.
                    For example, the 802.1p value is used on a VLAN, and the DSCP value is used on
                    an IP network. The device processes packets transmitted on the network as
                    follows:
                    ●   For all incoming packets, the device maps external priorities (including 802.1p
                        and DSCP values) to internal priorities, places the packets into specific queues
                        based on the mapping, and applies traffic shaping, congestion avoidance, or
                        congestion management to the queues.
                    ●   For outgoing packets, the device maps the internal priority to an external
                        priority, and other devices can provide QoS services based on the external
                        priority.


8.2 Understanding Priority Mapping
External Priority Fields
                    Certain fields in the packet header or frame header record QoS information so
                    that network devices can provide differentiated services. These fields include:
                    ●   Precedence field
                        As defined in RFC 791, the 8-bit Type of Service (ToS) field in an IP packet
                        header contains a 3-bit IP precedence field. Figure 8-1 shows the format of
                        the precedence field in an IP packet.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                               76
QoS Configuration
QoS Configuration                                                         8 Priority Mapping Configuration


                        Figure 8-1 IP Precedence/DSCP field




                        Bits 0 to 2 constitute the precedence field, representing values 7, 6, 5, 4, 3, 2,
                        1 and 0, listed in descending order of priority. The highest-priority values (7
                        and 6) are reserved for routing and network control communication updates.
                        User-level applications can use only priority values 0 to 5.
                        In addition to the precedence field, a ToS field contains the following sub-
                        fields:
                        –   Bit D indicates the delay. The value 0 represents a normal delay, and
                            value 1 represents a short delay.
                        –   Bit T indicates the throughput. The value 0 represents a normal
                            throughput, and value 1 represents a high throughput.
                        –   Bit R indicates the reliability. The value 0 represents normal reliability,
                            and value 1 represents high reliability.
                    ●   DSCP field
                        The ToS field in IP packets was defined in RFC 1349, with bit C (monetary
                        cost) added. Following that, the IETF DiffServ Working Group redefined bits 0
                        to 5 of a ToS field as the DSCP field in RFC 2474. In RFC 2474, the field name
                        is changed from ToS to differentiated service (DS). Figure 8-1 shows the
                        DSCP field in packets.
                        In the DS field, the first six bits (bits 0 to 5) are the DS CodePoint (DSCP) and
                        the last two bits (bits 6 and 7) are reserved. Additionally, the first three bits
                        (bits 0 to 2) are the Class Selector CodePoint (CSCP), which represents the
                        DSCP type. A DS node selects a PHB based on the DSCP value.
                    ●   802.1p value in an Ethernet frame header
                        Layer 2 devices exchange VLAN frames. As defined in IEEE 802.1Q, the PRI
                        field (802.1p value, also known as CoS) in the Ethernet frame header
                        identifies the QoS requirement. Figure 8-2 shows the format of the 802.1p
                        value in an Ethernet frame header.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                 77
QoS Configuration
QoS Configuration                                                        8 Priority Mapping Configuration


                        Figure 8-2 802.1p value in an Ethernet frame header




                        The 802.1Q header contains a 3-bit PRI field and a 1-bit CFI field. The PRI
                        field defines eight service priority values 7, 6, 5, 4, 3, 2, 1, and 0, listed in
                        descending order of priority. The CFI field defines the drop priority of packets.

Implementation
                    In Figure 8-3, voice, video, and data traffic from Host1, Host2, and Host3 traverses
                    DeviceA and DeviceB to reach the network. As voice, video, and data services have
                    priorities in descending order, the device needs to provide differentiated QoS
                    services based on their priorities.
                    Packets carry different precedence fields depending on the network type. For
                    example, packets carry the 802.1p value on a Layer 2 network and the DSCP value
                    on a Layer 3 network. When packets enter a device, the device maps their external
                    priorities to internal priorities and drop priorities, and provides differentiated QoS
                    services for the packets according to their internal priorities and drop priorities.
                    When packets leave the device, the device maps the internal priorities and drop
                    priorities to external priorities so that the network can provide corresponding QoS
                    services based on the external priorities of the packets.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                              78
QoS Configuration
QoS Configuration                                                       8 Priority Mapping Configuration


                    Figure 8-3 Implementation of priority mapping




                    The implementation of priority mapping is described as follows:
                    1.   On DeviceA, configure traffic policies in the inbound direction to re-mark
                         priorities of voice, video, and data packets into different 802.1p values.
                    2.   On DeviceB, map 802.1p values to internal and drop priorities in the inbound
                         direction to provide differentiated QoS services for packets based on the
                         internal and drop priorities.
                    3.   On DeviceB, map the internal and drop priorities to the corresponding DSCP
                         values in the outbound direction so that the Layer 3 network can provide
                         differentiated QoS services for the three types of services based on the DSCP
                         values.


8.3 Configuration Precautions for Priority Mapping

8.4 Default Settings for Priority Mapping
                    Table 8-1 describes the default settings for priority mapping.




Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                            79
QoS Configuration
QoS Configuration                                                               8 Priority Mapping Configuration


                    Table 8-1 Default settings for priority mapping

                     Parameter                                       Default Setting

                     Priority trusted on an interface                After packets arrive at an interface:
                                                                     ● Layer 2 forwarding mode
                                                                         If packets carry VLAN tags, the
                                                                         interface trusts 802.1p values;
                                                                         however, if they do not, the
                                                                         interface forwards them based on
                                                                         the default interface priority.
                                                                     ● Layer 3 forwarding mode
                                                                         By default, DSCP values are trusted.

                     Mapping PHBs to DSCP values of                  Disabled
                     outgoing packets on an interface

                     Mapping PHBs to 802.1p values of                Enabled
                     outgoing packets on an interface

                     DiffServ domain applied to an                   DiffServ domain default
                     interface

                     Interface priority                              0




                    In addition, the device defines default packet priority mappings, which are
                    referenced during priority mapping.


Default Priority Mappings for Incoming Packets on an Interface
                    Table 8-2 lists the default mappings from 802.1p values of VLAN packets to
                    internal priorities or drop priorities.

                         NOTE

                    The mappings of interface priorities to PHBs and colors are similar to the mappings of 802.1p
                    values to PHBs and colors.


                    Table 8-2 Mapping between 802.1p values and internal priorities or drop priorities
                    in the inbound direction on the device

                     802.1p Value                      Internal Priority (CoS)        Drop Priority (Color)

                     0                                 BE                             Green

                     1                                 AF1                            Green

                     2                                 AF2                            Green

                     3                                 AF3                            Green

                     4                                 AF4                            Green


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                         80
QoS Configuration
QoS Configuration                                                        8 Priority Mapping Configuration


                     802.1p Value                 Internal Priority (CoS)     Drop Priority (Color)

                     5                            EF                          Green

                     6                            CS6                         Green

                     7                            CS7                         Green




                    Table 8-3 lists the default mappings from DSCP values in IP packets to internal
                    priorities or drop priorities.

                    Table 8-3 Mappings from DSCP values in IP packets to internal priorities or drop
                    priorities in the inbound direction

                     DSCP         Internal Priority    Drop      DSCP       Internal Priority   Drop
                     Value        (CoS)                Priorit   Value      (CoS)               Priorit
                                                       y                                        y
                                                       (Color                                   (Color
                                                       )                                        )

                     0            BE                   Green     32         AF4                 Green

                     1            BE                   Green     33         AF4                 Green

                     2            BE                   Green     34         AF4                 Green

                     3            BE                   Green     35         AF4                 Green

                     4            BE                   Green     36         AF4                 Yellow

                     5            BE                   Green     37         AF4                 Green

                     6            BE                   Green     38         AF4                 Red

                     7            BE                   Green     39         AF4                 Green

                     8            AF1                  Green     40         EF                  Green

                     9            AF1                  Green     41         EF                  Green

                     10           AF1                  Green     42         EF                  Green

                     11           AF1                  Green     43         EF                  Green

                     12           AF1                  Yellow    44         EF                  Green

                     13           AF1                  Green     45         EF                  Green

                     14           AF1                  Red       46         EF                  Green

                     15           AF1                  Green     47         EF                  Green

                     16           AF2                  Green     48         CS6                 Green

                     17           AF2                  Green     49         CS6                 Green

                     18           AF2                  Green     50         CS6                 Green


Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                              81
QoS Configuration
QoS Configuration                                                      8 Priority Mapping Configuration


                     DSCP        Internal Priority   Drop      DSCP         Internal Priority   Drop
                     Value       (CoS)               Priorit   Value        (CoS)               Priorit
                                                     y                                          y
                                                     (Color                                     (Color
                                                     )                                          )

                     19          AF2                 Green     51           CS6                 Green

                     20          AF2                 Yellow    52           CS6                 Green

                     21          AF2                 Green     53           CS6                 Green

                     22          AF2                 Red       54           CS6                 Green

                     23          AF2                 Green     55           CS6                 Green

                     24          AF3                 Green     56           CS7                 Green

                     25          AF3                 Green     57           CS7                 Green

                     26          AF3                 Green     58           CS7                 Green

                     27          AF3                 Green     59           CS7                 Green

                     28          AF3                 Yellow    60           CS7                 Green

                     29          AF3                 Green     61           CS7                 Green

                     30          AF3                 Red       62           CS7                 Green

                     31          AF3                 Green     63           CS7                 Green




Mappings Between Internal Priorities and Inbound Queue Indexes
                    The internal priority on a device determines the queue from which packets are
                    forwarded. Table 8-4 describes the mappings between internal priorities and
                    queues.

                    Table 8-4 Mappings between internal priorities and queue indexes
                     Internal Priority (CoS)                   Queue Index

                     BE                                        0

                     AF1                                       1

                     AF2                                       2

                     AF3                                       3

                     AF4                                       4

                     EF                                        5

                     CS6                                       6




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                             82
QoS Configuration
QoS Configuration                                                        8 Priority Mapping Configuration


                     Internal Priority (CoS)                    Queue Index

                     CS7                                        7




Default Priority Mappings for Outgoing Packets on an Interface
                    Table 8-5 lists the default mappings from internal priorities or drop priorities to
                    802.1p values of outgoing VLAN packets.


                    Table 8-5 Mappings from internal priorities or drop priorities to 802.1p values in
                    the outbound direction

                     Internal Priority (CoS)       Drop Priority (Color)       802.1p Value

                     BE                            Green                       0

                     BE                            Yellow                      0

                     BE                            Red                         0

                     AF1                           Green                       1

                     AF1                           Yellow                      1

                     AF1                           Red                         1

                     AF2                           Green                       2

                     AF2                           Yellow                      2

                     AF2                           Red                         2

                     AF3                           Green                       3

                     AF3                           Yellow                      3

                     AF3                           Red                         3

                     AF4                           Green                       4

                     AF4                           Yellow                      4

                     AF4                           Red                         4

                     EF                            Green                       5

                     EF                            Yellow                      5

                     EF                            Red                         5

                     CS6                           Green                       6

                     CS6                           Yellow                      6

                     CS6                           Red                         6

                     CS7                           Green                       7


Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                 83
QoS Configuration
QoS Configuration                                                        8 Priority Mapping Configuration


                     Internal Priority (CoS)       Drop Priority (Color)       802.1p Value

                     CS7                           Yellow                      7

                     CS7                           Red                         7




                    Table 8-6 lists the default mappings from internal priorities or drop priorities to
                    DSCP values of outgoing IP packets.


                    Table 8-6 Mappings from internal priorities or drop priorities to DSCP values of IP
                    packets in the outbound direction

                     Internal Priority (CoS)       Drop Priority (Color)       DSCP Value

                     BE                            Green                       0

                     BE                            Yellow                      0

                     BE                            Red                         0

                     AF1                           Green                       10

                     AF1                           Yellow                      12

                     AF1                           Red                         14

                     AF2                           Green                       18

                     AF2                           Yellow                      20

                     AF2                           Red                         22

                     AF3                           Green                       26

                     AF3                           Yellow                      28

                     AF3                           Red                         30

                     AF4                           Green                       34

                     AF4                           Yellow                      36

                     AF4                           Red                         38

                     EF                            Green                       46

                     EF                            Yellow                      46

                     EF                            Red                         46

                     CS6                           Green                       48

                     CS6                           Yellow                      48

                     CS6                           Red                         48

                     CS7                           Green                       56


Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                 84
QoS Configuration
QoS Configuration                                                                  8 Priority Mapping Configuration


                     Internal Priority (CoS)                   Drop Priority (Color)    DSCP Value

                     CS7                                       Yellow                   56

                     CS7                                       Red                      56




8.5 Configuring DiffServ Domain-based Priority
Mapping
Context
                    The process of configuring DiffServ Domain-based priority mapping is described as
                    follows:
                    1.    Specify the packet priority trusted by an interface so that the device performs
                          priority mapping based on the trusted priority.
                    2.    Configure a DiffServ domain to specify the mapping between external and
                          internal priorities. The device can then provide differentiated QoS services
                          based on internal priorities.
                    3.    Apply the DiffServ domain to an object for the mapping in the DiffServ
                          domain to take effect. By doing this, the device can re-mark priorities of
                          packets according to these mappings.
                    4.    Verify the configuration of the DiffServ domain.

8.5.1 Specifying the Packet Priority Trusted on an Interface
Context
                    The priority trusted on an interface determines the type of priority to be mapped
                    for packets on the interface.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Enter the interface view.
                    interface interface-type interface-number

         Step 3 Specify the priority to be mapped for packets.
                    trust { 8021p { inner | outer } | dscp }

                           NOTE

                         The Ethernet interface working in Layer 3 mode trusts DSCP values by default, and the
                         priority to be mapped for packets cannot be specified.
                         Layer 3 sub-interfaces do not support the trust 8021p inner command.
                         The inner parameter, that is, performing mapping for packets based on the 802.1p priority
                         in the inner VLAN tag, is not supported by the S5755-S series.

                    ----End

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                    85
QoS Configuration
QoS Configuration                                                        8 Priority Mapping Configuration


8.5.2 Configuring a DiffServ Domain
Context
                    When the device is used as an edge node connecting the DiffServ domain and
                    another network, you need to configure mappings between internal and external
                    priorities.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Create a DiffServ domain and enter the DiffServ domain view, or enter the view of
                an existing DiffServ domain.
                    diffserv domain ds-domain-name

                    The domain default defines default mappings between external priorities and
                    internal or drop priorities. You can modify the mappings defined in the domain
                    default, but you cannot delete this domain.
         Step 3 Run the following commands as required.
                     Operation                                  Command

                     Map the 802.1p value of an incoming        8021p-inbound 8021p-value phb
                     VLAN packet to an internal priority        service-class [ color ]
                     and color the packet.

                     Map the internal priority or drop          8021p-outbound service-class color
                     priority to the 802.1p value of VLAN       map 8021p-value
                     packets in the outbound direction.

                     Map DSCP values of incoming IP             ip-dscp-inbound dscp-value phb
                     packets to internal priorities and color   service-class [ color ]
                     the packets.

                     Map the internal priority or drop          ip-dscp-outbound service-class color
                     priority to the DSCP value of IP           map dscp-value
                     packets in the outbound direction.




                    For details about the default priority mappings, see 8.4 Default Settings for
                    Priority Mapping.

                    ----End

8.5.3 Applying the DiffServ Domain
Context
                    You can bind a DiffServ domain to an inbound or outbound interface of packets so
                    that the device can implement mapping between external priorities and internal
                    priorities/colors according to the mappings defined in the DiffServ domain.

Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                            86
QoS Configuration
QoS Configuration                                                         8 Priority Mapping Configuration


Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Enter the interface view.
                    interface interface-type interface-number

         Step 3 Apply a DiffServ domain.
                    trust upstream { ds-domain-name | none }

                    By default, the domain default is applied to an interface.
                    If trust upstream none is configured on an interface, the device performs no
                    priority mapping on incoming packets.

                          NOTE

                        The trust upstream none command cannot be configured together with the qos phb
                        marking 8021p disable and qos phb marking dscp enable commands on the device.

         Step 4 (Optional) Enable the mapping between PHBs and DSCP values for outgoing
                packets.
                    qos phb marking dscp enable

         Step 5 (Optional) Disable the mapping between PHBs and 802.1p values for outgoing
                packets.
                    qos phb marking 8021p disable

                    ----End

8.5.4 Verifying the Configuration
Procedure
                    ●    Run the display diffserv domain [ brief | name ds-domain-name ] command
                         to check the DiffServ domain configuration.
                    ●    Run the display qos queue statistics { interface interface-name | interface
                         interface-type interface-number | slot slotid } command to check queue-
                         based traffic statistics.
                    ●    Run the display qos configuration interface [ { interface-type interface-
                         number | interface-name } ] command to check all QoS configurations on an
                         interface.
                    ----End

8.5.5 Example for Configuring Priority Mapping
Networking Requirements
                    In Figure 8-4, Host1 and Host2 are connected to DeviceB and DeviceC through
                    DeviceD, and then access the network through DeviceA.
                    The 802.1p values of packets sent from Host1 and Host2 are both 0. To ensure
                    normal running of low-latency services on Host1, the customer wants to configure
                    the CoS of Host1 to be higher than that of Host2, offering differentiated services
                    for Host1 and Host2.

Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                             87
QoS Configuration
QoS Configuration                                                                8 Priority Mapping Configuration


                    Figure 8-4 Network diagram of priority mapping
                          NOTE

                        In this example, interface 1, interface 2, interface 3, and interface 4 represent 10GE 1/0/1,
                        10GE 1/0/2, 10GE 1/0/3, and 10GE 1/0/4, respectively.




Procedure
         Step 1 Create VLANs and configure interfaces so that DeviceD can communicate with
                Host1, Host2, DeviceB, and DeviceC.

                    # Create VLAN 100 and VLAN 200.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceD
                    [DeviceD] vlan batch 100 200

                    # Set the link type of 10GE 1/0/3 and 10GE 1/0/4 on DeviceD to trunk, and add
                    10GE 1/0/1 to VLAN 100, 10GE 1/0/2 to VLAN 200, and 10GE 1/0/3 and 10GE
                    1/0/4 to both VLAN 100 and VLAN 200.
                    [DeviceD] interface 10ge 1/0/1
                    [DeviceD-10GE1/0/1] portswitch
                    [DeviceD-10GE1/0/1] port link-type access
                    [DeviceD-10GE1/0/1] port default vlan 100
                    [DeviceD-10GE1/0/1] quit
                    [DeviceD] interface 10ge 1/0/2
                    [DeviceD-10GE1/0/2] portswitch
                    [DeviceD-10GE1/0/2] port link-type access
                    [DeviceD-10GE1/0/2] port default vlan 200
                    [DeviceD-10GE1/0/2] quit
                    [DeviceD] interface 10ge 1/0/3
                    [DeviceD-10GE1/0/3] portswitch
                    [DeviceD-10GE1/0/3] port link-type trunk
                    [DeviceD-10GE1/0/3] port trunk allow-pass vlan 100 200
                    [DeviceD-10GE1/0/3] quit
                    [DeviceD] interface 10ge 1/0/4
                    [DeviceD-10GE1/0/4] portswitch
                    [DeviceD-10GE1/0/4] port link-type trunk
                    [DeviceD-10GE1/0/4] port trunk allow-pass vlan 100 200
                    [DeviceD-10GE1/0/4] quit

         Step 2 Configure interfaces to trust outer 802.1p values in packets.

Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                         88
QoS Configuration
QoS Configuration                                                          8 Priority Mapping Configuration


                    # Configure 10GE 1/0/1, 10GE 1/0/2, 10GE 1/0/3, and 10GE 1/0/4 to trust outer
                    802.1p values in packets. By default, a Layer 2 interface trusts outer 802.1p values.
                    In this case, skip this step.
                    [DeviceD] interface 10ge 1/0/1
                    [DeviceD-10GE1/0/1] trust 8021p outer
                    [DeviceD-10GE1/0/1] quit
                    [DeviceD] interface 10ge 1/0/2
                    [DeviceD-10GE1/0/2] trust 8021p outer
                    [DeviceD-10GE1/0/2] quit
                    [DeviceD] interface 10ge 1/0/3
                    [DeviceD-10GE1/0/3] trust 8021p outer
                    [DeviceD-10GE1/0/3] quit
                    [DeviceD] interface 10ge 1/0/4
                    [DeviceD-10GE1/0/4] trust 8021p outer
                    [DeviceD-10GE1/0/4] quit

         Step 3 Create DiffServ domains, configure priority mapping in the DiffServ domains, and
                bind the DiffServ domains to interfaces.
                    # Create DiffServ domains ds1 and ds2 on DeviceD and map the 802.1p values of
                    packets sent from Host1 and Host2 to different internal priorities.
                    [DeviceD] diffserv domain ds1
                    [DeviceD-dsdomain-ds1] 8021p-inbound 0 phb af4 green
                    [DeviceD-dsdomain-ds1] quit
                    [DeviceD] diffserv domain ds2
                    [DeviceD-dsdomain-ds2] 8021p-inbound 0 phb af2 green
                    [DeviceD-dsdomain-ds2] quit

                    # Bind DiffServ domains ds1 and ds2 to 10GE 1/0/1 and 10GE 1/0/2, respectively.
                    [DeviceD] interface 10ge 1/0/1
                    [DeviceD-10GE1/0/1] trust upstream ds1
                    [DeviceD-10GE1/0/1] quit
                    [DeviceD] interface 10ge 1/0/2
                    [DeviceD-10GE1/0/2] trust upstream ds2
                    [DeviceD-10GE1/0/2] quit

                    ----End

Verifying the Configuration
                    # Display queue statistics on the outbound interface. If there are packet statistics
                    in queue 2 corresponding to the internal priority AF2, and in queue 4
                    corresponding to the internal priority AF4, priority mapping is configured
                    successfully.

Configuration Scripts
                    DeviceD
                    #
                    sysname DeviceD
                    #
                    vlan batch 100 200
                    #
                    diffserv domain ds1
                     8021p-inbound 0 phb af4 green
                    #
                    diffserv domain ds2
                     8021p-inbound 0 phb af2 green
                    #
                    interface 10GE1/0/1
                     port link-type access


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                             89
QoS Configuration
QoS Configuration                                                                8 Priority Mapping Configuration

                     port default vlan 100
                     trust upstream ds1
                    #
                    interface 10GE1/0/2
                     port link-type access
                     port default vlan 200
                     trust upstream ds2
                    #
                    interface 10GE1/0/3
                     port link-type trunk
                     port trunk allow-pass vlan 100 200
                    #
                    interface 10GE1/0/4
                     port link-type trunk
                     port trunk allow-pass vlan 100 200
                    #
                    return



8.6 Configuring an Interface Priority
Context
                    The device provides differentiated services based on the interface priority in the
                    following scenarios:
                    ●     The trust upstream none command is configured on an interface.
                    ●     An interface receives untagged packets.
                                NOTE

                               ● The interface priority cannot be specified for Eth-Trunk member interfaces.
                               ● If no DiffServ domain is applied to an inbound interface and the interface priority
                                 is configured, the device adds a VLAN tag to received packets, uses the interface
                                 priority as the external priority, and sends packets to queues based on the interface
                                 priority.
                               ● When an Ethernet interface works in Layer 3 mode, the interface priority is 0 and
                                 cannot be configured.


Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Enter the interface view.
                    interface interface-type interface-number

         Step 3 Configure an interface priority.
                    port priority priority-value

                    ----End




Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                      90
QoS Configuration
QoS Configuration                                                           8 Priority Mapping Configuration




8.7 Configuring the Internal Priority of Protocol
Packets Sent by the Local Device
Context
                    A device processes packets based on their internal priorities. You can change the
                    internal priority of protocol packets sent by the local device to preferentially
                    process packets with a high internal priority on the local device.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Configure the internal priority of protocol packets sent by the local device.
                    set priority traffic-class traffic-class-value

                    By default, the internal priority is not configured for protocol packets sent by the
                    local device, and the local device sends all protocol packets based on their original
                    internal priorities.

                    ----End


8.8 Configuring the Mapping Between Internal
Priorities and Queues
Context
                    A device schedules packets based on queues. After external priorities are mapped
                    to internal priorities (PHBs), the device sends packets to queues based on the
                    mapping between internal priorities and queue indexes. By doing this, the device
                    provides differentiated services.
                    By default, internal priorities and port queues are mapped in one-to-one mode. In
                    practice, packets of two or more internal priorities are scheduled in the same
                    queue to conserve buffer resources on the device. The device sends packets to
                    different port queues based on the internal priority, and performs traffic shaping,
                    congestion avoidance, and queue scheduling for the queues.

                           NOTE

                         Only the S6730E-H-V2, S6730-H-V2, S6750E-S, S6750-S, S5755E-H, S5755-H, and S5732-H-
                         V2 series support this function.


Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Configure the mappings between internal priorities and queues.

Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                             91
QoS Configuration
QoS Configuration                                                              8 Priority Mapping Configuration

                    qos local-precedence-queue-map service-class queue-index

                    For the default mappings between internal priorities and queues, see 8.4 Default
                    Settings for Priority Mapping.

                    ----End


8.9 Configuring a Packet Priority
Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Configure the 802.1p value of packets.
                    set priority 8021p 8021p-value

                    By default, the 802.1p value is not configured for packets.
         Step 3 Configure the DSCP value of packets.
                    set priority dscp dscp-value

                    By default, the DSCP value is not configured for packets.

                    ----End


8.10 Troubleshooting Priority Mapping

8.10.1 Packets Enter Incorrect Queues
Fault Symptoms
                    Packets enter incorrect queues.

Possible Causes
                    Possible causes are as follows:
                    ●    Priority mappings configured in the DiffServ domain bound to the inbound
                         interface are incorrect.
                    ●    There are configurations affecting packet queuing on the inbound interface.
                    ●    There are configurations affecting packet queuing in the VLAN to which the
                         packets belong.
                    ●    There are configurations affecting packet queuing in the system.

Procedure
         Step 1 Check whether priority mappings are correct.
                    Run the display this command in the inbound interface view to check the
                    configuration of the trust upstream command. (If the trust upstream command
                    is not configured, the DiffServ domain default is applied to an interface.) Then

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                               92
QoS Configuration
QoS Configuration                                                        8 Priority Mapping Configuration


                    run the display diffserv domain [ name ds-domain-name | brief ] command to
                    check whether the priority mappings configured in the trusted DiffServ domain
                    are correct.
                    ●   If the priority mappings are incorrect, run the ip-dscp-inbound or 8021p-
                        inbound command to correctly configure priority mappings.
                    ●   If the priority mappings are correct, go to step 2.
         Step 2 Check whether any configurations are affecting packet queuing on the inbound
                interface.
                    The following configurations affect the queues that packets enter on the inbound
                    interface:
                    ●   If the packets match a traffic policy that is applied to the inbound direction
                        (using the traffic-policy command) and contains the action of remark local-
                        precedence, the device sends the packets to queues based on the re-marked
                        internal priorities.
                    ●   If the trust upstream none command is configured, the device does not
                        perform priority mapping for incoming packets on the interface. Instead, the
                        device places packets into queues based on interface priorities.
                    Check whether any of the preceding configurations exist on the inbound interface.
                    ●   If such configurations are found, delete or modify them.
                    ●   If none of the configurations is found, go to step 3.
         Step 3 Check whether any configurations are affecting packet queuing in the VLAN to
                which the packets belong.
                    The following configurations affect packet queuing in a VLAN:
                    ●   If the packets match a traffic policy that is applied to the inbound direction
                        (using the traffic-policy command) and contains the action of remark local-
                        precedence, the device sends the packets to queues based on the re-marked
                        internal priorities.
                    ●   If the packets match a traffic policy that is applied to the inbound direction
                        (using the traffic-policy command) and contains the action of remark
                        8021p, the device maps the re-marked priorities of packets to internal
                        priorities and sends the packets to queues based on the mapped internal
                        priorities.
                    Check whether any of the preceding configurations exist in the VLAN.
                    ●   If such configurations are found, delete or modify them.
                    ●   If none of the configurations is found, go to step 4.
         Step 4 Check whether any configurations are affecting packet queuing in the system.
                    The following configurations affect packet queuing in the system:
                    ●   If the qos local-precedence-queue-map command is configured, the device
                        sends packets to queues based on the mapping between internal priorities
                        and queues specified by this command.
                    ●   If the packets match a global policy that is applied to the inbound direction
                        (using the traffic-policy global command) and contains the action of remark
                        local-precedence, the device sends packets to queues based on the re-
                        marked internal priorities.

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                93
QoS Configuration
QoS Configuration                                                          8 Priority Mapping Configuration


                    ●   If the packets match a global policy that is applied to the inbound direction
                        (using the traffic-policy global command) and contains the action of remark
                        8021p, the device maps the re-marked priorities of packets to internal
                        priorities and sends the packets to queues based on the mapped internal
                        priorities.
                    Run the display current-configuration command to check for the preceding
                    configurations in the system. If such configurations are found, delete or modify
                    them.

                    ----End

8.10.2 Priority Mapping Results Are Incorrect
Fault Symptoms
                    Priority mapping results are incorrect.

Possible Causes
                    Possible causes are as follows:
                    ●   On the outbound interface, packets do not enter queues mapped to external
                        priorities.
                    ●   The priority types trusted by the inbound and outbound interfaces are
                        incorrect.
                    ●   The priority mappings configured in the DiffServ domains bound to the
                        inbound and outbound interfaces are incorrect.
                    ●   There are configurations affecting priority mapping on the inbound and
                        outbound interfaces.

Procedure
         Step 1 Check whether packets enter correct queues on the outbound interface.
                    Run the display qos queue statistics interface interface-type interface-number
                    command to check whether packets enter correct queues on the outbound
                    interface.
                    ●   If packets enter incorrect queues, locate the fault. For details, see 8.10.1
                        Packets Enter Incorrect Queues.
                    ●   If packets enter correct queues, go to step 2.
         Step 2 Check whether the priority types trusted by the inbound and outbound interface
                are correct.
                    Run the display this command in the inbound or outbound interface view to
                    check whether the priority type trusted using the trust command is correct.
                    ●   If the trusted priority type is incorrect, run the trust command to specify the
                        correct priority type.
                    ●   If the trusted priority type is correct, go to step 3.
         Step 3 Check whether the priority mappings in the DiffServ domain bound to the
                inbound or outbound interface are correct.

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                               94
QoS Configuration
QoS Configuration                                                        8 Priority Mapping Configuration


                    Run the display this command in the inbound or outbound interface view to
                    check whether the trust upstream command is configured.
                    Run the display diffserv domain [ name ds-domain-name | brief ] command to
                    check whether the mappings between internal priorities/colors and external
                    priorities are correct.
                    ●   If the priority mappings are incorrect, run the ip-dscp-outbound, or 8021p-
                        outbound command to configure the mappings between internal priorities/
                        colors and external priorities.
                    ●   If the priority mappings are correct, go to step 4.
         Step 4 Check whether there are configurations affecting priority mapping on the inbound
                and outbound interfaces.
                    On an interface:
                    ●   If the qos phb marking dscp enable command is not configured, the system
                        does not perform mapping between PHBs and DSCP values for outgoing
                        packets on the interface.
                    ●   If the qos phb marking 8021p disable command is configured, the system
                        does not perform mapping between PHBs and 802.1p values for outgoing
                        packets on the interface.
                    ●   If the trust upstream none command is configured, the system does not
                        perform priority mapping for outgoing packets on the interface.
                    ●   If packets match a traffic policy that is applied to the inbound or outbound
                        direction (using the traffic-policy command) and contains the action of
                        remark 8021p or remark dscp, the packet priority is the re-marked external
                        priority.
                    Run the display this command in the inbound or outbound interface view to
                    check whether there are any configurations affecting priority mapping. If such
                    configurations are found, delete or modify them.

                    ----End




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                             95
QoS Configuration                                          9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                          based Rate Limiting Configuration




          9
          Traffic Policing, Traffic Shaping, and
  Interface-based Rate Limiting Configuration

                    9.1 Overview of Traffic Policing, Traffic Shaping, and Interface-based Rate Limiting
                    9.2 Understanding Traffic Policing, Traffic Shaping, and Interface-based Rate
                    Limiting
                    9.3 Configuration Precautions for Traffic Policing, Traffic Shaping, and Interface-
                    based Rate Limiting
                    9.4 Default Settings for Traffic Policing, Traffic Shaping, and Interface-based Rate
                    Limiting
                    9.5 Configuring Traffic Policing
                    9.6 Configuring Traffic Shaping
                    9.7 Configuring Interface-based Rate Limiting
                    9.8 Configuring a QoS Profile
                    9.9 Maintaining Traffic Policing, Traffic Shaping, and Interface-based Rate Limiting


9.1 Overview of Traffic Policing, Traffic Shaping, and
Interface-based Rate Limiting
Definition
                    Traffic policing, traffic shaping, and interface-based rate limiting monitor and
                    control traffic rates and resource usage.

                    ●   Traffic policing: monitors the rate of traffic entering a network and discards
                        excess traffic to ensure the incoming traffic rate remains within a specified
                        range, conserving network resources.
                    ●   Traffic shaping: adjusts the rate at which traffic is sent to reduce traffic bursts,
                        thereby ensuring a stable transmission rate and preventing congestion on the
                        downstream device.

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                  96
QoS Configuration                                         9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                         based Rate Limiting Configuration


                    ●   Interface-based rate limiting: controls the rate at which packets are sent or
                        received on an interface. This mechanism is useful for limiting the rate of all
                        traffic on an interface, regardless of packet types.

Purpose
                    Network congestion may occur when the transmit rate on an upstream device is
                    higher than the receive rate on a downstream device or when the interface rate on
                    a downstream device is lower than the interface rate on an upstream device. If
                    users are allowed to send traffic at an unlimited rate, continuous traffic bursts
                    from many users may result in a congested network. Therefore, user traffic must
                    be rate-limited to ensure services remain stable even in scenarios where network
                    resources are limited. As such, traffic policing, traffic shaping, and interface-based
                    rate limiting can be used to control the traffic rate to improve network resource
                    utilization and service stability.


9.2 Understanding Traffic Policing, Traffic Shaping, and
Interface-based Rate Limiting

9.2.1 Traffic Metering and Token Bucket Mechanism
Overview
                    Traffic metering is the prerequisite for implementing traffic policing, traffic
                    shaping, and interface-based rate limiting. In traffic metering, network devices
                    determine whether the rate of incoming traffic exceeds a defined limit and take
                    appropriate actions. Generally, the token bucket mechanism is used to measure
                    traffic.
                    A token bucket is a container that stores a specific number of tokens. The system
                    places tokens into a token bucket at the configured rate. If the token bucket is full,
                    excess tokens overflow. The system determines whether the bucket has enough
                    tokens for packet forwarding. Otherwise, the traffic rate exceeds or violates the
                    rate limit.
                    RFC standards define the following token bucket algorithms:
                    ●   The single rate three color marker (srTCM) algorithm determines traffic bursts
                        based on the length of packets.
                    ●   The two rate three color marker (trTCM) algorithm determines traffic bursts
                        based on the rate of packets.
                    The token bucket algorithms mark packets red, yellow, or green based on the
                    result of traffic metering. The system then processes the packets according to their
                    marked color. The two aforementioned algorithms can work in color-aware and
                    color-blind modes. The color-blind mode is used as an example in the following
                    section.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                 97
QoS Configuration                                          9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                          based Rate Limiting Configuration


Single-Rate-Two-Bucket Mechanism
                    The single-rate-two-bucket mechanism uses the srTCM algorithm defined in RFC
                    2697 to measure traffic and marks packets green, yellow, or red based on the
                    metering result.
                    As shown in Figure 9-1, bucket C (also called CIR token bucket) and bucket E
                    (also called EIR token bucket) contain Tc and Te tokens, respectively. The single-
                    rate-two-bucket mechanism uses three parameters:
                    ●   Committed information rate (CIR): indicates the rate at which tokens are
                        placed into bucket C, that is, the average traffic rate that bucket C allows.
                    ●   Committed burst size (CBS): indicates the capacity of bucket C, that is, the
                        maximum volume of burst traffic that bucket C allows.
                    ●   Excess burst size (EBS): indicates the capacity of bucket E, that is, the
                        maximum volume of excess burst traffic that bucket E allows.
                    The system places tokens into bucket C at the CIR:
                    ●   If Tc is less than the CBS, Tc increases.
                    ●   If Tc is equal to the CBS but Te is less than the EBS, Te increases.
                    ●   If Tc is equal to the CBS and Te is equal to the EBS, Tc and Te do not increase.
                    B indicates the size of an arriving packet:
                    ●   If B is less than or equal to Tc, the packet is marked green, and Tc decreases
                        by B.
                    ●   If B is greater than Tc and less than or equal to Te, the packet is marked
                        yellow and Te decreases by B.
                    ●   If B is greater than Te, the packet is marked red, and Tc and Te remain
                        unchanged.
                    The single-rate-two-bucket mechanism allows burst traffic. When the traffic rate is
                    lower than the CIR, packets are marked green. When the burst traffic volume is
                    greater than the CBS but lower than the EBS, packets are marked yellow. When
                    the volume is greater than the EBS, packets are marked red.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                  98
QoS Configuration                                               9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                               based Rate Limiting Configuration


                    Figure 9-1 Single-rate-two-bucket mechanism




                    The preceding example uses the CIR of 1 Mbit/s and the CBS and EBS of 2000
                    bytes each. Buckets C and E are initially full of tokens. In single-rate-two-bucket
                    mode, the token buckets process packets as follows:

                          NOTE

                    Here, 1 Mbit/s is equal to 1 x 106 bit/s.
                    ●    Assuming that the first packet arriving at the interface is 1500 bytes long, the
                         packet is marked green because the number of tokens in bucket C is greater
                         than the packet length. The number of tokens in bucket C then decreases by
                         1500 bytes, with 500 bytes remaining. The number of tokens in bucket E
                         remains unchanged.
                    ●    Assuming that the second packet arriving at the interface after a delay of 1
                         ms is 1500 bytes long, additional 125-byte tokens are placed into bucket C
                         (CIR x time period = 1 Mbit/s x 1 ms = 1000 bits = 125 bytes). Bucket C now
                         has 625-byte tokens, which are insufficient for the 1500-byte second packet.
                         Bucket E has 2000-byte tokens, which are sufficient for the second packet.
                         Therefore, the second packet is marked yellow. Subsequently, the number of
                         tokens in bucket E decreases by 1500 bytes, with 500 bytes remaining. The
                         number of tokens in bucket C remains unchanged.
                    ●    Assuming that the third packet arriving at the interface after a delay of 1 ms
                         is 1000 bytes long, as with the second packet, additional 125-byte tokens are
                         placed into bucket C. Bucket C now has 750-byte tokens, which are
                         insufficient for the 1000-byte third packet. The tokens in bucket E are also


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                    99
QoS Configuration                                            9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                            based Rate Limiting Configuration


                         insufficient; therefore, the third packet is marked red. The numbers of tokens
                         in buckets C and E remain unchanged.
                    ●    Assuming that the fourth packet arriving at the interface after a delay of 20
                         ms is 1500 bytes long, as with the preceding packets, additional 2500-byte
                         tokens are placed into bucket C (CIR x time period = 1 Mbit/s x 20 ms =
                         20000 bits = 2500 bytes). Bucket C now has 3250-byte tokens. The 1250-byte
                         tokens exceeding the CBS (2000 bytes) are placed into bucket E. Now, bucket
                         E has 1750-byte tokens. The packet is marked green because the number of
                         tokens in bucket C is greater than the packet length. The number of tokens in
                         bucket C decreases by 1500 bytes, with 500 bytes remaining. The number of
                         tokens in bucket E remains unchanged.
                    Table 9-1 describes packet processing.

                    Table 9-1 Packet processing in single-rate-two-bucket mode
                                                                  Number of          Number of
                                        Pack             Toke     Tokens             Tokens After
                                        et               n        Before             Packet
                     Pack                                         Packet             Processing
                              Time      Lengt   Delay    Addit                                         Mark
                     et                                           Processing         (Bytes)
                              (ms)      h       (ms)     ion                                           ing
                     No.                                          (Bytes)
                                        (Byte            (Byte
                                        s)               s)       Buck      Buck     Buck     Buck
                                                                  et C      et E     et C     et E

                     -        -         -       -        -        2000      2000     2000     2000     -

                     1        0         1500    0        0        2000      2000     500      2000     Green

                     2        1         1500    1        125      625       2000     625      500      Yello
                                                                                                       w

                     3        2         1000    1        125      750       500      750      500      Red

                     4        22        1500    20       2500     2000      1750     500      1750     Green




Single-Rate-Single-Bucket Mechanism
                    If burst traffic is not allowed, the EBS must be set to 0 in single-rate-two-bucket
                    mode. In this case, only one token bucket is used because there are always 0
                    tokens in bucket E.
                    As shown in Figure 9-2, bucket C contains Tc tokens. The single-rate-single-bucket
                    mechanism uses two parameters:
                    ●    CIR: indicates the rate at which tokens are placed into bucket C, that is, the
                         average traffic rate that bucket C allows.
                    ●    CBS: indicates the capacity of bucket C, that is, the maximum volume of burst
                         traffic that bucket C allows.
                    The system places tokens into the bucket at the CIR. If Tc is less than the CBS, Tc
                    increases. If Tc is greater than or equal to the CBS, Tc remains unchanged.
                    B indicates the size of an arriving packet:

Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                100
QoS Configuration                                               9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                               based Rate Limiting Configuration


                    ●    If B is less than or equal to Tc, the packet is marked green, and Tc decreases
                         by B.
                    ●    If B is greater than Tc, the packet is marked red, and Tc remains unchanged.
                    The single-rate-single-bucket mechanism does not allow burst traffic. When the
                    traffic rate is lower than the CIR, packets are marked green. When the rate is
                    higher than the CIR, packets are marked red.

                    Figure 9-2 Single-rate-single-bucket mechanism




                    This example uses the CIR of 1 Mbit/s and the CBS of 2000 bytes. Bucket C is
                    initially full of tokens. In single-rate-single-bucket mode, the token bucket
                    processes packets as follows:

                          NOTE

                    Here, 1 Mbit/s is equal to 1 x 106 bit/s.
                    ●    Assuming that the first packet arriving at the interface is 1500 bytes long, the
                         packet is marked green because the number of tokens in bucket C is greater
                         than the packet length. The number of tokens in bucket C then decreases by
                         1500 bytes, with 500 bytes remaining.
                    ●    Assuming that the second packet arriving at the interface after a delay of 1
                         ms is 1500 bytes long, additional 125-byte tokens are placed into bucket C
                         (CIR x time period = 1 Mbit/s x 1 ms = 1000 bits = 125 bytes). Bucket C now
                         has 625-byte tokens. Tokens in bucket C are insufficient, so the second packet
                         is marked red.
                    ●    Assuming that the third packet arriving at the interface after a delay of 1 ms
                         is 1000 bytes long, additional 125-byte tokens are placed into bucket C.

Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                   101
QoS Configuration                                          9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                          based Rate Limiting Configuration


                         Bucket C now has 750-byte tokens. Tokens in bucket C are insufficient, so the
                         third packet is marked red.
                    ●    Assuming that the fourth packet arriving at the interface after a delay of 20
                         ms is 1500 bytes long, additional 2500-byte tokens are placed into bucket C
                         (CIR x time period = 1 Mbit/s x 20 ms = 20000 bits = 2500 bytes). Bucket C
                         now has 3250-byte tokens. However, bucket C can have a maximum of 2000-
                         byte tokens. Therefore, the CBS is 2000 bytes. The packet is marked green
                         because the number of tokens in bucket C is greater than the packet length.
                         The number of tokens in bucket C decreases by 1500 bytes, with 500 bytes
                         remaining.

                    Table 9-2 describes packet processing.

                    Table 9-2 Packet processing in single-rate-single-bucket mode

                     Packet     Time       Packet    Delay      Token        Numbe      Numbe      Markin
                     No.        (ms)       Length    (ms)       Additio      r of       r of       g
                                           (Bytes)              n            Tokens     Tokens
                                                                (Bytes)      Before     After
                                                                             Packet     Packet
                                                                             Process    Process
                                                                             ing        ing
                                                                             (Bytes)    (Bytes)

                     -          -          -         -          -            2000       2000       -

                     1          0          1500      0          0            2000       500        Green

                     2          1          1500      1          125          625        625        Red

                     3          2          1000      1          125          750        750        Red

                     4          22         1500      20         2500         2000       500        Green




Two-Rate-Two-Bucket Mechanism
                    The two-rate-two-bucket mechanism uses the trTCM algorithm defined in RFC
                    2698 to measure traffic and marks packets green, yellow, or red based on the
                    metering result.

                    As shown in Figure 9-3, buckets P and C contain Tp and Tc tokens respectively.
                    The two-rate-two-bucket mechanism uses four parameters:
                    ●    Peak information rate (PIR): indicates the rate at which tokens are placed into
                         bucket P, that is, the maximum traffic rate that bucket P allows. The PIR is
                         greater than the CIR.
                    ●    CIR: indicates the rate at which tokens are placed into bucket C, that is, the
                         average traffic rate that bucket C allows.
                    ●    Peak burst size (PBS): indicates the capacity of bucket P, that is, the maximum
                         volume of burst traffic that bucket P allows.
                    ●    CBS: indicates the capacity of bucket C, that is, the maximum volume of burst
                         traffic that bucket C allows.

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                102
QoS Configuration                                               9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                               based Rate Limiting Configuration


                    The system places tokens into bucket P at the PIR and places tokens into bucket C
                    at the CIR:
                    ●    If Tp is less than the PBS, Tp increases. If Tp is greater than or equal to the
                         PBS, Tp remains unchanged.
                    ●    If Tc is less than the CBS, Tc increases. If Tc is greater than or equal to the
                         CBS, Tp remains unchanged.
                    B indicates the size of an arriving packet:
                    ●    If B is greater than Tp, the packet is marked red.
                    ●    If B is greater than Tc and less than or equal to Tp, the packet is marked
                         yellow and Tp decreases by B.
                    ●    If B is less than or equal to Tc, the packet is marked green, and Tp and Tc
                         decrease by B.
                    The two-rate-two-bucket mechanism enables burst traffic rates. When the traffic
                    rate is lower than the CIR, packets are marked green. When the rate is higher than
                    the CIR but less than the PIR, packets are marked yellow. When the rate is higher
                    than the PIR, packets are marked red.

                    Figure 9-3 Two-rate-two-bucket mechanism




                    This example uses the CIR of 1 Mbit/s, the PIR of 2 Mbit/s, the CBS of 2000 bytes,
                    and the PBS of 3000 bytes. Buckets C and P are initially full of tokens. In two-rate-
                    two-bucket mode, the token buckets process packets as follows:

                          NOTE

                    Here, 1 Mbit/s is equal to 1 x 106 bit/s.


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                   103
QoS Configuration                                          9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                          based Rate Limiting Configuration


                    ●    If the first packet arriving at the interface is 1500 bytes long, the packet is
                         marked green because the numbers of tokens in both buckets P and C are
                         greater than the packet length. The numbers of tokens in both buckets P and
                         C then decrease by 1500 bytes, with 500 and 1500 bytes remaining in bucket
                         C and bucket P, respectively.
                    ●    Assume that the second packet arriving at the interface after a delay of 1 ms
                         is 1800 bytes long. Additional 250-byte tokens are placed into bucket P (PIR x
                         time period = 2 Mbit/s x 1 ms = 2000 bits = 250 bytes). Bucket P now has
                         1750-byte tokens, and is smaller than the packet length. Additional 125-byte
                         tokens are placed into bucket C (CIR x time period = 1 Mbit/s x 1 ms = 1000
                         bits = 125 bytes). Bucket C now has 625-byte tokens. Therefore, the second
                         packet is marked red, and the numbers of tokens in buckets P and C remain
                         unchanged.
                    ●    Assume that the third packet arriving at the interface after a delay of 1 ms is
                         1000 bytes long. Additional 250-byte tokens are placed into bucket P. Bucket
                         P now has 2000-byte tokens, and is larger than the packet length. Additional
                         125-byte tokens are placed into bucket C. Bucket C now has 750-byte tokens,
                         and is still smaller than the packet length. The packet is marked yellow. The
                         number of tokens in bucket P decreases by 1000 bytes, with 1000 bytes
                         remaining. The number of tokens in bucket C remains unchanged.
                    ●    Assume that the fourth packet arriving at the interface after a delay of 20 ms
                         is 1500 bytes long. Additional 5000-byte tokens are placed into bucket P (PIR
                         x time period = 2 Mbit/s x 20 ms = 40000 bits = 5000 bytes), but tokens that
                         exceed the PBS (3000 bytes) are dropped. Bucket P has 3000-byte tokens,
                         which are sufficient for the 1500-byte fourth packet. Additional 2500-byte
                         tokens are placed into bucket C (CIR x time period = 1 Mbit/s x 20 ms =
                         20000 bits = 2500 bytes), but tokens that exceed the CBS (2000 bytes) are
                         dropped. Bucket C then has 2000-byte tokens, which are sufficient for the
                         1500-byte fourth packet. Therefore, the fourth packet is marked green. The
                         number of tokens in bucket P decreases by 1500 bytes, with 1500 bytes
                         remaining. The number of tokens in bucket C decreases by 1500 bytes, with
                         500 bytes remaining.

                    Table 9-3 describes packet processing.

                    Table 9-3 Packet processing in two-rate-two-bucket mode

                                                                     Number of       Number of
                                     Pack                            Tokens          Tokens
                                                    Token
                                     et                              Before          After
                     Pack                   Dela    Addition
                             Time    Leng                            Packet          Packet           Mar
                     et                     y       (Bytes)
                             (ms)    th                              Processing      Processing       king
                     No.                    (ms)                     (Bytes)         (Bytes)
                                     (Byt
                                     es)            Buck     Buck    Buck    Buck    Buck     Buck
                                                    et C     et P    et C    et P    et C     et P

                     -       -       -      -       -        -       2000    3000    2000     3000    -

                     1       0       1500   0       0        0       2000    3000    500      1500    Gree
                                                                                                      n

                     2       1       1800   1       125      250     625     1750    625      1750    Red


Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                104
QoS Configuration                                            9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                            based Rate Limiting Configuration


                                                                       Number of          Number of
                                     Pack                              Tokens             Tokens
                                                      Token
                                     et                                Before             After
                     Pack                     Dela    Addition
                             Time    Leng                              Packet             Packet        Mar
                     et                       y       (Bytes)
                             (ms)    th                                Processing         Processing    king
                     No.                      (ms)                     (Bytes)            (Bytes)
                                     (Byt
                                     es)              Buck    Buck     Buck    Buck       Buck   Buck
                                                      et C    et P     et C    et P       et C   et P

                     3       2       1000     1       125     250      750     2000       750    1000   Yello
                                                                                                        w

                     4       22      1500     20      2500    5000     2000    3000       500    1500   Gree
                                                                                                        n




Difference and Application of Three Token Bucket Modes
                    Table 9-4 describes the difference and relationship between the three token
                    bucket modes.

                    Table 9-4 Difference and relationship of three token bucket modes

                     Difference             Single-Rate-            Single-Rate-Two-       Two-Rate-Two-
                                            Single-Bucket           Bucket                 Bucket

                     Parameters             CIR and CBS             CIR, CBS, and EBS      CIR, CBS, PIR, and
                                                                                           PBS

                     Mode in which          Tokens are placed       When bucket C is       Tokens are placed
                     tokens are placed      into bucket C at        full, excess tokens    into bucket C at
                                            the CIR. Excess         are placed into        the CIR and into
                                            tokens are              bucket E. When         bucket P at the
                                            dropped when            buckets C and E        PIR. Buckets C
                                            bucket C is full.       are not full,          and P are
                                                                    tokens are placed      independent.
                                                                    only into bucket       Excess tokens are
                                                                    C.                     dropped when
                                                                                           buckets C and P
                                                                                           are full.

                     Burst traffic          Traffic burst is not    Traffic burst is       Traffic burst is
                                            allowed. Packet         allowed. Tokens in     allowed. When
                                            processing is           bucket C are used      buckets C and P
                                            implemented only        first. When tokens     have sufficient
                                            when bucket C           in bucket C are        tokens, tokens in
                                            has a sufficient        insufficient,          both buckets are
                                            number of tokens.       tokens in bucket E     used. When
                                                                    are used.              tokens in bucket C
                                                                                           are insufficient,
                                                                                           tokens in only
                                                                                           bucket P are used.


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                 105
QoS Configuration                                          9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                          based Rate Limiting Configuration


                     Difference            Single-Rate-         Single-Rate-Two-        Two-Rate-Two-
                                           Single-Bucket        Bucket                  Bucket

                     Marking result        Green or red         Green, yellow, or       Green, yellow, or
                                                                red                     red

                     Relationship          In single-rate-two-bucket mode, if the EBS is 0, the effect is
                                           the same as that in single-rate-single-bucket mode.
                                           In two-rate-two-bucket mode, if the PIR is equal to the CIR,
                                           the effect is the same as that in single-rate-two-bucket
                                           mode.




                    Table 9-5 describes the functions and scenarios of the three token bucket modes.

                    Table 9-5 Functions and application scenarios of the three token bucket modes
                     Token Bucket Mode            Function                      Application Scenario

                     Single-rate-single-bucket    Limits bandwidth.             Discards low-priority
                                                                                services, such as extranet
                                                                                HTTP traffic and excess
                                                                                traffic.

                     Single-rate-two-bucket       Limits bandwidth, allows      Reserves bandwidth for
                                                  certain traffic bursts, and   important services or
                                                  distinguishes burst and       burst traffic (for
                                                  normal services.              example, email data).

                     Two-rate-two-bucket          Limits bandwidth,             Recommended for
                                                  allocates bandwidth, and      important services
                                                  determines whether the        because it better
                                                  bandwidth is less than        monitors burst traffic
                                                  the CIR or is in the range    and guides traffic
                                                  of the CIR and PIR.           analysis.




Color-Aware Mode
                    In color-aware mode, the color of an arriving packet affects the metering results
                    of the token bucket mechanism in the following ways:
                    ●   If the packet has been marked green, the metering mechanism is the same as
                        that in color-blind mode.
                    ●   If the packet has been marked yellow, the system marks the packet yellow if
                        it conforms to the limit or marks the packet red if it violates the limit. This
                        depends on the packet length and the number of tokens. In single-rate-single-
                        bucket mode, the packet is marked red directly.
                    ●   If the packet has been marked red, it is also marked red in the token bucket.




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                               106
QoS Configuration                                         9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                         based Rate Limiting Configuration


9.2.2 Traffic Policing
                    Traffic policing controls the rate of traffic entering a network within a specified
                    range by metering traffic and taking punitive actions on excess traffic to conserve
                    network resources.

                    Figure 9-4 Traffic policing components




                    As shown in Figure 9-4, traffic policing involves the following components:
                    ●   Meter: uses the token bucket mechanism to measure network traffic and
                        sends the result to the marker.
                    ●   Marker: colors packets green, yellow, or red based on the metering result
                        received from the meter.
                    ●   Action: performs actions based on the color of the packet. The actions are
                        defined as follows:
                        –    Pass: forwards the packets that conform to the limit.
                        –    Re-mark + pass: changes the local priorities of packets that exceed the
                             limit and forwards the packets.
                        –    Discard: drops packets that exceed the limit.
                    If the rate of a packet stream exceeds the limit, the system lowers the priority of
                    the excessive packets in the stream before forwarding them or discards the
                    packets. By default, the system forwards green and yellow packets, and discards
                    red packets.

9.2.3 Traffic Shaping
                    Traffic shaping adjusts the rate of outgoing traffic to reduce traffic bursts so that
                    outgoing packets can be transmitted at a stable rate. Traffic shaping uses a buffer
                    and token buckets to control the traffic rate. When packets are sent at a high rate,
                    the system buffers packets and then sends them evenly under the control of the
                    token buckets.
                    Figure 9-5 shows an example of traffic shaping, using flow-based queue shaping
                    in single-rate-single-bucket mode.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                                107
QoS Configuration                                          9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                          based Rate Limiting Configuration


                    Figure 9-5 Traffic shaping




                    The process of traffic shaping is as follows:

                    1.   When packets arrive, the system classifies packets and places them into
                         different queues.
                    2.   If a queue is not configured with traffic shaping, packets placed in this queue
                         are immediately sent. For the queues configured with traffic shaping, the
                         system proceeds to the next step.
                    3.   The system places tokens in the bucket at the specified rate (CIR):
                         –   If there are sufficient tokens in the bucket, the system sends the packets
                             and decreases the number of tokens accordingly.
                         –   If tokens in the bucket are insufficient for packet forwarding, the system
                             places the packets into the buffer queue. If the buffer queue is full, the
                             system discards the packets.
                    4.   When there are packets in the buffer queue, the system compares the number
                         of packets with the number of tokens in the token bucket. If there are
                         sufficient tokens, the system forwards packets until all the packets in the
                         buffer queue are sent.

9.2.4 Interface-based Rate Limiting
                    Interface-based rate limiting controls the total rate of all packets sent or received
                    on an interface.

                    It uses the token bucket mechanism to control traffic rates. If rate limiting is
                    configured on an interface, all packets passing through this interface must be
                    processed by the token bucket. If there are sufficient tokens in the token bucket
                    for packet forwarding, packets are sent out from the interface. If the number is
                    insufficient, packets are discarded or buffered.

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                108
QoS Configuration                                          9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                          based Rate Limiting Configuration


                    Interface-based rate limiting can be configured in the inbound or outbound
                    direction. Figure 9-6 illustrates interface-based rate limiting in the outbound
                    direction using the single-rate-single-bucket mechanism.

                    Figure 9-6 Interface-based rate limiting




                    The process of interface-based rate limiting is as follows:

                    1.   If there are sufficient tokens in the bucket, the system sends the packets and
                         decreases the number of tokens accordingly.
                    2.   If tokens in the bucket are insufficient for packet forwarding, the system
                         places the packets into the buffer queue. If the buffer queue is full, the
                         system discards the packets.
                    3.   When there are packets in the buffer queue, the system compares the number
                         of packets with the number of tokens in the token bucket. If there are
                         sufficient tokens, the system forwards packets until all the packets in the
                         buffer queue are sent.


9.3 Configuration Precautions for Traffic Policing,
Traffic Shaping, and Interface-based Rate Limiting

9.4 Default Settings for Traffic Policing, Traffic Shaping,
and Interface-based Rate Limiting
                    Table 9-6 describes the default settings for traffic policing, traffic shaping, and
                    interface-based rate limiting.

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                109
QoS Configuration                                          9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                          based Rate Limiting Configuration


                    Table 9-6 Default settings for traffic policing, traffic shaping, and interface-based
                    rate limiting

                     Parameter                                  Default Setting

                     MQC-based traffic policing (level-1        Disabled
                     CAR)

                     Logical operator between rules in a        OR
                     traffic classifier

                     Queue-based traffic shaping                Disabled

                     Queue-based traffic shaping rate           Maximum interface bandwidth

                     Interface-based rate limiting through      Disabled
                     traffic policing

                     Interface-based rate limiting through      Disabled
                     traffic shaping

                     Rate of interface-based rate limiting      Maximum interface bandwidth
                     through traffic shaping

                     Rate limit on the management               3000 pps
                     interface




9.5 Configuring Traffic Policing

9.5.1 Configuring MQC-based Traffic Policing (Level-1 CAR)
Context
                    There are various services on a network. When a large amount of service traffic
                    enters the network side, congestion may occur due to insufficient bandwidth. To
                    control a specific type of service traffic in the inbound or outbound direction on an
                    interface, MQC-based traffic policing (level-1 CAR) can be configured. MQC-based
                    traffic policing uses traffic classifiers to implement differentiated services. When
                    the receive or transmit rate of packets matching traffic classification rules exceeds
                    the specified limit, the device discards the packets.
                    When both QoS CAR and MQC are used on an interface for traffic statistics
                    collection, statistics about packets to which a QoS CAR profile is applied contain
                    only the packets that do not match the traffic policy.
                    The device performs the CAR action on interfaces of the same chip centrally, and
                    on interfaces of different chips separately. If a traffic policy containing traffic
                    policing is applied to an Eth-Trunk, a VLAN, or the system, and interfaces of the
                    object to which the traffic policy is applied belong to N chips, the actual rate limit
                    is N times as large as the CAR value.
                    For example, assume that interface 1 and interface 2 belong to chip 0 and
                    interface 3 and interface 4 belong to chip 1. If interfaces 1 to 4 join VLAN 10 and

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                110
QoS Configuration                                                    9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                    based Rate Limiting Configuration


                    a traffic policy containing the rate limit of 1 Mbit/s is applied to VLAN 10, the rate
                    limit of interface 1 and interface 2 is 1 Mbit/s and the rate limit of interface 3 and
                    interface 4 is 1 Mbit/s. That is, the total rate limit is 2 Mbit/s after the traffic policy
                    is applied to VLAN 10.

Procedure
         Step 1 Configure a traffic classifier.

                    For details about how to configure a traffic classifier, see 3.4 Configuring a Traffic
                    Classifier in "MQC Configuration".

         Step 2 Configure a traffic behavior.
                    1.   (Optional) Disable the device from counting the inter-frame gaps and
                         preambles when the device calculates the traffic policing rate.
                         qos car ifg disable

                    2.   Create a traffic behavior and enter the traffic behavior view, or enter the view
                         of an existing traffic behavior.
                         traffic behavior behavior-name

                    3.   Configure a CAR action.
                         car cir cir-value [ kbps | mbps | gbps ] [ pir pir-value [ kbps | mbps | gbps ] ] [ cbs cbs-value [ bytes
                         | kbytes | mbytes ] pbs pbs-value [ bytes | kbytes | mbytes ] ] [ share ] [ mode { color-blind | color-
                         aware } ] [ green pass [ service-class class color color ] | yellow { discard | pass [ service-class
                         class color color ] } | red { discard | pass [ service-class class color color ] } ] *
                    4.   Exit the traffic behavior view.
                         quit

         Step 3 Configure a traffic policy.
                    1.   Create a traffic policy and enter the traffic policy view, or enter the view of an
                         existing traffic policy.
                         traffic policy policy-name

                    2.   Bind a traffic behavior and a traffic classifier to the traffic policy.
                         classifier classifier-name behavior behavior-name [ precedence precedence-value ]

                    3.   Exit the traffic policy view.
                         quit

         Step 4 Apply a traffic policy.
                    ●    Apply a traffic policy to the system.
                         a.     Apply a traffic policy to the system.
                                traffic-policy policy-name global [ slot slot-id ] { inbound | outbound }

                    ●    Apply a traffic policy to an interface.
                         a.     Enter the interface view.
                                interface interface-type interface-number

                         b.     Apply a traffic policy to the interface.
                                traffic-policy policy-name { inbound | outbound }

                         c.     Exit the interface view.
                                quit

                    ●    Apply a traffic policy to a VLAN.
                         a.     Create a VLAN and enter the VLAN view.
                                vlan vlan-id


Issue 01 (2025-03-03)                  Copyright © Huawei Technologies Co., Ltd.                                             111
QoS Configuration                                                  9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                  based Rate Limiting Configuration


                        b.    Apply a traffic policy to the VLAN.
                              traffic-policy policy-name { inbound | outbound }

                        c.    Exit the VLAN view.
                              quit

                    ●   Apply a traffic policy to a VPN instance.
                        a.    Create a VPN instance and enter the VPN instance view.
                              ip vpn-instance vpn-instance-name

                        b.    Apply a traffic policy to the VPN instance.
                              traffic-policy policy-name inbound

                                      NOTE

                                     A traffic policy can be applied to a VPN instance only in the inbound direction.
                                     Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S,
                                     S6730E-H-V2, S5755E-H, S5755-S and S5755-H series support this function.
                        c.    Exit the VPN instance view.
                              quit

                    ●   Apply a traffic policy to a QoS group.
                        If the same traffic policy needs to be applied to multiple VLANs or interfaces,
                        you are advised to add the VLANs or interfaces to the same QoS group and
                        then apply the traffic policy to the QoS group.
                        a.    Create a QoS group and enter the QoS group view.
                              qos group group-name

                        b.    Add a specified interface or VLAN to the QoS group.
                              group-member { interface { interface-type interface-num | interface-name [ to interface-type
                              interface-num | interface-name ] } &<1-8> | vlan { vlanid [ to vlanid ] } &<1-8> }

                              Only one type of member can be specified.
                        c.    Apply a traffic policy to the QoS group.
                              traffic-policy policy-name { inbound | outbound }

                        d.    Exit the QoS group view.
                              quit

                    ----End

9.5.2 Configuring Traffic Policing (Level-2 CAR)

Prerequisites
                    Traffic policing (level-1 CAR) has been configured. For details, see 9.5.1
                    Configuring MQC-based Traffic Policing (Level-1 CAR).


Context
                    The device supports 2-level traffic policing. After using MQC to implement traffic
                    policing (level-1 CAR) for service flows matching a traffic classifier in a traffic
                    policy, the device aggregates all the service flows matching the traffic classifiers
                    associated with the level-1 CAR in the same traffic policy and performs traffic
                    policing (level-2 CAR) for the aggregated flow. 2-level traffic policing implements
                    multiplexing of traffic statistics and provides fine-grained service control.


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                         112
QoS Configuration                                                    9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                    based Rate Limiting Configuration


                          NOTE

                         Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S, S6730E-H-V2,
                         S5755E-H, S5755-S and S5755-H series support this function.


Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 (Optional) Disable the device from counting the inter-frame gaps and preambles
                when the device calculates the traffic policing rate.
                    qos car ifg disable

         Step 3 Configure a CAR profile.
                    qos car car-name { percent percent-value | cir cir-value [ kbps | mbps | gbps ] [ cbs cbs-value [ bytes |
                    kbytes | mbytes ] [ pbs pbs-value [ bytes | kbytes | mbytes ] ] | pir pir-value [ kbps | mbps | gbps ] [ cbs
                    cbs-value [ bytes | kbytes | mbytes ] pbs pbs-value [ bytes | kbytes | mbytes ] ] ] }

                          NOTE

                    The CIR of the QoS CAR profile must be greater than the CIR of the CAR configured in level-1
                    CAR.

         Step 4 Configure aggregated CAR.
                    1.   Enter the traffic behavior view.
                         traffic behavior behavior-name

                    2.   Configure aggregated CAR.
                         car car-name share

                                NOTE

                               A traffic policy containing the aggregated CAR action can be applied only to the
                               inbound direction.

                    ----End

9.5.3 Verifying the Configuration

Procedure

                    Operation                                               Command

                    Check the configured traffic classifiers.               display traffic classifier [ classifier-
                                                                            name ]

                    Check the configured traffic behaviors.                 display traffic behavior [ behavior-
                                                                            name ]

                    Check the configured traffic policy.                    display traffic policy [ policy-name
                                                                            [ classifier classifier-name ] ]


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                                113
QoS Configuration                                               9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                               based Rate Limiting Configuration


                    Operation                                         Command

                    Check the traffic policy application              display traffic-policy applied-record
                    records.

                    Check the traffic statistics after MQC-           display traffic-policy statistics
                    based traffic policing (level-1 CAR) is
                    enabled.

                    Check the CAR profile configuration.              display qos car [ name car-name ]



9.5.4 Example for Configuring MQC-based Traffic Policing
(Level-1 CAR)
Networking Requirements
                    In Figure 9-7, packets sent by Host1, Host2, and Host3 traverse DeviceA, DeviceB,
                    and DeviceC to reach the external network. Interface 1 (connected to Host1),
                    interface 2 (connected to Host2), and interface 3 (connected to Host3) join VLAN
                    10, VLAN 20, and VLAN 30, respectively.

                    Figure 9-7 Networking for configuring MQC to implement traffic policing
                         NOTE

                        In this example, interface 1, interface 2, interface 3, and interface 4 represent 10GE 1/0/1,
                        10GE 1/0/2, 10GE 1/0/3, and 10GE 1/0/4, respectively.




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                        114
QoS Configuration                                                9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                based Rate Limiting Configuration


                    The rates of traffic from tenants must be limited within proper ranges on DeviceB.
                    Table 9-7 lists the required CIR values for uplink traffic from tenants.

                    Table 9-7 CIR values for uplink traffic from tenants on DeviceB
                     Host                                         CIR (kbit/s)

                     Host1                                        2000

                     Host2                                        4000

                     Host3                                        8000




Procedure
         Step 1 Create VLANs and configure interfaces so that hosts can access the network
                through DeviceB.
                    # Create VLANs 10, 20, and 30 on DeviceA and add interfaces to the
                    corresponding VLANs.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceA
                    [DeviceA] vlan batch 10 20 30
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] portswitch
                    [DeviceA-10GE1/0/1] port link-type access
                    [DeviceA-10GE1/0/1] port default vlan 10
                    [DeviceA-10GE1/0/1] quit
                    [DeviceA] interface 10ge 1/0/2
                    [DeviceA-10GE1/0/2] portswitch
                    [DeviceA-10GE1/0/2] port link-type access
                    [DeviceA-10GE1/0/2] port default vlan 20
                    [DeviceA-10GE1/0/2] quit
                    [DeviceA] interface 10ge 1/0/3
                    [DeviceA-10GE1/0/3] portswitch
                    [DeviceA-10GE1/0/3] port link-type access
                    [DeviceA-10GE1/0/3] port default vlan 30
                    [DeviceA-10GE1/0/3] quit
                    [DeviceA] interface 10ge 1/0/4
                    [DeviceA-10GE1/0/4] portswitch
                    [DeviceA-10GE1/0/4] port link-type trunk
                    [DeviceA-10GE1/0/4] port trunk allow-pass vlan 10 20 30
                    [DeviceA-10GE1/0/4] quit

                    # Create VLANs 10, 20, and 30 on DeviceB and add 10GE 1/0/1 to the VLANs.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 10 20 30
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type trunk
                    [DeviceB-10GE1/0/1] port trunk allow-pass vlan 10 20 30
                    [DeviceB-10GE1/0/1] quit

         Step 2 Configure traffic classifiers.
                    # On DeviceB, configure traffic classifiers c1, c2, and c3 to match service flows
                    from different hosts based on VLAN IDs.
                    [DeviceB] traffic classifier c1
                    [DeviceB-classifier-c1] if-match vlan 10


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                   115
QoS Configuration                                                    9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                    based Rate Limiting Configuration

                    [DeviceB-classifier-c1] quit
                    [DeviceB] traffic classifier c2
                    [DeviceB-classifier-c2] if-match vlan 20
                    [DeviceB-classifier-c2] quit
                    [DeviceB] traffic classifier c3
                    [DeviceB-classifier-c3] if-match vlan 30
                    [DeviceB-classifier-c3] quit

         Step 3 Configure traffic behaviors and define traffic policing.

                    # On DeviceB, create traffic behaviors b1, b2, and b3 to police packets from
                    different hosts.
                    [DeviceB] traffic behavior b1
                    [DeviceB-behavior-b1] car cir 2000
                    [DeviceB-behavior-b1] statistics enable
                    [DeviceB-behavior-b1] quit
                    [DeviceB] traffic behavior b2
                    [DeviceB-behavior-b2] car cir 4000
                    [DeviceB-behavior-b2] statistics enable
                    [DeviceB-behavior-b2] quit
                    [DeviceB] traffic behavior b3
                    [DeviceB-behavior-b3] car cir 8000
                    [DeviceB-behavior-b3] statistics enable
                    [DeviceB-behavior-b3] quit

         Step 4 Configure a traffic policy and apply it to an inbound interface.

                    # On DeviceB, create traffic policy p1, bind configured traffic behaviors and traffic
                    classifiers to this traffic policy, and apply the traffic policy to the inbound direction
                    of 10GE 1/0/1 to police packets from hosts.
                    [DeviceB] traffic policy p1
                    [DeviceB-trafficpolicy-p1] classifier c1 behavior b1
                    [DeviceB-trafficpolicy-p1] classifier c2 behavior b2
                    [DeviceB-trafficpolicy-p1] classifier c3 behavior b3
                    [DeviceB-trafficpolicy-p1] quit
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] traffic-policy p1 inbound
                    [DeviceB-10GE1/0/1] quit

                    ----End

Verifying the Configuration
                    After the configuration is complete, check the traffic policing configuration on
                    DeviceB.

                    # Check the traffic classifier configuration.
                    [DeviceB] display traffic classifier
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match vlan 10

                      Classifier: c2
                       Type: OR
                       Rule(s):
                         if-match vlan 20

                      Classifier: c3
                       Type: OR
                       Rule(s):
                         if-match vlan 30


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                       116
QoS Configuration                                                      9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                      based Rate Limiting Configuration


                    Total classifier number is 3

                    # Check the traffic policy configuration.
                    [DeviceB] display traffic policy p1
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: OR
                        Behavior: b1
                          Committed Access Rate:
                           CIR 2000 (Kbps), PIR 2000 (Kbps), CBS 16000 (Bytes), PBS16000 (Bytes)
                           Color Mode: color blind
                           Conform Action: pass
                           Yellow Action: pass
                           Exceed Action: discard
                          Share car:
                           Car car1 share
                          Statistics: enable

                        Classifier: c2
                         Type: OR
                        Behavior: b2
                         Committed Access Rate:
                           CIR 4000 (Kbps), PIR 4000 (Kbps), CBS 32000 (Bytes), PBS 32000 (Bytes)
                           Color Mode: color blind
                           Conform Action: pass
                           Yellow Action: pass
                           Exceed Action: discard
                         Statistics: enable

                        Classifier: c3
                         Type: OR
                        Behavior: b3
                         Committed Access Rate:
                           CIR 8000 (Kbps), PIR 8000 (Kbps), CBS 64000 (Bytes), PBS 64000 (Bytes)
                           Color Mode: color blind
                           Conform Action: pass
                           Yellow Action: pass
                           Exceed Action: discard
                         Statistics: enable

                    # Check statistics about the traffic policy applied to 10GE 1/0/1.
                    [DeviceB] display traffic-policy statistics interface 10ge 1/0/1 inbound
                    Traffic policy: p1, inbound
                    --------------------------------------------------------------------------------
                     Slot: 1
                     Item                Packets              Bytes          pps          bps
                     -------------------------------------------------------------------------------
                     Matched                363949175            46585494400          8460795 8663854896
                      Passed              363949175            46585494400          8460795 8663854896
                      Dropped                   0                0           0          0
                       Filter               0                 0          0           0
                       CAR                    0                0           0          0
                     -------------------------------------------------------------------------------

                    The preceding command output shows that the traffic policy p1 is applied to 10GE
                    1/0/1.

Configuration Scripts
                    ●     DeviceB
                          #
                          sysname DeviceB
                          #
                          vlan batch 10 20 30


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                         117
QoS Configuration                                                  9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                  based Rate Limiting Configuration

                        #
                        traffic classifier c1 type or
                         if-match vlan 10
                        #
                        traffic classifier c2 type or
                         if-match vlan 20
                        #
                        traffic classifier c3 type or
                         if-match vlan 30
                        #
                        traffic behavior b1
                         statistics enable
                         car cir 2000 kbps
                        #
                        traffic behavior b2
                         statistics enable
                         car cir 4000 kbps
                        #
                        traffic behavior b3
                         statistics enable
                         car cir 8000 kbps
                        #
                        traffic policy p1
                         classifier c1 behavior b1 precedence 5
                         classifier c2 behavior b2 precedence 10
                         classifier c3 behavior b3 precedence 15
                        #
                        interface 10GE1/0/1
                         port link-type trunk
                         port trunk allow-pass vlan 10 20 30
                         traffic-policy p1 inbound
                        #
                        return

                    ●   DeviceA
                        #
                        sysname DeviceA
                        #
                        vlan batch 10 20 30
                        #
                        interface 10GE1/0/1
                         port link-type access
                         port default vlan 10
                        #
                        interface 10GE1/0/2
                         port link-type access
                         port default vlan 20
                        #
                        interface 10GE1/0/3
                         port link-type access
                         port default vlan 30
                        #
                        interface 10GE1/0/4
                         port link-type trunk
                         port trunk allow-pass vlan 10 20 30
                        #
                        return


9.5.5 Example for Configuring Traffic Policing (Level-2 CAR)

Networking Requirements
                         NOTE

                        Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S, S6730E-H-V2,
                        S5755E-H, S5755-S and S5755-H series support this example.


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                       118
QoS Configuration                                             9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                             based Rate Limiting Configuration


                    In Figure 9-8, DeviceB is connected to DeviceC through interface 2, and the
                    enterprise can access the network through DeviceB and DeviceC.

                    Figure 9-8 Network diagram for configuring traffic policing (level-2 CAR)
                         NOTE

                        In this example, interface 1 and interface 2 represent 10GE 1/0/1 and 10GE 1/0/2,
                        respectively.




                    On this network, the network-side bandwidth is lower than the enterprise's LAN
                    bandwidth. As a result, network congestion may occur on network-side interfaces,
                    causing data loss. Therefore, the total egress bandwidth needs to be limited to
                    12000 kbit/s. In addition, traffic policing needs to be performed on voice, video,
                    and data services to limit the traffic rates within proper ranges.

                    Voice, video, and data services are transmitted in VLAN 10, VLAN 30, and VLAN 20
                    respectively, and have different QoS requirements in descending order of priority.
                    Therefore, DeviceB needs to re-mark DSCP priorities of different service packets so
                    that DeviceC can process the packets based on their priorities, ensuring QoS.

                    Table 9-8 describes the configuration requirements.

                    Table 9-8 QoS guarantee provided by DeviceB for uplink traffic

                     Traffic        CIR (kbit/s)              PIR (kbit/s)                DSCP Priority
                     Type

                     Voice          2000                      10000                       46

                     Data           4000                      10000                       14

                     Video          4000                      10000                       30




Procedure
         Step 1 Create VLANs and configure interfaces.

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                   119
QoS Configuration                                                    9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                    based Rate Limiting Configuration


                    # On DeviceB, create VLANs 10, 20, and 30, and add interfaces to the
                    corresponding VLANs.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 10 20 30
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type trunk
                    [DeviceB-10GE1/0/1] port trunk allow-pass vlan 10 20 30
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type trunk
                    [DeviceB-10GE1/0/2] port trunk allow-pass vlan 10 20 30
                    [DeviceB-10GE1/0/2] quit

         Step 2 Configure a CAR profile.
                    [DeviceB] qos car car1 cir 12000

         Step 3 Configure traffic classifiers.
                    # On DeviceB, create traffic classifiers c1, c2, and c3 to classify different service
                    flows based on their VLAN IDs.
                    [DeviceB] traffic classifier c1
                    [DeviceB-classifier-c1] if-match vlan 10
                    [DeviceB-classifier-c1] quit
                    [DeviceB] traffic classifier c2
                    [DeviceB-classifier-c2] if-match vlan 20
                    [DeviceB-classifier-c2] quit
                    [DeviceB] traffic classifier c3
                    [DeviceB-classifier-c3] if-match vlan 30
                    [DeviceB-classifier-c3] quit

         Step 4 Configure traffic behaviors and define traffic policing.
                    # On DeviceB, create traffic behaviors b1, b2, and b3 to perform traffic policing
                    and priority re-marking for different service flows.
                    [DeviceB] traffic behavior b1
                    [DeviceB-behavior-b1] car cir 2000 pir 10000 green pass
                    [DeviceB-behavior-b1] car car1 share
                    [DeviceB-behavior-b1] remark dscp 46
                    [DeviceB-behavior-b1] statistics enable
                    [DeviceB-behavior-b1] quit
                    [DeviceB] traffic behavior b2
                    [DeviceB-behavior-b2] car cir 4000 pir 10000 green pass
                    [DeviceB-behavior-b2] remark dscp 14
                    [DeviceB-behavior-b2] statistics enable
                    [DeviceB-behavior-b2] quit
                    [DeviceB] traffic behavior b3
                    [DeviceB-behavior-b3] car cir 4000 pir 10000 green pass
                    [DeviceB-behavior-b3] remark dscp 30
                    [DeviceB-behavior-b3] statistics enable
                    [DeviceB-behavior-b3] quit

         Step 5 Configure a traffic policy and apply it to an inbound interface.
                    # On DeviceB, create traffic policy p1, bind traffic classifiers to traffic behaviors in
                    the traffic policy, and apply the traffic policy to the inbound direction of 10GE
                    1/0/1 to perform traffic policing and re-marking on packets.
                    [DeviceB] traffic policy p1
                    [DeviceB-trafficpolicy-p1] classifier c1 behavior b1
                    [DeviceB-trafficpolicy-p1] classifier c2 behavior b2
                    [DeviceB-trafficpolicy-p1] classifier c3 behavior b3
                    [DeviceB-trafficpolicy-p1] quit


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                       120
QoS Configuration                                                   9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                   based Rate Limiting Configuration

                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] traffic-policy p1 inbound
                    [DeviceB-10GE1/0/1] quit

                    ----End

Verifying the Configuration
                    # Check the traffic classifier configuration.
                    [DeviceB] display traffic classifier
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match vlan 10

                      Classifier: c2
                       Type: OR
                       Rule(s):
                         if-match vlan 20

                      Classifier: c3
                       Type: OR
                       Rule(s):
                         if-match vlan 30

                    Total classifier number is 3

                    # Check the configuration of traffic policy p1.
                    [DeviceB] display traffic policy p1
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: OR
                        Behavior: b1
                          Committed Access Rate:
                           CIR 2000 (Kbps), PIR 10000 (Kbps), CBS 16000 (Bytes), PBS 80000 (Bytes)
                           Color Mode: color blind
                           Conform Action: pass
                           Yellow Action: pass
                           Exceed Action: discard
                          Remark:
                           Remark dscp ef
                          Statistics: enable

                        Classifier: c2
                         Type: OR
                        Behavior: b2
                         Committed Access Rate:
                           CIR 4000 (Kbps), PIR 10000 (Kbps), CBS 32000 (Bytes), PBS 80000 (Bytes)
                           Color Mode: color blind
                           Conform Action: pass
                           Yellow Action: pass
                           Exceed Action: discard
                         Remark:
                           Remark dscp af13
                         Statistics: enable

                        Classifier: c3
                         Type: OR
                        Behavior: b3
                         Committed Access Rate:
                           CIR 4000 (Kbps), PIR 10000 (Kbps), CBS 32000 (Bytes), PBS 80000 (Bytes)
                           Color Mode: color blind
                           Conform Action: pass
                           Yellow Action: pass
                           Exceed Action: discard


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                      121
QoS Configuration                                                        9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                        based Rate Limiting Configuration

                         Remark:
                          Remark dscp af33
                         Statistics: enable

                    # Check statistics about the traffic policy applied to 10GE 1/0/1.
                    [DeviceB] display traffic-policy statistics interface 10ge 1/0/1 inbound
                    Traffic policy: p1, inbound
                    --------------------------------------------------------------------------------
                     Slot: 1
                     Item               Packets               Bytes          pps          bps
                    -------------------------------------------------------------------------------
                     Matched                   0                 0          0           0
                      Passed                 0                 0          0           0
                      Dropped                  0                0           0          0
                       Filter              0                 0          0           0
                       CAR                   0                0           0          0
                    -------------------------------------------------------------------------------

                    # Take voice packets as an example. Send voice packets with VLAN ID 10 to 10GE
                    1/0/1 at the rates of 1500 kbit/s and 12500 kbit/s. Then, run the display traffic-
                    policy statistics interface 10ge 1/0/1 inbound command to view traffic statistics
                    on the interface. If the configuration is successful, all packets with VLAN ID 10 and
                    sent at the rate of 1500 kbit/s can pass through 10GE1/0/1, and no packet is
                    discarded. Packets with VLAN ID 10 and sent at the rate of 12500 kbit/s pass
                    through 10GE 1/0/1 at the rate of 10000 kbit/s, and no packet is discarded. If
                    packets with VLAN ID 10, packets with VLAN ID 30, and packets with VLAN ID 20
                    are sent to 10GE 1/0/1 at the rates of 3000 kbit/s, 5000 kbit/s, and 5000 kbit/s
                    respectively, these packets pass through the interface at the rate of 12000 kbit/s,
                    and excess packets are discarded.

Configuration Scripts
                    ●     DeviceB
                          #
                          sysname DeviceB
                          #
                          vlan batch 10 20 30
                          #
                          qos car car1 cir 12000 kbps
                          #
                          traffic classifier c1 type or
                           if-match vlan 10
                          #
                          traffic classifier c2 type or
                           if-match vlan 20
                          #
                          traffic classifier c3 type or
                           if-match vlan 30
                          #
                          traffic behavior b1
                           statistics enable
                           car cir 2000 kbps pir 10000 kbps green pass
                           car car1 share
                           remark dscp ef
                          #
                          traffic behavior b2
                           statistics enable
                           car cir 4000 kbps pir 10000 kbps green pass
                           remark dscp af13
                          #
                          traffic behavior b3
                           statistics enable
                           car cir 4000 kbps pir 10000 kbps green pass
                           remark dscp af33


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                          122
QoS Configuration                                                  9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                  based Rate Limiting Configuration

                        #
                        traffic policy p1
                         classifier c1 behavior b1 precedence 5
                         classifier c2 behavior b2 precedence 10
                         classifier c3 behavior b3 precedence 15
                        #
                        interface 10GE1/0/1
                         port link-type trunk
                         port trunk allow-pass vlan 10 20 30
                         traffic-policy p1 inbound
                        #
                        interface 10GE1/0/2
                         port link-type trunk
                         port trunk allow-pass vlan 10 20 30
                        #
                        return


9.5.6 Example for Configuring Traffic Policing to Limit the
Rate of Each IP Address on a Network Segment
Networking Requirements
                    In Figure 9-9, users on an enterprise network send packets through DeviceA and
                    DeviceB, and access the external network through DeviceC. Users reside on two
                    different network segments. It is required that the rate of traffic from each IP
                    address on network segment 192.168.1.0/24 be limited to 64 kbit/s and the rate of
                    traffic from each IP address on network segment 192.168.2.0/24 be limited to 128
                    kbit/s.

                    Figure 9-9 Network diagram for configuring traffic policing to limit the rate of
                    each IP address on a network segment
                         NOTE

                        In this example, interface 1 and interface 2 represent 10GE 1/0/1 and 10GE 1/0/2,
                        respectively.




Procedure
         Step 1 Create VLANs and configure interfaces so that enterprise users can access the
                network through DeviceB.
                    # Create VLANs 10 and 20 on DeviceB and add 10GE 1/0/1 to the VLANs.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                       123
QoS Configuration                                                    9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                    based Rate Limiting Configuration

                    [DeviceB] vlan batch 10 20
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type trunk
                    [DeviceB-10GE1/0/1] port trunk allow-pass vlan 10 20
                    [DeviceB-10GE1/0/1] quit

                    # Configure VLANIF interfaces on DeviceB and configure IP addresses for them.
                    [DeviceB] interface vlanif 10
                    [DeviceB-Vlanif10] ip address 192.168.1.1 24
                    [DeviceB-Vlanif10] quit
                    [DeviceB] interface vlanif 20
                    [DeviceB-Vlanif20] ip address 192.168.2.1 24
                    [DeviceB-Vlanif20] quit

         Step 2 Configure traffic classifiers.

                    # On DeviceB, create ACL rules to match packets from enterprise users, and create
                    traffic classifiers c1 and c2 to classify service flows from different enterprise users
                    based on their IP addresses.
                    [DeviceB] acl 2001
                    [DeviceB-acl4-basic-2001] rule permit source 192.168.1.0 0.0.0.255
                    [DeviceB-acl4-basic-2001] quit
                    [DeviceB] acl 2002
                    [DeviceB-acl4-basic-2002] rule permit source 192.168.2.0 0.0.0.255
                    [DeviceB-acl4-basic-2002] quit
                    [DeviceB] traffic classifier c1
                    [DeviceB-classifier-c1] if-match acl 2001
                    [DeviceB-classifier-c1] quit
                    [DeviceB] traffic classifier c2
                    [DeviceB-classifier-c2] if-match acl 2002
                    [DeviceB-classifier-c2] quit

         Step 3 Configure traffic behaviors and define traffic policing.

                    # On DeviceB, create traffic behaviors b1 and b2 to perform traffic policing for
                    packets from different enterprise users.
                    [DeviceB] traffic behavior b1
                    [DeviceB-behavior-b1] car cir 64
                    [DeviceB-behavior-b1] quit
                    [DeviceB] traffic behavior b2
                    [DeviceB-behavior-b2] car cir 128
                    [DeviceB-behavior-b2] quit

         Step 4 Configure a traffic policy and apply it to the inbound direction of an inbound
                interface.

                    # On DeviceB, create traffic policy p1, bind traffic classifiers to traffic behaviors in
                    the traffic policy, and apply the traffic policy to the inbound direction of 10GE
                    1/0/1 to perform traffic policing for packets from two different network segments.
                    [DeviceB] traffic policy p1
                    [DeviceB-trafficpolicy-p1] classifier c1 behavior b1
                    [DeviceB-trafficpolicy-p1] classifier c2 behavior b2
                    [DeviceB-trafficpolicy-p1] quit
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] traffic-policy p1 inbound
                    [DeviceB-10GE1/0/1] quit

                    ----End



Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                       124
QoS Configuration                                                  9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                  based Rate Limiting Configuration


Verifying the Configuration
                    After the preceding configurations are complete, check the traffic policing
                    configuration on DeviceB.
                    # Check the traffic classifier configuration.
                    [DeviceB] display traffic classifier
                     Traffic Classifier Information:
                      Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match acl 2001

                      Classifier: c2
                       Type: OR
                       Rule(s):
                         if-match acl 2002

                    Total classifier number is 2

                    # Check the traffic policy configuration.
                    [DeviceB] display traffic policy p1
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: OR
                        Behavior: b1
                          Committed Access Rate:
                           CIR 64 (Kbps), PIR 64 (Kbps), CBS 10000 (Bytes), PBS 10000 (Bytes)
                           Color Mode: color blind
                           Conform Action: pass
                           Yellow Action: pass
                           Exceed Action: discard

                        Classifier: c2
                         Type: OR
                        Behavior: b2
                         Committed Access Rate:
                           CIR 128 (Kbps), PIR 128 (Kbps), CBS 10000 (Bytes), PBS 10000 (Bytes)
                           Color Mode: color blind
                           Conform Action: pass
                           Yellow Action: pass
                           Exceed Action: discard


Configuration Scripts
                    DeviceB
                    #
                    sysname DeviceB
                    #
                    vlan batch 10 20
                    #
                    acl 2001
                     rule permit source 192.168.1.0 0.0.0.255
                    #
                    acl 2002
                     rule permit source 192.168.2.0 0.0.0.255
                    #
                    traffic classifier c1 type or
                     if-match acl 2001
                    #
                    traffic classifier c2 type or
                     if-match acl 2002
                    #
                    traffic behavior b1
                     car cir 64


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                     125
QoS Configuration                                                    9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                    based Rate Limiting Configuration

                    #
                    traffic behavior b2
                     car cir 128
                    #
                    traffic policy p1
                     classifier c1 behavior b1
                     classifier c2 behavior b2
                    #
                    interface Vlanif10
                     ip address 192.168.1.1 255.255.255.0
                    #
                    interface Vlanif20
                     ip address 192.168.2.1 255.255.255.0
                    #
                    interface 10GE1/0/1
                     port link-type trunk
                     port trunk allow-pass vlan 10 20
                     traffic-policy p1 inbound
                    #
                    return



9.6 Configuring Traffic Shaping
Context
                    Traffic accessing a network is discarded due to the bandwidth limit on the
                    network. In contrast with traffic policing, which discards packets exceeding the
                    rate limit, traffic shaping buffers excess packets and sends them out at an even
                    rate.

9.6.1 Configuring Traffic Shaping for a Queue

Context
                    Packets received on an interface enter queues based on priority mapping. Then
                    the device uses different traffic shaping parameter settings for queues of different
                    priorities, providing differentiated services.

                    Before configuring traffic shaping for queues on an interface, configure priority
                    mapping to map packet priorities to per hop behaviors (PHBs) so that packets of
                    different services enter different queues.


Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Enter the interface view.
                    interface { interface-type interface-number | interface-name }

         Step 3 Configure the traffic shaping rate for queues on an interface.
                    qos queue queue-index shaping { percent cir cir-percent-value [ pir pir-percent-value ] | cir cir-value
                    [ kbps | mbps | gbps ] [ cbs cbs-value [ bytes | kbytes | mbytes ] | pir pir-value [ kbps | mbps | gbps ]
                    [ cbs cbs-value [ bytes | kbytes | mbytes ] pbs pbs-value [ bytes | kbytes | mbytes ] ] ] }

                    By default, the traffic shaping rate for a queue is the maximum bandwidth of the
                    interface.

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                                   126
QoS Configuration                                               9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                               based Rate Limiting Configuration


                         NOTE

                        Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S, S6730E-H-V2,
                        S5755E-H, S5755-S and S5755-H series support the mbytes parameter.

                    ----End

9.6.2 Verifying the Configuration
Procedure
                    ●   Run the display qos configuration interface [ { interface-type interface-
                        number | interface-name } ] command to check all QoS configurations on an
                        interface.
                    ●   Run the display qos queue statistics { slot slotid | interface { interface-type
                        interface-number | interface-name } } command to check queue-based traffic
                        statistics.
                    ----End

9.6.3 Example for Configuring Traffic Shaping to Limit the
Rate of Different Services
Networking Requirements
                    In Figure 9-10, three servers are deployed to provide voice, video, and data
                    services, and service packets traverse DeviceA, DeviceB, and DeviceC to reach the
                    external network. The interface connected to the voice service host joins VLAN 10;
                    the interface connected to the video service host joins VLAN 20; the interface
                    connected to the data service host joins VLAN 30.

                    Figure 9-10 Network diagram for configuring traffic shaping
                         NOTE

                        In this example, interface 1, interface 2, interface 3, and interface 4 represent 10GE 1/0/1,
                        10GE 1/0/2, 10GE 1/0/3, and 10GE 1/0/4, respectively.




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                        127
QoS Configuration                                            9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                            based Rate Limiting Configuration




                    Packets of voice, video, and data services are identified by 802.1p priorities 5, 3,
                    and 2 respectively. However, jitter may occur when packets from interface 2 on
                    DeviceB reach DeviceC. Table 9-9 lists the bandwidth requirements to limit jitter
                    and ensure services.

                    Table 9-9 Bandwidth for each service on DeviceB
                     Service Type                    CIR (kbit/s)                 PIR (kbit/s)

                     Voice                           3000                         5000

                     Video                           5000                         8000

                     Data                            2000                         3000




Procedure
         Step 1 On DeviceB, create VLANs and add interfaces to these VLANs so that users can
                access the network through DeviceB.
                    # Create VLANs 10, 20, and 30.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 10 20 30

                    # Set the access mode of interfaces 10GE 1/0/1 and 10GE 1/0/2 to trunk, and add
                    them to VLANs 10, 20, and 30.
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                128
QoS Configuration                                                        9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                        based Rate Limiting Configuration

                    [DeviceB-10GE1/0/1] port link-type trunk
                    [DeviceB-10GE1/0/1] port trunk allow-pass vlan 10 20 30
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type trunk
                    [DeviceB-10GE1/0/2] port trunk allow-pass vlan 10 20 30
                    [DeviceB-10GE1/0/2] quit

         Step 2 Set priorities for DeviceA's interfaces connected to the hosts to differentiate
                packets of different services.
                    # On DeviceA, set the priorities of 10GE 1/0/1, 10GE 1/0/2, and 10GE 1/0/3 to 5, 3,
                    and 2, respectively, and add 10GE 1/0/4 to VLANs 10, 20, and 30.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceA
                    [DeviceA] vlan batch 10 20 30
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] portswitch
                    [DeviceA-10GE1/0/1] port link-type access
                    [DeviceA-10GE1/0/1] port default vlan 10
                    [DeviceA-10GE1/0/1] port priority 5
                    [DeviceA-10GE1/0/1] quit
                    [DeviceA] interface 10ge 1/0/2
                    [DeviceA-10GE1/0/2] portswitch
                    [DeviceA-10GE1/0/2] port link-type access
                    [DeviceA-10GE1/0/2] port default vlan 20
                    [DeviceA-10GE1/0/2] port priority 3
                    [DeviceA-10GE1/0/2] quit
                    [DeviceA] interface 10ge 1/0/3
                    [DeviceA-10GE1/0/3] portswitch
                    [DeviceA-10GE1/0/3] port link-type access
                    [DeviceA-10GE1/0/3] port default vlan 30
                    [DeviceA-10GE1/0/3] port priority 2
                    [DeviceA-10GE1/0/3] quit
                    [DeviceA] interface 10ge 1/0/4
                    [DeviceA-10GE1/0/4] portswitch
                    [DeviceA-10GE1/0/4] port link-type trunk
                    [DeviceA-10GE1/0/4] port trunk allow-pass vlan 10 20 30
                    [DeviceA-10GE1/0/4] quit

         Step 3 Configure queue-based traffic shaping to limit the bandwidth of voice, video, and
                data services.
                    # Configure queue-based traffic shaping on DeviceB. Set the CIR values of voice,
                    video, and data services to 3000 kbit/s, 5000 kbit/s, and 2000 kbit/s, respectively.
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] qos queue 5 shaping cir 3000 pir 5000 kbps
                    [DeviceB-10GE1/0/2] qos queue 3 shaping cir 5000 pir 8000 kbps
                    [DeviceB-10GE1/0/2] qos queue 2 shaping cir 2000 pir 3000 kbps
                    [DeviceB-10GE1/0/2] quit

                    ----End

Verifying the Configuration
                    # Display statistics about queues in the outbound direction on 10GE 1/0/2.
                    [DeviceB] display qos queue statistics interface 10ge 1/0/2
                     Queue CIR/PIR                 Passed      Pass Rate            Dropped       Drop Rate Drop Time
                           (% or kbps) (Packets/Bytes) (pps/bps)                (Packets/Bytes) (pps/
                    bps)
                     ----------------------------------------------------------------------------------------------
                        0        0               0           0               0           0         -
                           10000000                  0           0               0           0


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                          129
QoS Configuration                                                           9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                           based Rate Limiting Configuration

                    ----------------------------------------------------------------------------------------------
                       1        0               0           0               0           0          -
                          10000000                  0           0               0           0
                    ----------------------------------------------------------------------------------------------
                       2     2000                 54584           0               0           0        -
                             3000                5676736          0               0           0
                    ----------------------------------------------------------------------------------------------
                       3     5000                 49648           0               0           0        -
                             8000                5163392            0               0           0
                    ----------------------------------------------------------------------------------------------
                       4        0               0           0               0           0          -
                          10000000                  0           0               0           0
                    ----------------------------------------------------------------------------------------------
                       5     3000                49998            0               0           0        -
                             5000               5199792              0               0           0
                    ----------------------------------------------------------------------------------------------
                       6        0               0           0               0           0          -
                          10000000                  0           0               0           0
                    ----------------------------------------------------------------------------------------------
                       7        0               0           0               0           0          -
                          10000000                  0           0               0           0
                    ----------------------------------------------------------------------------------------------


Configuration Scripts
                    ●    DeviceB
                         #
                         sysname DeviceB
                         #
                         vlan batch 10 20 30
                         #
                         interface 10GE1/0/1
                          port link-type trunk
                          port trunk allow-pass vlan 10 20 30
                         #
                         interface 10GE1/0/2
                          port link-type trunk
                          port trunk allow-pass vlan 10 20 30
                          qos queue 2 shaping cir 2000 kbps pir 3000 kbps
                          qos queue 3 shaping cir 5000 kbps pir 8000 kbps
                          qos queue 5 shaping cir 3000 kbps pir 5000 kbps
                         #
                         return

                    ●    DeviceA
                         #
                         sysname DeviceA
                         #
                         vlan batch 10 20 30
                         #
                         interface 10GE1/0/1
                          port link-type access
                          port default vlan 10
                          port priority 5
                         #
                         interface 10GE1/0/2
                          port link-type access
                          port default vlan 20
                          port priority 3
                         #
                         interface 10GE1/0/3
                          port link-type access
                          port default vlan 30
                          port priority 2
                         #
                         interface 10GE1/0/4
                          port link-type trunk
                          port trunk allow-pass vlan 10 20 30


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                             130
QoS Configuration                                             9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                             based Rate Limiting Configuration

                        #
                        return


9.6.4 Example for Configuring Traffic Shaping Based on
Priority Mapping in a DiffServ Domain

Networking Requirements
                    In Figure 9-11, packets of voice, video, and data services from the user side
                    traverse DeviceA, DeviceB, and DeviceC to reach the external network.


                    Figure 9-11 Network diagram for configuring traffic shaping
                         NOTE

                        In this example, interface 1 and interface 2 represent 10GE 1/0/1 and 10GE 1/0/2,
                        respectively.




                    Packets of voice, video, and data services are identified by 802.1p priorities 6, 5,
                    and 2, respectively. The interface bandwidth is limited to 10000 kbit/s. However,
                    jitter may occur when packets from interface 2 on DeviceB reach DeviceC. To
                    reduce jitter and ensure the bandwidth for various services, the following
                    bandwidth requirements must be met:


                    Table 9-10 Bandwidth provided for each service on DeviceB

                     Service Type                   CIR (kbit/s)                    PIR (kbit/s)

                     Voice                          3000                            5000

                     Video                          5000                            8000

                     Data                           2000                            3000




Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                   131
QoS Configuration                                                 9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                 based Rate Limiting Configuration


Procedure
         Step 1 On DeviceB, create a VLAN and configure interfaces so that users can access the
                network through DeviceB.
                    # Create VLAN 10.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 10

                    # Add 10GE 1/0/1 and 10GE 1/0/2 to VLAN 10 as trunk interfaces.
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type trunk
                    [DeviceB-10GE1/0/1] port trunk allow-pass vlan 10
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type trunk
                    [DeviceB-10GE1/0/2] port trunk allow-pass vlan 10
                    [DeviceB-10GE1/0/2] quit

         Step 2 Configure priority mapping.
                    # Create DiffServ domain ds1 and map 802.1p priorities 6, 5, and 2 to PHBs CS7,
                    EF, and AF2, respectively.
                    [DeviceB] diffserv domain ds1
                    [DeviceB-dsdomain-ds1] 8021p-inbound 6 phb cs7 //Map 802.1p priorities of different service packets to
                    different PHBs to ensure that the service packets enter different queues.
                    [DeviceB-dsdomain-ds1] 8021p-inbound 5 phb ef
                    [DeviceB-dsdomain-ds1] 8021p-inbound 2 phb af2
                    [DeviceB-dsdomain-ds1] quit
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] trust upstream ds1
                    [DeviceB-10GE1/0/1] quit

         Step 3 Configure traffic shaping on an interface.
                    # Configure traffic shaping on an interface of DeviceB to limit the rate of the
                    interface to 10000 kbit/s.
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] qos lr cir 10000 outbound //Configure interface-based rate limiting in the outbound
                    direction of the interface to limit the total bandwidth.

         Step 4 Configure queue-based traffic shaping on an interface.
                    # Configure queue-based traffic shaping on an interface of DeviceB. Set the CIR
                    values of voice, video, and data service packets to 3000 kbit/s, 5000 kbit/s, and
                    2000 kbit/s, respectively, and their PIR values to 5000 kbit/s, 8000 kbit/s, and 3000
                    kbit/s, respectively.
                    [DeviceB-10GE1/0/2] qos queue 7 shaping cir 3000 pir 5000 //Set the CIR value of voice packets entering
                    queue 7 to 3000 kbit/s according to the default mappings between PHBs and local priorities.
                    [DeviceB-10GE1/0/2] qos queue 5 shaping cir 5000 pir 8000
                    [DeviceB-10GE1/0/2] qos queue 2 shaping cir 2000 pir 3000
                    [DeviceB-10GE1/0/2] quit

                    ----End

Verifying the Configuration
                    # Check queue-based traffic statistics in the outbound direction of 10GE 1/0/2.

Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                           132
QoS Configuration                                                        9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                        based Rate Limiting Configuration

                    [DeviceB] display qos queue statistics interface 10ge 1/0/2
                     Queue CIR/PIR                 Passed      Pass Rate            Dropped       Drop Rate Drop Time
                           (% or kbps) (Packets/Bytes) (pps/bps)                (Packets/Bytes) (pps/
                    bps)
                     ----------------------------------------------------------------------------------------------
                        0        0               0           0               0           0         -
                           10000000                  0           0               0           0
                     ----------------------------------------------------------------------------------------------
                        1        0               0           0               0           0         -
                           10000000                  0           0               0           0
                     ----------------------------------------------------------------------------------------------
                        2     2000              54584            0                0           0        -
                              3000            5676736             0                0          0
                     ----------------------------------------------------------------------------------------------
                        3        0               0           0               0           0         -
                           10000000                  0           0               0           0
                     ----------------------------------------------------------------------------------------------
                        4        0               0           0               0           0         -
                           10000000                  0           0               0           0
                     ----------------------------------------------------------------------------------------------
                        5     5000              49648            0                0           0        -
                              8000            5163392             0                0          0
                     ----------------------------------------------------------------------------------------------
                        6        0               0           0               0           0         -
                           10000000                  0           0               0           0
                     ----------------------------------------------------------------------------------------------
                        7     3000              49998            0                0           0        -
                              5000            5199792             0                0          0
                     ----------------------------------------------------------------------------------------------


Configuration Scripts
                    ●     DeviceB
                          #
                          sysname DeviceB
                          #
                          vlan batch 10
                          #
                          diffserv domain ds1
                           8021p-inbound 6 phb cs7 green
                           8021p-inbound 5 phb ef green
                           8021p-inbound 2 phb af2 green
                          #
                          interface 10GE1/0/1
                           port link-type trunk
                           port trunk allow-pass vlan 10
                           trust upstream ds1
                          #
                          interface 10GE1/0/2
                           port link-type trunk
                           port trunk allow-pass vlan 10
                           qos lr cir 10000 outbound
                           qos queue 2 shaping cir 2000 kbps pir 3000 kbps
                           qos queue 5 shaping cir 5000 kbps pir 8000 kbps
                           qos queue 7 shaping cir 3000 kbps pir 5000 kbps
                          #
                          return


9.6.5 Example for Configuring Traffic Shaping Based on
Trusted 802.1p Priorities

Networking Requirements
                    In Figure 9-11, packets of voice, video, and data services from the user side
                    traverse DeviceA, DeviceB, and DeviceC to reach the external network.

Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                          133
QoS Configuration                                                9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                based Rate Limiting Configuration


                    Figure 9-12 Network diagram for configuring traffic shaping
                          NOTE

                        In this example, interface 1 and interface 2 represent 10GE 1/0/1 and 10GE 1/0/2,
                        respectively.




                    Packets of voice, video, and data services are identified by 802.1p priorities 6, 5,
                    and 2, respectively. The interface bandwidth is limited to 10000 kbit/s. However,
                    jitter may occur when packets from interface 2 on DeviceB reach DeviceC. To
                    reduce jitter and ensure the bandwidth for various services, the following
                    bandwidth requirements must be met:

                    Table 9-11 Bandwidth provided for each service on DeviceB
                     Service Type                     CIR (kbit/s)                    PIR (kbit/s)

                     Voice                            3000                            5000

                     Video                            5000                            8000

                     Data                             2000                            3000




Procedure
         Step 1 On DeviceB, create a VLAN and configure interfaces so that users can access the
                network through DeviceB.
                    # Create VLAN 10.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 10

                    # Add 10GE 1/0/1 and 10GE 1/0/2 to VLAN 10 as trunk interfaces.
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type trunk
                    [DeviceB-10GE1/0/1] port trunk allow-pass vlan 10
                    [DeviceB-10GE1/0/1] quit


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                     134
QoS Configuration                                                        9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                        based Rate Limiting Configuration

                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type trunk
                    [DeviceB-10GE1/0/2] port trunk allow-pass vlan 10
                    [DeviceB-10GE1/0/2] quit

         Step 2 Configure the packet priority trusted by an interface.
                    # Configure an interface to trust 802.1p priorities of packets.
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] trust 8021p outer //Configure the interface to trust 802.1p priorities so that packets
                    enter different queues based on the default mappings between 802.1p priorities, local priorities, and queues.
                    [DeviceB-10GE1/0/1] quit

         Step 3 Configure traffic shaping on an interface.
                    # Configure traffic shaping on an interface of DeviceB to limit the rate of the
                    interface to 10000 kbit/s.
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] qos lr cir 10000 outbound //Configure interface-based rate limiting in the outbound
                    direction of the interface to limit the total bandwidth.

         Step 4 Configure queue-based traffic shaping on an interface.
                    # Configure queue-based traffic shaping on an interface of DeviceB. Set the CIR
                    values of voice, video, and data service packets to 3000 kbit/s, 5000 kbit/s, and
                    2000 kbit/s, respectively, and their PIR values to 5000 kbit/s, 8000 kbit/s, and 3000
                    kbit/s, respectively.
                    [DeviceB-10GE1/0/2] qos queue 6 shaping cir 3000 pir 5000 //Set the CIR value of voice packets entering
                    queue 6 to 3000 kbit/s according to the default mappings between PHBs and local priorities.
                    [DeviceB-10GE1/0/2] qos queue 5 shaping cir 5000 pir 8000
                    [DeviceB-10GE1/0/2] qos queue 2 shaping cir 2000 pir 3000
                    [DeviceB-10GE1/0/2] quit

                    ----End

Verifying the Configuration
                    # Check queue-based traffic statistics in the outbound direction of 10GE 1/0/2.
                    [DeviceB] display qos queue statistics interface 10ge 1/0/2
                     Queue CIR/PIR                 Passed      Pass Rate            Dropped       Drop Rate Drop Time
                           (% or kbps) (Packets/Bytes) (pps/bps)                (Packets/Bytes) (pps/
                    bps)
                     ----------------------------------------------------------------------------------------------
                        0        0               0           0               0           0         -
                           10000000                  0           0               0           0
                     ----------------------------------------------------------------------------------------------
                        1        0               0           0               0           0         -
                           10000000                  0           0               0           0
                     ----------------------------------------------------------------------------------------------
                        2     2000              54584            0                0           0        -
                              3000           5676736              0                0          0
                     ----------------------------------------------------------------------------------------------
                        3        0               0           0               0           0         -
                           10000000                  0           0               0           0
                     ----------------------------------------------------------------------------------------------
                        4        0               0           0               0           0         -
                           10000000                  0           0               0           0
                     ----------------------------------------------------------------------------------------------
                        5     5000              49648            0                0           0        -
                              8000            5163392             0                0          0
                     ----------------------------------------------------------------------------------------------
                        6     3000              49998            0                0           0        -


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                              135
QoS Configuration                                                           9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                           based Rate Limiting Configuration

                             5000            5199792             0                0          0
                    ----------------------------------------------------------------------------------------------
                       7        0               0           0               0           0         -
                          10000000                  0           0               0           0
                    ----------------------------------------------------------------------------------------------


Configuration Scripts
                    ●    DeviceB
                         #
                         sysname DeviceB
                         #
                         vlan batch 10
                         #
                         interface 10GE1/0/1
                          port link-type trunk
                          port trunk allow-pass vlan 10
                          trust 8021p outer
                         #
                         interface 10GE1/0/2
                          port link-type trunk
                          port trunk allow-pass vlan 10
                          qos lr cir 10000 outbound
                          qos queue 2 shaping cir 2000 kbps pir 3000 kbps
                          qos queue 5 shaping cir 5000 kbps pir 8000 kbps
                          qos queue 6 shaping cir 3000 kbps pir 5000 kbps
                         #
                         return



9.7 Configuring Interface-based Rate Limiting
Context
                    Interface-based rate limiting controls the total rate of all packets passing through
                    an interface to ensure that the bandwidth utilization of the interface remains
                    within the allowed range. You can configure interface-based rate limiting in either
                    the inbound or outbound direction, or both.

9.7.1 Configuring Traffic Policing to Implement Interface-
based Rate Limiting
Context
                    If the rate of traffic sent from users is not limited, burst data continuously sent by
                    users may cause the network to become congested. Therefore, to limit the rate of
                    traffic entering an interface, traffic policing can be configured in the inbound
                    direction of the interface on a network device. When the rate of received packets
                    exceeds the specified traffic policing rate, the device discards excess packets.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 (Optional) Disable the device from counting the inter-frame gaps and preambles
                when the device calculates the traffic policing rate.
                    qos car ifg disable


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                             136
QoS Configuration                                                    9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                    based Rate Limiting Configuration


         Step 3 Create and configure a CAR profile.
                    qos car car-name { percent percent-value | cir cir-value [ kbps | mbps | gbps ] [ cbs cbs-value [ bytes |
                    kbytes | mbytes ] [ pbs pbs-value [ bytes | kbytes | mbytes ] ] | pir pir-value [ kbps | mbps | gbps ] [ cbs
                    cbs-value [ bytes | kbytes | mbytes ] pbs pbs-value [ bytes | kbytes | mbytes ] ] ] }

                    The CAR profile defines the rate limit of traffic policing.

         Step 4 Enter the interface view.
                    interface { interface-type interface-number | interface-name }

         Step 5 Apply the CAR profile to the inbound direction of the interface.
                    qos car inbound car-name

                    After the CAR profile is applied to the inbound direction of the interface, the
                    device rate-limits all incoming service traffic on the interface.

                    ----End


Verifying the Configuration
                    ●    Run the display qos car [ name car-name ] command to check the CAR
                         profile configuration.
                    ●    Run the display qos car statistics interface { interface-type interface-number
                         | interface-name } inbound command to check statistics about packets
                         forwarded and discarded on a specified interface on which traffic policing is
                         configured.

9.7.2 Configuring Traffic Shaping to Implement Interface-
based Rate Limiting

Context
                    When a large amount of traffic is sent from the downstream device to its
                    upstream device, to prevent congestion or packet loss, configure traffic shaping on
                    the outbound interface of the device. Traffic shaping adjusts the rate of outgoing
                    traffic on the interface to reduce traffic bursts so that outgoing packets can be
                    transmitted at a stable rate. With traffic shaping, packets exceeding the rate limit
                    enter the buffer queue, and are only sent out at an even rate when there are
                    sufficient tokens in the token bucket. In addition, if the buffer queue is full, the
                    system discards new packets.

                    If packets are encapsulated through VPN tunnels, the calculated packet length
                    during rate limiting or traffic statistics collection on a WAN-side interface includes
                    the lengths of link-layer and physical-layer packet headers and the lengths of
                    tunnel protocol encapsulation headers. As a result, the actual user bandwidth
                    calculated using this method is smaller than the expected value. To minimize the
                    difference between the actual bandwidth and the expected bandwidth, you can
                    disable the device from counting the inter-frame gaps and preambles when it
                    calculates the packet length, and configure the function of excluding the tunnel
                    encapsulation length of packets on the tunnel interface. In this way, the calculated
                    packet length excludes the lengths of the physical-layer packet header and tunnel
                    protocol encapsulation header.


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                                137
QoS Configuration                                                    9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                    based Rate Limiting Configuration


                          NOTE

                         Currently, traffic shaping can only be used to rate-limit outgoing traffic on an interface.
                         The system will generate an alarm when the rate of outgoing traffic on an interface
                         exceeds the alarm threshold for rate-limiting outgoing traffic.


Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Enter the interface view.
                    interface { interface-type interface-number | interface-name }

         Step 3 Configure the rate limit for outgoing traffic on the interface.
                    qos lr cir cir-value [ kbps | mbps | gbps ] [ cbs cbs-value [ bytes | kbytes | mbytes ] ] [ outbound ]

                    By default, the rate limit on an interface is the maximum bandwidth of the
                    interface.

                          NOTE

                         Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S, S6730E-H-V2,
                         S5755E-H, S5755-S and S5755-H series support the mbytes parameter.

                    ----End

9.7.3 Configuring Rate Limiting on the Management Interface
Context
                    If there is heavy traffic on the management interface due to malicious attacks or
                    network exceptions, the CPU of the device becomes overloaded, and system
                    operations are impacted. You can configure rate limiting on the management
                    interface to limit the rate of traffic entering the device through the management
                    interface, thereby ensuring the system runs properly.

                          NOTE

                         This function is supported only by the S6780-H, S6750E-S, S6750-S, S6730E-H-V2, S6730-H-
                         V2, S6750-H, S5735E-L-V2, S5735-L-V2, S5735R-S-V2, S5735E-S-V2, S5735-S-V2, S5735R-L-
                         V2, S5755-S, S5755E-H, S5755-H, and S5732-H-V2 series.
                         This function can be configured only on the management interface.


Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Enter the management interface view.
                    interface meth 0/0/0

         Step 3 Configure rate limiting on the management interface.
                    qos lr pps packets

                    By default, the rate limit on the management interface is 3000 pps.

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                                138
QoS Configuration                                              9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                              based Rate Limiting Configuration


                          NOTE

                        A small rate limit may impact the FTP, Telnet, SFTP, STelnet, and SSH functions.

                    ----End

Verifying the Configuration
                    ●    Run the display this interface command in the management interface view.
                         The Over-car-pps field in the command output indicates the packets that are
                         discarded due to rate limiting on the management interface.
                    ●    Run the display interface meth 0/0/0 command in any view. The Over-car-
                         pps field in the command output indicates the packets that are discarded due
                         to rate limiting on the management interface.

9.7.4 Example for Configuring Traffic Policing to Limit the
Rate on an Interface

Networking Requirements
                    In Figure 9-13, the host sends packets through DeviceA. It is required that the
                    bandwidth of the packets sent by the host should not exceed 100 Mbit/s.

                    Figure 9-13 Networking of interface-based rate limiting
                          NOTE

                        In this example, interface 1 represents 10GE 1/0/1.




Procedure
         Step 1 Configure a CAR profile.

                    # On DeviceA, create a CAR profile named car1 to police traffic from the host.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceA
                    [DeviceA] qos car car1 cir 100000

         Step 2 Apply the CAR profile.

Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                  139
QoS Configuration                                                           9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                           based Rate Limiting Configuration


                    # On DeviceA, apply CAR profile car1 to the inbound direction of 10GE 1/0/1 to
                    police the traffic from the host.
                    [DeviceA] interface 10ge 1/0/1
                    [DeviceA-10GE1/0/1] qos car inbound car1
                    [DeviceA-10GE1/0/1] quit

                    ----End

Verifying the Configuration
                    # Check the CAR profile configuration.
                    [DeviceA] display qos car name car1
                     ----------------------------------------------------------------
                      CAR Name : car1
                      CAR Index : 0
                       car cir 100000 kbps cbs 800000 bytes
                      Applied number on behavior : 0
                      Applied number on interface inbound : 1
                       10GE1/0/1
                      Applied number on Eth-Trunk inbound : 0
                      Applied number on interface outbound : 0
                      Applied number on Eth-Trunk outbound : 0

                    # Send packets to 10GE 1/0/1 at the rates of 60000 kbit/s and 110000 kbit/s,
                    respectively, and then run the display qos car statistics command to check the
                    traffic statistics. If the configuration is successful, all packets are successfully
                    forwarded when they are sent to 10GE 1/0/1 at 60000 kbit/s; however, some
                    packets are discarded when packets are sent to 10GE 1/0/1 at 110000 kbit/s.

Configuration Scripts
                    DeviceA
                    #
                    sysname DeviceA
                    #
                    qos car car1 cir 100000 kbps
                    #
                    interface 10GE1/0/1
                     qos car inbound car1
                    #
                    return



9.8 Configuring a QoS Profile
Context
                    You can configure traffic policing and packet processing priorities in the QoS
                    profile view. Currently, a QoS profile can be bound only to an AAA service scheme
                    for user authorization.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Create a QoS profile and enter the QoS profile view.

Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                            140
QoS Configuration                                                    9 Traffic Policing, Traffic Shaping, and Interface-
QoS Configuration                                                                    based Rate Limiting Configuration

                    qos-profile profile-name

         Step 3 Configure traffic policing and packet processing priorities in the QoS profile view.
                    ●    Configure traffic policing.
                         car cir cir-value [ pir pir-value ] [ cbs cbs-value pbs pbs-value ] { inbound | outbound }

                    ●    Configure the device to re-mark the DSCP priority of IP packets.
                         remark dscp dscp-value { inbound | outbound }

                    ●    Configure the device to re-mark the 802.1p priority of VLAN packets.
                         remark 8021p 8021p-value { inbound | outbound }

                    ----End

Verifying the Configuration
                    ●    Run the display qos-profile name profile-name command to check the
                         configuration of a specified QoS profile.
                    ●    Run the display qos-profile all command to check the configuration of all
                         QoS profiles.


9.9 Maintaining Traffic Policing, Traffic Shaping, and
Interface-based Rate Limiting
Context


                        NOTICE

                    Flow-based traffic statistics cannot be restored after they are cleared. Exercise
                    caution when running the below commands.


Procedure
                    ●    Clear the statistics about packets forwarded and discarded on a specified
                         interface for which traffic policing is configured to implement interface-based
                         rate limiting.
                         reset qos car statistics interface { interface-type interface-number | interface-name } inbound

                    ●    Clear queue-based traffic statistics.
                         reset qos queue statistics { interface { interface-type interface-number | interface-name } | slot slot-
                         id }


                    ----End




Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                                 141
QoS Configuration
QoS Configuration                                                 10 Congestion Avoidance Configuration




10                    Congestion Avoidance Configuration


                    10.1 Overview of Congestion Avoidance
                    10.2 Understanding Congestion Avoidance
                    10.3 Configuration Precautions for Congestion Avoidance
                    10.4 Default Settings for Congestion Avoidance
                    10.5 Configuring WRED
                    10.6 Configuring the CFI as the Internal Drop Priority
                    10.7 Verifying the Configuration
                    10.8 Maintaining Congestion Avoidance
                    10.9 Example for Configuring WRED


10.1 Overview of Congestion Avoidance
Definition
                    Congestion avoidance is a mechanism for controlling congestion. It monitors
                    network resources such as queues and memory buffers and discards packets when
                    congestion occurs or worsens.

Purpose
                    On traditional networks, quality of service (QoS) issues are mainly caused by
                    network congestion that arises from insufficient network resources. Congestion
                    extends the delay of packet transmission, lowers the throughput, and consumes
                    many resources. However, network congestion is very common in a complex
                    environment with a variety of services. Congestion avoidance uses specific packet
                    drop algorithms to prevent congestion from intensifying and make full use of
                    network bandwidth.


10.2 Understanding Congestion Avoidance

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                          142
QoS Configuration
QoS Configuration                                                 10 Congestion Avoidance Configuration


10.2.1 WRED Fundamentals
                    Tail drop and Weighted Random Early Detection (WRED) are common methods
                    for congestion avoidance.

Tail Drop
                    Tail drop is the conventional method for discarding packets. When the length of a
                    queue reaches the maximum value, the device enabled with tail drop discards all
                    new packets buffered at the tail of the queue.
                    The tail drop method may cause global TCP synchronization, preventing TCP
                    connections from being set up. In the following figure, the three colors represent
                    three TCP connections. When packets from multiple TCP connections are
                    discarded, these TCP connections enter the congestion avoidance and slow start
                    state. Traffic volume decreases, before peaking once again. This is repeated over
                    and over, resulting in unstable traffic that is constantly changing in volume.

                    Figure 10-1 Tail drop




WRED
                    Random Early Detection (RED) is used to avoid global TCP synchronization that
                    occurs with tail drop. It does this by randomly discarding packets so that the
                    transmission speed of multiple TCP connections is not reduced simultaneously.
                    This results in more stable rates of TCP traffic and other network traffic.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                           143
QoS Configuration
QoS Configuration                                                10 Congestion Avoidance Configuration


                    Figure 10-2 RED




                    However, RED does not accommodate QoS differentiation. Based on RED, WRED
                    uses drop profiles to implement congestion avoidance. A drop profile defines the
                    upper drop threshold, lower drop threshold, and drop probability. When a drop
                    profile is applied to an interface or an interface queue, packets are discarded
                    based on settings in the drop profile. When the length of a queue is smaller than
                    the lower drop threshold, the device does not discard packets. When the length of
                    a queue is between the lower drop threshold and the upper drop threshold, the
                    device randomly discards new packets, with a larger drop probability for longer
                    queues. When the length of a queue exceeds the upper drop threshold, the device
                    discards all new packets in the queue.
                    Figure 10-3 shows the curve of the WRED drop probability.

                    Figure 10-3 Curve of the WRED drop probability




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                         144
QoS Configuration
QoS Configuration                                                  10 Congestion Avoidance Configuration


10.2.2 Fundamentals of CFI (Used as the Internal Drop
Priority)
                    Layer 2 devices exchange VLAN frames. As defined in IEEE 802.1Q, the Canonical
                    Format Indicator (CFI), also known as the Drop Eligible Indicator (DEI), in the
                    VLAN frame header identifies the drop priority of packets. Figure 10-4 shows the
                    CFI field in a VLAN frame.

                    Figure 10-4 CFI field in a VLAN frame




                    In the VLAN tag, the value of the CFI field is 0 or 1. When the rate of packets on
                    the device exceeds the committed information rate (CIR), the value of the DEI
                    field is set to 1, indicating a high drop priority. When congestion occurs, the device
                    first discards the packets with the DEI field value of 1.


10.3 Configuration Precautions for Congestion
Avoidance

10.4 Default Settings for Congestion Avoidance
                    Table 10-1 Default settings for congestion avoidance
                     Parameter                                  Default Setting

                     WRED                                       Disabled

                     Lower drop threshold, in percentage        100

                     Upper drop threshold, in percentage        100

                     Maximum drop probability, in               100
                     percentage

                     Absolute values of the upper and           Not configured
                     lower drop thresholds




Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                            145
QoS Configuration
QoS Configuration                                                               10 Congestion Avoidance Configuration




10.5 Configuring WRED
Context
                    WRED randomly discards packets based on WRED parameter settings, preventing
                    global TCP synchronization and ensuring packets with higher priorities are less
                    likely to be discarded. According to a packet's color (drop priority), a WRED drop
                    profile defines absolute values of upper and lower drop thresholds, upper and
                    lower drop thresholds in percentage and the maximum drop probability.
                    Colors are used to determine whether packets are discarded during congestion
                    avoidance implementation and are independent of the mapping between internal
                    priorities and queues.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Create a WRED drop profile and enter the WRED drop profile view.
                    drop-profile drop-profile-name

                    By default, a WRED drop profile named default exists on the device.
                    A maximum of 63 WRED drop profiles, including the default drop profile, can be
                    configured on the device For the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-
                    H-V2, S6750E-S, S6730E-H-V2, S5755E-H, S5755-S and S5755-H series. The default
                    drop profile cannot be deleted, and only parameters in the profile can be
                    modified.
                    A maximum of 30 WRED drop profiles, including the default drop profile, can be
                    configured on the deviceFor S5735-L-V2, S5735-S-V2, S5735I-L-V2, S5735I-S-V2,
                    S5735I-H-V2, S5735R-L-V2, S3710-H, S5735R-S-V2, S5735E-L-V2, S5735E-S-V2
                    series. The default drop profile cannot be deleted, and only parameters in the
                    profile can be modified.
         Step 3 Set WRED parameters.
                    color { green | red | yellow } { buffer-size low-limit low-buffer-size high-limit high-buffer-size | buffer-
                    size cell low-limit low-buffer-size-cell high-limit high-buffer-size-cell | low-limit low-limit-percentage
                    high-limit high-limit-percentage } discard-percentage discard-percentage

                    By default, the upper drop threshold in percentage, lower drop threshold in
                    percentage, and maximum drop probability in a WRED drop profile are all 100,
                    and absolute values of upper and lower drop thresholds are not configured.

                            NOTE

                           Only the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S, S6730E-H-V2,
                           S5755E-H, S5755-S and S5755-H series support setting of the buffer-size parameter, that is,
                           setting of absolute values of upper and lower drop thresholds.

         Step 4 Exit the WRED drop profile view.
                    quit

         Step 5 Enter the interface view.

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                                 146
QoS Configuration
QoS Configuration                                                              10 Congestion Avoidance Configuration

                    interface { interface-type interface-number | interface-name }

         Step 6 Apply the WRED drop profile to a port queue.
                    qos queue queue-index wred drop-profile-name

         Step 7 Exit the interface view.
                    quit

                    ----End


10.6 Configuring the CFI as the Internal Drop Priority
Context
                    The Canonical Format Indicator (CFI) field, also known as the Drop Eligible
                    Indicator (DEI), in a VLAN tag identifies the drop priority of a packet. When the
                    rate of packets on the device exceeds the CIR, the value of the DEI field is set to 1.
                    In this case, the drop priority of the packets is high. When congestion occurs, the
                    device first discards the packets with the DEI field value of 1.
                    To configure the device to discard packets whose rate exceeds the CIR, configure
                    the CFI as the internal drop priority.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Enter the interface view.
                    interface { interface-type interface-number | interface-name }

         Step 3 Configure the CFI as the internal drop priority.
                    dei enable

         Step 4 Exit the interface view.
                    quit

                    ----End


10.7 Verifying the Configuration
Procedure
                    ●      Run the display drop-profile [ name drop-profile-name | brief ] command to
                           check the configuration of a WRED drop profile.
                    ●      Run the display qos configuration interface [ { interface-type interface-
                           number | interface-name } ] command to check all QoS configurations on an
                           interface.
                    ●      Run the display qos queue statistics { slot slotid | interface { interface-type
                           interface-number | interface-name } } command to check queue-based traffic
                           statistics.
                    ----End

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                   147
QoS Configuration
QoS Configuration                                                              10 Congestion Avoidance Configuration




10.8 Maintaining Congestion Avoidance
Context


                        NOTICE

                    Statistics cannot be restored after they are cleared. Exercise caution when clearing
                    the statistics.



Procedure
                    ●   Clear queue-based traffic statistics.
                        reset qos queue statistics { interface { interface-type interface-number | interface-name } | slot slot-
                        id }

                    ----End


10.9 Example for Configuring WRED
Networking Requirements
                    Host1 and Host2 provide voice, video, and data services, for which traffic is
                    transmitted through DeviceB and then DeviceA. To reduce the impact of network
                    congestion and guarantee high-priority, latency-sensitive services, set congestion
                    avoidance parameters according to Table 10-2.


                    Table 10-2 Congestion avoidance parameters

                     Service          Color             Lower             Upper              Drop              CoS
                     Type                               Drop              Drop               Probabilit
                                                        Threshold         Threshold          y (%)
                                                        (%)               (%)

                     Voice            Green             80                100                10                EF

                     Video            Yellow            60                80                 20                AF3

                     Data             Red               40                60                 40                AF1




                    Figure 10-5 Network diagram of congestion avoidance
                         NOTE

                        In this example, interface 1, interface 2, and interface 3 represent 10GE 1/0/1, 10GE 1/0/2,
                        and 10GE 1/0/3, respectively.



Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                                 148
QoS Configuration
QoS Configuration                                                            10 Congestion Avoidance Configuration




Procedure
         Step 1 Configure VLANs for interfaces so that devices can communicate with each other
                at the link layer.
                    # Configure 10GE 1/0/3 on DeviceB as a trunk interface. Add 10GE 1/0/1 to VLAN
                    100, 10GE 1/0/2 to VLAN 200, and 10GE 1/0/3 to VLAN 100 and VLAN 200.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 100 200
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type access
                    [DeviceB-10GE1/0/1] port default vlan 100
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type access
                    [DeviceB-10GE1/0/2] port default vlan 200
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] portswitch
                    [DeviceB-10GE1/0/3] port link-type trunk
                    [DeviceB-10GE1/0/3] port trunk allow-pass vlan 100 200
                    [DeviceB-10GE1/0/3] quit

         Step 2 Configure priority mapping to map 802.1p values in voice, data, and video packets
                to different CoS values and colors.
                    # Create the DiffServ domain ds1, map 802.1p values 6, 5, and 2 to CoS values EF,
                    AF3, and AF1, respectively, and color packets green, yellow, and red.
                    [DeviceB] diffserv domain ds1
                    [DeviceB-dsdomain-ds1] 8021p-inbound 6 phb ef green
                    [DeviceB-dsdomain-ds1] 8021p-inbound 5 phb af3 yellow
                    [DeviceB-dsdomain-ds1] 8021p-inbound 2 phb af1 red
                    [DeviceB-dsdomain-ds1] quit

                    # Bind the DiffServ domain to the inbound interfaces 10GE 1/0/1 and 10GE 1/0/2
                    on DeviceB.
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] trust upstream ds1
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                   149
QoS Configuration
QoS Configuration                                                                        10 Congestion Avoidance Configuration

                    [DeviceB-10GE1/0/2] trust upstream ds1
                    [DeviceB-10GE1/0/2] quit

         Step 3 Configure a WRED drop profile to mitigate or eliminate congestion.
                    # Create WRED drop profile wred1 on DeviceB and set WRED parameters for
                    green, yellow, and red packets in this drop profile.
                    [DeviceB] drop-profile wred1
                    [DeviceB-drop-wred1] color green low-limit 80 high-limit 100 discard-percentage 10
                    [DeviceB-drop-wred1] color yellow low-limit 60 high-limit 80 discard-percentage 20
                    [DeviceB-drop-wred1] color red low-limit 40 high-limit 60 discard-percentage 40
                    [DeviceB-drop-wred1] quit

                    # Apply the WRED drop profile wred1 to queues on the outbound interface of
                    DeviceB.
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] qos queue 5 wred wred1
                    [DeviceB-10GE1/0/3] qos queue 3 wred wred1
                    [DeviceB-10GE1/0/3] qos queue 1 wred wred1
                    [DeviceB-10GE1/0/3] quit

                    ----End

Verifying the Configuration
                    # Check the configuration of the DiffServ domain ds1. You can see that 802.1p
                    values 6, 5, and 2 are mapped to CoS values EF, AF3, and AF1 and to colors green,
                    yellow, and red, respectively.
                    <DeviceB> display diffserv domain name ds1
                    Diffserv domain name:ds1
                     8021p-inbound 0 phb be green
                     8021p-inbound 1 phb af1 green
                     8021p-inbound 2 phb af1 red
                     8021p-inbound 3 phb af3 green
                     8021p-inbound 4 phb af4 green
                     8021p-inbound 5 phb af3 yellow
                     8021p-inbound 6 phb ef green
                     8021p-inbound 7 phb cs7 green
                     8021p-outbound be green map 0
                     8021p-outbound be yellow map 0
                     8021p-outbound be red map 0
                     ...

                    # Check the configuration of the WRED drop profile wred1. You can see WRED
                    parameter settings of green, yellow, and red packets in this profile.
                    <DeviceB> display drop-profile name wred1
                    Drop-profile[1]: wred1
                    Color Mode             Low-limit High-limit Unit           Discard(%)
                    -----------------------------------------------------------------
                    Green Percentage 80                 100        %         10
                    Yellow Percentage 60                80         %        20
                    Red      Percentage 40             60         %        40
                    -----------------------------------------------------------------

                    # Check the configuration of 10GE 1/0/3. You can see the scheduling parameters
                    of queues with different CoS values.
                    <DeviceB> display qos configuration interface 10GE 1/0/3
                     interface 10GE1/0/3
                     --------------------------------------------------------------------------
                     trust flag       : outer 8021p
                     diffserv domain : default
                     dei enable         : disable


Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                           150
QoS Configuration
QoS Configuration                                                                       10 Congestion Avoidance Configuration

                    port priority     :0
                    phb marking 8021p : enable
                    phb marking dscp : disable
                    phb marking exp : -
                    port wred          :-
                    port lr         : cir = -, cbs = -
                    port car inbound : -
                    port car outbound : -
                    schedule profile : -
                    --------------------------------------------------------------------------
                    queue          shaping        schedule      wred
                              cir     pir
                              cbs      pbs
                    --------------------------------------------------------------------------
                    0            -       - pq          -
                                -       -

                    1           -       -   pq        wred1
                               -       -

                    2           -       -   pq        -
                               -       -

                    3           -       -   pq        wred1
                               -       -

                    4           -       -   pq        -
                               -       -

                    5           -       -   pq         wred1
                               -       -

                    6           -       -   pq        -
                               -       -

                    7           -       -   pq        -
                               -       -

                    --------------------------------------------------------------------------


Configuration Scripts
                    DeviceB
                    #
                    sysname DeviceB
                    #
                    drop-profile wred1
                     color green low-limit 80 high-limit 100 discard-percentage 10
                     color yellow low-limit 60 high-limit 80 discard-percentage 20
                     color red low-limit 40 high-limit 60 discard-percentage 40
                    #
                    vlan batch 100 200
                    #
                    diffserv domain ds1
                     8021p-inbound 2 phb af1 red
                     8021p-inbound 5 phb af3 yellow
                     8021p-inbound 6 phb ef green
                    #
                    interface 10GE1/0/1
                     port link-type access
                     port default vlan 100
                     trust upstream ds1
                    #
                    interface 10GE1/0/2
                     port link-type access
                     port default vlan 200
                     trust upstream ds1
                    #


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                           151
QoS Configuration
QoS Configuration                                                   10 Congestion Avoidance Configuration

                    interface 10GE1/0/3
                     port link-type trunk
                     port trunk allow-pass vlan 100 200
                     qos queue 1 wred wred1
                     qos queue 3 wred wred1
                     qos queue 5 wred wred1
                    #
                    return




Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                         152
QoS Configuration
QoS Configuration                                             11 Congestion Management Configuration




                            11                Congestion Management
                                                        Configuration

                    11.1 Overview of Congestion Management
                    11.2 Understanding Congestion Management
                    11.3 Configuration Precautions for Congestion Management
                    11.4 Default Settings for Congestion Management
                    11.5 Configuring Congestion Management
                    11.6 (Optional) Configuring Buffer Management
                    11.7 (Optional) Configuring Congestion Monitoring
                    11.8 Maintaining Congestion Management
                    11.9 Example for Configuring Congestion Management
                    11.10 Example for Configuring Congestion Avoidance and Congestion
                    Management (PQ+WDRR Scheduling and WRED Profile)
                    11.11 Example for Configuring Congestion Monitoring
                    11.12 Microburst Detection


11.1 Overview of Congestion Management
Definition
                    When a network is intermittently congested and latency-sensitive services require
                    higher bandwidth than others, congestion management adjusts the order in which
                    packets are scheduled.

Purpose
                    On a traditional network, QoS issues are primarily caused by network congestion
                    due to insufficient network resources. Congestion extends the delay of packet
                    transmission, lowers the throughput, and consumes many resources. However,

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                        153
QoS Configuration
QoS Configuration                                              11 Congestion Management Configuration


                    network congestion is very common in a complex environment with a variety of
                    services. When network congestion occurs, packets are buffered in queues.
                    Congestion management schedules the packets based on a scheduling algorithm,
                    ensuring that QoS-demanding services (such as latency-sensitive services) are
                    preferentially scheduled.


11.2 Understanding Congestion Management
                    As network services continue to increase and users demand even higher network
                    quality, the limited bandwidth cannot meet such lofty requirements. As a result,
                    issues begin to arise due to congestion, such as longer delays and signal loss.
                    Congestion management is required when a network is intermittently congested,
                    and delay-sensitive services require higher QoS than delay-insensitive services. If
                    congestion persists on the network after congestion management is configured, it
                    is necessary to increase the bandwidth. Congestion management implements
                    queuing and scheduling when sending packet flows.

                    The device has eight queues on each interface in the outbound direction, and the
                    queues are identified by index numbers ranging from 0 to 7. The device
                    automatically sends classified packets to queues based on mappings between
                    local priorities and queues, and then schedules the packets using queue
                    scheduling mechanisms.

Common Queue Scheduling Mechanisms
                    ●   Priority Queuing (PQ) scheduling
                        PQ scheduling, also referred to as strict priority (SP) scheduling, schedules
                        packets in descending order of queue priority. This means that packets in
                        queues with a low priority can be scheduled only after all packets in high
                        priority queues have been scheduled. The device places core services into
                        high-priority queues and non-core services (such as email services) into low-
                        priority queues, ensuring that core services are processed preferentially. In this
                        case, non-core services are processed only when all core services are
                        processed.
                        In Figure 11-1, queues 7 to 0 are arranged in descending order of priority.
                        When the link transmits packets, queue 7 is preferentially processed, with
                        subsequent queues processed only when queue 7 becomes empty. Packets in
                        lower-priority queues are sent at the link rate when higher-priority queues are
                        empty. The packets in queue 6 are sent at the link rate when packets in queue
                        6 need to be sent and queue 7 is empty. The packets in queue 5 are sent at
                        the link rate when queue 6 and queue 7 are empty, and so on.
                        PQ scheduling is valid for short-delay services. If we assume that data flow X
                        is placed in queue 7 on each node, the packets of data flow X are processed
                        first when they reach a node.
                        PQ scheduling, however, may result in the starvation of packets in queues
                        with lower priorities. For example, if data flows placed into queue 7 arrive at
                        100% link rate in a period, the scheduler does not process flows in queues 0
                        to 6.
                        To prevent starvation of packets in some queues, upstream devices must
                        accurately define the service characteristics of data flows so that data flows
                        placed in queue 7 do not exceed a given percentage of the link capacity.

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                             154
QoS Configuration
QoS Configuration                                             11 Congestion Management Configuration


                        Consequently, queue 7 is not full and the scheduler can process packets in
                        queues with lower priorities.

                        Figure 11-1 PQ scheduling




                    ●   Weighted Deficit Round Robin (WDRR) scheduling
                        WDRR schedules packets by taking the packet length into account, ensuring
                        that packets in all the queues are scheduled in turn.
                        In WDRR scheduling, the deficit indicates the bandwidth deficit of each queue.
                        The initial value is 0, and the system allocates bandwidth to each queue
                        based on the weight, with the deficit calculated as follows: If the deficit of a
                        queue is greater than 0, the queue participates in scheduling. The device
                        sends a packet and calculates the deficit based on that packet's length. If the
                        deficit of a queue is less than 0, the queue does not participate in scheduling,
                        and the current deficit is used as the initial value in the next round of
                        scheduling.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                            155
QoS Configuration
QoS Configuration                                               11 Congestion Management Configuration


                        Figure 11-2 Queue weights




                        In Figure 11-2, the weights of queues 7, 6, 5, 4, 3, 2, 1, and 0 are set to 40,
                        30, 20, 10, 40, 30, 20, and 10, respectively. During scheduling, queues 7, 6, 5,
                        4, 3, 2, 1, and 0 obtain 20%, 15%, 10%, 5%, 20%, 15%, 10%, and 5% of the
                        bandwidth, respectively. Queues 7 and 6 are used as examples to describe
                        WDRR scheduling. For this example, assume that queue 7 obtains 400 byte/s
                        bandwidth and queue 6 obtains 300 byte/s bandwidth.
                        –   First round of scheduling
                            Deficit[7][1] = 0 + 400 = 400
                            Deficit[6][1] = 0 + 300 = 300
                            After a packet of 900 bytes in queue 7 and a packet of 400 bytes in
                            queue 6 are sent, the values are as follows:
                            Deficit[7][1] = 400 – 900 = –500
                            Deficit[6][1] = 300 – 400 = –100
                        –   Second round of scheduling
                            Deficit [7][2] = –500 + 400 = –100
                            Deficit [6][2] = –100 + 300 = 200
                            The packet in queue 7 is not scheduled because the deficit of queue 7 is
                            negative. After a packet of 300 bytes in queue 6 is sent, the value is as
                            follows:
                            Deficit [6][2] = 200 – 300 = –100

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                             156
QoS Configuration
QoS Configuration                                             11 Congestion Management Configuration


                        –    Third round of scheduling
                             Deficit[7][3] = –100 + 400 = 300
                             Deficit[6][3] = –100 + 300 = 200
                             After a packet of 600 bytes in queue 7 and a packet of 500 bytes in
                             queue 6 are sent, the values are as follows:
                             Deficit[7][3] = 300 – 600 = –300
                             Deficit[6][3] = 200 – 500 = –300
                             This process is repeated, leading to queue 7 and queue 6 respectively
                             obtaining 20% and 15% of the bandwidth. As such, you can obtain the
                             required bandwidth by setting proper weights.
                        WDRR scheduling prevents packets in queues with lower priorities from being
                        starved out for a long time in PQ scheduling and uneven bandwidth
                        allocation when the packet lengths of queues are different or vary greatly.
                        However, WDRR scheduling has a disadvantage that services requiring a short
                        delay (such as voice services) cannot be scheduled in a timely manner.

Scheduling Sequence in Different Scheduling Modes
                    All eight interface queues can be configured with one scheduling mode or a
                    combination of different scheduling modes. PQ+WDRR is the most commonly
                    used scheduling mode.
                    If only PQ scheduling is used, the packets in lower priority queues may fail to
                    obtain bandwidth for long periods. If only WDRR scheduling is used, short-delay
                    services such as voice services cannot be scheduled preferentially. PQ+WDRR
                    scheduling offers the advantages of both PQ and WDRR scheduling while
                    offsetting their disadvantages.
                    Eight queues on the device interface are classified into two groups, and you can
                    specify PQ scheduling for certain queues and WDRR scheduling for others.

                    Figure 11-3 PQ+WDRR scheduling




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                          157
QoS Configuration
QoS Configuration                                              11 Congestion Management Configuration


                    In Figure 11-3, the device first schedules traffic in queues 7, 6, and 5 in PQ mode,
                    before then scheduling traffic in queues 4, 3, 2, 1, and 0 in WDRR mode. Queues 4,
                    3, 2, 1, and 0 have their own weights.
                    Important protocol packets or short-delay service packets must be placed in
                    queues using PQ scheduling in order to be scheduled first. Other packets are
                    placed in queues using WDRR scheduling.


11.3 Configuration Precautions for Congestion
Management

11.4 Default Settings for Congestion Management
                    Table 11-1 Default settings for congestion management
                     Parameter                                 Default Setting

                     Congestion management                     Enabled

                     Interface queue scheduling                PQ scheduling

                     Weight in WDRR scheduling mode            1

                     Burst traffic buffering mode              Standard

                     Manually configured buffer                Not configured




11.5 Configuring Congestion Management
Context
                    Congestion management is a queue-based technology. With congestion
                    management configured, if packets are buffered in queues due to congestion on a
                    network, the device determines the sequence at which packets are forwarded
                    according to the defined scheduling policy, thereby preferentially scheduling high-
                    priority services.
                    There are eight queues on each interface, and each queue is able to use a
                    different scheduling mode. During queue scheduling, PQ queues are scheduled
                    first. Multiple PQ queues are scheduled in descending order of priority, with a
                    larger queue index indicating a higher priority. After completing PQ scheduling,
                    the device schedules the queues using WDRR scheduling.

Procedure
                    ●   For the S6780-H, S6750-S, S6730-H-V2, S6750-H, S5732-H-V2, S6750E-S,
                        S6730E-H-V2, S5755E-H, S5755-S and S5755-H series:
                        a.   Enter the system view.
                             system-view


Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                             158
QoS Configuration
QoS Configuration                                                        11 Congestion Management Configuration


                        b.    Enter the interface view.
                              interface { interface-type interface-number | interface-name }

                        c.    Configure a scheduling mode for queues on the interface.
                              qos { pq { start-queue-index [ to end-queue-index ] } &<1-8> | drr { start-queue-index [ to end-
                              queue-index ] } &<1-8> }*

                              By default, the queue scheduling mode of an interface is PQ.
                        d.    (Optional) Configure the queue weight in WDRR scheduling mode.
                              qos queue queue-index drr weight weight-value

                              By default, the queue weight in WDRR scheduling mode is 1.
                    ●   For S5735-L-V2, S5735-S-V2, S5735I-L-V2, S5735I-S-V2, S5735I-H-V2, S5735R-
                        L-V2, S3710-H, S5735R-S-V2, S5735E-L-V2, S5735E-S-V2 series:
                        a.    Enter the system view.
                              system-view

                        b.    Create a global scheduling profile and enter the scheduling profile view.
                              qos schedule-profile profile-name

                              By default, the device predefines a global scheduling profile named
                              default.

                                      NOTE

                                     In addition to the default global scheduling profile, a maximum of 11 global
                                     scheduling profiles can be created on the device. For the default global
                                     scheduling profile, you can only modify its queue scheduling mode and queue
                                     scheduling weight, but cannot delete it.
                        c.    Configure a scheduling mode for queues on an interface.
                              qos { pq { start-queue-index [ to end-queue-index ] } &<1-8> | drr { start-queue-index [ to end-
                              queue-index ] } &<1-8> }*

                              By default, the queue scheduling mode of an interface is PQ.
                        d.    (Optional) Configure the queue weight in WDRR scheduling mode.
                              qos queue queue-index drr weight weight-value

                              By default, the queue weight in WDRR scheduling mode is 1.
                        e.    Exit the scheduling profile view.
                              quit

                        f.    Enter the interface view.
                              interface { interface-type interface-number | interface-name }

                        g.    Apply the scheduling profile.
                              qos schedule-profile profile-name

                              By default, the default scheduling profile is applied to an interface.

                    ----End


Verifying the Configuration
                    Run the display qos queue statistics { slot slotid | interface { interface-type
                    interface-number | interface-name } } command to check queue-based traffic
                    statistics.

Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                            159
QoS Configuration
QoS Configuration                                               11 Congestion Management Configuration




11.6 (Optional) Configuring Buffer Management

11.6.1 Understanding Buffer Management
                    When packets are sent from an interface, the data buffer caches them to prevent
                    packet loss caused by congestion due to traffic bursts. When the data buffer is full,
                    the device stops caching packets. Instead, it discards the packets that do not enter
                    the buffer. If only queue scheduling is used, high-priority services cannot be
                    preferentially forwarded during heavy traffic bursts. Buffer management allows
                    the device to properly allocate the buffer to adjust and improve device
                    performance.
                    The buffer space available to a chip is shared by all interfaces of the chip, and the
                    buffer space available to an interface is shared by all queues on the interface. The
                    buffer can be classified into the chip, interface, and queue levels.

11.6.2 Configuring a Burst Traffic Buffering Mode
Context
                    The data buffer is allocated in static+dynamic mode. By default, each interface is
                    allocated with some static buffer to ensure the basic forwarding capability of
                    queues, while the remaining buffer is used as a dynamic buffer to ensure the
                    capability of forwarding burst traffic in queues. When burst packets enter a queue,
                    this dynamic buffer can be utilized. Table 11-2 lists the buffering modes
                    supported by the device.
                    When multiple interfaces send traffic to an interface, if there is burst traffic with a
                    volume exceeding the maximum allocated buffer, the device discards excess
                    packets. In this case, run the qos burst-mode enhanced command to set the
                    burst traffic buffering mode to enhanced to improve the device's capability to
                    forward burst traffic.

                    Table 11-2 Device buffering modes
                     Buffering Mode               Description                  Application Scenario

                     Standard                     Each interface reserves      Light burst traffic exists
                                                  some static buffer. When     on an interface.
                                                  traffic bursts, each
                                                  interface queue can
                                                  preempt a small part of
                                                  the global dynamic
                                                  buffer.




Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                              160
QoS Configuration
QoS Configuration                                                        11 Congestion Management Configuration


                     Buffering Mode                    Description                             Application Scenario

                     Enhanced                          Each interface reserves                 Heavy burst traffic exists
                                                       some static buffer. When                on an interface.
                                                       traffic bursts, each
                                                       interface queue can
                                                       preempt a large part of
                                                       the global dynamic
                                                       buffer.

                     Extreme                           Each interface reserves                 Extremely heavy burst
                                                       some static buffer. When                traffic exists on an
                                                       traffic bursts, each                    interface.
                                                       interface queue can
                                                       preempt most of the
                                                       global dynamic buffer.




Procedure
                    ●   Configure a burst traffic buffering mode on the device.
                        a.    Enter the system view.
                              system-view

                        b.    Configure a burst traffic buffering mode.

                              For the S5732-H-V2, S6730E-H-V2, S6730-H-V2, S5735R-S-V2, S5735E-S-
                              V2, S5735-S-V2, S5735I-S-V2, S5735I-H-V2, S5735I-L-V2, S5735R-L-V2,
                              S5735E-L-V2 and S5735-L-V2:
                              qos burst-mode enhanced slot slot-id

                              For the S6780-H, S6750E-S, S6750-S, S6750-H, S5755-S S5755E-H and
                              S5755-H:
                              qos burst-mode { enhanced | extreme } slot slot-id

                    ●   Configure a burst traffic buffering mode on an interface.
                        a.    Enter the system view.
                              system-view

                        b.    Enter the interface view.
                              interface { interface-type interface-number | interface-name }

                        c.    Configure a burst traffic buffering mode.

                              For the S5732-H-V2, S6730E-H-V2, S6730-H-V2, S5735R-S-V2, S5735E-S-
                              V2, S5735-S-V2, S5735I-S-V2, S5735I-H-V2, S5735I-L-V2, S5735R-L-V2,
                              S5735E-L-V2 and S5735-L-V2:
                              qos burst-mode enhanced

                              For the S6780-H, S6750E-S, S6750-S, S6750-H, S5755E-H, S5755-H,
                              S5755-S:
                              qos burst-mode { enhanced | extreme }

                    ----End

Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                            161
QoS Configuration
QoS Configuration                                                          11 Congestion Management Configuration


11.6.3 Configuring the Buffer Size
Context
                    If the device's current buffer cannot meet requirements, you can manually divide
                    the buffer.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Enter the interface view.
                    interface { interface-type interface-number | interface-name }

         Step 3 Configure the queue-level service buffer for outbound queues.
                    For the S6750E-S, S6750-S, S5755-S, S5755E-H, S5755-H, S6730E-H-V2, S6730-H-
                    V2, S5732-H-V2, S3710-H, S5735R-S-V2, S5735E-S-V2, S5735-S-V2, S5735I-S-V2,
                    S5735I-L-V2, S5735I-H-V2, S5735R-L-V2, S5735E-L-V2 and S5735-L-V2:
                    qos buffer queue queue-index shared-threshold dynamic dynamic-value

                    For the S6780-H and S6750-H:
                    qos buffer queue queue-index shared-threshold { static bytes-value { bytes | kbytes | mbytes } |
                    dynamic dynamic-value }

                          NOTE

                         Before running this command, ensure that the qos burst-mode command is not configured
                         in the system view or interface view.
                         For S5735-L-V2, S5735-S-V2, S5735I-L-V2, S5735I-S-V2, S5735I-H-V2, S5735R-L-V2, S3710-
                         H, S5735R-S-V2, S5735E-L-V2, S5735E-S-V2 series, the qos buffer queue command can be
                         configured on a maximum of 28 interfaces on the device.

                    ----End

11.6.4 Configuring the Device to Generate an Alarm When the
Queue Buffer Usage Exceeds the Threshold
Context
                    The supported bandwidth on each interface is fixed. Once the traffic rate exceeds
                    the interface bandwidth and the used queue buffer exceeds the configured
                    threshold, the device starts discarding excess traffic. Run the qos buffer overrun
                    threshold command to set the threshold of the queue buffer usage, and run the
                    qos buffer overrun alarm enable command to enable the device to generate an
                    alarm when the queue buffer usage exceeds the threshold.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Set the threshold of the queue buffer usage.

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                          162
QoS Configuration
QoS Configuration                                                 11 Congestion Management Configuration

                    qos buffer overrun threshold percent

         Step 3 Configure the device to generate an alarm when the queue buffer usage exceeds
                the threshold.
                    qos buffer overrun alarm enable

                    ----End

11.6.5 Verifying the Configuration
Procedure
                    ●    Run the display qos buffer-usage command to check the buffer usage.

                    ----End


11.7 (Optional) Configuring Congestion Monitoring
                          NOTE

                        The congestion monitoring function is supported only by the S6750-H, S6780-H, S5755-S
                        series.


11.7.1 Understanding Congestion Monitoring
                    Queue
                    A queue is used to schedule packets and limit the bandwidth of an interface. Each
                    queue has a buffer space. If the device is congested, packets will go into the buffer
                    space of queues for scheduling to determine a packet forwarding sequence.
                    Buffer threshold
                    The buffer threshold measures the buffer usage of a queue, and includes the
                    lower and upper buffer thresholds. On a device enabled with queue-based
                    congestion monitoring, if the buffer usage of a queue reaches the upper buffer
                    threshold and then falls below the lower buffer threshold, the device saves
                    obtained historical congestion monitoring information so that you can check the
                    buffer usage of each queue.

11.7.2 Configuring Congestion Monitoring
Context
                    On an enterprise network, congestion mainly affects network performance,
                    leading to transmission delay and signal loss. Performing congestion monitoring
                    on each interface queue on a network device helps you to learn the buffer usage
                    of queues. It also provides information about packets that cause congestion,
                    guiding planning and adjusting network traffic.

Procedure
         Step 1 Enter the system view.
                    system-view


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                163
QoS Configuration
QoS Configuration                                                          11 Congestion Management Configuration


         Step 2 Enter the interface view.
                    interface { interface-type interface-number | interface-name }

         Step 3 Enable queue-based congestion monitoring.
                    qos [ queue queue-index ] buffer-monitoring enable

                          NOTE

                         When queue-based congestion monitoring is enabled, the lower buffer threshold is 10% of
                         the queue buffer space and the upper buffer threshold is 90% of the queue buffer space by
                         default.

         Step 4 (Optional) Configure the upper and lower buffer thresholds.
                    qos [ queue queue-index ] buffer-monitoring percent low low-percent high high-percent

                    ----End

Verifying the Configuration
                    ●    Run the display qos buffer-monitoring result interface { interface-type
                         interface-number | interface-name } command to check the real-time buffer
                         usage of queues.
                    ●    Run the display qos buffer-monitoring result history interface { interface-
                         type interface-number | interface-name } [ queue queue-index ] [ record-
                         number record-number ] command to check historical congestion monitoring
                         information about queues.



11.8 Maintaining Congestion Management
Context


                        NOTICE

                    Statistics cannot be restored after they are cleared. Exercise caution when clearing
                    the statistics.


Procedure
                    ●    Clear queue-based traffic statistics.
                         reset qos queue statistics { interface { interface-type interface-number | interface-name } | slot slot-
                         id }
                    ●    Clear statistics on the buffer usage.
                         reset qos buffer-usage [ slot slot-id | interface { interface-type interface-number | interface-name } ]

                    ----End


11.9 Example for Configuring Congestion Management
Networking Requirements
                    Host1 and Host2 provide voice, video, and data services. Traffic from these services
                    is transmitted through DeviceB and then DeviceA. To reduce the impact of

Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                                164
QoS Configuration
QoS Configuration                                                     11 Congestion Management Configuration


                    network congestion and guarantee high-priority services that require low latency,
                    set congestion management parameters according to Table 11-3.

                    Table 11-3 Congestion management parameters

                     Service Type        Color                  CoS            Scheduling          Scheduling
                                                                               Mode                Weight

                     Voice               Green                  EF             PQ                  -

                     Video               Yellow                 AF3            WDRR                100

                     Data                Red                    AF1            WDRR                50




                    Figure 11-4 Network diagram of congestion management
                          NOTE

                        In this example, interface 1, interface 2, and interface 3 represent 10GE 1/0/1, 10GE 1/0/2,
                        and 10GE 1/0/3, respectively.




Procedure
         Step 1 Configure VLANs for each interface so that devices can communicate with each
                other at the link layer.
                    # Configure 10GE 1/0/3 on DeviceB as a trunk interface. Add 10GE 1/0/1 to VLAN
                    100, 10GE 1/0/2 to VLAN 200, and 10GE 1/0/3 to VLAN 100 and VLAN 200.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 100 200
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type access
                    [DeviceB-10GE1/0/1] port default vlan 100
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type access
                    [DeviceB-10GE1/0/2] port default vlan 200


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                     165
QoS Configuration
QoS Configuration                                                     11 Congestion Management Configuration

                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] portswitch
                    [DeviceB-10GE1/0/3] port link-type trunk
                    [DeviceB-10GE1/0/3] port trunk allow-pass vlan 100 200
                    [DeviceB-10GE1/0/3] quit

         Step 2 Configure priority mapping to map 802.1p values in voice, video, and data packets
                to different CoS values and colors.
                    # Create DiffServ domain ds1, map 802.1p values 6, 5, and 2 to CoS values EF,
                    AF3, and AF1, respectively, and color the packets green, yellow, and red,
                    respectively.
                    [DeviceB] diffserv domain ds1
                    [DeviceB-dsdomain-ds1] 8021p-inbound 6 phb ef green
                    [DeviceB-dsdomain-ds1] 8021p-inbound 5 phb af3 yellow
                    [DeviceB-dsdomain-ds1] 8021p-inbound 2 phb af1 red
                    [DeviceB-dsdomain-ds1] quit

                    # Bind the DiffServ domain to the inbound interfaces 10GE 1/0/1 and 10GE 1/0/2
                    on DeviceB.
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] trust 8021p outer
                    [DeviceB-10GE1/0/1] trust upstream ds1
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] trust 8021p outer
                    [DeviceB-10GE1/0/2] trust upstream ds1
                    [DeviceB-10GE1/0/2] quit

         Step 3 Configure congestion management. Set scheduling parameters such as the
                scheduling mode and weight to implement differentiated scheduling for queues
                with different priorities.
                    # Configure scheduling parameters for queues with different CoS values on the
                    outbound interface 10GE 1/0/3 of DeviceB.
                    The following uses the configuration of the S6780-H, S6750E-S, S6750-S, S6730E-
                    H-V2, S6730-H-V2, S6750-H, S5732-H-V2, S5755-S, S5755E-H and S5755-H series
                    as an example. For the configuration of other models, see 11.5 Configuring
                    Congestion Management.
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] qos pq 5 to 7 drr 0 to 4
                    [DeviceB-10GE1/0/3] qos queue 3 drr weight 100
                    [DeviceB-10GE1/0/3] qos queue 1 drr weight 50
                    [DeviceB-10GE1/0/3] quit
                    [DeviceB] quit

                    ----End

Verifying the Configuration
                    # Check the configuration of DiffServ domain ds1.
                    <DeviceB> display diffserv domain name ds1
                    Diffserv domain name:ds1
                     8021p-inbound 0 phb be green
                     8021p-inbound 1 phb af1 green
                     8021p-inbound 2 phb af1 red
                     8021p-inbound 3 phb af3 green
                     8021p-inbound 4 phb af4 green
                     8021p-inbound 5 phb af3 yellow


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                             166
QoS Configuration
QoS Configuration                                                                   11 Congestion Management Configuration

                    8021p-inbound 6 phb ef green
                    8021p-inbound 7 phb cs7 green
                    8021p-outbound be green map 0
                    8021p-outbound be yellow map 0
                    8021p-outbound be red map 0
                    ...

                    In the DiffServ domain, 802.1p values 6, 5, and 2 are mapped to CoS values EF,
                    AF3, and AF1, respectively, and packets are colored green, yellow, and red,
                    respectively.
                    # Check the configuration of 10GE 1/0/3. You can see the scheduling parameters
                    of queues with different CoS values.
                    <DeviceB> display qos configuration interface 10ge 1/0/3
                     interface 10GE1/0/3
                     --------------------------------------------------------------------------
                     trust flag        : outer 8021p
                     diffserv domain : default
                     dei enable          : disable
                     port priority      :0
                     phb marking 8021p : disable
                     phb marking dscp : disable
                     phb marking exp : -
                     port wred           :-
                     port lr         : cir = -, cbs = -
                     port car inbound : -
                     port car outbound : -
                     schedule profile : -
                     --------------------------------------------------------------------------
                     queue          shaping        schedule      wred
                               cir      pir
                               cbs        pbs
                     --------------------------------------------------------------------------
                     0            -        - drr        -
                                 -        - weight = 1

                    1           -        -   drr       -
                               -        -    weight = 50

                    2           -        -   drr       -
                               -        -    weight = 1

                    3           -        -   drr       -
                               -        -    weight = 100

                    4           -        -   drr       -
                               -        -    weight = 1

                    5           -        -   pq         -
                               -        -

                    6           -        -   pq         -
                               -        -

                    7           -        -   pq         -
                               -        -

                    --------------------------------------------------------------------------


Configuration Scripts
                    DeviceB
                    #
                    sysname DeviceB
                    #
                    vlan batch 100 200


Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                       167
QoS Configuration
QoS Configuration                                                11 Congestion Management Configuration

                    #
                    diffserv domain ds1
                     8021p-inbound 2 phb af1 red
                     8021p-inbound 5 phb af3 yellow
                     8021p-inbound 6 phb ef green
                    #
                    interface 10GE1/0/1
                     port link-type access
                     port default vlan 100
                     trust upstream ds1
                    #
                    interface 10GE1/0/2
                     port link-type access
                     port default vlan 200
                     trust upstream ds1
                    #
                    interface 10GE1/0/3
                     port link-type trunk
                     port trunk allow-pass vlan 100 200
                     qos drr 0 to 4
                     qos queue 1 drr weight 50
                     qos queue 3 drr weight 100
                    #
                    return



11.10 Example for Configuring Congestion Avoidance
and Congestion Management (PQ+WDRR Scheduling
and WRED Profile)
Networking Requirements
                    DeviceB is connected to DeviceA through interface 1. The 802.1p priorities of
                    voice, video, and data service packets from the Internet are 6, 5, and 2,
                    respectively. Packets of these services can reach users through DeviceA and
                    DeviceB, as shown in Figure 11-5. Because the rate of the inbound interface
                    interface 1 on DeviceB is higher than the rates of outbound interfaces interface 2
                    and interface 3, congestion may occur on the two outbound interfaces.
                    To reduce the impact of network congestion and guarantee high-priority and
                    latency-sensitive services, set congestion avoidance and congestion management
                    parameters according to Table 11-4 and Table 11-5.

                    Table 11-4 Congestion avoidance parameter settings
                     Service Type         Color           Lower           Upper         Drop
                                                          Threshold       Threshold     Probability
                                                          (%)             (%)

                     Voice                Green           80              100           10

                     Video                Yellow          60              80            20

                     Data                 Red             40              60            40




Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                          168
QoS Configuration
QoS Configuration                                                      11 Congestion Management Configuration


                    Table 11-5 Congestion management parameter settings

                     Service Type                      CoS Value                      WDRR

                     Voice                             EF                             0

                     Video                             AF3                            100

                     Data                              AF1                            50




                    Figure 11-5 Network diagram for configuring congestion avoidance and
                    congestion management
                          NOTE

                        In this example, interface 1, interface 2, and interface 3 represent 10GE 1/0/1, 10GE 1/0/2,
                        and 10GE 1/0/3, respectively.




Procedure
         Step 1 Configure VLANs for each interface so that devices can communicate with each
                other at the link layer.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 2 5 6
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type trunk
                    [DeviceB-10GE1/0/1] port trunk allow-pass vlan 2 5 6
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type trunk
                    [DeviceB-10GE1/0/2] port trunk allow-pass vlan 2 5 6


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                     169
QoS Configuration
QoS Configuration                                                         11 Congestion Management Configuration

                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] portswitch
                    [DeviceB-10GE1/0/3] port link-type trunk
                    [DeviceB-10GE1/0/3] port trunk allow-pass vlan 2 5 6
                    [DeviceB-10GE1/0/3] quit

         Step 2 Configure priority mapping to map 802.1p values in voice, video, and data packets
                to different CoS values and colors.
                    # Create DiffServ domain ds1, map 802.1p values 6, 5, and 2 to CoS values EF,
                    AF3, and AF1, respectively, and color the packets green, yellow, and red,
                    respectively.
                    [DeviceB] diffserv domain ds1
                    [DeviceB-dsdomain-ds1] 8021p-inbound 6 phb ef green
                    [DeviceB-dsdomain-ds1] 8021p-inbound 5 phb af3 yellow
                    [DeviceB-dsdomain-ds1] 8021p-inbound 2 phb af1 red
                    [DeviceB-dsdomain-ds1] quit

                    # Bind the DiffServ domain to the inbound interface 10GE 1/0/1 of DeviceB.
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] trust 8021p inner
                    [DeviceB-10GE1/0/1] trust upstream ds1
                    [DeviceB-10GE1/0/1] quit

         Step 3 Configure congestion avoidance.
                    # On DeviceB, create WRED drop profile wred1 and set parameters for green,
                    yellow, and red packets in the WRED drop profile.
                    [DeviceB] drop-profile wred1
                    [DeviceB-drop-wred1] color green low-limit 80 high-limit 100 discard-percentage 10 //Configure the
                    WRED drop profile and set the upper and lower drop thresholds and maximum drop probability for green
                    packets.
                    [DeviceB-drop-wred1] color yellow low-limit 60 high-limit 80 discard-percentage 20 //Configure the
                    device to discard packets with the maximum drop probability of 20% when the percentage of the yellow
                    packet length to the queue length reaches 60%. Configure the device to discard all newly arrived packets
                    when the percentage of the yellow packet length to the queue length reaches 80%.
                    [DeviceB-drop-wred1] color red low-limit 40 high-limit 60 discard-percentage 40
                    [DeviceB-drop-wred1] quit

                    # Apply WRED drop profile wred1 to outbound interfaces 10GE 1/0/2 and 10GE
                    1/0/3 on DeviceB.
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] qos queue 5 wred wred1
                    [DeviceB-10GE1/0/2] qos queue 3 wred wred1
                    [DeviceB-10GE1/0/2] qos queue 1 wred wred1
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] qos queue 5 wred wred1
                    [DeviceB-10GE1/0/3] qos queue 3 wred wred1
                    [DeviceB-10GE1/0/3] qos queue 1 wred wred1
                    [DeviceB-10GE1/0/3] quit

         Step 4 Configure congestion management. Set scheduling parameters such as the
                scheduling mode and weight to implement differentiated scheduling for queues
                with different priorities.
                    # Set scheduling parameters for queues with different CoS values on outbound
                    interfaces 10GE 1/0/2 and 10GE 1/0/3 on DeviceB.
                    The following uses the configuration of the S6780-H, S6750E-S, S6750-S, S6730E-
                    H-V2, S6730-H-V2, S6750-H, S5732-H-V2, S5755-S, S5755E-H and S5755-H series

Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                              170
QoS Configuration
QoS Configuration                                                                 11 Congestion Management Configuration


                    as an example. For the configuration of other models, see 11.5 Configuring
                    Congestion Management.
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] qos pq 5 to 7 drr 0 to 4 //Configure PQ scheduling for queues 5 to 7 and WDRR
                    scheduling for queues 0 to 4.
                    [DeviceB-10GE1/0/2] qos queue 3 drr weight 100 //Set the WDRR scheduling weight of queue 3 to 100.
                    [DeviceB-10GE1/0/2] qos queue 1 drr weight 50 //Set the WDRR scheduling weight of queue 1 to 50.
                    According to the preceding configurations, packets in queue 1 and queue 3 are scheduled based on the
                    ratio of 1:2.
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] qos pq 5 to 7 drr 0 to 4 //Configure PQ scheduling for queues 5 to 7 and WDRR
                    scheduling for queues 0 to 4.
                    [DeviceB-10GE1/0/3] qos queue 3 drr weight 100
                    [DeviceB-10GE1/0/3] qos queue 1 drr weight 50
                    [DeviceB-10GE1/0/3] quit
                    [DeviceB] quit

                    ----End

Verifying the Configuration
                    # Check the configuration of DiffServ domain ds1.
                    <DeviceB> display diffserv domain name ds1
                    Diffserv domain name:ds1
                     8021p-inbound 0 phb be green
                     8021p-inbound 1 phb af1 green
                     8021p-inbound 2 phb af1 red
                     8021p-inbound 3 phb af3 green
                     8021p-inbound 4 phb af4 green
                     8021p-inbound 5 phb af3 yellow
                     8021p-inbound 6 phb ef green
                     8021p-inbound 7 phb cs7 green
                     8021p-outbound be green map 0
                     8021p-outbound be yellow map 0
                     8021p-outbound be red map 0
                     ...

                    In the DiffServ domain, 802.1p values 6, 5, and 2 are mapped to CoS values EF,
                    AF3, and AF1, respectively, and packets are colored green, yellow, and red,
                    respectively.
                    # Check the WRED drop profile configuration.
                    [DeviceB] display drop-profile name wred1
                    Drop-profile[7]: wred1
                    Color     Mode       Low-limit High-limit Unit Discard(%)
                    -----------------------------------------------------------------
                    Green Percentage 80               100         %       10
                    Yellow Percentage 60               80        %       20
                    Red      Percentage 40            60        %       40
                    -----------------------------------------------------------------


Configuration Scripts
                    DeviceB
                    #
                    sysname DeviceB
                    #
                    vlan batch 2 5 to 6
                    #
                    diffserv domain ds1
                     8021p-inbound 2 phb af1 red


Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                       171
QoS Configuration
QoS Configuration                                                          11 Congestion Management Configuration

                     8021p-inbound 5 phb af3 yellow
                     8021p-inbound 6 phb ef green
                    #
                    drop-profile wred1
                     color green low-limit 80 high-limit 100 discard-percentage 10
                     color yellow low-limit 60 high-limit 80 discard-percentage 20
                     color red low-limit 40 high-limit 60 discard-percentage 40
                    #
                    interface 10GE1/0/2
                     port link-type trunk
                     port trunk allow-pass vlan 2 5 to 6
                     qos drr 0 to 4
                     qos queue 1 drr weight 50
                     qos queue 3 drr weight 100
                     qos queue 1 wred wred1
                     qos queue 3 wred wred1
                     qos queue 5 wred wred1
                    #
                    interface 10GE1/0/3
                     port link-type trunk
                     port trunk allow-pass vlan 2 5 to 6
                     qos drr 0 to 4
                     qos queue 1 drr weight 50
                     qos queue 3 drr weight 100
                     qos queue 1 wred wred1
                     qos queue 3 wred wred1
                     qos queue 5 wred wred1
                    #
                    interface 10GE1/0/1
                     port link-type trunk
                     port trunk allow-pass vlan 2 5 to 6
                     trust upstream ds1
                     trust 8021p inner
                    #
                    return



11.11 Example for Configuring Congestion Monitoring

Networking Requirements
                    Servers store information about voice, video, and data services, and users obtain
                    data from the servers through DeviceB. When a large amount of data is
                    transmitted from the servers to hosts, the total rate of inbound interfaces is higher
                    than the rate of the outbound interface. As a result, congestion may occur on the
                    outbound interface of DeviceB.
                    When packet loss occurs on the outbound interface of DeviceB due to queue
                    congestion, the administrator needs to know the buffer usage of each queue and
                    information about the packets that cause congestion to determine subsequent
                    network traffic planning and adjustment.

                    Figure 11-6 Network diagram for configuring congestion monitoring
                          NOTE

                         In this example, interface 1, interface 2, and interface 3 represent 10GE 1/0/1, 10GE 1/0/2,
                         and 10GE 1/0/3, respectively.




Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                     172
QoS Configuration
QoS Configuration                                                      11 Congestion Management Configuration




                          NOTE

                        This example is supported only by the S6750-H and S6780-H.


Procedure
         Step 1 Configure VLANs for each interface so that devices can communicate with each
                other at the link layer.

                    # Configure 10GE 1/0/3 on DeviceB as a trunk interface. Add 10GE 1/0/1 to VLAN
                    100, 10GE 1/0/2 to VLAN 200, and 10GE 1/0/3 to VLAN 100 and VLAN 200.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 100 200
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type access
                    [DeviceB-10GE1/0/1] port default vlan 100
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type access
                    [DeviceB-10GE1/0/2] port default vlan 200
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] portswitch
                    [DeviceB-10GE1/0/3] port link-type trunk
                    [DeviceB-10GE1/0/3] port trunk allow-pass vlan 100 200
                    [DeviceB-10GE1/0/3] quit

         Step 2 Enable congestion monitoring. Use the default lower and upper buffer thresholds.
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] qos buffer-monitoring enable
                    [DeviceB-10GE1/0/3] quit

                    ----End

Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                              173
QoS Configuration
QoS Configuration                                                         11 Congestion Management Configuration


Verifying the Configuration
                    # Check real-time congestion monitoring information about queues on 10GE
                    1/0/3.
                    <DeviceB> display qos buffer-monitoring result interface 10ge 1/0/3
                    Queue Time                       BufferUsage(Bytes)         Percent(%)
                    --------------------------------------------------------------------
                    0 2015-11-04 09:46:27.095                       0            0
                    1 2015-11-04 09:46:27.101                       0            0
                    2 2015-11-04 09:46:27.107                       0            0
                    3 2015-11-04 09:46:27.113                   1598064            100
                    4 2015-11-04 09:46:27.118                       0            0
                    5 2015-11-04 09:46:27.124                       0            0
                    6 2015-11-04 09:46:27.130                       0            0
                    7 2015-11-04 09:46:27.136                       0            0
                    --------------------------------------------------------------------


Configuration Scripts
                    DeviceB
                    #
                    sysname DeviceB
                    #
                    vlan batch 100 200
                    #
                    interface 10GE1/0/1
                     port link-type access
                     port default vlan 100
                    #
                    interface 10GE1/0/2
                     port link-type access
                     port default vlan 200
                    #
                    interface 10GE1/0/3
                     port link-type trunk
                     port trunk allow-pass vlan 100 200
                     qos buffer-monitoring enable
                    #
                    return



11.12 Microburst Detection
                          NOTE

                         Only the S6750E-S, S6750-S, S6730E-H-V2, S6730-H-V2, S5755E-H, S5755-H, and S5732-H-
                         V2 series support this function.


11.12.1 Configuring Microburst Detection
Context
                    Microbursts are short spikes in network traffic, typically lasting several
                    milliseconds. During a microburst, the instantaneous data rate is tens or hundreds
                    times higher than the average rate, and in some cases even exceeds the interface
                    bandwidth. When a microburst exceeds the forwarding capability of a device, the
                    device buffers the burst data for later transmission. If the device does not have
                    sufficient available buffer, the excess data is discarded. This is known as packet
                    loss due to congestion. Such issues cannot be effectively handled using traditional

Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                174
QoS Configuration
QoS Configuration                                                       11 Congestion Management Configuration


                    fault locating methods, which are complex and difficult. With such traditional
                    methods, after packets are lost due to congestion in the outbound direction of an
                    interface, outgoing packets are obtained to analyze the traffic trend and identify
                    the characteristics of burst traffic. Microburst detection can be used to detect
                    instantaneous (millisecond-level) burst traffic in the outbound direction of an
                    interface, helping maintenance personnel determine whether packet loss is caused
                    by microbursts. Microburst detection helps to identify potential congestion risks
                    before congestion occurs and, if congestion does occur, quickly locate abnormal
                    traffic.
                    The following microburst detection modes are supported:
                    ●   Default mode: Packets are sampled at an interval of 5 ms. In this mode,
                        microburst detection can be enabled on multiple interfaces.
                    ●   Enhanced mode: Packets are sampled at an interval of 1 ms. In this mode,
                        microburst detection can be enabled on only one interface.
                    The measurement period of microburst detection is 5 minutes. That is, the key
                    performance indicators of interfaces are collected every 5 minutes and related
                    entries are generated accordingly. The device can store statistics collected up to
                    300 minutes after microburst detection is enabled.
                    The key performance indicators of microburst detection are as follows:
                    ●   Average rate of burst traffic forwarded to an interface from any other
                        interface on the same device
                    ●   Peak rate of burst traffic forwarded to an interface from any other interface
                        on the same device
                    ●   Number of discarded packets on an interface
                    ●   Average buffer usage of an interface
                    ●   Peak buffer usage of an interface
                    ●   Buffer usage of interface queues when the interface buffer usage reaches the
                        peak value in a measurement period

Procedure
                    ●   Configuring microburst detection on the device
                        a.   Enter the system view.
                             system-view
                        b.   Configure the microburst detection mode.
                             qos micro-burst detection [ enhanced ] enable
                    ●   Configuring microburst detection on an interface
                        a.   Enter the system view.
                             system-view
                        b.   Enter the interface view.
                             interface { interface-type interface-number | interface-name }
                        c.   Enable the microburst detection function on the interface.
                             qos micro-burst detection enable


11.12.2 Verifying the Configuration
                    After the microburst detection function is enabled on the device, you can check
                    burst traffic and packet loss information.

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                175
QoS Configuration
QoS Configuration                                                        11 Congestion Management Configuration


                    ●   Check microburst detection statistics on an interface.
                        display qos micro-burst statistics interface { interface-name | interface-type interface-number }

                    ●   Check all interfaces enabled with microburst detection and packet loss
                        information on the interfaces.
                        display qos micro-burst status all

                    ●   Check the peak buffer usage on an interface and the buffer usage of queues
                        on the interface.
                        display qos micro-burst peak-buffer verbose interface { interface-name | interface-type interface-
                        number }




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                                 176
QoS Configuration
QoS Configuration                                                            12 MPLS QoS Configuration




                             12                 MPLS QoS Configuration


                    12.1 Overview of MPLS QoS
                    12.2 Understanding MPLS QoS
                    12.3 Configuration Precautions for MPLS QoS
                    12.4 Default Settings for MPLS QoS
                    12.5 Configuring a DiffServ Mode
                    12.6 Configuring Priority Mapping
                    12.7 Checking the MPLS QoS Configuration
                    12.8 Example for Configuring MPLS QoS


12.1 Overview of MPLS QoS
Definition
                    MPLS QoS is a specific QoS service that works through the DiffServ model on an
                    MPLS network. MPLS QoS provides differentiated services for packets passing
                    through the MPLS network to meet diversified requirements.

Purpose
                    MPLS uses label-based forwarding in place of traditional route-based forwarding.
                    It provides powerful and flexible functions to meet network requirements of
                    various new applications, and supports multiple network protocols such as IPv4
                    and IPv6. Currently, MPLS is widely used for building large networks. Because IP
                    QoS cannot be used on an MPLS network, MPLS QoS is used instead.
                    Traditional IP QoS differentiates service classes based on priorities of IP packets.
                    Similarly, MPLS QoS differentiates data flows based on the EXP value of packets to
                    implement differentiated services. MPLS QoS ensures low delay and low packet
                    loss rate for voice and video data flows, as well as high network usage.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                           177
QoS Configuration
QoS Configuration                                                               12 MPLS QoS Configuration




12.2 Understanding MPLS QoS
                    On an MPLS network, MPLS QoS distinguishes data flows based on the EXP field
                    in the MPLS header and provides differentiated services. This is known as the E-
                    LSP solution. For packets that enter an MPLS network or pass from an MPLS
                    network to an IP network, you need to configure the device to determine whether
                    to trust the original IP precedence, DSCP value, 802.1p value, or EXP value carried
                    in the packets. Based on this, related standards define three MPLS DiffServ tunnel
                    modes: uniform, pipe, and short pipe.


E-LSP Solution
                    IP QoS implements differentiated services by classifying traffic at the network
                    edge. Traffic is classified into multiple priorities or service classes. If packets are
                    marked using the IP precedence in the IP header, they can be classified into a
                    maximum of eight classes; if the DSCP value is used, they can be classified into a
                    maximum of 64 classes. On each node through which packets pass, the IP
                    precedence or DSCP value is checked to determine QoS requirements of the
                    packets.

                    On an MPLS network, however, a device does not check the contents of IP
                    headers. Therefore, traffic classification cannot be implemented based on the IP
                    precedence or DSCP value. Currently, the device uses the E-LSP solution to classify
                    traffic on an MPLS network.

                    The E-LSP solution uses the EXP field in the MPLS header (shown in Figure 12-1)
                    to carry differentiated service information. The device forwards packets based on
                    their labels and determines forwarding behaviors such as scheduling and policing
                    — otherwise known as per-hop behaviors (PHBs) — based on the EXP field. The
                    EXP value can be copied from the DSCP value or IP precedence of IP packets
                    transmitted over a label switched path (LSP), or alternatively be configured.

                    Figure 12-1 MPLS packet encapsulation format




                    In Figure 12-2, when MPLS packets enter LSR2, the packets are classified and EXP
                    values in the packets are mapped to internal class of service (CoS) values and
                    drop priorities of the device. After traffic classification is performed, QoS
                    operations such as traffic shaping, traffic policing, and congestion avoidance are
                    implemented on the MPLS network in the same manner as those on an IP
                    network. When packets leave LSR2, the EXP values carried in the packets are
                    retained.

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                               178
QoS Configuration
QoS Configuration                                                           12 MPLS QoS Configuration


                    Figure 12-2 E-LSP solution




MPLS DiffServ Tunnel Modes
                    The DiffServ model allows intermediate nodes in a DiffServ domain to check and
                    modify the IP precedence, DSCP value, 802.1p value, or EXP value, which are called
                    CoS values. As a result, the CoS values of packets may change during packet
                    transmission on both IP and MPLS networks. Therefore, when a packet enters an
                    MPLS network or passes from an MPLS network to an IP network, the MPLS edge
                    router needs to determine whether to trust the CoS information carried in the IP/
                    MPLS packet based on the configuration. Related standards define three MPLS
                    DiffServ tunnel modes: uniform, pipe, and short pipe.
                    ●   Uniform mode
                        When the CoS value (IP precedence, DSCP value, or 802.1p value) carried in a
                        packet from an IP network can be trusted, you can use the uniform mode. In
                        this mode, the MPLS ingress node copies the CoS value carried in the packet
                        to the EXP field in the outer MPLS label. In this manner, the same QoS
                        guarantee is provided on the MPLS network. When the packet leaves the
                        MPLS network, the egress node maps the EXP value to the IP precedence,
                        DSCP value, or 802.1p value in the IP packet.
                        On the L3VPN network shown in Figure 12-3, PE_1 maps the DSCP value of
                        an IP packet to the outer and inner MPLS EXP values (5); P_1 changes the
                        outer MPLS EXP value to 6; P_2 removes the outer MPLS label and sets the
                        inner MPLS EXP value to the outer MPLS EXP value; PE_2 changes the DSCP
                        value of the packet to 48 based on the mapping.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                         179
QoS Configuration
QoS Configuration                                                             12 MPLS QoS Configuration


                        Figure 12-3 Uniform mode




                        As its name implies, uniform mode ensures the same priority of packets on
                        the IP and MPLS networks. That is, priority mapping is performed for packets
                        entering and leaving an MPLS network. However, if the EXP value of a packet
                        changes on an MPLS network, the PHB for the packet also changes after the
                        packet leaves the MPLS network. As a result, the original CoS value of the
                        packet cannot be retained.
                        On the network shown in Figure 12-4, two sites belong to different branches
                        of the same enterprise. The enterprise network transmits voice, video, and
                        data services, listed in descending order of priority. When traffic of different
                        VPN services enters the MPLS network, the priorities of the three types of
                        services need to be differentiated on the MPLS network to ensure services are
                        correctly prioritized. In addition, different QoS services are provided for the
                        three types of services based on their priorities. In this case, the priorities
                        carried in the original packets can be trusted, and so the uniform mode is
                        used.


                        Figure 12-4 Differentiating priorities of different services in a VPN




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                            180
QoS Configuration
QoS Configuration                                                          12 MPLS QoS Configuration


                    ●   Pipe mode
                        When the CoS value carried in a packet from an IP network cannot be trusted,
                        you can use the pipe mode. In this mode, the CoS value carried in the packet
                        is ignored, and the ingress node of an MPLS network assigns a new value to
                        the EXP field in the outer MPLS label. The packet is scheduled from the
                        ingress node to the egress node according to your requirements. After leaving
                        the MPLS network, the packet is forwarded according to the original CoS
                        value.
                        On the L3VPN network shown in Figure 12-5, PE_1 sets the outer MPLS EXP
                        value to 1 and the inner MPLS EXP value to 2; P_2 removes the outer MPLS
                        label and sets the inner MPLS EXP value to the outer MPLS EXP value; PE_2
                        does not change the IP DSCP value of the original packet, and selects a PHB
                        based on the inner MPLS EXP value.

                        Figure 12-5 Pipe mode




                        In pipe mode, the ingress node does not copy the IP precedence, DSCP value,
                        or 802.1p value to the EXP field when a packet enters an MPLS network.
                        Likewise, when the packet leaves the MPLS network, the egress node does not
                        copy the EXP value to the IP precedence, DSCP value, or 802.1p value. If the
                        EXP value of a packet is changed on an MPLS network, the change is valid
                        only on the MPLS network. After the packet leaves the MPLS network, the
                        original CoS value carried in the packet remains valid.
                    ●   Short pipe mode
                        The short pipe mode is an enhancement of the pipe mode. In short pipe
                        mode, the ingress node of an MPLS network processes packets in the same
                        way as that in pipe mode. Where this mode differs is that the egress node of
                        an MPLS network removes the label before performing QoS scheduling. That
                        is, the egress node of an MPLS network schedules packets based on the
                        original CoS value carried in the packets, and performs QoS scheduling based
                        on your requirements from when the packets enter the MPLS network to the
                        penultimate hop.
                        On the L3VPN network shown in Figure 12-6, PE_1 sets the outer MPLS EXP
                        value to 1 and the inner MPLS EXP value to 2; P_2 removes the outer MPLS

Issue 01 (2025-03-03)          Copyright © Huawei Technologies Co., Ltd.                         181
QoS Configuration
QoS Configuration                                                             12 MPLS QoS Configuration


                        label and sets the inner MPLS EXP value to the outer MPLS EXP value; PE_2
                        does not change the IP DSCP value of the original packet, and selects a PHB
                        based on the IP DSCP value of the packet.

                        Figure 12-6 Short pipe mode




                        In pipe or short pipe mode, you can plan QoS on the internal network based
                        on your requirements, without changing the QoS on the customer edge (CE)
                        side.
                        The difference between pipe mode and short pipe mode lies in how QoS
                        marking is implemented for outgoing traffic from a PE (PE_2 in this example)
                        to a CE. In pipe mode, QoS marking configured by you is used; in short pipe
                        mode, QoS marking configured on the CE side is used.
                        In Figure 12-7, CE_1 and CE_3 belong to VPN_1 and connect to two branches
                        of enterprise A; CE_2 and CE_4 belong to VPN_2 and connect to two branches
                        of enterprise B. When service flows from different VPNs enter the MPLS
                        network, devices on the MPLS network must differentiate priorities of the
                        services to ensure that service flows from enterprise A have higher priorities
                        than those from enterprise B. The devices then provide differentiated QoS
                        services to the service flows based on their priorities. In this case, you need to
                        specify the EXP values of packets in different VPNs when the packets enter
                        the MPLS network. Therefore, the pipe or short pipe mode is used.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                             182
QoS Configuration
QoS Configuration                                                                12 MPLS QoS Configuration


                           Figure 12-7 Differentiating priorities of services in different VPNs




                    Table 12-1 Comparison between three tunnel modes
                     Tu      Whether the             Whether the      QoS               Wheth     Applica
                     nn      Ingress Node            Egress Node      Scheduling        er the    tion
                     el      Trusts the Original     Trusts the       Behavior on       Origin    Scenari
                     Mo      CoS Value Carried       EXP Value or     the Egress        al CoS    o
                     de      in an IP Packet         the Original     Node              Value
                                                     CoS Value                          Carried
                                                     Carried in an                      in an
                                                     IP Packet                          IP
                                                                                        Packet
                                                                                        Chang
                                                                                        es

                     Uni     Trusted. The CoS        The EXP value    Scheduling        Yes       Differen
                     for     value carried in an     is trusted and   based on                    tiating
                     m       IP packet is copied     mapped to        mapping of                  prioritie
                             to the EXP field in     the CoS value    the EXP value               s of
                             the outer MPLS          of an IP                                     differen
                             label.                  packet.                                      t
                                                                                                  services
                                                                                                  in a
                                                                                                  VPN

                     Pip     Untrusted. A new        The original     Scheduling        No        Differen
                     e       value can be            CoS value        based on                    tiating
                             assigned to the EXP     carried in an    mapping of                  prioritie
                             field in the outer      IP packet is     the EXP value               s of
                             MPLS label.             trusted and                                  services
                                                     retained.                                    in
                                                                                                  differen
                                                                                                  t VPNs




Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                            183
QoS Configuration
QoS Configuration                                                            12 MPLS QoS Configuration


                     Tu    Whether the            Whether the      QoS                Wheth     Applica
                     nn    Ingress Node           Egress Node      Scheduling         er the    tion
                     el    Trusts the Original    Trusts the       Behavior on        Origin    Scenari
                     Mo    CoS Value Carried      EXP Value or     the Egress         al CoS    o
                     de    in an IP Packet        the Original     Node               Value
                                                  CoS Value                           Carried
                                                  Carried in an                       in an
                                                  IP Packet                           IP
                                                                                      Packet
                                                                                      Chang
                                                                                      es

                     Sh    Untrusted. A new       The original     Scheduling         No
                     ort   value can be           CoS value        based on
                     pip   assigned to the EXP    carried in an    mapping of
                     e     field in the outer     IP packet is     the original
                           MPLS label.            trusted and      CoS value
                                                  retained.        carried in a
                                                                   packet




12.3 Configuration Precautions for MPLS QoS

12.4 Default Settings for MPLS QoS
                    Table 12-2 describes the default mappings of EXP values to PHBs and colors when
                    packets enter the device.

                    Table 12-2 Default mappings of EXP values to PHBs and colors in the inbound
                    direction of the device
                     EXP Value                     PHB                        Color

                     0                             BE                         Green

                     1                             AF1                        Green

                     2                             AF2                        Green

                     3                             AF3                        Green

                     4                             AF4                        Green

                     5                             EF                         Green

                     6                             CS6                        Green

                     7                             CS7                        Green




Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                           184
QoS Configuration
QoS Configuration                                                          12 MPLS QoS Configuration


                    Table 12-3 describes the default mappings of PHBs and colors to EXP values when
                    packets leave the device.

                    Table 12-3 Default mappings of PHBs and colors to EXP values in the outbound
                    direction of the device
                     PHB                         Color                     EXP Value

                     BE                          Green                     0

                     BE                          Yellow                    0

                     BE                          Red                       0

                     AF1                         Green                     1

                     AF1                         Yellow                    1

                     AF1                         Red                       1

                     AF2                         Green                     2

                     AF2                         Yellow                    2

                     AF2                         Red                       2

                     AF3                         Green                     3

                     AF3                         Yellow                    3

                     AF3                         Red                       3

                     AF4                         Green                     4

                     AF4                         Yellow                    4

                     AF4                         Red                       4

                     EF                          Green                     5

                     EF                          Yellow                    5

                     EF                          Red                       5

                     CS6                         Green                     6

                     CS6                         Yellow                    6

                     CS6                         Red                       6

                     CS7                         Green                     7

                     CS7                         Yellow                    7

                     CS7                         Red                       7




Issue 01 (2025-03-03)          Copyright © Huawei Technologies Co., Ltd.                        185
QoS Configuration
QoS Configuration                                                                      12 MPLS QoS Configuration




12.5 Configuring a DiffServ Mode
Prerequisites
                    Before configuring a DiffServ mode, you have completed the following tasks:

                    ●   Complete basic configurations of the MPLS network.
                    ●   Configure the VPN service.


Context
                    MPLS DiffServ has three tunnel modes: uniform, pipe, and short pipe.

                    ●   In uniform mode, the original priority carried in a packet is trusted when the
                        packet enters an MPLS network. This mode applies to a scenario where
                        priorities of different services in a VPN are differentiated.
                    ●   In pipe or short pipe mode, you can specify the EXP value of the private
                        network label when packets enter the MPLS network. These modes apply to a
                        scenario where priorities of different services in different VPNs are
                        differentiated.

                    If you do not want to change the priority carried in original packets, you are
                    advised to use pipe or short pipe mode. This is because, in uniform mode, the
                    priority carried in original packets may be changed. In addition, the egress node
                    selects PHBs based on EXP values of packets in uniform and pipe modes, and
                    selects PHBs based on DSCP values of packets in short pipe mode.


Procedure
                    ●   Configuring a DiffServ mode for L3VPN
                        a.   Enter the system view.
                             system-view

                        b.   Enter the VPN instance view.
                             ip vpn-instance vpn-instance-name

                        c.   Select a type and enter the corresponding address family view.

                             ▪    Enter the IPv4 address family view.
                                  ipv4-family

                             ▪    Enter the IPv6 address family view.
                                  ipv6-family

                        d.   Configure a DiffServ mode for the VPN instance.
                             diffserv-mode { pipe { mpls-exp exp-value | domain ds-name } | short-pipe [ mpls-exp exp-
                             value ] domain ds-name | uniform [ domain ds-name ] }

                             By default, the DiffServ mode of a VPN instance is uniform and the
                             default domain is used.




Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                          186
QoS Configuration
QoS Configuration                                                                      12 MPLS QoS Configuration


                                   NOTE

                                 ● The DiffServ mode can be used only on PEs.
                                 ● After exp-value is specified in the diffserv-mode { pipe { mpls-exp exp-value
                                   | domain ds-name } | short-pipe [ mpls-exp exp-value ] domain ds-name }
                                   command, the EXP value in the MPLS private network label is set to the
                                   specified value.
                    ●   Configuring a DiffServ mode for L2VPN (VLL networking mode)
                              NOTE

                             Only the S6780-H, S6750-H, S6750E-S, S6750-S, S6730E-H-V2, S6730-H-V2, S5755-S,
                             S5755E-H, S5755-H and S5732-H-V2 series support this configuration.
                        a.   Enter the system view.
                             system-view

                        b.   Enter the AC-side interface view.
                             interface interface-type interface-number

                        c.   Change the interface working mode to Layer 3.
                             undo portswitch

                             Only the S6780-H, S6750-H, S6750E-S, S6750-S, S6730E-H-V2, S6730-H-
                             V2, S5755-S, S5755E-H, S5755-H and S5732-H-V2 series support this step.

                             Determine whether to perform this step based on the current interface
                             working mode.
                        d.   Configure a DiffServ mode for VLL.
                             diffserv-mode { pipe { mpls-exp exp-value | domain ds-name } | short-pipe [ mpls-exp exp-
                             value ] domain ds-name | uniform [ domain ds-name ] }

                             By default, the DiffServ mode supported by VLL is uniform.

                                   NOTE

                                 ● The DiffServ mode can be used only on PEs.
                                 ● After exp-value is specified in the diffserv-mode { pipe { mpls-exp exp-value
                                   | domain ds-name } | short-pipe [ mpls-exp exp-value ] domain ds-name }
                                   command, the EXP value in the MPLS private network label is set to the
                                   specified value.
                    ●   Configuring a DiffServ mode for L2VPN (VPLS networking mode)
                              NOTE

                             Only the S6780-H, S6750-H, S6750E-S, S6750-S, S6730E-H-V2, S6730-H-V2, S5755-S,
                             S5755E-H, S5755-H and S5732-H-V2 series support this configuration.
                        a.   Enter the system view.
                             system-view

                        b.   Enter the VSI view.
                             vsi vsi-name

                        c.   Configure a DiffServ mode for VPLS.
                             diffserv-mode { pipe { mpls-exp exp-value | domain ds-name } | short-pipe [ mpls-exp exp-
                             value ] domain ds-name | uniform [ domain ds-name ] }

                             By default, the DiffServ mode supported by VPLS is uniform.

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                          187
QoS Configuration
QoS Configuration                                                                   12 MPLS QoS Configuration


                                    NOTE

                                   ● The DiffServ mode can be used only on PEs.
                                   ● After exp-value is specified in the diffserv-mode { pipe { mpls-exp exp-value
                                     | domain ds-name } | short-pipe [ mpls-exp exp-value ] domain ds-name }
                                     command, the EXP value in the MPLS private network label is set to the
                                     specified value.

                    ----End


12.6 Configuring Priority Mapping
Context
                    The process of configuring the priority mapping function is as follows:

                    1.   Configure a DiffServ domain to determine mappings between EXP values and
                         internal priorities so that the device can provide differentiated services based
                         on internal priorities.
                    2.   Apply the DiffServ domain to an object so that the mappings in the DiffServ
                         domain take effect.

12.6.1 Configuring a DiffServ Domain

Context
                    When the device is used as an edge node connecting a DiffServ domain and
                    another network, you need to configure mappings between internal and external
                    priorities.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Create a DiffServ domain and enter the DiffServ domain view.
                    diffserv domain ds-domain-name

                    By default, the default domain exists. It can be modified but not deleted. For
                    details about the default priority mapping, see Table 12-2 and Table 12-3 in
                    "Default Settings for MPLS QoS."

                    You can also create a DiffServ domain and define the priority mapping.

         Step 3 Define the priority mapping of the device based on actual requirements.
                    ●    Map EXP values of incoming MPLS packets on an interface to PHBs and
                         colors.
                         mpls-exp-inbound exp-value phb service-class color

                    ●    Map PHBs and colors of outgoing MPLS packets on an interface to EXP
                         values.
                         mpls-exp-outbound service-class color map exp-value

                    ----End

Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                   188
QoS Configuration
QoS Configuration                                                                        12 MPLS QoS Configuration


12.6.2 Applying a DiffServ Domain
Context
                    You can bind a DiffServ domain to the inbound interface of packets so that the
                    device maps priorities of incoming packets to corresponding PHBs and colors
                    according to the mappings defined in the DiffServ domain.
                    You can bind a DiffServ domain to the outbound interface of packets so that the
                    device maps PHBs and colors of outgoing packets to corresponding priorities
                    according to the mappings defined in the DiffServ domain.

                         NOTE

                        ● When MPLS is disabled globally, no default configuration exists and this function does
                          not take effect.
                        ● After MPLS is enabled globally, the default configuration exists. This function takes
                          effect only when it is configured before a public network tunnel is established.
                        ● If this function is configured after a public network tunnel is established and needs to
                          take effect immediately, you must restart the MPLS LDP session.
                        ● If the DiffServ mode of an interface, VSI, or VPN instance or the global DiffServ domain
                          is changed and the configuration needs to take effect immediately, you must re-
                          establish an MPLS LDP session.


Procedure
                    ●   Configure the ingress device to perform priority mapping for packets
                        transmitted over a public network tunnel based on the EXP value.
                        a.   Enter the system view.
                             system-view

                        b.   Configure the ingress device to perform priority mapping for packets
                             transmitted over a public network tunnel based on the EXP value.
                             mpls-qos ingress { use vpn-label-exp | trust upstream { ds-name | default | none } }

                             By default, priority mapping is performed for packets transmitted over a
                             public network tunnel based on the EXP value according to the settings in
                             the default domain.
                    ●   Configure the transit device to perform priority mapping for packets
                        transmitted over a public network tunnel based on the EXP value.
                        a.   Enter the system view.
                             system-view

                        b.   Configure the transit device to perform priority mapping for packets
                             transmitted over a public network tunnel based on the EXP value.
                             mpls-qos transit trust upstream { none | default | ds-name }

                             By default, priority mapping is performed for packets transmitted over a
                             public network tunnel based on the EXP value according to the settings in
                             the default domain.
                    ●   Configure the egress device to perform priority mapping for packets
                        transmitted over a public network tunnel based on the EXP value.
                        a.   Enter the system view.
                             system-view


Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                          189
QoS Configuration
QoS Configuration                                                                       12 MPLS QoS Configuration


                        b.    Configure the egress device to perform priority mapping for packets
                              transmitted over a public network tunnel based on the EXP value.
                              mpls-qos egress trust upstream { none | default | ds-name }

                              By default, priority mapping is performed for packets transmitted over a
                              public network tunnel based on the EXP value according to the settings in
                              the default domain.
                    ----End


12.7 Checking the MPLS QoS Configuration
Procedure
                    ●   Run the display diffserv domain [ brief | name ds-domain-name ] command
                        to check the DiffServ domain configuration.
                    ●   Run the display qos configuration interface [ interface-type interface-
                        number ] command to check all QoS configurations on an interface.
                    ----End


12.8 Example for Configuring MPLS QoS
Networking Requirements
                    Enterprises A and B use BGP/MPLS IP VPN to connect their headquarters and
                    branches. In Figure 12-8, CE1 and CE3 connect to the headquarters and branch of
                    enterprise A, and CE2 and CE4 connect to the headquarters and branch of
                    enterprise B. Enterprise A uses VPN vpna, and enterprise B uses VPN vpnb.
                    Enterprise A has a high service level and requires better QoS guarantee.

                    Figure 12-8 Network diagram of MPLS QoS
                         NOTE

                        In this example, interface1, interface2, and interface3 represent 10GE1/0/1, 10GE1/0/2, and
                        10GE1/0/3, respectively.




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                                    190
QoS Configuration
QoS Configuration                                                              12 MPLS QoS Configuration




Configuration Roadmap
                    Configure MPLS QoS on PE1 and PE2, enable the pipe mode for VPNs vpna and
                    vpnb, and set the MPLS EXP values of VPNs vpna and vpnb to 4 and 3,
                    respectively, to provide better QoS guarantee for services of enterprise A.


Procedure
         Step 1 Configure OSPF on the MPLS backbone network so that the PEs and P on the
                backbone network can communicate with each other.

                    # Configure PE1.
                    <HUAWEI> system-view
                    [HUAWEI] sysname PE1
                    [PE1] interface loopback 1
                    [PE1-LoopBack1] ip address 1.1.1.9 32
                    [PE1-LoopBack1] quit
                    [PE1] interface 10ge 1/0/3
                    [PE1-10GE1/0/3] undo portswitch
                    [PE1-10GE1/0/3] ip address 172.16.1.1 24
                    [PE1-10GE1/0/3] quit
                    [PE1] ospf 1
                    [PE1-ospf-1] area 0
                    [PE1-ospf-1-area-0.0.0.0] network 172.16.1.0 0.0.0.255
                    [PE1-ospf-1-area-0.0.0.0] network 1.1.1.9 0.0.0.0
                    [PE1-ospf-1-area-0.0.0.0] quit
                    [PE1-ospf-1] quit

                    # Configure P.

Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                        191
QoS Configuration
QoS Configuration                                                              12 MPLS QoS Configuration

                    <HUAWEI> system-view
                    [HUAWEI] sysname P
                    [P] interface loopback 1
                    [P-LoopBack1] ip address 2.2.2.9 32
                    [P-LoopBack1] quit
                    [P] interface 10ge 1/0/1
                    [P-10GE1/0/1] undo portswitch
                    [P-10GE1/0/1] ip address 172.16.1.2 24
                    [P-10GE1/0/1] quit
                    [P] interface 10ge 1/0/2
                    [P-10GE1/0/2] undo portswitch
                    [P-10GE1/0/2] ip address 172.17.1.1 24
                    [P-10GE1/0/2] quit
                    [P] ospf
                    [P-ospf-1] area 0
                    [P-ospf-1-area-0.0.0.0] network 172.16.1.0 0.0.0.255
                    [P-ospf-1-area-0.0.0.0] network 172.17.1.0 0.0.0.255
                    [P-ospf-1-area-0.0.0.0] network 2.2.2.9 0.0.0.0
                    [P-ospf-1-area-0.0.0.0] quit
                    [P-ospf-1] quit

                    # Configure PE2.
                    <HUAWEI> system-view
                    [HUAWEI] sysname PE2
                    [PE2] interface loopback 1
                    [PE2-LoopBack1] ip address 3.3.3.9 32
                    [PE2-LoopBack1] quit
                    [PE2] interface 10ge 1/0/3
                    [PE2-10GE1/0/3] undo portswitch
                    [PE2-10GE1/0/3] ip address 172.17.1.2 24
                    [PE2-10GE1/0/3] quit
                    [PE2] ospf
                    [PE2-ospf-1] area 0
                    [PE2-ospf-1-area-0.0.0.0] network 172.17.1.0 0.0.0.255
                    [PE2-ospf-1-area-0.0.0.0] network 3.3.3.9 0.0.0.0
                    [PE2-ospf-1-area-0.0.0.0] quit
                    [PE2-ospf-1] quit

                    After the configuration is complete, OSPF neighbor relationships can be
                    established between PE1, P, and PE2. Run the display ip routing-table command.
                    The command output shows that the PEs have learned the routes to each other's
                    Loopback1.
         Step 2 Configure basic MPLS functions and MPLS LDP on the MPLS backbone network to
                establish LDP LSPs.
                    # Configure PE1.
                    [PE1] mpls lsr-id 1.1.1.9
                    [PE1] mpls
                    [PE1-mpls] quit
                    [PE1] mpls ldp
                    [PE1-mpls-ldp] quit
                    [PE1] interface 10ge 1/0/3
                    [PE1-10GE1/0/3] mpls
                    [PE1-10GE1/0/3] mpls ldp
                    [PE1-10GE1/0/3] quit

                    # Configure P.
                    [P] mpls lsr-id 2.2.2.9
                    [P] mpls
                    [P-mpls] quit
                    [P] mpls ldp
                    [P-mpls-ldp] quit
                    [P] interface 10ge 1/0/1
                    [P-10GE1/0/1] mpls
                    [P-10GE1/0/1] mpls ldp


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                        192
QoS Configuration
QoS Configuration                                                                     12 MPLS QoS Configuration

                    [P-10GE1/0/1] quit
                    [P] interface 10ge 1/0/2
                    [P-10GE1/0/2] mpls
                    [P-10GE1/0/2] mpls ldp
                    [P-10GE1/0/2] quit

                    # Configure PE2.
                    [PE2] mpls lsr-id 3.3.3.9
                    [PE2] mpls
                    [PE2-mpls] quit
                    [PE2] mpls ldp
                    [PE2-mpls-ldp] quit
                    [PE2] interface 10ge 1/0/3
                    [PE2-10GE1/0/3] mpls
                    [PE2-10GE1/0/3] mpls ldp
                    [PE2-10GE1/0/3] quit

                    After the configuration is complete, LDP sessions can be established between PE1
                    and P and between P and PE2. Run the display mpls ldp session command. The
                    command output shows that the Status field displays Operational.
                    The following example uses the command output on PE1.
                    [PE1] display mpls ldp session
                      LDP Session(s) in Public Network Codes: LAM(Label Advertisement Mode), SsnAge Unit(DDDD:HH:MM)
                    A '*' before a session means the session is being deleted.
                     ------------------------------------------------------------------------------
                     PeerID            Status      LAM SsnRole SsnAge             KASent/Rcv
                     ------------------------------------------------------------------------------
                     2.2.2.9:0        Operational DU Active 0000:00:01 6/6
                     ------------------------------------------------------------------------------
                     TOTAL: 1 session(s) Found.

         Step 3 Configure VPN instances on PEs and connect CEs to PEs.
                    # Configure PE1.
                    [PE1] ip vpn-instance vpna
                    [PE1-vpn-instance-vpna] ipv4-family
                    [PE1-vpn-instance-vpna-af-ipv4] route-distinguisher 100:1
                    [PE1-vpn-instance-vpna-af-ipv4] vpn-target 111:1 both
                    [PE1-vpn-instance-vpna-af-ipv4] quit
                    [PE1-vpn-instance-vpna] quit
                    [PE1] ip vpn-instance vpnb
                    [PE1-vpn-instance-vpnb] ipv4-family
                    [PE1-vpn-instance-vpnb-af-ipv4] route-distinguisher 100:2
                    [PE1-vpn-instance-vpnb-af-ipv4] vpn-target 222:2 both
                    [PE1-vpn-instance-vpnb-af-ipv4] quit
                    [PE1-vpn-instance-vpnb] quit
                    [PE1] interface 10ge 1/0/1
                    [PE1-10GE1/0/1] undo portswitch
                    [PE1-10GE1/0/1] ip binding vpn-instance vpna
                    [PE1-10GE1/0/1] ip address 10.1.1.2 24
                    [PE1-10GE1/0/1] quit
                    [PE1] interface 10ge 1/0/2
                    [PE1-10GE1/0/2] undo portswitch
                    [PE1-10GE1/0/2] ip binding vpn-instance vpnb
                    [PE1-10GE1/0/2] ip address 10.2.1.2 24
                    [PE1-10GE1/0/2] quit

                    # Configure PE2.
                    [PE2] ip vpn-instance vpna
                    [PE2-vpn-instance-vpna] ipv4-family
                    [PE2-vpn-instance-vpna-af-ipv4] route-distinguisher 200:1
                    [PE2-vpn-instance-vpna-af-ipv4] vpn-target 111:1 both
                    [PE2-vpn-instance-vpna-af-ipv4] quit
                    [PE2-vpn-instance-vpna] quit


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                      193
QoS Configuration
QoS Configuration                                                                    12 MPLS QoS Configuration

                    [PE2] ip vpn-instance vpnb
                    [PE2-vpn-instance-vpnb] ipv4-family
                    [PE2-vpn-instance-vpnb-af-ipv4] route-distinguisher 200:2
                    [PE2-vpn-instance-vpnb-af-ipv4] vpn-target 222:2 both
                    [PE2-vpn-instance-vpnb-af-ipv4] quit
                    [PE2-vpn-instance-vpnb] quit
                    [PE2] interface 10ge 1/0/1
                    [PE2-10GE1/0/1] undo portswitch
                    [PE2-10GE1/0/1] ip binding vpn-instance vpna
                    [PE2-10GE1/0/1] ip address 10.3.1.2 24
                    [PE2-10GE1/0/1] quit
                    [PE2] interface 10ge 1/0/2
                    [PE2-10GE1/0/2] undo portswitch
                    [PE2-10GE1/0/2] ip binding vpn-instance vpnb
                    [PE2-10GE1/0/2] ip address 10.4.1.2 24
                    [PE2-10GE1/0/2] quit

                    # Configure IP addresses for interfaces on CEs according to Figure 12-8. The
                    configuration procedure is not mentioned here.

                    After the configuration is complete, each PE can ping the connected CE
                    successfully.

                          NOTE

                        If multiple interfaces on a PE are bound to the same VPN, you need to specify the source IP
                        address when running the ping -vpn-instance command to ping the CE connected to the
                        remote PE. That is, you need to specify the -a source-ip-address parameter when running
                        the ping -vpn-instance vpn-instance-name -a source-ip-address command. Otherwise, the
                        ping may fail.

                    The following example uses the command output on PE1 to show that PE1 can
                    ping CE1.
                    [PE1] ping -vpn-instance vpna 10.1.1.1
                     PING 10.1.1.1: 56 data bytes, press CTRL_C to break
                       Reply from 10.1.1.1: bytes=56 Sequence=1 ttl=255 time=5 ms
                       Reply from 10.1.1.1: bytes=56 Sequence=2 ttl=255 time=3 ms
                       Reply from 10.1.1.1: bytes=56 Sequence=3 ttl=255 time=3 ms
                       Reply from 10.1.1.1: bytes=56 Sequence=4 ttl=255 time=3 ms
                       Reply from 10.1.1.1: bytes=56 Sequence=5 ttl=255 time=16 ms

                     --- 10.1.1.1 ping statistics ---
                       5 packet(s) transmitted
                       5 packet(s) received
                       0.00% packet loss
                       round-trip min/avg/max = 3/6/16 ms

         Step 4 Establish an MP-IBGP peer relationship between PEs.

                    # Configure PE1.
                    [PE1] bgp 100
                    [PE1-bgp] peer 3.3.3.9 as-number 100
                    [PE1-bgp] peer 3.3.3.9 connect-interface loopback 1
                    [PE1-bgp] ipv4-family vpnv4
                    [PE1-bgp-af-vpnv4] peer 3.3.3.9 enable
                    [PE1-bgp-af-vpnv4] quit
                    [PE1-bgp] quit

                    # Configure PE2.
                    [PE2] bgp 100
                    [PE2-bgp] peer 1.1.1.9 as-number 100
                    [PE2-bgp] peer 1.1.1.9 connect-interface loopback 1
                    [PE2-bgp] ipv4-family vpnv4
                    [PE2-bgp-af-vpnv4] peer 1.1.1.9 enable


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                   194
QoS Configuration
QoS Configuration                                                                                    12 MPLS QoS Configuration

                    [PE2-bgp-af-vpnv4] quit
                    [PE2-bgp] quit

                    After the configuration is complete, run the display bgp peer command on PEs.
                    The command output shows that a BGP peer relationship has been established
                    between PEs and is in Established state.
                    [PE1] display bgp peer

                    BGP local router ID : 1.1.1.9
                    Local AS number : 100
                    Total number of peers : 1                Peers in established state : 1

                     Peer         V    AS MsgRcvd MsgSent OutQ Up/Down                  State            PrefRcv

                     3.3.3.9      4 100       12     6       0 00:02:21       Established       0

         Step 5 Establish EBGP peer relationships between PEs and CEs and import VPN routes.
                    # Configure CE1.
                    <HUAWEI> system-view
                    [HUAWEI] sysname CE1
                    [CE1] bgp 65410
                    [CE1-bgp] peer 10.1.1.2 as-number 100
                    [CE1-bgp] import-route direct

                    The configurations of CE2, CE3, and CE4 are similar to the configuration of CE1,
                    and are not mentioned here.
                    # Configure PE1.
                    [PE1] bgp 100
                    [PE1-bgp] ipv4-family vpn-instance vpna
                    [PE1-bgp-vpna] peer 10.1.1.1 as-number 65410
                    [PE1-bgp-vpna] import-route direct
                    [PE1-bgp-vpna] quit
                    [PE1-bgp] ipv4-family vpn-instance vpnb
                    [PE1-bgp-vpnb] peer 10.2.1.1 as-number 65420
                    [PE1-bgp-vpnb] import-route direct
                    [PE1-bgp-vpnb] quit
                    [PE1-bgp] quit

                    The configuration of PE2 is similar to that of PE1, and is not mentioned here.
                    After the configuration is complete, run the display bgp vpnv4 vpn-instance peer
                    command on PEs. The command output shows that BGP peer relationships have
                    been established between PEs and CEs and are in Established state.
                    The following example uses the command output on PE1 to show that a peer
                    relationship has been established between PE1 and CE1.
                    [PE1] display bgp vpnv4 vpn-instance vpna peer

                    BGP local router ID : 1.1.1.9
                    Local AS number : 100
                    Total number of peers : 1                Peers in established state : 1

                     Peer         V    AS MsgRcvd MsgSent OutQ Up/Down                  State       PrefRcv

                     10.1.1.1     4 65410       11       9     0 00:07:25     Established       1

         Step 6 Configure a DiffServ mode.
                    # Configure PE1.
                    [PE1] mpls-qos ingress use vpn-label-exp
                    [PE1] ip vpn-instance vpna


Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                           195
QoS Configuration
QoS Configuration                                                                   12 MPLS QoS Configuration

                    [PE1-vpn-instance-vpna] ipv4-family
                    [PE1-vpn-instance-vpna-af-ipv4] diffserv-mode pipe mpls-exp 4
                    [PE1-vpn-instance-vpna-af-ipv4] quit
                    [PE1-vpn-instance-vpna] quit
                    [PE1] ip vpn-instance vpnb
                    [PE1-vpn-instance-vpnb] ipv4-family
                    [PE1-vpn-instance-vpnb-af-ipv4] diffserv-mode pipe mpls-exp 3
                    [PE1-vpn-instance-vpnb-af-ipv4] quit
                    [PE1-vpn-instance-vpnb] quit

                    # Configure PE2.
                    [PE2] mpls-qos ingress use vpn-label-exp
                    [PE2] ip vpn-instance vpna
                    [PE1-vpn-instance-vpna] ipv4-family
                    [PE2-vpn-instance-vpna-af-ipv4] diffserv-mode pipe mpls-exp 4
                    [PE2-vpn-instance-vpna-af-ipv4] quit
                    [PE1-vpn-instance-vpna] quit
                    [PE2] ip vpn-instance vpnb
                    [PE1-vpn-instance-vpnb] ipv4-family
                    [PE2-vpn-instance-vpnb-af-ipv4] diffserv-mode pipe mpls-exp 3
                    [PE2-vpn-instance-vpnb-af-ipv4] quit
                    [PE1-vpn-instance-vpnb] quit

                    ----End

Configuration Scripts
                    ●    PE1
                         #
                         sysname PE1
                         #
                         mpls-qos ingress use vpn-label-exp
                         #
                         ip vpn-instance vpna
                          ipv4-family
                           route-distinguisher 100:1
                           vpn-target 111:1 export-extcommunity
                           vpn-target 111:1 import-extcommunity
                           diffserv-mode pipe mpls-exp 4
                         #
                         ip vpn-instance vpnb
                          ipv4-family
                           route-distinguisher 100:2
                           vpn-target 222:2 export-extcommunity
                           vpn-target 222:2 import-extcommunity
                           diffserv-mode pipe mpls-exp 3
                         #
                         mpls lsr-id 1.1.1.9
                         mpls
                         #
                         mpls ldp
                         #
                         interface 10ge1/0/1
                          undo portswitch
                          ip binding vpn-instance vpna
                          ip address 10.1.1.2 255.255.255.0
                         #
                         interface 10ge1/0/2
                          undo portswitch
                          ip binding vpn-instance vpnb
                          ip address 10.2.1.2 255.255.255.0
                         #
                         interface 10ge1/0/3
                          undo portswitch
                          ip address 172.16.1.1 255.255.255.0
                          mpls
                          mpls ldp


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                             196
QoS Configuration
QoS Configuration                                                             12 MPLS QoS Configuration

                        #
                        interface LoopBack1
                         ip address 1.1.1.9 255.255.255.255
                        #
                        bgp 100
                         peer 3.3.3.9 as-number 100
                         peer 3.3.3.9 connect-interface LoopBack1
                         #
                         ipv4-family unicast
                          undo synchronization
                          peer 3.3.3.9 enable
                         #
                         ipv4-family vpnv4
                          policy vpn-target
                          peer 3.3.3.9 enable
                         #
                         ipv4-family vpn-instance vpna
                          import-route direct
                          peer 10.1.1.1 as-number 65410
                         #
                         ipv4-family vpn-instance vpnb
                          import-route direct
                          peer 10.2.1.1 as-number 65420
                        #
                        ospf 1
                         area 0.0.0.0
                          network 1.1.1.9 0.0.0.0
                          network 172.16.1.0 0.0.0.255
                        #
                        return
                    ●   P
                        #
                        sysname P
                        #
                        mpls lsr-id 2.2.2.9
                        mpls
                        #
                        mpls ldp
                        #
                        interface 10ge1/0/1
                         undo portswitch
                         ip address 172.16.1.2 255.255.255.0
                         mpls
                         mpls ldp
                        #
                        interface 10ge1/0/2
                         undo portswitch
                         ip address 172.17.1.1 255.255.255.0
                         mpls
                         mpls ldp
                        #
                        interface LoopBack1
                         ip address 2.2.2.9 255.255.255.255
                        #
                        ospf 1
                         area 0.0.0.0
                          network 2.2.2.9 0.0.0.0
                          network 172.16.1.0 0.0.0.255
                          network 172.17.1.0 0.0.0.255
                        #
                        return
                    ●   PE2
                        #
                        sysname PE2
                        #
                        mpls-qos ingress use vpn-label-exp
                        #
                        ip vpn-instance vpna


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                        197
QoS Configuration
QoS Configuration                                                             12 MPLS QoS Configuration

                         ipv4-family
                          route-distinguisher 200:1
                          vpn-target 111:1 export-extcommunity
                          vpn-target 111:1 import-extcommunity
                          diffserv-mode pipe mpls-exp 4
                        #
                        ip vpn-instance vpnb
                         ipv4-family
                          route-distinguisher 200:2
                          vpn-target 222:2 export-extcommunity
                          vpn-target 222:2 import-extcommunity
                          diffserv-mode pipe mpls-exp 3
                        #
                        mpls lsr-id 3.3.3.9
                        mpls
                        #
                        mpls ldp
                        #
                        interface 10ge1/0/1
                         undo portswitch
                         ip binding vpn-instance vpna
                         ip address 10.3.1.2 255.255.255.0
                        #
                        interface 10ge1/0/2
                         undo portswitch
                         ip binding vpn-instance vpnb
                         ip address 10.4.1.2 255.255.255.0
                        #
                        interface 10ge1/0/3
                         undo portswitch
                         ip address 172.17.1.2 255.255.255.0
                         mpls
                         mpls ldp
                        #
                        interface LoopBack1
                         ip address 3.3.3.9 255.255.255.255
                        #
                        bgp 100
                         peer 1.1.1.9 as-number 100
                         peer 1.1.1.9 connect-interface LoopBack1
                         #
                         ipv4-family unicast
                          undo synchronization
                          peer 1.1.1.9 enable
                         #
                         ipv4-family vpnv4
                          policy vpn-target
                          peer 1.1.1.9 enable
                         #
                         ipv4-family vpn-instance vpna
                          import-route direct
                          peer 10.3.1.1 as-number 65430
                         #
                         ipv4-family vpn-instance vpnb
                          import-route direct
                          peer 10.4.1.1 as-number 65440
                        #
                        ospf 1
                         area 0.0.0.0
                          network 3.3.3.9 0.0.0.0
                          network 172.17.1.0 0.0.0.255
                        #
                        return
                    ●   CE1 at the headquarters egress of enterprise A
                        #
                        sysname CE1
                        #
                        interface 10ge1/0/1
                         undo portswitch


Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                        198
QoS Configuration
QoS Configuration                                                             12 MPLS QoS Configuration

                         ip address 10.1.1.1 255.255.255.0
                        #
                        bgp 65410
                         peer 10.1.1.2 as-number 100
                         #
                         ipv4-family unicast
                          undo synchronization
                          import-route direct
                          peer 10.1.1.2 enable
                        #
                        return

                    ●   CE2 at the headquarters egress of enterprise B
                        #
                        sysname CE2
                        #
                        interface 10ge1/0/1
                         undo portswitch
                         ip address 10.2.1.1 255.255.255.0
                        #
                        bgp 65420
                         peer 10.2.1.2 as-number 100
                         #
                         ipv4-family unicast
                          undo synchronization
                          import-route direct
                          peer 10.2.1.2 enable
                        #
                        return

                    ●   CE3 at the branch egress of enterprise A
                        #
                        sysname CE3
                        #
                        interface 10ge1/0/1
                         undo portswitch
                         ip address 10.3.1.1 255.255.255.0
                        #
                        bgp 65430
                         peer 10.3.1.2 as-number 100
                         #
                         ipv4-family unicast
                          undo synchronization
                          import-route direct
                          peer 10.3.1.2 enable
                        #
                        return

                    ●   CE4 at the branch egress of enterprise B
                        #
                        sysname CE4
                        #
                        vlan batch 50
                        #
                        interface 10ge1/0/1
                         undo portswitch
                         ip address 10.4.1.1 255.255.255.0
                        #
                        bgp 65440
                         peer 10.4.1.2 as-number 100
                         #
                         ipv4-family unicast
                          undo synchronization
                          import-route direct
                          peer 10.4.1.2 enable
                        #
                        return




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                        199
QoS Configuration
QoS Configuration                                                  13 Experience Assurance Configuration




  13                    Experience Assurance Configuration


                    13.1 Overview of Experience Assurance
                    13.2 Understanding Experience Assurance
                    13.3 Configuration Precautions for Experience Assurance
                    13.4 Default Settings for Experience Assurance
                    13.5 Configuring Experience Assurance


13.1 Overview of Experience Assurance
Definition
                    Experience assurance uses application identification technology to quickly and
                    accurately identify the application to which packets belong and manage
                    application traffic based on the identification results.

Purpose
                    As network and multimedia technologies develop rapidly, network applications are
                    becoming more and more diversified, audio and video conferences are widely used
                    for collaboration at work, and audio and video conference traffic on the network
                    increases rapidly. Other types of services preempt bandwidth for audio and video
                    services, affecting users' audio and video experience severely. When both key and
                    non-key application traffic is transmitted together, non-key services occupy a lot of
                    bandwidth resources, packets of key services are discarded, the delay and jitter are
                    uncontrollable, and the service quality cannot be guaranteed. If the traffic of key
                    and non-key applications cannot be accurately identified, differentiated policy
                    control cannot be implemented. After experience assurance is configured, the
                    device can identify applications and manage application traffic based on the
                    identification results.


13.2 Understanding Experience Assurance

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                            200
QoS Configuration
QoS Configuration                                                  13 Experience Assurance Configuration


                    Experience assurance uses application identification technology to identify traffic
                    and uses packet re-marking technology to set the priority field of specified
                    application packets. In this way, these packets can be scheduled or forwarded
                    based on the re-marked priority, making it possible to manage application traffic
                    and ensure users' Internet access experience.
                    For example, packets identified as belonging to a video conference application
                    that has high requirements on delay and service quality can be re-marked with a
                    higher priority. As such, these packets have a higher scheduling weight in
                    subsequent forwarding, thereby ensuring video conference experience. Similarly,
                    for applications that do not have special requirements on delay or service quality,
                    the priority of their packets can be reduced so that sufficient network resources
                    are provided for applications that do have such requirements.

Implementation Process
                    Figure 13-1 shows the implementation process of experience assurance.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                            201
QoS Configuration
QoS Configuration                                                   13 Experience Assurance Configuration


                    Figure 13-1 Implementation process of experience assurance




                    The following phases are involved in the implementation process of experience
                    assurance:
                    1.   Updating a service awareness signature database (SA-SDB)
                         Because different applications have different signatures, obtaining the unique
                         signatures of application protocols is the key for devices to accurately identify
                         various applications. Huawei periodically analyzes the signatures of common
                         applications on networks and stores the signatures in an SA-SDB. After being
                         loaded on a device, an SA-SDB exists as a predefined application. It can be

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                             202
QoS Configuration
QoS Configuration                                                  13 Experience Assurance Configuration


                         referenced by other signatures as a matching condition, but cannot be
                         modified or deleted.
                         Huawei Security Center (isecurity.huawei.com) periodically releases the
                         latest SA-SDB to keep up with the ever-changing application software. With
                         an up-to-date SA-SDB, devices are fully capable of identifying applications in
                         traffic. It is recommended that the SA-SDB be updated once a week.
                    2.   Configuring MQC-based experience assurance
                         The experience assurance function re-marks packets of identified application
                         traffic. Currently, this function supports the following packet re-marking
                         modes:
                         –    Re-marking 802.1p values of VLAN packets
                         –    Re-marking DSCP values of IP packets


13.3 Configuration Precautions for Experience
Assurance
Licensing Requirements
                    Experience Assurance is not under license control.


Hardware Requirements

                    Table 13-1 Hardware requirements

                     Series                         Models

                     S5755E-H                       S5755E-H24HB2Y2CZ

                     S6730E-H-V2                    S6730E-H6FX4Y2CZ-V2

                     S6750E-S                       S6750E-S24T16X8Y2CZ, S6750E-S16X10Y2CZ

                     S5735E-S-V2                    S5735E-S48HS4XE-V2, S5735E-S24HS4XE-V2

                     S5755-H                        S5755-H48T4Y2CZ-B, S5755-H48UTM4X4Y2C-T,
                                                    S5755-H24UM4Y2CZ, S5755-H24UM4Y2CZ-T,
                                                    S5755-H24UTM4X4Y2C-T, S5755-H48UM4Y2CZ,
                                                    S5755-H48UM4Y2CZ-T, S5755-H48UTM4X4Y2C,
                                                    S5755-H24HB2Y2CZ, S5755-H24N4Y-A, S5755-
                                                    H24UN4Y2CZ, S5755-H24P4Y2CZ, S5755-
                                                    H24T4Y2CZ, S5755-H24U4Y2CZ, S5755-H48N4Y-
                                                    A, S5755-H48UN4Y2CZ, S5755-H48P4Y2CZ,
                                                    S5755-H48T4Y2CZ, S5755-H48U4Y2CZ, S5755-
                                                    H24UTM4X4Y2C

                     S5732-H-V2                     S5732-H48UM4Y2CZ-TV2, S5732-H24S4X6QZ-V2,
                                                    S5732-H44S4X6QZ-V2, S5732-H24UM4Y2CZ-V2,
                                                    S5732-H48UM4Y2CZ-V2, S5732-H24S4X6QZ-TV2,
                                                    S5732-H44S4X6QZ-TV2, S5732-H24UM4Y2CZ-TV2


Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                          203
QoS Configuration
QoS Configuration                                                   13 Experience Assurance Configuration


                     Series                         Models

                     S5735E-L-V2                    S5735E-L16LP2X-QA-V2, S5735E-L48LP4XE-A-V2,
                                                    S5735E-L48S4XE-A-V2, S5735E-L24T4XE-A-V2,
                                                    S5735E-L48T4XE-A-V2, S5735E-L24ST4XE-A-V2,
                                                    S5735E-L24P4S-A-V2, S5735E-L16LP2UM2X-QA-
                                                    V2, S5735E-L8T4X-QA-V2, S5735E-L8P4X-QA-V2,
                                                    S5735E-L24P4XE-A-V2, S5735E-L48LP4S-A-V2

                     S6730-H-V2                     S6730-H6FX4Y2CZ-V2, S6730-H48Y6C-TV2,
                                                    S6730-H28X6CZ-TV2, S6730-H24X6C-TV2, S6730-
                                                    H48X6C-TV2, S6730-H48X6CZ-TV2, S6730-
                                                    H48X6CZ-V2, S6730-H24X6C-V2, S6730-H48X6C-
                                                    V2, S6730-H48Y6C-V2, S6730-H28X6CZ-V2

                     S5755-S                        S5755-S48U8YZ, S5755-S24T8J8YZ, S5755-
                                                    S48U8Y, S5755-S24U8Y, S5755-S48T8YZ, S5755-
                                                    S24P8J8YZ, S5755-S48P8Y, S5755-S24T8Y, S5755-
                                                    S24U8J8YZ, S5755-S48T8Y, S5755-S24P8Y, S5755-
                                                    S48P8YZ

                     S6750-S                        S6750-S16X10Y2CZ, S6750-S24T16X8Y2CZ,
                                                    S6750-S16X8YZ




Query Tools
                    To obtain hardware specifications and component mappings, visit Hardware
                    Center. To obtain the key specifications and full software specifications, visit
                    Specification Query.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                              204
QoS Configuration
QoS Configuration                                                  13 Experience Assurance Configuration


Feature Requirements

                    Table 13-2 Feature requirements
                     Featu    Feature Requirements                               Series      Models
                     re

                     Forw     When the device is deployed on a network, if       S5755E-H    S5755E-
                     ardin    the SA function is not enabled on interfaces       S6730E-     H24HB2Y
                     g and    according to the recommended configurations,       H-V2        2CZ
                     Flow     the traffic statistics collection direction                    S6730E-
                     Table    displayed on the analyzer Campus Insight is        S6750E-S
                                                                                             H6FX4Y2
                     Servic   opposite to the actual direction.                  S5735E-S-   CZ-V2
                     es       For fixed access devices, you are advised to run   V2
                                                                                             S6750E-
                              the sa enable command on downlink                  S5755-H     S24T16X8
                              interfaces.                                        S5732-H-    Y2CZ,
                                                                                 V2          S6750E-
                                                                                 S6730-H-    S16X10Y2
                                                                                 V2          CZ
                                                                                 S5735E-L-   S5735E-
                                                                                 V2          S48HS4XE
                                                                                             -V2,
                                                                                 S5755-S     S5735E-
                                                                                 S6750-S     S24HS4XE
                                                                                             -V2
                                                                                             S5755-
                                                                                             H48T4Y2
                                                                                             CZ-B,
                                                                                             S5755-
                                                                                             H48UTM
                                                                                             4X4Y2C-T,
                                                                                             S5755-
                                                                                             H24UM4
                                                                                             Y2CZ,
                                                                                             S5755-
                                                                                             H24UM4
                                                                                             Y2CZ-T,
                                                                                             S5755-
                                                                                             H24UTM
                                                                                             4X4Y2C-T,
                                                                                             S5755-
                                                                                             H48UM4
                                                                                             Y2CZ,
                                                                                             S5755-
                                                                                             H48UM4
                                                                                             Y2CZ-T,
                                                                                             S5755-
                                                                                             H48UTM
                                                                                             4X4Y2C,
                                                                                             S5755-
                                                                                             H24HB2Y
                                                                                             2CZ,


Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                             205
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5755-
                                                                                         H24N4Y-
                                                                                         A, S5755-
                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           206
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           207
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           208
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5755-
                                                                                         S48P8YZ
                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           209
QoS Configuration
QoS Configuration                                                  13 Experience Assurance Configuration


                    Featu    Feature Requirements                               Series      Models
                    re

                    Forw     For application identification service flow        S5755E-H    S5755E-
                    ardin    tables, each flow in one direction of a            S6730E-     H24HB2Y
                    g and    bidirectional flow occupies a flow table.          H-V2        2CZ
                    Flow                                                                    S6730E-
                    Table                                                       S6750E-S
                                                                                            H6FX4Y2
                    Servic                                                      S5735E-S-   CZ-V2
                    es                                                          V2
                                                                                            S6750E-
                                                                                S5755-H     S24T16X8
                                                                                S5732-H-    Y2CZ,
                                                                                V2          S6750E-
                                                                                S6730-H-    S16X10Y2
                                                                                V2          CZ
                                                                                S5735E-L-   S5735E-
                                                                                V2          S48HS4XE
                                                                                            -V2,
                                                                                S5755-S     S5735E-
                                                                                S6750-S     S24HS4XE
                                                                                            -V2
                                                                                            S5755-
                                                                                            H48T4Y2
                                                                                            CZ-B,
                                                                                            S5755-
                                                                                            H48UTM
                                                                                            4X4Y2C-T,
                                                                                            S5755-
                                                                                            H24UM4
                                                                                            Y2CZ,
                                                                                            S5755-
                                                                                            H24UM4
                                                                                            Y2CZ-T,
                                                                                            S5755-
                                                                                            H24UTM
                                                                                            4X4Y2C-T,
                                                                                            S5755-
                                                                                            H48UM4
                                                                                            Y2CZ,
                                                                                            S5755-
                                                                                            H48UM4
                                                                                            Y2CZ-T,
                                                                                            S5755-
                                                                                            H48UTM
                                                                                            4X4Y2C,
                                                                                            S5755-
                                                                                            H24HB2Y
                                                                                            2CZ,
                                                                                            S5755-
                                                                                            H24N4Y-
                                                                                            A, S5755-



Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                            210
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           211
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           212
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           213
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           214
QoS Configuration
QoS Configuration                                                13 Experience Assurance Configuration


                    Featu    Feature Requirements                             Series      Models
                    re

                    Forw     When a user-defined application is identified    S5755E-H    S5755E-
                    ardin    based on the port number or IP address, you      S6730E-     H24HB2Y
                    g and    are advised to configure the source and          H-V2        2CZ
                    Flow     destination port numbers and addresses in two                S6730E-
                    Table    rules to prevent the application from being      S6750E-S
                                                                                          H6FX4Y2
                    Servic   identified as other protocols.                   S5735E-S-   CZ-V2
                    es                                                        V2
                                                                                          S6750E-
                                                                              S5755-H     S24T16X8
                                                                              S5732-H-    Y2CZ,
                                                                              V2          S6750E-
                                                                              S6730-H-    S16X10Y2
                                                                              V2          CZ
                                                                              S5735E-L-   S5735E-
                                                                              V2          S48HS4XE
                                                                                          -V2,
                                                                              S5755-S     S5735E-
                                                                              S6750-S     S24HS4XE
                                                                                          -V2
                                                                                          S5755-
                                                                                          H48T4Y2
                                                                                          CZ-B,
                                                                                          S5755-
                                                                                          H48UTM
                                                                                          4X4Y2C-T,
                                                                                          S5755-
                                                                                          H24UM4
                                                                                          Y2CZ,
                                                                                          S5755-
                                                                                          H24UM4
                                                                                          Y2CZ-T,
                                                                                          S5755-
                                                                                          H24UTM
                                                                                          4X4Y2C-T,
                                                                                          S5755-
                                                                                          H48UM4
                                                                                          Y2CZ,
                                                                                          S5755-
                                                                                          H48UM4
                                                                                          Y2CZ-T,
                                                                                          S5755-
                                                                                          H48UTM
                                                                                          4X4Y2C,
                                                                                          S5755-
                                                                                          H24HB2Y
                                                                                          2CZ,
                                                                                          S5755-
                                                                                          H24N4Y-
                                                                                          A, S5755-



Issue 01 (2025-03-03)          Copyright © Huawei Technologies Co., Ltd.                           215
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           216
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           217
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           218
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           219
QoS Configuration
QoS Configuration                                                13 Experience Assurance Configuration


                    Featu    Feature Requirements                             Series      Models
                    re

                    Forw     1. When application identification is            S5755E-H    S5755E-
                    ardin    implemented in an access scenario, the sa        S6730E-     H24HB2Y
                    g and    enable command needs to be configured on         H-V2        2CZ
                    Flow     the downlink interface.                                      S6730E-
                    Table                                                     S6750E-S
                             2. When the forward and return paths of                      H6FX4Y2
                    Servic   packets from a connected downstream device       S5735E-S-   CZ-V2
                    es       are not on the same access interface, the        V2
                                                                                          S6750E-
                             application type cannot be accurately            S5755-H     S24T16X8
                             identified.                                      S5732-H-    Y2CZ,
                                                                              V2          S6750E-
                                                                              S6730-H-    S16X10Y2
                                                                              V2          CZ
                                                                              S5735E-L-   S5735E-
                                                                              V2          S48HS4XE
                                                                                          -V2,
                                                                              S5755-S     S5735E-
                                                                              S6750-S     S24HS4XE
                                                                                          -V2
                                                                                          S5755-
                                                                                          H48T4Y2
                                                                                          CZ-B,
                                                                                          S5755-
                                                                                          H48UTM
                                                                                          4X4Y2C-T,
                                                                                          S5755-
                                                                                          H24UM4
                                                                                          Y2CZ,
                                                                                          S5755-
                                                                                          H24UM4
                                                                                          Y2CZ-T,
                                                                                          S5755-
                                                                                          H24UTM
                                                                                          4X4Y2C-T,
                                                                                          S5755-
                                                                                          H48UM4
                                                                                          Y2CZ,
                                                                                          S5755-
                                                                                          H48UM4
                                                                                          Y2CZ-T,
                                                                                          S5755-
                                                                                          H48UTM
                                                                                          4X4Y2C,
                                                                                          S5755-
                                                                                          H24HB2Y
                                                                                          2CZ,
                                                                                          S5755-
                                                                                          H24N4Y-
                                                                                          A, S5755-



Issue 01 (2025-03-03)          Copyright © Huawei Technologies Co., Ltd.                           220
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           221
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           222
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           223
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           224
QoS Configuration
QoS Configuration                                                 13 Experience Assurance Configuration


                    Featu    Feature Requirements                                Series     Models
                    re

                    Forw     When application identification is enabled          S6730E-    S6730E-
                    ardin    using the sa enable command on an interface,        H-V2       H6FX4Y2
                    g and    flow table statistics can be generated only         S5732-H-   CZ-V2
                    Flow     after NetStream is configured on the interface.     V2         S5732-
                    Table    For details about the recommended NetStream                    H48UM4
                    Servic   configuration, see the attachment.                  S6730-H-
                                                                                 V2         Y2CZ-TV2,
                    es       1. Traffic statistics collection depends on the                S5732-
                             NetStream sampling mechanism. There may be                     H24S4X6
                             a certain error in a short period of time, but                 QZ-V2,
                             the traffic statistics displayed in a long period              S5732-
                             of time are normal. 2. NetStream traffic                       H44S4X6
                             statistics reported before the SAC flow table is               QZ-V2,
                             generated are not recorded in traffic statistics               S5732-
                             in the SAC flow table.                                         H24UM4
                                                                                            Y2CZ-V2,
                                                                                            S5732-
                                                                                            H48UM4
                                                                                            Y2CZ-V2,
                                                                                            S5732-
                                                                                            H24S4X6
                                                                                            QZ-TV2,
                                                                                            S5732-
                                                                                            H44S4X6
                                                                                            QZ-TV2,
                                                                                            S5732-
                                                                                            H24UM4
                                                                                            Y2CZ-TV2
                                                                                            S6730-
                                                                                            H6FX4Y2
                                                                                            CZ-V2,
                                                                                            S6730-
                                                                                            H48Y6C-
                                                                                            TV2,
                                                                                            S6730-
                                                                                            H28X6CZ-
                                                                                            TV2,
                                                                                            S6730-
                                                                                            H24X6C-
                                                                                            TV2,
                                                                                            S6730-
                                                                                            H48X6C-
                                                                                            TV2,
                                                                                            S6730-
                                                                                            H48X6CZ-
                                                                                            TV2,
                                                                                            S6730-
                                                                                            H48X6CZ-
                                                                                            V2,
                                                                                            S6730-


Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                            225
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           226
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                    Appli The experience assurance function and the M-       S5755E-H    S5755E-
                    catio   LAG function are mutually exclusive.             S6730E-     H24HB2Y
                    n                                                        H-V2        2CZ
                    Identif                                                              S6730E-
                    icatio                                                   S6750E-S
                                                                                         H6FX4Y2
                    n                                                        S5735E-S-   CZ-V2
                    Policy                                                   V2
                                                                                         S6750E-
                                                                             S5755-H     S24T16X8
                                                                             S5732-H-    Y2CZ,
                                                                             V2          S6750E-
                                                                             S6730-H-    S16X10Y2
                                                                             V2          CZ
                                                                             S5735E-L-   S5735E-
                                                                             V2          S48HS4XE
                                                                                         -V2,
                                                                             S5755-S     S5735E-
                                                                             S6750-S     S24HS4XE
                                                                                         -V2
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ-B,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C,
                                                                                         S5755-
                                                                                         H24HB2Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24N4Y-
                                                                                         A, S5755-



Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           227
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           228
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           229
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           230
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           231
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                    Appli The SA function configuration and the CSS or       S5755E-H    S5755E-
                    catio   stack interface configuration are mutually       S6730E-     H24HB2Y
                    n       exclusive on an interface.                       H-V2        2CZ
                    Identif                                                              S6730E-
                    icatio                                                   S6750E-S
                                                                                         H6FX4Y2
                    n                                                        S5735E-S-   CZ-V2
                    Policy                                                   V2
                                                                                         S6750E-
                                                                             S5755-H     S24T16X8
                                                                             S5732-H-    Y2CZ,
                                                                             V2          S6750E-
                                                                             S6730-H-    S16X10Y2
                                                                             V2          CZ
                                                                             S5735E-L-   S5735E-
                                                                             V2          S48HS4XE
                                                                                         -V2,
                                                                             S5755-S     S5735E-
                                                                             S6750-S     S24HS4XE
                                                                                         -V2
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ-B,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C,
                                                                                         S5755-
                                                                                         H24HB2Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24N4Y-
                                                                                         A, S5755-



Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           232
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           233
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           234
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           235
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           236
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                    Appli Application identification is not performed in     S5755E-H    S5755E-
                    catio   MPLS scenarios.                                  S6730E-     H24HB2Y
                    n                                                        H-V2        2CZ
                    Identif                                                              S6730E-
                    icatio                                                   S6750E-S
                                                                                         H6FX4Y2
                    n                                                        S5735E-S-   CZ-V2
                    Policy                                                   V2
                                                                                         S6750E-
                                                                             S5755-H     S24T16X8
                                                                             S5732-H-    Y2CZ,
                                                                             V2          S6750E-
                                                                             S6730-H-    S16X10Y2
                                                                             V2          CZ
                                                                             S5735E-L-   S5735E-
                                                                             V2          S48HS4XE
                                                                                         -V2,
                                                                             S5755-S     S5735E-
                                                                             S6750-S     S24HS4XE
                                                                                         -V2
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ-B,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C,
                                                                                         S5755-
                                                                                         H24HB2Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24N4Y-
                                                                                         A, S5755-



Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           237
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           238
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           239
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           240
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           241
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                    Appli Application identification is not performed in     S5755E-H    S5755E-
                    catio   SRv6 scenarios.                                  S6730E-     H24HB2Y
                    n                                                        H-V2        2CZ
                    Identif                                                              S6730E-
                    icatio                                                   S6750E-S
                                                                                         H6FX4Y2
                    n                                                        S5735E-S-   CZ-V2
                    Policy                                                   V2
                                                                                         S6750E-
                                                                             S5755-H     S24T16X8
                                                                             S5732-H-    Y2CZ,
                                                                             V2          S6750E-
                                                                             S6730-H-    S16X10Y2
                                                                             V2          CZ
                                                                             S5735E-L-   S5735E-
                                                                             V2          S48HS4XE
                                                                                         -V2,
                                                                             S5755-S     S5735E-
                                                                             S6750-S     S24HS4XE
                                                                                         -V2
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ-B,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C,
                                                                                         S5755-
                                                                                         H24HB2Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24N4Y-
                                                                                         A, S5755-



Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           242
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           243
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           244
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           245
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           246
QoS Configuration
QoS Configuration                                                 13 Experience Assurance Configuration


                    Featu     Feature Requirements                              Series      Models
                    re

                    Appli     Application identification is not performed in    S5755E-H    S5755E-
                    catio     wireless CAPWAP DTLS encryption scenarios         S6730E-     H24HB2Y
                    n         (in such scenarios, inner tunnel packets cannot   H-V2        2CZ
                    Identif   be parsed and 6-tuple information cannot be                   S6730E-
                    icatio    obtained, and therefore identification cannot     S6750E-S
                                                                                            H6FX4Y2
                    n         be performed). Generally, CAPWAP control          S5735E-S-   CZ-V2
                    Policy    packets are encrypted, and data packets are       V2
                              not encrypted.                                                S6750E-
                                                                                S5755-H     S24T16X8
                                                                                S5732-H-    Y2CZ,
                                                                                V2          S6750E-
                                                                                S6730-H-    S16X10Y2
                                                                                V2          CZ
                                                                                S5735E-L-   S5735E-
                                                                                V2          S48HS4XE
                                                                                            -V2,
                                                                                S5755-S     S5735E-
                                                                                S6750-S     S24HS4XE
                                                                                            -V2
                                                                                            S5755-
                                                                                            H48T4Y2
                                                                                            CZ-B,
                                                                                            S5755-
                                                                                            H48UTM
                                                                                            4X4Y2C-T,
                                                                                            S5755-
                                                                                            H24UM4
                                                                                            Y2CZ,
                                                                                            S5755-
                                                                                            H24UM4
                                                                                            Y2CZ-T,
                                                                                            S5755-
                                                                                            H24UTM
                                                                                            4X4Y2C-T,
                                                                                            S5755-
                                                                                            H48UM4
                                                                                            Y2CZ,
                                                                                            S5755-
                                                                                            H48UM4
                                                                                            Y2CZ-T,
                                                                                            S5755-
                                                                                            H48UTM
                                                                                            4X4Y2C,
                                                                                            S5755-
                                                                                            H24HB2Y
                                                                                            2CZ,
                                                                                            S5755-
                                                                                            H24N4Y-
                                                                                            A, S5755-



Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                            247
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           248
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           249
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           250
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           251
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                    Appli Application identification is not performed in     S5755E-H    S5755E-
                    catio   GRE tunnel scenarios.                            S6730E-     H24HB2Y
                    n                                                        H-V2        2CZ
                    Identif                                                              S6730E-
                    icatio                                                   S6750E-S
                                                                                         H6FX4Y2
                    n                                                        S5735E-S-   CZ-V2
                    Policy                                                   V2
                                                                                         S6750E-
                                                                             S5755-H     S24T16X8
                                                                             S5732-H-    Y2CZ,
                                                                             V2          S6750E-
                                                                             S6730-H-    S16X10Y2
                                                                             V2          CZ
                                                                             S5735E-L-   S5735E-
                                                                             V2          S48HS4XE
                                                                                         -V2,
                                                                             S5755-S     S5735E-
                                                                             S6750-S     S24HS4XE
                                                                                         -V2
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ-B,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C,
                                                                                         S5755-
                                                                                         H24HB2Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24N4Y-
                                                                                         A, S5755-



Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           252
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           253
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           254
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           255
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           256
QoS Configuration
QoS Configuration                                                  13 Experience Assurance Configuration


                    Featu     Feature Requirements                               Series      Models
                    re

                    Appli     After applications are identified for the first    S5755E-H    S5755E-
                    catio     time based on application traffic, some            S6730E-     H24HB2Y
                    n         applications require second-time identification.   H-V2        2CZ
                    Identif   The second-time identification result may                      S6730E-
                    icatio    change.                                            S6750E-S
                                                                                             H6FX4Y2
                    n         In this scenario, you need to configure the        S5735E-S-   CZ-V2
                    Policy    policies corresponding to the application          V2
                                                                                             S6750E-
                              results before and after second-time               S5755-H     S24T16X8
                              identification to ensure that experience           S5732-H-    Y2CZ,
                              assurance always takes effect.                     V2          S6750E-
                                                                                 S6730-H-    S16X10Y2
                                                                                 V2          CZ
                                                                                 S5735E-L-   S5735E-
                                                                                 V2          S48HS4XE
                                                                                             -V2,
                                                                                 S5755-S     S5735E-
                                                                                 S6750-S     S24HS4XE
                                                                                             -V2
                                                                                             S5755-
                                                                                             H48T4Y2
                                                                                             CZ-B,
                                                                                             S5755-
                                                                                             H48UTM
                                                                                             4X4Y2C-T,
                                                                                             S5755-
                                                                                             H24UM4
                                                                                             Y2CZ,
                                                                                             S5755-
                                                                                             H24UM4
                                                                                             Y2CZ-T,
                                                                                             S5755-
                                                                                             H24UTM
                                                                                             4X4Y2C-T,
                                                                                             S5755-
                                                                                             H48UM4
                                                                                             Y2CZ,
                                                                                             S5755-
                                                                                             H48UM4
                                                                                             Y2CZ-T,
                                                                                             S5755-
                                                                                             H48UTM
                                                                                             4X4Y2C,
                                                                                             S5755-
                                                                                             H24HB2Y
                                                                                             2CZ,
                                                                                             S5755-
                                                                                             H24N4Y-
                                                                                             A, S5755-



Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                            257
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           258
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           259
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           260
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           261
QoS Configuration
QoS Configuration                                                13 Experience Assurance Configuration


                    Featu   Feature Requirements                              Series      Models
                    re

                    Appli Application identification is performed only for    S5755E-H    S5755E-
                    catio   TCP/UDP packets.                                  S6730E-     H24HB2Y
                    n                                                         H-V2        2CZ
                    Identif                                                               S6730E-
                    icatio                                                    S6750E-S
                                                                                          H6FX4Y2
                    n                                                         S5735E-S-   CZ-V2
                    Policy                                                    V2
                                                                                          S6750E-
                                                                              S5755-H     S24T16X8
                                                                              S5732-H-    Y2CZ,
                                                                              V2          S6750E-
                                                                              S6730-H-    S16X10Y2
                                                                              V2          CZ
                                                                              S5735E-L-   S5735E-
                                                                              V2          S48HS4XE
                                                                                          -V2,
                                                                              S5755-S     S5735E-
                                                                              S6750-S     S24HS4XE
                                                                                          -V2
                                                                                          S5755-
                                                                                          H48T4Y2
                                                                                          CZ-B,
                                                                                          S5755-
                                                                                          H48UTM
                                                                                          4X4Y2C-T,
                                                                                          S5755-
                                                                                          H24UM4
                                                                                          Y2CZ,
                                                                                          S5755-
                                                                                          H24UM4
                                                                                          Y2CZ-T,
                                                                                          S5755-
                                                                                          H24UTM
                                                                                          4X4Y2C-T,
                                                                                          S5755-
                                                                                          H48UM4
                                                                                          Y2CZ,
                                                                                          S5755-
                                                                                          H48UM4
                                                                                          Y2CZ-T,
                                                                                          S5755-
                                                                                          H48UTM
                                                                                          4X4Y2C,
                                                                                          S5755-
                                                                                          H24HB2Y
                                                                                          2CZ,
                                                                                          S5755-
                                                                                          H24N4Y-
                                                                                          A, S5755-



Issue 01 (2025-03-03)          Copyright © Huawei Technologies Co., Ltd.                           262
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           263
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           264
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           265
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           266
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                    Appli Application identification is not performed on     S5755E-H    S5755E-
                    catio   fragmented packets.                              S6730E-     H24HB2Y
                    n                                                        H-V2        2CZ
                    Identif                                                              S6730E-
                    icatio                                                   S6750E-S
                                                                                         H6FX4Y2
                    n                                                        S5735E-S-   CZ-V2
                    Policy                                                   V2
                                                                                         S6750E-
                                                                             S5755-H     S24T16X8
                                                                             S5732-H-    Y2CZ,
                                                                             V2          S6750E-
                                                                             S6730-H-    S16X10Y2
                                                                             V2          CZ
                                                                             S5735E-L-   S5735E-
                                                                             V2          S48HS4XE
                                                                                         -V2,
                                                                             S5755-S     S5735E-
                                                                             S6750-S     S24HS4XE
                                                                                         -V2
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ-B,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H24UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C-T,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ,
                                                                                         S5755-
                                                                                         H48UM4
                                                                                         Y2CZ-T,
                                                                                         S5755-
                                                                                         H48UTM
                                                                                         4X4Y2C,
                                                                                         S5755-
                                                                                         H24HB2Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24N4Y-
                                                                                         A, S5755-



Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           267
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           268
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           269
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           270
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S6750-
                                                                                         S16X10Y2
                                                                                         CZ,
                                                                                         S6750-
                                                                                         S24T16X8
                                                                                         Y2CZ,
                                                                                         S6750-
                                                                                         S16X8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           271
QoS Configuration
QoS Configuration                                                  13 Experience Assurance Configuration


                    Featu     Feature Requirements                               Series      Models
                    re

                    Appli     In a scenario where packets pass through a         S5755E-H    S5755E-
                    catio     tunnel, outer packets are used for application     S6730E-     H24HB2Y
                    n         identification, and the correct application type   H-V2        2CZ
                    Identif   cannot be identified.                                          S6730E-
                    icatio                                                       S6750E-S
                                                                                             H6FX4Y2
                    n                                                            S5735E-S-   CZ-V2
                    Policy                                                       V2
                                                                                             S6750E-
                                                                                 S5755-H     S24T16X8
                                                                                 S5732-H-    Y2CZ,
                                                                                 V2          S6750E-
                                                                                 S6730-H-    S16X10Y2
                                                                                 V2          CZ
                                                                                 S5735E-L-   S5735E-
                                                                                 V2          S48HS4XE
                                                                                             -V2,
                                                                                 S5755-S     S5735E-
                                                                                 S6750-S     S24HS4XE
                                                                                             -V2
                                                                                             S5755-
                                                                                             H48T4Y2
                                                                                             CZ-B,
                                                                                             S5755-
                                                                                             H48UTM
                                                                                             4X4Y2C-T,
                                                                                             S5755-
                                                                                             H24UM4
                                                                                             Y2CZ,
                                                                                             S5755-
                                                                                             H24UM4
                                                                                             Y2CZ-T,
                                                                                             S5755-
                                                                                             H24UTM
                                                                                             4X4Y2C-T,
                                                                                             S5755-
                                                                                             H48UM4
                                                                                             Y2CZ,
                                                                                             S5755-
                                                                                             H48UM4
                                                                                             Y2CZ-T,
                                                                                             S5755-
                                                                                             H48UTM
                                                                                             4X4Y2C,
                                                                                             S5755-
                                                                                             H24HB2Y
                                                                                             2CZ,
                                                                                             S5755-
                                                                                             H24N4Y-
                                                                                             A, S5755-



Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                            272
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         H24UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H24P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48N4Y-
                                                                                         A, S5755-
                                                                                         H48UN4Y
                                                                                         2CZ,
                                                                                         S5755-
                                                                                         H48P4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48T4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H48U4Y2
                                                                                         CZ,
                                                                                         S5755-
                                                                                         H24UTM
                                                                                         4X4Y2C
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-TV2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-V2,
                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H48UM4
                                                                                         Y2CZ-V2,
                                                                                         S5732-
                                                                                         H24S4X6
                                                                                         QZ-TV2,
                                                                                         S5732-
                                                                                         H44S4X6
                                                                                         QZ-TV2,


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           273
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         S5732-
                                                                                         H24UM4
                                                                                         Y2CZ-TV2
                                                                                         S6730-
                                                                                         H6FX4Y2
                                                                                         CZ-V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         TV2,
                                                                                         S6730-
                                                                                         H48X6CZ-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H24X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48X6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H48Y6C-
                                                                                         V2,
                                                                                         S6730-
                                                                                         H28X6CZ-
                                                                                         V2
                                                                                         S5735E-
                                                                                         L16LP2X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L48LP4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L48S4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24T4XE-
                                                                                         A-V2,
                                                                                         S5735E-


Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           274
QoS Configuration
QoS Configuration                                               13 Experience Assurance Configuration


                    Featu   Feature Requirements                             Series      Models
                    re

                                                                                         L48T4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L24ST4XE
                                                                                         -A-V2,
                                                                                         S5735E-
                                                                                         L24P4S-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L16LP2U
                                                                                         M2X-QA-
                                                                                         V2,
                                                                                         S5735E-
                                                                                         L8T4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L8P4X-
                                                                                         QA-V2,
                                                                                         S5735E-
                                                                                         L24P4XE-
                                                                                         A-V2,
                                                                                         S5735E-
                                                                                         L48LP4S-
                                                                                         A-V2
                                                                                         S5755-
                                                                                         S48U8YZ,
                                                                                         S5755-
                                                                                         S24T8J8Y
                                                                                         Z, S5755-
                                                                                         S48U8Y,
                                                                                         S5755-
                                                                                         S24U8Y,
                                                                                         S5755-
                                                                                         S48T8YZ,
                                                                                         S5755-
                                                                                         S24P8J8Y
                                                                                         Z, S5755-
                                                                                         S48P8Y,
                                                                                         S5755-
                                                                                         S24T8Y,
                                                                                         S5755-
                                                                                         S24U8J8Y
                                                                                         Z, S5755-
                                                                                         S48T8Y,
                                                                                         S5755-
                                                                                         S24P8Y,
                                                                                         S5755-
                                                                                         S48P8YZ




Issue 01 (2025-03-03)         Copyright © Huawei Technologies Co., Ltd.                           275
QoS Configuration
QoS Configuration                                                  13 Experience Assurance Configuration


                     Featu   Feature Requirements                                Series      Models
                     re

                                                                                             S6750-
                                                                                             S16X10Y2
                                                                                             CZ,
                                                                                             S6750-
                                                                                             S24T16X8
                                                                                             Y2CZ,
                                                                                             S6750-
                                                                                             S16X8YZ




13.4 Default Settings for Experience Assurance
                    Table 13-3 describes the default settings for experience assurance.

                    Table 13-3 Default settings for experience assurance

                     Parameter                                 Default Setting

                     Status of the SA function on an           Disabled
                     interface

                     Application identification statistics     Disabled
                     collection

                     Aging period of the application           By default, the aging period of the
                     identification flow table                 application identification flow table is
                                                               300 seconds.




13.5 Configuring Experience Assurance

13.5.1 Updating a Service Awareness Signature Database

Prerequisites
                    ●   For information about how to prepare for the update of a signature database,
                        see "Preparations for Signature Database Update" in CLI Configuration Guide
                        > System Management > Signature Database Update Configuration.
                    ●   Before updating the service awareness signature database using the online
                        update function, you need to configure the device to communicate with the
                        Huawei Security Center. For details, see "Configuring the Device to
                        Communicate with the Huawei Security Center" in CLI Configuration Guide >
                        System Management > Signature Database Update Configuration >
                        Configuring Online Signature Database Update.

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                            276
QoS Configuration
QoS Configuration                                                  13 Experience Assurance Configuration


Context
                    By default, devices are preconfigured with service awareness signature databases.
                    The Huawei Security Center (https://isecurity.huawei.com/) periodically releases
                    the latest service awareness signature database. To help devices effectively identify
                    applications, update devices' service awareness signature databases to the latest
                    version.
                    Devices support the update of the service awareness signature databases used by
                    an AC and AP, as described in Table 13-4.

                    Table 13-4 Introduction to service awareness signature databases
                     Signature            Signature            Description
                     Database Name        Database Alias

                     SA signature         SA-SDB               The service awareness signature
                     database                                  database used by a device that does
                                                               not support the native AC function
                                                               describes the signatures of common
                                                               applications/protocols on the network.
                                                               These signatures are used by
                                                               application identification to identify
                                                               applications/protocols in traffic.
                                                               NOTE
                                                                This signature database is supported only
                                                                on the S6730E-H-V2, S6730-H-V2, S5732-
                                                                H-V2, S5755E-H, S5755-H, S6750E-S,
                                                                S5755-S, and S6750-S.

                     APSA signature       APSA-SDB             In a native AC scenario, the device
                     database                                  functions as an AC to manage the AP,
                                                               which installs and updates this
                                                               database through the AC. The
                                                               procedure is as follows:
                                                               1. The AC downloads or updates the
                                                                  service awareness signature
                                                                  database used by the AP.
                                                               2. The AC traverses the service
                                                                  awareness signature database
                                                                  support and version of the online
                                                                  AP, and then transfers the latest
                                                                  signature database file to the AP.
                                                               3. The AP loads the service awareness
                                                                  signature database.
                                                               NOTE
                                                                This signature database is supported only
                                                                on the S6730E-H-V2, S6730-H-V2, S5732-
                                                                H-V2, S5755E-H, and S5755-H.




                    This section provides only the basic steps for updating a signature database online.
                    For details about offline signature database update and other related update

Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                               277
QoS Configuration
QoS Configuration                                                              13 Experience Assurance Configuration


                    information, see "Signature Database Update Configuration" in CLI Configuration
                    Guide > System Management Configuration.

Procedure
         Step 1 Configure the DNS server to ensure that the device can correctly parse the domain
                name of the security center.
                    system-view
                    dns resolve
                    dns server ip-address

         Step 2 Update a service awareness signature database.

                    The device supports online and offline updates of service awareness signature
                    databases. Online updates are classified as automatic or manual online update.

                    ●    Automatically update a service awareness signature database online.
                         a.    Enable the automatic update function.
                               update schedule { sa-sdb | apsa-sdb } enable

                         b.    Configure an automatic update time.
                               update schedule sa-sdb { daily | weekly { mon | tue | wed | thu | fri | sat | sun } } time
                               update schedule apsa-sdb { hourly minute | { daily | weekly { mon | tue | wed | thu | fri | sat
                               | sun } } time }

                               The scheduled update time can be manually configured.
                               It is recommended that a service awareness signature database be
                               updated once a week.
                    ●    Manually update a service awareness signature database online.
                         update online { sa-sdb | apsa-sdb }

                         After the command is executed, the device immediately queries and
                         downloads the signature database, and then updates the local signature
                         database to the latest version.
                    ●    Update a service awareness signature database offline.
                         a.    Log in to Huawei Security Center (isecurity.huawei.com) and download
                               the latest service awareness signature database file.
                         b.    Upload the signature database file to the device.
                               The signature database file is in .zip format and does not need to be
                               decompressed. You can upload the file to the root directory (flash:/) of
                               the device through FTP, SFTP, or TFTP. For details, see "File System
                               Management Configuration" in CLI Configuration Guide > Basic
                               Configuration.
                         c.    Use the signature database file uploaded to the root directory for offline
                               update.
                               update local { sa-sdb | apsa-sdb } file file-name

                    ----End

Verifying the Configuration
                    ●    Run the display update configuration command to check whether the
                         configuration for the signature database update is correct.

Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                              278
QoS Configuration
QoS Configuration                                                                   13 Experience Assurance Configuration


                    ●    After the signature database is updated, run the display engine information
                         command to check the running status of the ASE and version information of
                         the signature database. Ensure that the signature database has been updated
                         to the latest version.

13.5.2 Configuring MQC-based Experience Assurance

Context
                    After the experience assurance function is configured, the device re-marks the
                    priority of packets matching an application-based policy to control network traffic.

Procedure
         Step 1 Enable the SA function.
                    1.   Enter the interface view.
                         interface interface-type interface-number
                    2.   Enable the SA function on the interface.
                         sa enable
                         By default, the SA function is disabled. After this function is enabled, the
                         device can identify application traffic passing through the interface.
         Step 2 (Optional) Configure an application identification whitelist.
                    sa whitelist acl { acl-number | acl-name }

                    By default, no application identification whitelist is configured.

                          NOTE

                         An advanced ACL must have been created and configured.

         Step 3 Configure a traffic classifier.
                    1.   Create a traffic classifier and enter the traffic classifier view, or enter the view
                         of an existing traffic classifier.
                         traffic classifier classifier-name [ type { and | or } ]
                    2.   Configure application identification.
                         if-match application application-name [ time-range time-name ]
                    3.   Exit the traffic classifier view.
                         quit

         Step 4 Configure a traffic behavior.
                    1.   Create a traffic behavior and enter the traffic behavior view, or enter the view
                         of an existing traffic behavior.
                         traffic behavior behavior-name
                    2.   Configure a matching rule as required.
                         –      Re-marking 802.1p values of VLAN packets
                                remark 8021p 8021p-value
                         –      Re-marking DSCP values of IP packets
                                remark dscp { dscp-name | dscp-value }

         Step 5 Configure a traffic policy.
                    1.   Create a traffic policy and enter the traffic policy view, or enter the view of an
                         existing traffic policy.

Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                       279
QoS Configuration
QoS Configuration                                                            13 Experience Assurance Configuration

                         traffic policy policy-name

                    2.   Bind a traffic behavior and a traffic classifier to the traffic policy.
                         classifier classifier-name behavior behavior-name [ precedence precedence-value ]

                    3.   Exit the traffic policy view.
                         quit

         Step 6 Apply the traffic policy to an interface.
                    1.   Enter the interface view.
                         interface interface-type interface-number

                    2.   Apply the traffic policy to the interface.
                         traffic-policy policy-name { inbound | outbound }

                    3.   Exit the interface view.
                         quit

                    ----End

13.5.3 (Optional) Configuring Global Parameters for
Experience Assurance
Context
                    In the system view, you can configure global parameters for the experience
                    assurance function. Table 13-5 lists the global parameters that can be configured.

Procedure
         Step 1 Enter the system view.
                    system-view

         Step 2 Configure global parameters for experience assurance.




Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                  280
QoS Configuration
QoS Configuration                                                  13 Experience Assurance Configuration


                    Table 13-5 Global parameters for experience assurance
                     Operation               Command                Description                        Ap
                                                                                                       plic
                                                                                                       ati
                                                                                                       on
                                                                                                       Sce
                                                                                                       nar
                                                                                                       io

                     Enable the              sa application-        After the application              To
                     application             statistic enable       identification statistics          obt
                     identification                                 collection function is             ain
                     statistics collection                          enabled, you can run the           pac
                     function.                                      display sa flow-table              ket
                                                                    command to query the               stat
                                                                    traffic statistics of identified   istic
                                                                    applications.                      s of
                                                                                                       an
                                                                                                       app
                                                                                                       lica
                                                                                                       tion
                                                                                                       ,
                                                                                                       use
                                                                                                       this
                                                                                                       fun
                                                                                                       ctio
                                                                                                       n.

                     Configure the aging     sa flow-table          By default, the aging period       To
                     period of the           aging-time time-       of the application                 adj
                     application             value                  identification flow table is       ust
                     identification flow                            300 seconds.                       the
                     table.                                                                            agi
                                                                                                       ng
                                                                                                       peri
                                                                                                       od
                                                                                                       of
                                                                                                       the
                                                                                                       flo
                                                                                                       w
                                                                                                       tabl
                                                                                                       e,
                                                                                                       use
                                                                                                       this
                                                                                                       fun
                                                                                                       ctio
                                                                                                       n.




Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                               281
QoS Configuration
QoS Configuration                                                 13 Experience Assurance Configuration


                    Operation               Command                Description                    Ap
                                                                                                  plic
                                                                                                  ati
                                                                                                  on
                                                                                                  Sce
                                                                                                  nar
                                                                                                  io

                    Configure the           sa flow-table          By default, the chip reports   To
                    interval at which the   statistic report-      statistics about the           adj
                    chip reports            cycle cycle-value      application identification     ust
                    statistics about the                           flow table at an interval of   the
                    application                                    120 seconds.                   inte
                    identification flow                                                           rval
                    table.                                                                        at
                                                                                                  whi
                                                                                                  ch
                                                                                                  the
                                                                                                  chi
                                                                                                  p
                                                                                                  rep
                                                                                                  orts
                                                                                                  stat
                                                                                                  istic
                                                                                                  s
                                                                                                  abo
                                                                                                  ut
                                                                                                  the
                                                                                                  app
                                                                                                  lica
                                                                                                  tion
                                                                                                  ide
                                                                                                  ntifi
                                                                                                  cati
                                                                                                  on
                                                                                                  flo
                                                                                                  w
                                                                                                  tabl
                                                                                                  e,
                                                                                                  use
                                                                                                  this
                                                                                                  fun
                                                                                                  ctio
                                                                                                  n.




Issue 01 (2025-03-03)           Copyright © Huawei Technologies Co., Ltd.                           282
QoS Configuration
QoS Configuration                                                  13 Experience Assurance Configuration


                     Operation              Command                 Description                   Ap
                                                                                                  plic
                                                                                                  ati
                                                                                                  on
                                                                                                  Sce
                                                                                                  nar
                                                                                                  io

                     Configure the alarm    sa flow-table           By default, the lower and     To
                     percentage of the      usage-alarm             upper alarm percentage        adj
                     application            percentage lower-       thresholds for the            ust
                     identification flow    percent upper-          application identification    the
                     table usage.           percent                 flow table usage are 50%      alar
                                                                    and 80%, respectively.        m
                                                                                                  thr
                                                                                                  esh
                                                                                                  old
                                                                                                  for
                                                                                                  the
                                                                                                  app
                                                                                                  lica
                                                                                                  tion
                                                                                                  ide
                                                                                                  ntifi
                                                                                                  cati
                                                                                                  on
                                                                                                  flo
                                                                                                  w
                                                                                                  tabl
                                                                                                  e
                                                                                                  usa
                                                                                                  ge,
                                                                                                  use
                                                                                                  this
                                                                                                  fun
                                                                                                  ctio
                                                                                                  n.



                    ----End

13.5.4 Verifying the Configuration

Procedure
                     Operation                                 Command

                     Check the enabling status of the SA       display interface sa-configuration
                     function on an interface.                 [ interface-name | interface-type
                                                               interface-number ]
                     Check global SA configurations.           display sa global configuration


Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                          283
QoS Configuration
QoS Configuration                                                       13 Experience Assurance Configuration


                     Operation                                      Command

                     Check information about the                    display sa flow-table [ statistic ] { all
                     application identification flow table.         | { application application-name |
                                                                    source-ip src-ipv4-address |
                                                                    destination-ip dest-ipv4-address |
                                                                    source-port src-port | destination-
                                                                    port dest-port-number | protocol { tcp
                                                                    | udp } | vpn-instance vpn-instance }
                                                                    *} slot slotid
                                                                    NOTE
                                                                     Before you specify a VPN instance in this
                                                                     command, the VPN instance must have
                                                                     been created.

                     Check the configuration status of the          display sa whitelist applied-record
                     application identification whitelist.


13.5.5 Example for Configuring MQC-based Experience
Assurance

Networking Requirements
                    On an enterprise network shown in the figure, packets sent from Host1 and Host2
                    to DeviceB are identified by different VLAN IDs. The VLAN ID for Host1 is 10, and
                    that for Host2 is 20. To ensure the voice and video conference experience of the
                    enterprise, you need to configure experience assurance on DeviceB so that voice
                    and video application traffic is preferentially forwarded.

                    Figure 13-2 Network diagram for configuring experience assurance
                         NOTE

                        In this example, interface1, interface2, and interface3 represent 10GE1/0/1, 10GE1/0/2, and
                        10GE1/0/3, respectively.




Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                       284
QoS Configuration
QoS Configuration                                                       13 Experience Assurance Configuration


Configuration Roadmap
                    The configuration roadmap is as follows:
                    1. Create VLANs and configure interfaces so that DeviceB can communicate with
                    Host1, Host2, and DeviceA.
                    2. Enable application identification on inbound interfaces.
                    3. Configure a traffic policy to match a specific application, and configure a rule of
                    re-marking the packet priority in the traffic behavior.
                    4. Apply the traffic policy to interfaces to ensure that the corresponding
                    application traffic is preferentially forwarded.

Procedure
         Step 1 Create VLANs and configure interfaces so that DeviceB can communicate with
                Host1, Host2, and DeviceA.
                    # Create VLAN 10, VLAN 20, and VLAN 30 on DeviceB.
                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 10 20 30

                    # Configure 10GE1/0/1 as a trunk interface and add it to VLAN 30. Configure
                    10GE1/0/2 and 10GE1/0/3 as access interfaces and add them to VLAN 10 and
                    VLAN 20, respectively.
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type trunk
                    [DeviceB-10GE1/0/1] port trunk allow-pass vlan 30
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type access
                    [DeviceB-10GE1/0/2] port default vlan 10
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] portswitch
                    [DeviceB-10GE1/0/3] port link-type access
                    [DeviceB-10GE1/0/3] port default vlan 20
                    [DeviceB-10GE1/0/3] quit

                    # Create VLANIF 10, VLANIF 20, and VLANIF 30, and configure IP addresses for
                    them.
                    [DeviceB] interface vlanif 10
                    [DeviceB-Vlanif10] ip address 192.168.10.1 24
                    [DeviceB-Vlanif10] quit
                    [DeviceB] interface vlanif 20
                    [DeviceB-Vlanif20] ip address 192.168.20.1 24
                    [DeviceB-Vlanif20] quit
                    [DeviceB] interface vlanif 30
                    [DeviceB-Vlanif30] ip address 192.168.100.1 24
                    [DeviceB-Vlanif30] quit

         Step 2 Enable application identification on inbound interfaces.
                    # Enable application identification on access-side interfaces of DeviceB.
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] sa enable


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                             285
QoS Configuration
QoS Configuration                                                            13 Experience Assurance Configuration

                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] sa enable
                    [DeviceB-10GE1/0/3] quit

         Step 3 Configure a traffic policy to match a specific application, and configure a rule of
                re-marking the packet priority in the traffic behavior.
                    # Create and configure a traffic classifier c1 on DeviceB to match the application
                    type TencentMeeting.
                    [DeviceB] traffic classifier c1
                    [DeviceB-classifier-c1] if-match application TencentMeeting
                    [DeviceB-classifier-c1] quit

                    # Create and configure a traffic behavior b1 on DeviceB to re-mark 802.1p values
                    of packets to 5.
                    [DeviceB] traffic behavior b1
                    [DeviceB-behavior-b1] remark 8021p 5
                    [DeviceB-behavior-b1] quit

                    # Create and configure a traffic policy p1 on DeviceB, and bind the traffic classifier
                    to the traffic behavior in the traffic policy.
                    [DeviceB] traffic policy p1
                    [DeviceB-trafficpolicy-p1] classifier c1 behavior b1
                    [DeviceB-trafficpolicy-p1] quit

         Step 4 Apply the traffic policy to interfaces to ensure that the corresponding application
                traffic is preferentially forwarded.
                    # Apply the traffic policy p1 to inbound interfaces of DeviceB to ensure that the
                    corresponding application traffic is preferentially forwarded.
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] traffic-policy p1 inbound
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] traffic-policy p1 inbound
                    [DeviceB-10GE1/0/3] quit

                    ----End

Verifying the Configuration
                    After the preceding configurations are complete, check the experience assurance
                    configuration on DeviceB.
                    # Check the traffic classifier configuration.
                    <DeviceB> display traffic classifier
                     Traffic Classifier Information:
                       Classifier: c1
                        Type: OR
                        Rule(s):
                          if-match application TencentMeeting
                    Total classifier number is 1

                    # Check the traffic policy configuration.
                    <DeviceB> display traffic policy
                     Traffic Policy Information:
                      Policy: p1
                        Classifier: c1
                          Type: OR


Issue 01 (2025-03-03)               Copyright © Huawei Technologies Co., Ltd.                                 286
QoS Configuration
QoS Configuration                                                                        13 Experience Assurance Configuration

                        Behavior: b1
                         Remark:
                          Remark 8021p 5

                    Total policy number is 1

                    # Check the traffic policy application records.
                    <DeviceB> display traffic-policy applied-record
                    Total records : 2
                    --------------------------------------------------------------------------------
                    Policy Type/Name                     Apply Parameter              Slot State
                    --------------------------------------------------------------------------------
                    p1                             10GE1/0/2(IN)                1 success

                    p1                             10GE1/0/3(IN)                1 success
                    --------------------------------------------------------------------------------


Configuration Script
                    DeviceB
                    #
                    sysname DeviceB
                    #
                    vlan batch 10 20 30
                    #
                    interface 10GE1/0/1
                     port link-type trunk
                     port trunk allow-pass vlan 30
                    #
                    interface 10GE1/0/2
                     port link-type access
                     port default vlan 10
                     sa enable
                     traffic-policy p1 inbound
                    #
                    interface 10GE1/0/3
                     port link-type access
                     port default vlan 20
                     sa enable
                     traffic-policy p1 inbound
                    #
                    interface vlanif 10
                     ip address 192.168.10.1 255.255.255.0
                    #
                    interface vlanif 20
                     ip address 192.168.20.1 255.255.255.0
                    #
                    interface vlanif 30
                     ip address 192.168.100.1 255.255.255.0
                    #
                    traffic classifier c1 type or
                     if-match application TencentMeeting
                    #
                    traffic behavior b1
                     remark 8021p 5
                    #
                    traffic policy p1
                     classifier c1 behavior b1 precedence 5
                    #
                    return


13.5.6 Example for Configuring Experience Assurance for
Traffic Statistics Collection

Issue 01 (2025-03-03)                 Copyright © Huawei Technologies Co., Ltd.                                           287
QoS Configuration
QoS Configuration                                                       13 Experience Assurance Configuration


Networking Requirements
                    In Figure 13-3, packets sent from Host1 and Host2 to DeviceB are identified by
                    different VLAN IDs. The VLAN ID for Host1 is 10, and that for Host2 is 20. To
                    record application traffic statistics on terminals, enable application identification
                    and application identification statistics collection on DeviceB.

                         NOTE

                        Only the S6730E-H-V2, S6730-H-V2 and S5732-H-V2 series support this example.


                    Figure 13-3 Network diagram for configuring experience assurance
                         NOTE

                        In this example, interface1, interface2, and interface3 represent 10GE1/0/1, 10GE1/0/2, and
                        10GE1/0/3, respectively.




Configuration Roadmap
                    The configuration roadmap is as follows:

                    1. Create VLANs and configure interfaces so that DeviceB can communicate with
                    Host1, Host2, and DeviceA.

                    2. Enable application identification on inbound interfaces.

                    3. Enable the application identification statistics collection function.

                    4. Configure NetStream on DeviceB because application identification statistics
                    collection depends on NetStream.

Procedure
         Step 1 Create VLANs and configure interfaces so that DeviceB can communicate with
                Host1, Host2, and DeviceA.

                    # Create VLAN 10, VLAN 20, and VLAN 30 on DeviceB.

Issue 01 (2025-03-03)            Copyright © Huawei Technologies Co., Ltd.                                     288
QoS Configuration
QoS Configuration                                                             13 Experience Assurance Configuration

                    <HUAWEI> system-view
                    [HUAWEI] sysname DeviceB
                    [DeviceB] vlan batch 10 20 30

                    # Configure 10GE1/0/1 as a trunk interface and add it to VLAN 30. Configure
                    10GE1/0/2 and 10GE1/0/3 as access interfaces and add them to VLAN 10 and
                    VLAN 20, respectively.
                    [DeviceB] interface 10ge 1/0/1
                    [DeviceB-10GE1/0/1] portswitch
                    [DeviceB-10GE1/0/1] port link-type trunk
                    [DeviceB-10GE1/0/1] port trunk allow-pass vlan 30
                    [DeviceB-10GE1/0/1] quit
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] portswitch
                    [DeviceB-10GE1/0/2] port link-type access
                    [DeviceB-10GE1/0/2] port default vlan 10
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] portswitch
                    [DeviceB-10GE1/0/3] port link-type access
                    [DeviceB-10GE1/0/3] port default vlan 20
                    [DeviceB-10GE1/0/3] quit

                    # Create VLANIF 10, VLANIF 20, and VLANIF 30, and configure IP addresses for
                    them.
                    [DeviceB] interface vlanif 10
                    [DeviceB-Vlanif10] ip address 192.168.10.1 24
                    [DeviceB-Vlanif10] quit
                    [DeviceB] interface vlanif 20
                    [DeviceB-Vlanif20] ip address 192.168.20.1 24
                    [DeviceB-Vlanif20] quit
                    [DeviceB] interface vlanif 30
                    [DeviceB-Vlanif30] ip address 192.168.100.1 24
                    [DeviceB-Vlanif30] quit

         Step 2 Enable application identification on inbound interfaces.
                    # Enable application identification on access-side interfaces of DeviceB.
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] sa enable
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] sa enable
                    [DeviceB-10GE1/0/3] quit

         Step 3 Enable the application identification statistics collection function.
                    # Enable application identification statistics collection on DeviceB.
                    [DeviceB] sa application-statistic enable

         Step 4 Configure NetStream on which traffic statistics collection depends.
                    # Switch to the NetStream resource mode on DeviceB.
                    [DeviceB] assign forward enp netstream enable

                    # Configure NetStream globally on DeviceB. The recommended configurations are
                    as follows:
                    [DeviceB] netstream timeout ip active 300
                    [DeviceB] netstream timeout vxlan inner-ip active 300
                    [DeviceB] netstream timeout ip inactive 180
                    [DeviceB] netstream timeout vxlan inner-ip inactive 180
                    [DeviceB] netstream timeout ip tcp-session
                    [DeviceB] netstream timeout vxlan inner-ip tcp-session


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                   289
QoS Configuration
QoS Configuration                                                           13 Experience Assurance Configuration


                    # Configure a flexible flow statistics template on DeviceB, including 5-tuple,
                    statistics collection, and sampling rate information. The recommended
                    configurations are as follows. You are advised to perform the configurations
                    according to the recommended configurations. Otherwise, traffic statistics may be
                    incorrect.
                    [DeviceB] netstream record sac_v4 ip
                    [DeviceB-netstream-record-ipv4-sac_v4] collect counter bytes
                    [DeviceB-netstream-record-ipv4-sac_v4] collect counter packets
                    [DeviceB-netstream-record-ipv4-sac_v4] collect interface sampler-info
                    [DeviceB-netstream-record-ipv4-sac_v4] match ip destination-address
                    [DeviceB-netstream-record-ipv4-sac_v4] match ip destination-port
                    [DeviceB-netstream-record-ipv4-sac_v4] match ip protocol
                    [DeviceB-netstream-record-ipv4-sac_v4] match ip source-address
                    [DeviceB-netstream-record-ipv4-sac_v4] match ip source-port
                    [DeviceB-netstream-record-ipv4-sac_v4] quit
                    [DeviceB] netstream record sac_vxlan vxlan inner-ip
                    [DeviceB-netstream-record-vxlan-sac_vxlan] collect counter bytes
                    [DeviceB-netstream-record-vxlan-sac_vxlan] collect counter packets
                    [DeviceB-netstream-record-vxlan-sac_vxlan] collect interface sampler-info
                    [DeviceB-netstream-record-vxlan-sac_vxlan] match inner-ip destination-address
                    [DeviceB-netstream-record-vxlan-sac_vxlan] match inner-ip destination-port
                    [DeviceB-netstream-record-vxlan-sac_vxlan] match inner-ip protocol
                    [DeviceB-netstream-record-vxlan-sac_vxlan] match inner-ip source-address
                    [DeviceB-netstream-record-vxlan-sac_vxlan] match inner-ip source-port
                    [DeviceB-netstream-record-vxlan-sac_vxlan] quit

                    # Configure sampling for both incoming and outgoing traffic on the interface
                    configured with sa enable. The recommended sampling rate is 256.
                    [DeviceB] interface 10ge 1/0/2
                    [DeviceB-10GE1/0/2] netstream inbound ip
                    [DeviceB-10GE1/0/2] netstream outbound ip
                    [DeviceB-10GE1/0/2] netstream record sac_v4 ip inbound
                    [DeviceB-10GE1/0/2] netstream record sac_v4 ip outbound
                    [DeviceB-10GE1/0/2] netstream record sac_vxlan vxlan inner-ip inbound
                    [DeviceB-10GE1/0/2] netstream record sac_vxlan vxlan inner-ip outbound
                    [DeviceB-10GE1/0/2] netstream sampler ip random-packets 256 inbound
                    [DeviceB-10GE1/0/2] netstream sampler ip random-packets 256 outbound
                    [DeviceB-10GE1/0/2] quit
                    [DeviceB] interface 10ge 1/0/3
                    [DeviceB-10GE1/0/3] netstream inbound ip
                    [DeviceB-10GE1/0/3] netstream outbound ip
                    [DeviceB-10GE1/0/3] netstream record sac_v4 ip inbound
                    [DeviceB-10GE1/0/3] netstream record sac_v4 ip outbound
                    [DeviceB-10GE1/0/3] netstream record sac_vxlan vxlan inner-ip inbound
                    [DeviceB-10GE1/0/3] netstream record sac_vxlan vxlan inner-ip outbound
                    [DeviceB-10GE1/0/3] netstream sampler ip random-packets 256 inbound
                    [DeviceB-10GE1/0/3] netstream sampler ip random-packets 256 outbound
                    [DeviceB-10GE1/0/3] quit

                    ----End

Verifying the Configuration
                    After the preceding configurations are complete, check the experience assurance
                    configuration on DeviceB.
                    # Check the interfaces on which application identification is enabled.
                    <DeviceB> display interface sa-configuration
                    ------------------
                    InterfaceName
                    ------------------
                    10GE 1/0/2
                    10GE 1/0/3
                    ------------------


Issue 01 (2025-03-03)              Copyright © Huawei Technologies Co., Ltd.                                 290
QoS Configuration
QoS Configuration                                                                  13 Experience Assurance Configuration


                    # Check the global configuration of application identification.
                    <DeviceB> display sa global configuration
                    Application statistic                   : Enable
                    SA flow-table aging-time (sec)               : 300
                    SA flow-table statistic report-cycle (sec)     : 120
                    SA flow-table usage-alarm lower percentage          : 50
                    SA flow-table usage-alarm upper percentage           : 80

                    # Check information about the application identification flow table.
                    <DeviceB> display sa flow-table all slot 1
                    This operation may take a long time. Please wait....done.
                    ------------------------------------------------------------
                     Source IPv4 address                 : 192.168.1.2
                     Source port                   : 2354
                     Destination IPv4 address              : 192.168.1.1
                     Destination port                 : 53
                     Protocol                    : TCP
                     VPN instance                    : _public_
                     Vrf ID                     :0
                     Create time                   : 2024-12-26 16:31:15
                     Latest update time                 : 2024-12-26 16:49:15
                     Application name                    : DNS
                     Application ID                 : 425
                     Total ingress packets              : 246558976
                     Total ingress bytes              : 2337919488
                     Total egress packets               :0
                     Total egress bytes               :0
                    ------------------------------------------------------------


Configuration Script
                    DeviceB
                    #
                    sysname DeviceB
                    #
                    vlan batch 10 20 30
                    #
                    interface 10GE1/0/1
                     port link-type trunk
                     port trunk allow-pass vlan 30
                    #
                    interface 10GE1/0/2
                     port link-type access
                     port default vlan 10
                     sa enable
                     traffic-policy p1 inbound
                     netstream inbound ip
                     netstream outbound ip
                     netstream record sac_v4 ip inbound
                     netstream record sac_v4 ip outbound
                     netstream record sac_vxlan vxlan inner-ip inbound
                     netstream record sac_vxlan vxlan inner-ip outbound
                     netstream sampler ip random-packets 256 inbound
                     netstream sampler ip random-packets 256 outbound
                    #
                    interface 10GE1/0/3
                     port link-type access
                     port default vlan 20
                     sa enable
                     traffic-policy p1 inbound
                     netstream inbound ip
                     netstream outbound ip
                     netstream record sac_v4 ip inbound
                     netstream record sac_v4 ip outbound
                     netstream record sac_vxlan vxlan inner-ip inbound
                     netstream record sac_vxlan vxlan inner-ip outbound


Issue 01 (2025-03-03)                Copyright © Huawei Technologies Co., Ltd.                                      291
QoS Configuration
QoS Configuration                                                       13 Experience Assurance Configuration

                     netstream sampler ip random-packets 256 inbound
                     netstream sampler ip random-packets 256 outbound
                    #
                    interface vlanif 10
                     ip address 192.168.10.1 255.255.255.0
                    #
                    interface vlanif 20
                     ip address 192.168.20.1 255.255.255.0
                    #
                    interface vlanif 30
                     ip address 192.168.100.1 255.255.255.0
                    #
                    sa application-statistic enable
                     assign forward enp netstream enable
                     netstream timeout ip active 300
                     netstream timeout vxlan inner-ip active 300
                     netstream timeout ip inactive 180
                     netstream timeout vxlan inner-ip inactive 180
                     netstream timeout ip tcp-session
                     netstream timeout vxlan inner-ip tcp-session
                     netstream record sac_v4 ip
                     collect counter bytes
                     collect counter packets
                     collect interface sampler-info
                     match ip destination-address
                     match ip destination-port
                     match ip protocol
                     match ip source-address
                     match ip source-port
                     netstream record sac_vxlan vxlan inner-ip
                     collect counter bytes
                     collect counter packets
                     collect interface sampler-info
                     match inner-ip destination-address
                     match inner-ip destination-port
                     match inner-ip protocol
                     match inner-ip source-address
                     match inner-ip source-port
                    #
                    return




Issue 01 (2025-03-03)             Copyright © Huawei Technologies Co., Ltd.                              292
