---
id: collect-261121-veille-ia/bluetooth-audio/bluetooth-hdt-lifts-le-to-7-5-mbps
title: "Bluetooth HDT Lifts LE to 7.5 Mbps"
domain: bluetooth-audio
role: reference
task: article
actors: []
dates: []
keywords: ["mbps", "bluetooth", "audio", "core", "lifts", "lossless", "earbuds", "rate"]
source: https://hwbusters.com/news/bluetooth-hdt-lifts-le-to-7-5-mbps-but-hi-res-lossless-audio-rides-on-the-optional-tiers/
source_anchor: ""
source_lines: [1, 22]
section: "Bluetooth_Audio"
---
**Bluetooth HDT, the headline feature of the newly adopted Core 7.0 specification, lifts Bluetooth LE from 2 Mbps to as much as 7.5 Mbps. That is enough headroom for genuinely lossless wireless audio, but only the slower HDT tiers are mandatory, so the logo alone will not tell you what a pair of earbuds can actually do.**


The Bluetooth SIG has adopted Core Specification 7.0, and the part everyone will care about is Bluetooth HDT, short for High Data Throughput. It is a new physical layer for Bluetooth LE that tops out at 7.5 Mbps, close to four times the 2 Mbps of the LE 2M PHY that arrived back with Bluetooth 5.0. The SIG’s own HDT overview names premium wireless audio as the first flagship use, ahead of XR headsets, AI wearables and faster firmware updates.

## Five speeds, three of them required

HDT is not a single rate. According to a breakdown by Bluetooth design house Argenox, the spec defines HDT2, HDT3, HDT4, HDT6 and HDT7.5. A controller that claims HDT support has to do the 2, 3 and 4 Mbps modes. The 6 Mbps mode is optional, and 7.5 Mbps is optional on top of that, since supporting it also requires HDT6. The idea is to let small, cheap silicon join in without carrying the complexity of the fastest modes.

Devices do not start a link on HDT, either. They discover each other and connect over the ordinary 1M GFSK PHY, then negotiate the switch. HDT does not handle advertising at all, and a link can fall back to GFSK when range or interference degrades the channel. Once running, the rate can change packet by packet: each packet’s control header goes out in sturdier pi/4 QPSK so the receiver can read the format before it tackles the faster payload. A large-transfer packet format can bundle up to four payloads and split them into blocks, so a corrupted block can be resent without repeating the whole thing.

## What 7.5 Mbps buys you in audio

The audio math is why Tom’s Hardware pitched HDT as the feature that might finally win over audiophiles. Uncompressed CD audio, 16-bit at 44.1 kHz in stereo, needs about 1.4 Mbps. That does not squeeze reliably through LE 2M once protocol overhead and retransmissions are counted, which is why today’s earbuds lean on lossy codecs such as AAC, aptX, LDAC and LC3. Step up to 24-bit at 48 kHz and you need around 2.3 Mbps raw; 24-bit at 96 kHz needs roughly 4.6 Mbps. Only 24-bit at 192 kHz, at about 9.2 Mbps, overshoots the ceiling.

Lossless compression such as FLAC shrinks those numbers further, so CD-quality lossless should sit comfortably within even the mandatory tiers. Hi-res at 96 kHz is a different story. Delivered raw, it wants HDT6 or better, and that is exactly the part a manufacturer is allowed to leave out.

## The caveats

Three things are worth keeping in mind before anyone starts planning a headphone upgrade. First, 7.5 Mbps is a PHY bit rate, not the throughput an application will see after headers, acknowledgements and scheduling gaps. Second, a product can be qualified against Core 7.0 without implementing HDT at all, so “Bluetooth 7.0” on a box says very little on its own. Third, the radio is only half the job. Lossless playback also needs a codec and an audio profile on top, plus a phone or PC whose Bluetooth controller supports the same HDT modes as the earbuds. The phone side tends to move slowly.

None of that makes HDT less significant. It is the biggest jump in Bluetooth LE data rate since 2016, and chipmakers including Intel, Qualcomm and Nordic Semiconductor have already lined up behind it on the SIG’s announcement page. Core 7.0 also brings Encryption Production Debugging, which lets engineers inspect encrypted links during development without shipping debug keys, plus on-the-fly parameter updates for isochronous audio streams. Expect the first HDT silicon in premium earbuds and laptops before the cheaper end of the market sees it.
