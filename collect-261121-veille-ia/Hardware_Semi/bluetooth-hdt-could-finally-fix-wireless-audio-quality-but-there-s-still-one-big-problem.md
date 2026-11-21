---
id: collect-261121-veille-ia/hardware-semi/bluetooth-hdt-could-finally-fix-wireless-audio-quality-but-there-s-still-one-big-problem
title: "Bluetooth HDT could finally fix wireless audio quality, but there's still one big problem"
domain: hardware-semi
role: reference
task: article
actors: []
dates: []
keywords: ["bluetooth", "audio", "mbps", "wireless", "quality", "lossless", "could", "hi-res"]
source: https://mobile.androidauthority.com/bluetooth-hdt-one-big-problem-3733124/
source_anchor: ""
source_lines: [1, 82]
section: "Hardware_Semi"
---
# Bluetooth HDT could finally fix wireless audio quality, but there's still one big problem

News that Bluetooth has officially adopted its new High Data Throughput (HDT) specification should be music to audiophile ears far and wide. And there’s a good chance your future pair of wireless headphones could sound considerably better as a result.

If you missed the announcement, Bluetooth HDT extends the capabilities of Bluetooth Low Energy (LE), boosting its maximum physical-layer data rate (PHY, pronounced “fie”) from 2Mbps to a comparatively colossal 7.5Mbps. That’s almost four times the maximum rate of conventional Bluetooth LE, and 2.5 times faster than Bluetooth Enhanced Data Rate (EDR), which tops out at 3Mbps.

On paper, that’s a significant boost to data throughput, even accounting for transmission overheads. The Bluetooth SIG lists several potential applications for this extra bandwidth, including extended reality (XR), spatial computing, and AI-enabled devices. But what really caught my eye is the promise of standardized high-resolution and lossless audio.

The only downside is that HDT requires all new radio hardware (we’ll get to the why later), which means all new smartphones and accessories are required before we can make use of it. Thankfully, Qualcomm’s FastConnect 8800 wireless setup claims Bluetooth HDT capabilities, so we might see phones supporting faster data features sooner than you think. Qualcomm does note that its original HDT implementation was based on a working draft of the specification, however, so actual product support still needs to be confirmed.

## Hi-Res for next-gen wireless headphones

If you’re even moderately interested in premium wireless audio, you’ll probably be aware that standard Bluetooth equipment offers relatively limited bandwidth for high-quality audio. Depending on the codec, typical wireless audio streams operate anywhere from around 200 kbps to just over 1 Mbps.

In any case, those bitrates are generally insufficient for transmitting uncompressed CD-quality or Hi-Res audio. Your smartphone (or other transmitter) must compress the audio stream, throwing out some information to make it fit through the narrow Bluetooth pipe.

There’s a range of official and third-party Bluetooth audio codecs that promise improvements over the default formula. AAC, LC3, and aptX HD offer different approaches to improving audio quality and efficiency over legacy Bluetooth SBC. But if you really want to push the limits of wireless audio today, you’ll probably be looking at LDAC, LHDC, or aptX Lossless.

Today, Bluetooth is typically a lossy streaming experience.

These three codec families can reach considerably higher bitrates than conventional Bluetooth audio, with LDAC and LHDC offering modes around 900 kbps to 1 Mbps, and aptX Lossless dynamically reaching up to 1 Mbps. Qualcomm’s aptX Lossless is particularly interesting because it can deliver bit-exact CD-quality audio when radio conditions permit. However, even these technologies have limitations, and true lossless Hi-Res playback remains difficult to achieve over conventional Bluetooth connections.

Bluetooth HDT paints a very different picture for lossless and Hi-Res audio. Its 7.5 Mbps maximum PHY rate provides considerably more headroom for streaming bit-perfect master tracks:

- 16-bit / 44.1 kHz (CD Quality): ~1.4 Mbps (previously choked standard LE connections)
- 24-bit / 48 kHz (Studio Quality): ~2.3 Mbps (fits easily within HDT’s mid-tier steps)
- 24-bit / 96 kHz (Hi-Res): ~4.6 Mbps (achievable natively over HDT 6/7.5 Mbps)
- 24-bit / 192 kHz (Ultra Hi-Res): ~9.2 Mbps (possibly accommodated via compression)

Even accounting for protocol overhead, HDT brings bit-perfect CD and 24-bit/96 kHz audio comfortably inside wireless reach without relying on aggressive, lossy compression. Even 192 kHz audio, which requires 9.2 Mbps of raw bandwidth in 24-bit stereo PCM, could potentially be accommodated through efficient lossless compression.

Theoretically, HDT could support lossless 24/96 Hi-Res.

Another important part of the Bluetooth HDT picture is its adaptive data rate capability. Rather than stubbornly transmitting at the maximum 7.5 Mbps, HDT can adjust its transmission rate according to the quality of the wireless connection. That should help maintain a reliable connection as signal conditions change, without constantly pushing the radio beyond its limits.

It’s worth distinguishing this from variable bitrate audio encoding, though. HDT adapts the wireless transmission speed, while the audio codec determines how much data the music itself requires.

## An end to the audio codec wars?

OK, this all sounds very promising, but there’s a major piece of the puzzle missing: a standardized lossless audio codec.

An important distinction is that Bluetooth HDT is merely the transmission layer — think of it as building a wider multi-lane highway, rather than deciding what kind of car drives on it. Audio still requires a dedicated codec to package, time-align, compress, and error-correct the music that travels over the air. Unfortunately, the current LE Audio standard doesn’t automatically solve the problem; its default LC3 codec was engineered for ultra-low power consumption and efficiency, meaning it is strictly a lossy format that discards audio data you supposedly can’t hear.

Nor can we simply stream raw, uncompressed PCM audio down that wider pipe. Real-world wireless environments are messy, and protocol overhead, packet headers, and interference from Wi-Fi eat up throughput fast.

More bandwidth is a win for lossless audio, but we need a codec too.

The Bluetooth SIG has been working on precisely this problem. Its High-Resolution/Lossless Codec project aims to introduce standardized high-resolution and lossless audio support into LE Audio. The SIG’s original requirements call for support for 24-bit/96 kHz high-resolution audio, alongside a separate lossless codec capable of reproducing the original audio samples bit-for-bit.

There has also been tangible progress, but we are still a while away from a universal high-resolution Bluetooth codec hitting the market. Formal multi-vendor interoperability testing is scheduled for March 2027. If all goes well, the finalized codec standard should emerge later in 2027, with consumer products following thereafter.

Unfortunately, we still don’t know exactly which codec technology is being tested. It could build on existing technology such as Fraunhofer IIS’ LC3plus Lossless, or it could be an entirely new codec developed specifically for Bluetooth’s next-generation audio requirements. Neither possibility has been publicly confirmed.

With no finalized lossless codec, we might not be out of the codec quagmire just yet.

So we’re waiting on Bluetooth to pair HDT with a standardized codec and the necessary LE Audio profiles. In the meantime, it’s entirely possible that third-party vendors will introduce their own proprietary codecs to take advantage of the extra bandwidth. We might not be out of the codec wars just yet.

All of this means there’s no guarantee that next-generation headphones will automatically benefit from vastly improved audio quality. HDT itself is an optional feature of Bluetooth Core 7.0, so manufacturers aren’t required to implement it. Brands will still be free to make conventional LE Audio and Classic Bluetooth products if they wish.

Furthermore, manufacturers could choose to implement proprietary codecs alongside the new standardized solution, potentially leaving the market in the same fragmented state it’s currently in. The good news is that the Bluetooth SIG is targeting standardized premium wireless audio for 2027, which should help bring some much-needed consistency to the market.

## How can Bluetooth suddenly be so much faster?

Bluetooth’s speed depends partly on how it turns digital data into radio signals. This is handled by the physical layer, or PHY. Think of it as the wireless link’s transmission system: it determines how information is encoded onto radio waves and how quickly it can be sent.

Until now, Bluetooth LE has relied exclusively on a relatively straightforward signalling method called Gaussian Frequency Shift Keying (GFSK). HDT changes that by introducing optional, higher-order modulation schemes alongside GFSK: π/4-QPSK, 8PSK, and 16QAM. These techniques pack multiple bits into each radio symbol, pushing Bluetooth LE’s peak physical speed from 2 Mbps to 7.5 Mbps.

HDT combines these modulation techniques with Forward Error Correction (FEC), which adds redundant information to transmitted data so receivers can recover bit errors over noisy channels without needing retransmissions. It also introduces new packet formats, larger payloads, and block-based transmissions to maximize spectral efficiency.

HDT will be one of the biggest changes to Bluetooth audio in years, but requires new hardware.

There is a hardware catch, however. Unlike a simple software upgrade, HDT requires an entirely new radio architecture capable of generating and receiving these coherent modulation schemes. Existing Bluetooth LE devices cannot gain HDT functionality through a firmware update alone.

HDT is backward compatible at the protocol level, meaning compatible devices initially establish a conventional LE connection over standard GFSK before negotiating up to HDT when both ends support it. Older devices without the necessary radio silicon will simply continue operating over their existing Bluetooth PHYs.

## When can you buy HDT headphones?

The Bluetooth SIG officially adopted HDT into Bluetooth Core 7.0 on November 4, 2026, but that doesn’t mean compatible headphones are about to hit store shelves immediately.

The first major application for HDT is expected to be standardized premium wireless audio, with the Bluetooth SIG targeting 2027 for its rollout. Qualcomm’s FastConnect 8800 is an early indication that chipset manufacturers are preparing for the shift, but we’ll still need compatible smartphones, headphone silicon, and the finalized audio codec and profiles before the ecosystem is truly operational.

Until the codec, profiles, and compatible hardware all fall into place, I’d hold off on budgeting for that shiny new lossless wireless setup. The physical bandwidth is finally here — now the software layer just needs to catch up.
