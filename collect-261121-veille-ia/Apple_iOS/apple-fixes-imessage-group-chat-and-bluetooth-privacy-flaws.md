---
id: collect-261121-veille-ia/apple-ios/apple-fixes-imessage-group-chat-and-bluetooth-privacy-flaws
title: "Apple Fixes iMessage Group Chat and Bluetooth Privacy Flaws"
domain: apple-ios
role: reference
task: article
actors: ["Apple"]
dates: []
keywords: ["bluetooth", "apple", "imessage", "group", "issue", "later", "privacy", "flaws"]
source: https://www.techbooky.com/apple-fixes-imessage-group-chat-and-bluetooth-privacy-flaws/
source_anchor: ""
source_lines: [1, 15]
section: "Apple_iOS"
---
Apple has patched two iMessage group-chat privacy flaws and several Bluetooth pairing weaknesses in a security update for iPhones and iPads. One Messages flaw could have allowed a remote attacker to join a group conversation and receive its messages. Another could have exposed messages sent before a newly added participant joined. Apple published the details with its 9 November release of iOS 26.7.3 and iPadOS 26.7.3.

The company’s security advisory describes the remote-join issue as CVE-2026-113961, an authorisation problem fixed with improved validation. The earlier-message exposure is CVE-2026-113960; Apple says it removed the vulnerable code. Neither description means that every iMessage conversation was accessed. Apple has not said these bugs were exploited in attacks, and its public notice does not provide a victim count.

Apple also lists a CoreUtils flaw that could have let a malicious Bluetooth device establish a pairing without user interaction. A separate CoreUtils logic issue could have let an attacker within Bluetooth range bypass limits on pairing-code attempts and pair without the user’s approval. Other entries address a Bluetooth memory-corruption issue and an AirPlay pairing-rate-limit weakness. These are distinct vulnerabilities with different conditions, not one universal remote takeover method.

The distinction matters for readers assessing their risk. The Messages authorisation issue is described as remotely reachable, while the Bluetooth scenarios depend on proximity or interaction with a malicious device. Apple’s advisory does not claim that the vulnerabilities can be chained together, nor does it say that messages are automatically exposed merely because Bluetooth is switched on.

The fixes apply to iPhone 11 and later and a range of supported iPads, including the third-generation 12.9-inch iPad Pro and later, the first-generation 11-inch iPad Pro and later, iPad Air 3 and later, iPad 8 and later, and iPad mini 5 and later. Users should check their device’s Software Update screen for the version available to them, as the appropriate release can vary by model.

Group messaging carries work discussions, family information and private material that participants reasonably expect to remain limited to the invited members. A flaw allowing an outsider to join a conversation is therefore a privacy issue even without evidence of widespread exploitation. The separate historical-message bug also challenges the assumption that adding someone to a chat cannot expose earlier exchanges.

For organisations that issue iPhones, the update is a prompt to check fleet compliance rather than assume automatic updates have already completed. Staff should install the offered update, allow the device to restart and verify the installed version. Anyone investigating suspicious group membership should review the actual chat participants; the advisory alone is not evidence that a particular conversation was compromised.

TechBooky has previously covered emergency Apple security updates, but this release has its own set of flaws and an important caveat. Apple’s notice confirms the fixes and their potential impact. It does not, at publication, describe an active campaign exploiting these iMessage or Bluetooth issues.
