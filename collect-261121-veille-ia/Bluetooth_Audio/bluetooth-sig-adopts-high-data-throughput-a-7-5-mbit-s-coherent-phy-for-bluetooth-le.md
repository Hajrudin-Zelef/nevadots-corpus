---
id: collect-261121-veille-ia/bluetooth-audio/bluetooth-sig-adopts-high-data-throughput-a-7-5-mbit-s-coherent-phy-for-bluetooth-le
title: "Bluetooth SIG adopts High Data Throughput, a 7.5 Mbit/s coherent PHY for Bluetooth LE"
domain: bluetooth-audio
role: reference
task: article
actors: []
dates: ["2026-11-04"]
keywords: ["bluetooth", "mbit", "data", "throughput", "rate", "coherent", "high", "first"]
source: https://www.wirelessfieldguide.com/news/2026-11-04-bluetooth-hdt-adopted-core-7-0/
source_anchor: ""
source_lines: [1, 9]
section: "Bluetooth_Audio"
---
The Bluetooth SIG announced on 4 November 2026 that it has adopted Bluetooth High Data Throughput (HDT), an optional feature of the Bluetooth Core 7.0 specification. HDT adds a new physical layer for Bluetooth LE with a top bit rate of 7.5 Mbit/s. That is close to four times the 2 Mbit/s of the LE 2M PHY.

The SIGâs technical overview describes the radio. HDT stays in the 2.4 GHz ISM band with the existing LE channel bandwidth and spacing. Instead of GFSK it uses coherent Ï/4 QPSK, 8PSK and 16QAM, protected by a rate-1/2 convolutional code with puncturing. All five rates (2, 3, 4, 6 and 7.5 Mbit/s) run at 2 Msym/s. A device that supports HDT must support HDT2 to HDT4. HDT6 and HDT7.5 are optional, and HDT7.5 requires HDT6. The control header of every packet is sent in Ï/4 QPSK, so the receiver can read the payload format first and the rate can change packet by packet. Training sequences can be inserted inside long packets to refresh the channel estimate.

A connection always starts on a GFSK PHY and can then switch to HDT, with fallback. The first version of HDT does not support advertising, and it needs new radio hardware. The SIG names standardised premium audio, expected in 2027, as the first application. Hardware Busters points out that a product can qualify against Core 7.0 without implementing HDT at all.

## Why it matters

HDT is the first Bluetooth LE PHY that needs a coherent receiver: carrier and phase tracking, channel estimation and a convolutional decoder, instead of a frequency discriminator. That moves LE radio design closer to Wi-Fi and cellular practice. 16QAM needs more SNR than Ï/4 QPSK for the same error rate, so the optional 6 and 7.5 Mbit/s modes will hold only on short or clean links, and per-packet rate adaptation is how a link backs off. audioXpress and Hardware Busters both note that 7.5 Mbit/s is a PHY bit rate, so application throughput will be lower. Check data sheets for the HDT modes a chip supports, not the Core version.
