---
id: collect-261121-veille-ia/bluetooth-audio/bluetooth-sig-adopts-high-data-throughput-setting-the-stage-for-standardized-lossless-wireless-audio
title: "Bluetooth SIG Adopts High Data Throughput, Setting the Stage for Standardized Lossless Wireless Audio"
domain: bluetooth-audio
role: reference
task: article
actors: []
dates: []
keywords: ["bluetooth", "audio", "wireless", "standardized", "data", "mbps", "lossless", "high"]
source: https://audioxpress.com/news/bluetooth-sig-adopts-high-data-throughput-setting-the-stage-for-standardized-lossless-wireless-audio
source_anchor: ""
source_lines: [1, 52]
section: "Bluetooth_Audio"
---
The Bluetooth Special Interest Group (SIG) has adopted Bluetooth High Data Throughput (HDT), increasing Bluetooth LE’s maximum supported bit rate to 7.5 Mbps and establishing a foundation for standardized lossless and high-resolution wireless audio. An optional feature of Bluetooth Core 7.0, HDT combines higher capacity with improved efficiency and reliability, with the first standardized premium audio application expected in 2027.

 

Announced November 4, 2026, the adoption marks a significant step for developers working toward higher-performance Bluetooth audio systems. HDT introduces a new physical layer (PHY) that expands the information carried over a Bluetooth LE link, addressing bandwidth constraints that have shaped wireless audio architectures for years. The Bluetooth SIG identifies premium wireless audio as its first flagship application, including lossless and high-resolution reproduction, multichannel surround sound, and spatial audio. The announcement and accompanying explanation distinguish the adoption of the underlying transport from the standardized audio application expected next year.


The organization behind this highly antecipated development has been coordinating Bluetooth technology since 1998, when Ericsson, IBM, Intel, Nokia, and Toshiba established the SIG to support a common short-range wireless standard. From those beginnings, Bluetooth has developed into an ecosystem spanning consumer electronics, computing, automotive, and industrial applications. With wireless audio as its most important application by a large margin, the SIG now reports more than 43,000 member companies and annual shipments exceeding five billion Bluetooth-enabled products, giving a new transport capability a substantial potential market once it reaches interoperable implementations. Bluetooth’s history provides the background to that expansion.

 

For audio developers, Bluetooth HDT follows the earlier transition to LE Audio, whose completed specifications were announced in July 2022. That effort introduced a new audio architecture incorporating the LC3 codec, support for hearing aids, and Auracast broadcast audio. HDT extends the development trajectory by increasing the transport capacity available for more demanding applications. While LE Audio established capabilities for delivering audio over Bluetooth LE, now HDT provides the much needed additional bandwidth on which future audio specifications and implementations can be built.


Technically, HDT retains for now Bluetooth LE’s 2.4 GHz operation and channel spacing, increasing spectral efficiency through coherent π/4 QPSK, 8PSK, and 16QAM modulation combined with forward error correction. Five operating rates — 2, 3, 4, 6, and 7.5 Mbps — use a fixed symbol rate of 2 Msymbol/s. Within an HDT implementation, rates through 4 Mbps are mandatory; 6 and 7.5 Mbps are optional. Developers therefore need to examine supported capabilities beyond a Bluetooth Core version number.


As previously confirmed, the Bluetooth SIG expects to complete its roadmap with near-term specification releases and longer-term work to extend Bluetooth Low Energy (LE) also into the 5GHz and 6GHz bands. The central message is that greater bandwidth, new interoperability standards, and access to additional spectrum will support a broader range of listening applications, from personal devices to gaming and professional systems.


The packet architecture complements those changes with payload aggregation, block-based transmission, and selective retransmission, limiting how much data must be resent following an error. Packet-by-packet rate adaptation and optional link-quality feedback help accommodate changing RF conditions. Connections begin on an existing GFSK PHY and can subsequently switch to HDT, with fallback available. Implementing HDT requires new radio hardware; existing GFSK-only devices cannot acquire its higher rates through software alone. For system budgeting, the advertised 7.5 Mbps is a PHY bit rate, so usable application throughput must account for protocol overhead, as detailed in the Bluetooth HDT technical overview.

 

For battery-powered audio products this update is particularly important since the faster speeds can actually translate to efficiencies. Moving a given amount of data faster can shorten radio activity and leave more time for lower-power operation, while improved spectral efficiency makes better use of available wireless resources. Those gains create opportunities to balance audio quality, capacity, and energy consumption according to the product’s requirements. The SIG also identifies faster synchronization, software transfers, AI wearables, and immersive computing as applications that can benefit from the same improvements.


The adoption also follows substantial implementation work already covered by audioXpress. In July, the reported availability of the HDT draft specification, encouraged developers to examine the technology before its anticipated adoption later in 2026. The November announcement moves that work from draft review to an adopted Core feature, providing a firmer reference for ongoing product development.


On the silicon side, virtually all the Bluetooth chip vendors, from Airoha to Qualcomm, are ready to meet demand. Realtek demonstrated its RTL8773J Bluetooth HDT audio SoC at COMPUTEX 2026, combining Bluetooth audio connectivity with processing resources for applications including adaptive hybrid active noise cancellation. Earlier in the year, Realtek and Rohde & Schwarz demonstrated HDT testing using the CMP180 radio communication tester with Realtek’s RTL8922D connectivity device and RTL8773J audio SoC.


Another implementation path emerged through Ceva’s collaboration with Actions Technology on the ATS296X audio SoC family. Incorporating Ceva-Waves Bluetooth HDT technology, the series targets applications including speakers, wireless microphones, and multichannel home audio. The partnership builds on more than 100 million wireless audio SoCs delivered using Ceva technologies, while interoperability work has supported development ahead of formal specification adoption.


“As the world’s most widely used wireless standard, Bluetooth technology has continually evolved to meet the growing demands of an increasingly connected world,” says Neville Meijers, CEO of the Bluetooth SIG. “The Bluetooth SIG member community has once again risen to the challenge. Bluetooth HDT brings a new level of performance to Bluetooth technology, expanding the role it can play across consumer, commercial, and industrial markets and creating a powerful foundation for the next generation of wireless innovation.”


For product teams, the next milestone is the standardized premium wireless audio application expected in 2027, with the announcement regarding a lossless codec supporting high resolution audio, which will be a hot topic already at CES 2027. HDT supplies additional transport capacity, but codec and profile support, host integration, RF performance, and interoperability will determine the finished experience. With silicon platforms and test solutions already under development, the announcement gives the audio industry a clearer foundation for designing the next generation of Bluetooth products.

www.bluetooth.com

 

 # Bluetooth SIG Adopts High Data Throughput, Setting the Stage for Standardized Lossless Wireless Audio

The Bluetooth Special Interest Group (SIG) has adopted Bluetooth High Data Throughput (HDT), increasing Bluetooth LE’s maximum supported bit rate to 7.5 Mbps and establishing a foundation for standardized lossless and high-resolution wireless audio. An optional feature of Bluetooth Core 7.0, HDT combines higher capacity with improved efficiency and reliability, with the first standardized premium audio application expected in 2027.

  **About Joao Martins**

  Since 2013, João Martins has served as editor-in-chief of audioXpress, the leading US-based magazine and website dedicated to audio electronics, product development, and design. He also acts as international editor for Voice Coil, the premier periodical for th... Read more
