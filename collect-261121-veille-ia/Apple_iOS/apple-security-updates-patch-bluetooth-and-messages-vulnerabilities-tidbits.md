---
id: collect-261121-veille-ia/apple-ios/apple-security-updates-patch-bluetooth-and-messages-vulnerabilities-tidbits
title: "Apple Security Updates Patch Bluetooth and Messages Vulnerabilities - TidBITS"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: ["2026-11-09"]
keywords: ["vulnerabilities", "bluetooth", "fixed", "messages", "updates", "apple", "ipados", "macos"]
source: https://tidbits.com/2026/11/09/apple-security-updates-patch-bluetooth-and-messages-vulnerabilities/
source_anchor: ""
source_lines: [1, 26]
section: "Apple_iOS"
---
Two weeks after the OS 27.2 release (see “OS 27.2 Gives Siri AI a Memory and Brings Small Improvements,” 26 October 2026), Apple has published updates to the current versions of iOS, iPadOS, macOS, watchOS, and visionOS, plus several older versions, to address security vulnerabilities focusing on Bluetooth, AirPlay, and Messages.

Although none of the vulnerabilities appear to have been exploited in the wild, I recommend updating soon because the Bluetooth-related bugs add up to a concerning attack vector. Several could let an attacker within Bluetooth range pair a device with yours without any interaction from you. Others bypass the limit on pairing code attempts, which normally prevents malicious apps from guessing every possible code. The last Bluetooth-related vulnerability could let a nearby attacker corrupt process memory, potentially crashing or hijacking an app.

AirPlay gets two closely related fixes. One stops an attacker in Bluetooth range from bypassing the limit on AirPlay pairing code attempts, and the other stops anyone on the same Wi-Fi network from doing the same.

Apple credits one of the Bluetooth pairing bugs to an anonymous researcher and takes credit for the other Bluetooth and AirPlay fixes itself, which makes me suspect the researcher’s report sent Apple’s engineers hunting for related problems. I wouldn’t be surprised if AI code-analysis tools helped with that search.

The Messages vulnerabilities are worrying, too. One could let a remote attacker join an iMessage group chat without authorization and read its messages. Another could allow someone added to an iMessage group to read messages sent before they joined. Neither fix appears in iOS 18.7.12 or watchOS 26.6.1; I suspect those older versions aren’t vulnerable.

iOS 27.2.1, iPadOS 27.2.1, watchOS 27.2.1, and visionOS 27.2.1 (but not macOS 27.2.1?) also fix a vulnerability in App Intents, the system that lets apps offer actions to Siri, Shortcuts, and Spotlight. It could have allowed an app to access sensitive user data.

I recommend prioritizing updates on any device that spends time out in the world, within Bluetooth range of strangers’ devices, or connected to public Wi-Fi networks. The updates are:

- **iOS 27.2.1 and iPadOS 27.2.1** : 9 vulnerabilities fixed
- **iOS 26.7.3 and iPadOS 26.7.3** : 8 vulnerabilities fixed
- **iOS 18.7.12 and iPadOS 18.7.12** : 6 vulnerabilities fixed
- **macOS 27.2.1 Golden Gate** : 8 vulnerabilities fixed
- **macOS 26.7.3 Tahoe** : 8 vulnerabilities fixed
- **macOS 15.8.3 Sequoia** : 8 vulnerabilities fixed
- **watchOS 27.2.1** : 9 vulnerabilities fixed
- **watchOS 26.6.1** : 5 vulnerabilities fixed
- **visionOS 27.2.1** : 9 vulnerabilities fixed


## Start the discussion in the TidBITS Discourse forum
