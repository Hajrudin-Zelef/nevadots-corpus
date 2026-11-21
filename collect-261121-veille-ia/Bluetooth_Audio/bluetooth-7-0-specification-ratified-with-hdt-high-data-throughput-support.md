---
id: collect-261121-veille-ia/bluetooth-audio/bluetooth-7-0-specification-ratified-with-hdt-high-data-throughput-support
title: "Bluetooth 7.0 specification ratified with HDT (High Data Throughput) support"
domain: bluetooth-audio
role: reference
task: article
actors: []
dates: ["2026-11-18"]
keywords: ["bluetooth", "encryption", "support", "specification", "high", "data", "throughput", "ratified"]
source: https://www.cnx-software.com/2026/11/18/bluetooth-7-0-specification-ratified-with-hdt-high-data-throughput-support/
source_anchor: ""
source_lines: [1, 50]
section: "Bluetooth_Audio"
---
The Bluetooth SIG ratified the Bluetooth 7.0 specification on November 3, 2026, with the highlight of the new Bluetooth Core specification being optional support for HDT (High Data Throughput), delivering up to 7.5 Mbps bitrate.

We first covered HDT in 2025 when initial/early support was implemented into the Ceva-Waves Links200 multi-protocol platform IP, and followed last September with the Realtek RTL8773J Bluetooth 7-ready Cortex-M55/M33 microcontroller. The main news is that the specification for Bluetooth 7.0 and the associated HDT feature is officially ratified/adopted.

Bluetooth 7.0 highlights:

- **Bluetooth High Data Throughput (HDT)** introduces a new Bluetooth LE physical layer that increases the maximum Bluetooth bit rate to 7.5 Mbps while improving wireless efficiency and robustness. The higher bitrate is enabled by a combination of higher-order modulation, convolutional forward error correction, synchronization enhancements, adaptive rate selection, and enhanced packet architectures. It will be especially useful for high-resolution and lossless audio streaming.
- **Bluetooth LE Isochronous Parameter Update (IPU)** allows the parameters of an active Connected Isochronous Stream (CIS) to be updated after the stream has been established. Rather than removing and recreating a Connected Isochronous Group (CIG) when bandwidth, latency, transport, or scheduling requirements change, IPU enables devices to transition between alternative stream configurations while maintaining the existing connection.
- **Bluetooth Encryption Key Schedule (EKS)** introduces a new framework for establishing and managing encryption keys used by BLE connections. Rather than deriving encryption material directly from a Long-Term Key (LTK), EKS combines ECDH-based key establishment with a CMAC-based key derivation hierarchy to generate session-specific encryption material. This enables devices to refresh encryption keys, maintain separate security contexts for different transport types, and support enhanced Message Integrity Code (MIC) protection. EKS serves as the security foundation for Bluetooth High Data Throughput (HDT), but remains an independent Bluetooth feature that could support additional Bluetooth use cases in the future.
- **Bluetooth Transient Application Key (TAK)** secures BLE connections by enabling encryption without relying on pairing or bonding. The authentication and key establishment are handled by a higher-layer application, which establishes a shared 128-bit security-strength secret and provides it to the Bluetooth Host. The TAK is then used to establish Link Layer encryption to create secure connections without forming a long-term trust relationship. The Host discards the TAK after initiating the Encryption Start Procedure, while the higher-layer application determines how its copy of the key is managed.
- **Bluetooth Encryption Production Debugging (EPD)** introduces a standardized mechanism that allows a Bluetooth Host to receive the active encryption credentials used by an encrypted connection. EPD helps preserve protocol analysis and interoperability debugging as Bluetooth encryption evolves beyond traditional long-term key-based operation.


HDT is not a small change. As the diagram above shows, most of the Bluetooth LE stack had to be changed, except for the Security Manager (SMP) block.

Summary of under-the-hood changes for Bluetooth HDT devices:

- Physical Layer (PHY)
  - New non-GFSK PHY based on higher-order coherent modulation schemes and forward error correction with puncturing
  - Support for five different bitrates up to 7.5 Mbps
- Link Layer (LL)
  - A new packet structure
  - Three new packet format variants
  - A new procedure for HDT Configuration information exchange
  - Updated procedures for PHY Update and for CIS Creation information exchange
  - HDT encryption and longer MIC sizes
- Host Controller Interface (HCI)
  - New commands to set parameters, read payloads, enable testing, and more
  - New events to report HDT rates, encryption enablement, and more

The host also needs to implement a few changes in the following layers:

- Logical Link Control and Adaptation Protocol (L2CAP)
- Generic Access Profile (GAP)
- Attribute Protocol (ATT)
- Generic Attribute Profile (GATT)

The technical overview has the full details, but note that it’s highly technical and probably only understandable by people who have already worked on the Bluetooth stack.

If you’d rather watch a high-level explanation for HDT, Alfredo Perez, Bluetooth SIG technical marketing manager, presents a marketing-based overview followed by a high-level technical overview aimed at broad Bluetooth audiences.

Beyond the specification and technical overview, Nordic Semiconductor has been working on Bluetooth 7 and HDT, and has already implemented it on a test chip/development platform. In the demo below, they transfer the same 2 MB file over every LE 2M, LE2M, and the new HDT PHYs (HDT2 to HDT7.5). The average throughput was about 3.8 Mbps, topping at about 3.9 Mbps with HDT7.5, and the transfer took about 4 seconds. LE 2M averaged and peaked at about 1.4 Mbps and took 12 seconds, while LE 1M averaged about 0.76 Mbps, peaking at about 0.77 Mbps, and took 22 seconds.

As noted in the demo video below, LE 2M/1M is still ideal for low-power applications, but HDT will be great for syncing fitness data from wearables, PC peripherals with small displays, and external AI processing (on a smartphone) for smart glasses.

Jean-Luc started CNX Software in 2010 as a part-time endeavor, before quitting his job as a software engineering manager, and starting to write daily news, and reviews full time later in 2011.

**Support CNX Software! Donate via cryptocurrencies, become a Patron on Patreon, or purchase goods on Amazon or Aliexpress**. We also use affiliate links in articles to earn commissions if you make a purchase after clicking on those links.

Seems similar in use cases to dect nr+ except for the latter relying on mesh topology
