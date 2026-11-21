---
id: collect-261121-veille-ia/bluetooth-audio/bluetooth-hdt-7-5-mbps-for-lossless-wireless-audio
title: "Bluetooth HDT: 7.5 Mbps for lossless wireless audio"
domain: bluetooth-audio
role: reference
task: article
actors: []
dates: []
keywords: ["mbps", "bluetooth", "audio", "lossless", "about", "optional", "headphones", "support"]
source: https://tbreak.com/bluetooth-hdt-7-5-mbps-lossless-audio/
source_anchor: ""
source_lines: [1, 26]
section: "Bluetooth_Audio"
---
The Bluetooth SIG has adopted Bluetooth High Data Throughput (HDT), raising Bluetooth LE’s top bit rate from 2 Mbps to 7.5 Mbps. HDT is an optional feature of the new Bluetooth Core 7.0.

For headphones, the extra headroom could end Bluetooth’s reliance on compressed codecs. As Tom’s Hardware points out, today’s 2 Mbps link can’t reliably carry CD-quality lossless audio (about 1.4 Mbps), so music gets squeezed through SBC, AAC, aptX, LDAC, or LC3. At 7.5 Mbps, uncompressed stereo up to 24-bit 96 kHz fits, though 192 kHz still doesn’t.

| Format (uncompressed stereo) | Bit rate | Fits under 7.5 Mbps? | 
|---|---|---|
| CD quality | About 1.4 Mbps | Yes | 
| 24-bit 48 kHz | About 2.3 Mbps | Yes | 
| 24-bit 96 kHz | About 4.6 Mbps | Yes | 
| 24-bit 192 kHz | About 9.2 Mbps | No | 

HDT is a new radio mode that uses higher-order modulation (forms of QPSK and QAM) while staying in the 2.4 GHz band. It runs at five rates: 2, 3, 4, 6, and 7.5 Mbps. Only the rates up to 4 Mbps are mandatory in an HDT device; 6 and 7.5 Mbps are optional. Since HDT itself is optional, support can vary between products with the same Core 7.0 badge.

The 7.5 Mbps figure is a ceiling, not a speed every device will sustain. Both the phone and the earbuds will need compatible hardware, and codecs and tuning will still shape what you hear.

Bluetooth® HDT brings meaningful advances in throughput, capacity, power efficiency and reliability that can help enable experiences such as high-resolution lossless audio between phones and earbuds

Chano Gómez, Senior Director, Product Management, Qualcomm

Audio isn’t the only target. The SIG also pitches faster sensor syncs for smartwatches, smart glasses offloading work to a phone, and shorter firmware updates for smart-home gear. AppleInsider says Apple hasn’t publicly commented on adopting HDT.

## Will my current headphones get HDT?

No. Older radios can’t gain the new physical layer through a firmware update, so you’ll need headphones and a phone that both support HDT. No headphone or chipset maker has announced HDT support yet.

The SIG expects its first flagship use, standardised premium wireless audio, to arrive in compatible products in 2027. When those headphones appear, look for HDT support on both ends, and for the optional 6 and 7.5 Mbps rates if lossless is the point.
