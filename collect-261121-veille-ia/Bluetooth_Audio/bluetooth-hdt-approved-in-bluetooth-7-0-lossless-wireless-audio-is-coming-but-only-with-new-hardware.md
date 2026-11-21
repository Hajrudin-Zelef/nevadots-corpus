---
id: collect-261121-veille-ia/bluetooth-audio/bluetooth-hdt-approved-in-bluetooth-7-0-lossless-wireless-audio-is-coming-but-only-with-new-hardware
title: "Bluetooth HDT Approved in Bluetooth 7.0: Lossless Wireless Audio Is Coming, but Only With New Hardware"
domain: bluetooth-audio
role: reference
task: article
actors: []
dates: []
keywords: ["bluetooth", "audio", "lossless", "mbps", "wireless", "only", "data", "optional"]
source: https://pbxscience.com/bluetooth-hdt-approved-in-bluetooth-7-0-lossless-wireless-audio-is-coming-but-only-with-new-hardware/
source_anchor: ""
source_lines: [1, 44]
section: "Bluetooth_Audio"
---
## Bluetooth HDT Approved in Bluetooth 7.0: Lossless Wireless Audio Is Coming, but Only With New Hardware

The Bluetooth Special Interest Group (SIG) announced on November 4 that it has formally adopted Bluetooth High Data Throughput (HDT), an optional feature of the newly adopted Bluetooth Core 7.0 specification. It raises the top data rate of Bluetooth Low Energy (LE) from 2 Mbps to 7.5 Mbps, nearly a fourfold jump, and paves the way for standardized lossless audio on wireless headphones. There is a catch: you will need new phones and new headphones to benefit.

### What HDT Actually Changes

Since LE 2M arrived in 2016, the fastest Bluetooth LE radio mode has been 2 Mbps. HDT stays in the same 2.4 GHz band but packs more bits into each radio symbol, using higher-order modulation and error correction. The SIG says the result is faster transfers, more capacity, better energy efficiency and more reliable connections. Because the radio can finish sending data sooner, it can spend more time in low-power states.

The bandwidth is what makes lossless and high-resolution audio practical over a standard Bluetooth link, without relying on each company’s proprietary codec. The SIG says the first major application will be standardized premium wireless audio, covering lossless and high-resolution playback, multichannel surround sound and spatial audio, and it expects that audio standard to be released in 2027. Beyond headphones, the SIG points to smartwatches syncing workout data faster, smart glasses offloading data for AI processing, and quicker firmware updates for smart home devices.

### Who Is Ready and Who Isn’t

**Qualcomm** is furthest ahead. Its FastConnect 8800 connectivity chip, unveiled at Mobile World Congress in March, supports HDT. The chip is the connectivity system in the Snapdragon 8 Elite Gen 6 and Snapdragon 8 Elite Extreme Gen 6 platforms that Qualcomm introduced on September 22, with flagship phones from brands including Xiaomi, OnePlus, OPPO and vivo due to follow.

**Nordic Semiconductor**, a major maker of Bluetooth chips, announced that it was the first to prove HDT in silicon, and it plans to demonstrate the technology at electronica 2026 in Munich.

**Samsung** has publicly backed the feature: Joonsuk Kim, an executive vice president at Samsung Electronics’ chip business, was quoted in the SIG’s announcement. However, Samsung has not announced any HDT-capable Galaxy product. Its newly launched Galaxy Buds On, the company’s first clip-on earbuds, use Bluetooth 6.2.

**Apple** has made no public HDT commitment. AirPods currently rely on the lossy AAC codec, commonly cited at around 256 kbps, and only the USB-C AirPods Max supports lossless audio, over a wired connection. **Sony’s** LDAC tops out at 990 kbps and is also lossy; there is no announced HDT plan.

### How This Compares With Today’s Options

Qualcomm’s aptX Lossless is one of the few ways to get CD-quality audio over Bluetooth today, but both the phone and the headphones must support it. Support is thin: iPhones support neither aptX nor LDAC, and Sennheiser has said that fewer than 1 percent of current smartphones work with aptX Lossless. The appeal of HDT is that it is part of the core Bluetooth specification, so lossless audio should no longer depend on which brands you mix.

### Three Things to Know Before Buying

1. **7.5 Mbps is a ceiling, not a guarantee.** HDT defines five speeds. The three lowest are mandatory for devices that implement HDT, while the two highest are optional, so manufacturers can leave them out. These are raw radio rates, not what an app will see in practice.

| HDT mode | Speed | Support | 
|---|---|---|
| HDT2 | 2 Mbps | Mandatory | 
| HDT3 | 3 Mbps | Mandatory | 
| HDT4 | 4 Mbps | Mandatory | 
| HDT6 | 6 Mbps | Optional | 
| HDT7.5 | 7.5 Mbps | Optional | 

1. **“Bluetooth 7.0” alone is not enough.** HDT is an optional feature within Core 7.0, so a product can carry the 7.0 label without supporting it. Check that the specifications explicitly list HDT.
2. **Both devices must support it.** If either the phone or the headphones lack HDT, the connection falls back to standard Bluetooth LE. Existing phones and headphones will not gain HDT through updates, so new hardware is required.

### When Can You Buy It?

No specific headphone model has been confirmed to support HDT. The SIG’s 2027 timeline refers to the audio standard, which is not the same as a compatible product reaching store shelves. Some outlets expect early devices at CES in January 2027, but that is speculation, and no company has made an official commitment.

**Bottom line:**HDT is a real, formally adopted upgrade with chips already announced, but lossless audio on mainstream Bluetooth headphones is a 2027 story. For now, check the spec sheet for HDT rather than the version number alone.
