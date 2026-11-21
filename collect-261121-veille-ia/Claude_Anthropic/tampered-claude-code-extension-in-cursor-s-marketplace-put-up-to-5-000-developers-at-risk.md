---
id: collect-261121-veille-ia/claude-anthropic/tampered-claude-code-extension-in-cursor-s-marketplace-put-up-to-5-000-developers-at-risk
title: "Tampered Claude Code extension in Cursor's marketplace put up to 5,000 developers at risk"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: []
keywords: ["extension", "cursor", "marketplace", "code", "tampered", "anthropic", "wired", "claude"]
source: https://www.egov.mn/en/article/tampered-claude-code-extension-in-cursors-marketplace-put-up-to-5000-developers-at-risk
source_anchor: ""
source_lines: [1, 11]
section: "Claude_Anthropic"
---
A maliciously modified version of Anthropic's **Claude Code** extension (versions 2.1.312 and 2.1.313) was available in **Cursor**'s VS Code Extension Marketplace from about 2:27am to 1:35pm Pacific time on **October 29**, Cursor said in a security notice. The company told reporters the tampered extension was downloaded by **"at most 5,000 users,"** according to WIRED and Ars Technica.

According to Cursor, the extension tried to install a **persistent SSH backdoor** by adding an attacker-controlled SSH key, enabling the local SSH service and opening a reverse SSH tunnel. **Microsoft** security researchers, who analyzed the injected code, said it also collected host details such as the hostname, username and operating system, and the **names** (not values) of environment variables containing terms such as KEY, TOKEN, SECRET or AWS. Ars Technica noted that this would let an attacker inventory which machines held cloud credentials and prioritize follow-up attacks. Bugs in the malware, however, prevented SSH access from working on most macOS and Windows hosts.

"Everyone was so lucky here, this was potentially a really dangerous foothold for an attacker," Calum Hall, CEO of detection firm **Phorion**, which first spotted the extension, told WIRED. How the malicious versions reached the marketplace remains unexplained. Researchers cited by Ars Technica said they were distributed through Anthropic's official account on Cursor. Anthropic's Felix Rieseberg wrote on X that the incident "wasn't a compromise of Anthropic's systems or employee laptops," adding that "a malicious actor used access to Cursor's marketplace to publish a tampered copy" and that Anthropic's official channels were not affected. Microsoft confirmed that the official Visual Studio Marketplace and npm packages were not involved. Cursor, which WIRED notes is owned by SpaceX, did not answer questions about how the upload happened.

Cursor said it removed the tampered versions, worked with the hosting provider to cut off the attacker's server and temporarily shut its extension marketplace to rotate keys and confirm no other extensions were affected. It published checks and remediation scripts for macOS and Windows. Users who installed or auto-updated the extension during the window are advised to check their SSH authorized keys and rotate credentials from a trusted device; Microsoft warned that updating the extension does not remove artifacts already written to a machine.

*Sources: Cursor, Microsoft Security, WIRED, Ars Technica. Image: WIRED*

Get the latest news delivered to your inbox.
