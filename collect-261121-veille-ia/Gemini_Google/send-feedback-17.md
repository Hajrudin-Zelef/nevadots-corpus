---
id: collect-261121-veille-ia/gemini-google/send-feedback-17
title: "Send Feedback"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["drive", "firmware", "fortigate", "file", "connect", "device", "send", "feedback"]
source: https://docs.fortinet.com/document/fortigate/7.4.5/administration-guide/183352
source_anchor: ""
source_lines: [1, 22]
section: "Gemini_Google"
---
# Restoring from a USB drive

# Restoring from a USB drive

The FortiGate firmware can be manually restored from a USB drive, or installed automatically from a USB drive after a reboot.

###### To restore the firmware from a USB drive:

1. Copy the firmware file to the root directory on the USB drive.
2. Connect the USB drive to the USB port of the FortiGate device.
3. Connect to the FortiGate CLI using the RJ-45 to USB (or DB-9) or null modem cable.
4. Enter the following command:execute restore image usb <filename> The FortiGate unit responds with the following message: This operation will replace the current firmware version! Do you want to continue? (y/n)
5. Type `y` . The FortiGate unit restores  the firmware and restarts. This process takes a few minutes.
6. Update the antivirus and attack definitions:execute update-now

###### To install firmware automatically from a USB drive:

1. Go to *System > Settings* .
2. In the *Start Up Settings* section, enable*Detect firmware* and enter the name of the firmware file.
3. Copy the firmware file to the root directory on the USB drive.
4. Connect the USB drive to the USB port of the FortiGate device.
5. Reboot the FortiGate device.
