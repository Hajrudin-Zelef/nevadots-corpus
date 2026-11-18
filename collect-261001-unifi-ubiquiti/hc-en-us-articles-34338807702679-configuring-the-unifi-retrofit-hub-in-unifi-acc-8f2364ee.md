Configuring the UniFi Retrofit Hub in UniFi Access
The UniFi Retrofit Hub (UA-Retrofit-Hub-2) lets you upgrade your existing access control system without replacing your wiring. It works with both third-party Wiegand readers and UniFi Retrofit Readers (UA-Retrofit-Reader). With this setup, you can provide secure door access using NFC cards, Touch Pass, PIN, Mobile Unlock, or Remote Unlock, while managing everything through the UniFi Access platform.
This article guides you through configuring the Retrofit Hub and connecting it to a Wiegand or Retrofit Reader. If you're looking to configure UniFi Access Control Hubs, click here.
| Update your UniFi OS, Access application, and Access devices to the latest versions for the newest features and optimal performance. | 
Overview
- The Retrofit Hub supports up to two doors and two readers (i.e., Retrofit Reader or third-party Wiegand reader).
- Supported door unlock methods:
  - NFC card
  - PIN (supported only on certain Wiegand readers)
  - UniFi Endpoint mobile app
    - Touch Pass (available only when the Retrofit Hub is connected to Retrofit Readers)
    - Mobile Unlock (available only when the Retrofit Hub is connected to Retrofit Readers)
    - Remote Unlock
Requirements
- UniFi OS 4.4.6 or later
- UniFi Access 4.1.35 or later
- UniFi Endpoint mobile app
- Retrofit Hub with 12V DC power supply
  - Requires connecting to a UniFi Retrofit PSU 12V (UACC-Retrofit-PSU-12V), or a third-party power supply unit rated for at least 12 V DC 4.2 A (50 W) or 24 V DC 2.1 A (50 W).
- Retrofit Reader (UA-Retrofit-Reader) or a compatible third-party Wiegand reader
Compatible Third-Party Wiegand Readers
HID
- Signo™ Reader 20
- Signo™ Reader 40
- Signo™ Keypad Reader 40 (Supports door unlocking with PIN)
- iCLASS® SE™ R10
- iCLASS® SE™ R15
- iCLASS® SE™ R40
- multiCLASS SE® RP10
- multiCLASS SE® RP15
- multiCLASS SE® RP40
- multiCLASS SE® RPK40 (Supports door unlocking with PIN)
- MiniProx® 5365
Rosslare
- AY-H6255
- AY-K35
- AY-M6255
- AY-Q65 (Supports door unlocking with PIN)
- AY-Q6255
- AYC-E60B (Supports door unlocking with PIN)
- AYC-E60N (Supports door unlocking with PIN)
- AYC-F60
- AYC-Q60
AWID
- SP-6820
Schlage
- aptiQ MT11
ZKTeco
- KR500-M
- KR502-E (Supports door unlocking with PIN)
- KR503-E
- KR602-E (Supports door unlocking with PIN)
Dahua Technology
- DHI-ASR2101A (Supports door unlocking with PIN)
Lenel
- LNL-MT11-485
Installing the Retrofit Hub
For safety, always power off the hub whenever modifying any wiring. See the Retrofit Hub and Retrofit Reader installation guides for more wiring information.
- Power the Retrofit Hub using a Retrofit PSU 12V or a third-party 12V DC adapter.
- Connect the hub to your console or switch.
- Connect the Retrofit Reader or third-party Wiegand reader to the hub's READER terminals.
- Connect the lock, door position sensor, request-to-exit, and auxiliary devices to the LOCK, DOOR POSITION, EXIT REQUEST, and AUX terminals, respectively.
Connecting a Reader to the Retrofit Hub
For Retrofit Reader
- Wire the Retrofit Reader directly to the Retrofit Hub's READER terminals. See the Retrofit Hub and Retrofit Reader installation guides for more wiring information.
  - Reader GND → Hub GND
  - Reader 12V → Hub 12V
  - Reader A → Hub D1/A
  - Reader B → Hub D0/B
- Once connected, follow the steps in the "Configuring the Retrofit Hub Terminals" section below to discover the reader in the UniFi Access application.
For Third-Party Wiegand Reader
- Wire the Wiegand reader directly to the Retrofit Hub's READER terminals. See the Retrofit Hub installation guide for more wiring information.
  - Reader Data 0 → Hub D0/B
  - Reader Data 1 → Hub D1/A
  - Reader RLED → Hub RLED
  - Reader GLED → Hub GLED
  - Reader +VDC → Hub 12V
  - Reader GND → Hub GND
  - Reader BEEPER → Hub BEEP
- Configure the output data on your Wiegand reader platform. Here we use the HID Reader Manager as an example.
  - Open the HID Reader Manager app on your mobile device.
  - Navigate to Home > Scan For Readers > 1. Inspect > SOFTWARE CONFIGURATION > Detailed Configuration > READER SETTINGS > ISO14443A UID Output Format > Add to the template > Apply Selected Items.
- Once configured, follow the steps in the "Configuring the Retrofit Hub Terminals" section below to select the card format for the Wiegand reader in the UniFi Access application.
Configuring the Retrofit Hub Terminals
- Navigate to Access application > Devices and adopt the UA Retrofit Hub 2.
- Navigate to Access application > Devices > UA Retrofit Hub 2 > Terminal.
- 
Reader 1 and Reader 2 settings. Ensure the readers are already connected to the hub.
  - 
UA Retrofit Reader: Click Discover Reader if the connected Retrofit Reader is not detected and adopted automatically.
    - During discovery, the reader will be temporarily unavailable for card reading.
    - If the reader is not discovered or adopted, it will not appear in Access application > Devices.
  - 
Wiegand Reader: Select the Card Format. Ensure that the output data format is configured on the third-party Wiegand reader’s platform. The selected card format must match on both the reader and the Access application.
    - Raw Data: (Recommended) Sends the card’s data exactly as it is, with no formatting.
    - Wiegand 26-bit (H10301): Common format that uses both a facility code and a card number.
    - Wiegand 34-bit (H10306): Extended format with a facility code and a much larger card number range.
    - 
Others
      - Wiegand 26-bit without Facility Code: Same as 26-bit, but only uses the card number (up to ~65k unique IDs).
      - Wiegand 34-bit without Facility Code: Same as 34-bit, but only uses the card number (up to ~16M unique IDs).
- 
UA Retrofit Reader: Click Discover Reader if the connected Retrofit Reader is not detected and adopted automatically.
- 
Door 1 and Door 2 settings:
  - Lock: Choose how long the door remains unlocked (1 second to 1 minute).
  - Door Position: Connect a door position sensor to monitor whether the door is open or closed.
  - 
Exit Request:
    - Unlock Door: Connect a motion sensor, push-to-exit button, or panic bar to unlock the door or allow easy egress.
    - Ring/Trigger Chime: Connect a doorbell button and chime for audible alerts when the button is pressed, notifying personnel of a visitor’s presence.
- 
AUX output settings:
  - Door Opener: Configure automated door opening and Delay Door Opening.
  - Siren: Set the alarm to trigger Instantly, after a delay (5 seconds to 1 minute), or Never Trigger Alarm.
  - Chime: Set the ring duration (0.1 to 10 seconds).
- 
Emergency: Choose between the Evacuation or Lockdown mode.
  - (Optional) Select Activate This Mode for All Locations at This Site to lock/unlock all doors simultaneously.
- Click Apply Changes.
FAQs
Can doors still unlock if the console is offline or malfunctions?
Yes. NFC credentials are stored locally on the Retrofit Hub. As long as the credentials have already been synced to the hub, users can continue unlocking doors even if the connected console malfunctions or loses internet connection.
Can I control two doors using one reader?
No. The Retrofit Hub's Reader 1 controls Door 1, and Reader 2 controls Door 2 independently.
Can I connect other Access Reader models to the Retrofit Hub?
Can I connect the Retrofit Reader to other Access Control Hub models?
No. The Retrofit Reader is only compatible with the Retrofit Hub.
Does the Retrofit Hub support third-party OSDP readers?
No. Currently, the Retrofit Hub only supports compatible third-party Wiegand readers.
How does the Access application communicate with the Retrofit Reader?
Communication flows through the Retrofit Hub. The Access application does not communicate directly with the Retrofit Reader.
Can I use the same NFC card across multiple Retrofit Hubs for door unlock?
Yes. You can, if using Retrofit Readers or compatible third-party Wiegand readers that share the same card format.
Does the Retrofit Hub support gate access?
Yes. The gate can be unlocked using an NFC card, Touch Pass, and Mobile Unlock (Mobile Button or Mobile Tap). License Plate Unlock is not supported at this time.
Are Retrofit Readers automatically paired when connected to the Retrofit Hub?
Yes. Once physically connected and powered, Retrofit Readers are automatically recognized by the Retrofit Hub upon discovery.
How do I configure the Retrofit Reader connected to the Retrofit Hub?
Navigate to Devices > Readers > UA Retrofit Reader > Settings and configure the reader settings:
- Name
- Direction (Entry/Exit)
- Access Methods: NFC Card, Touch Pass, and Mobile Unlock (Mobile Button or Mobile Tap)
- Greeting: Set a custom message and salutation.
- Advanced: Configure Status Light and Status Sound.
For third-party Wiegand readers, all settings must be configured on the reader’s platform, not through UniFi Access.
How do I pair a Protect camera with the Retrofit Hub?
To monitor door activity with a UniFi Protect camera, navigate to Access application > Devices > Hubs > UA Retrofit Hub 2 > Overview > Locations > select a door > Paired Devices.
Does the Retrofit Hub support daisy-chaining readers?
Yes. You can daisy-chain up to two Retrofit Readers per door. If a second reader is added after the Retrofit Hub and the first reader have already been adopted, you will need to run Discover Reader in Access application > Devices > UA Retrofit Hub 2 > Terminal > Reader > Reader Type > Retrofit Reader to detect and adopt the second reader.
The Retrofit Hub does not currently support daisy-chaining Wiegand readers.
Can I add a backup battery to the Retrofit Hub for power redundancy?
Yes. You can connect a 12V SLA backup battery to the Retrofit PSU 12V (UACC-Retrofit-PSU-12V) to keep the Retrofit Hub running during power outages.
What happens to the registered NFC cards if I change the card format?
If the card format is changed in the Access application or on a third-party Wiegand platform, all NFC cards must be re-registered. A mismatch between the reader and card formats will prevent the reader from recognizing the cards, resulting in door unlock failures.
Can I connect a Retrofit Reader and a third-party Wiegand reader to the same Retrofit Hub?
We don't recommend mixing UniFi Retrofit Readers and third-party Wiegand readers on the same Retrofit Hub because NFC card registrations made on readers that use different card formats will not synchronize properly across readers. This can lead to data mismatches and door unlocking failures.
For the best compatibility and performance, you should typically stick to using all UniFi Readers, or all third-party Wiegand readers with the same card format.