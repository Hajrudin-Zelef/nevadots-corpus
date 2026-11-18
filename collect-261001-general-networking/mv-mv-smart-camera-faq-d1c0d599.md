MV Smart Camera FAQ
Overview
This is a summary of the frequently asked questions on MV smart cameras. It is divided into the following sections:
MV52 (Bullet) Camera FAQs
How do I factory reset an MV52?
The factory reset button for the MV52 is located beneath the top hood of the camera. To access it, slide the hood away from the front lens in the direction of the mounting plate. Next, slide/remove the longer cover located under the hood. Once removed, you will find a pinhole that allows you to perform a factory reset on the camera.
It may require significant force to slide the plastic hood away from the camera body.
What firmware should the MV52 be on?
The MV52 must operate on the most recent Beta Firmware version.
My MV52 is out of focus
Drag the focus motor slider to its maximum position at the far end, then adjust it to the near end. After completing this, initiate the AutoFocus function for the entire frame. If the issue persists, contact the support team for further assistance.
My MV52 is extremely hot. Is this expected?
The MV52 has a high processing power as it tries to support all the latest intelligence features, 4K, HDR, and even long-range IR. Due to this, the thermal temperature on the camera is always high. Ensure safety while deploying it and keep it away from flammable materials even while deployed.
Does my MV52 connect wirelessly to the Access Point?
The MV52 supports 5GHz wireless connectivity exclusively and requires a 12V DC PoE adapter for operation.
My MV52 abruptly went offline? My MV52 seems to lose connectivity only during the night?
MV52 is powered by an 802.3at PoE+ connection with 30W of power wholly dedicated to its port. Certain non-enterprise switches display a maximum of 60W of power within their datasheet but share this load among other connected devices. Such situations can disallow the MV52 from coming online, especially during the night for IR consumption.
While running an export for 1 hour on my MV52, it tends to not record for a certain period?
Due to the processing limitations of the MV52, exports can become demanding, especially when a significant amount of motion is captured in each frame. To ensure smooth processing, consider breaking the export into smaller parts.
My MV52 video streaming is very unstable and choppy?
Adding a single MV52 video stream at 4K consumes approximately 8 Mbps for one tile on the video wall. Adding four tiles would require around 32 Mbps of WAN bandwidth for the video wall to operate.
- Ensure your uplink supports high-bitrate streaming, and verify that both your browser and drivers are up to date.
- The latest generation hardware must be used for 4K viewing.
IR reflection
To prevent the issues described below, ensure the camera is mounted in a way that avoids streetlights reflecting off the camera's IR LEDs or the presence of reflective surfaces, such as positioning it in front of a glass windowpane, etc.
MV32 (Fisheye) Camera FAQs
The Meraki MV32 is a fisheye camera equipped with the same edge analytics capabilities built into other second-generation cameras (MV12, MV22, and MV72) and stocked full of many new and exciting features. To get details on the camera’s hardware and resolution specifications, check out the datasheet.
Is the MV32 a Pan, Tilt, Zoom (PTZ) Camera?
The MV32 is a fisheye camera with a 180-degree view of the surrounding area. The term fisheye comes from the camera’s use of a fisheye lens which creates a wide-angle hemispherical image. Using digital zoom, the user can virtually/digitally pan, tilt, or zoom (DPTZ) around the scene without losing any information from areas they are currently not viewing. In the case of PTZ cameras, the only area being recorded is presently in view.
Do I require any special equipment or apps to view the video?
To view the video, log in to the dashboard and access the camera from your camera management page. Fisheye and DPTZ modes can be viewed without any special equipment. Virtual Reality (VR) mode is officially supported on standalone VR headsets, such as the Oculus Go, and is unofficially supported on mobile browsers when used with a mobile VR headset, such as Google Cardboard. To use VR mode, access the dashboard from the device and follow the steps outlined in this article.
What storage capacity does the MV32 have?
The camera has 256 GB of storage. 128 GB storage options are not available.
What video retention period estimations can I expect?
The MV32 runs a high-resolution video stream, boasting more than twice the recording resolution of the MV12. With 256 GB of storage, we can expect up to 20 days of video storage depending on the video quality when recording 24/7. To obtain more effective retention rates, enable motion-based retention or cloud archive.
Can I place the MV32 outdoors?
The MV32 has no weather-resistance rating and should be kept in a temperature and humidity-controlled indoor environments. The MV72, an IP67 weather-resistant varifocal camera, can be used for scenarios requiring a smart security camera outdoors.
Does the MV32 support wireless configuration?
Like other second-generation cameras, the MV32 can run through a wireless connection once it is configured and supplied with power. See this article for more information.
Does the MV32 have night vision?
The MV32 does not have night vision. It contains no built-in IR illuminators for very dark scenes. However, it has an IR-cut filter that allows it to be more sensitive to lower-light scenes.
Can I view multiple angles from a single MV32 in a video wall?
Yes! You can. read more here.
Can I mount the MV32 on the wall?
If placed on a wall, half of the camera’s field of view will face the perpendicular ceiling. This does not properly use of the 360-degree field of view of this camera. The MV32 is better installed on a ceiling, where the more of the field of view captures useful content. Additionally, the digital pan-tilt-zoom (DPTZ) feature of the MV32 is currently configured for ceiling mounting.
Furthermore, the camera’s computer vision analytics function on training algorithms based on a top-down view of its surroundings. Orientations differing from this model will limit the performance of the camera’s analytics. Therefore, it is not recommended to install the MV32 perpendicular to the ground.
What happens if I place the camera on a surface higher than the recommended 10-14 ft?
Mounting the MV32 higher than the recommended maximum height of 14 feet will result in decreased image quality of the surrounding area and impact the performance of the built-in people detection analytics. Placement at this height will enable an overview of larger areas but impact the amount of recognizable detail in the image. If your surface is very high, you can use the Telescoping Pendant Mount to lower the mounting height of the camera.
Can I replace all my cameras with a single MV32?
The MV32 is a great option to deploy if you desire a broad overview of the scene and to gain situational awareness. In general, we recommend utilizing the fisheye camera to detect an event of interest, and a fixed lens or varifocal camera to identify key details of the scene.
My MV32 gets very hot, is this normal?
It is normal for the camera to get warm in operation. Ensure the camera is properly deployed with the included mount plate, which is an important part of cooling the camera.
Vision Portal FAQs
What additional features does Vision Portal have over Dashboard?
- You can zoom into every tile by clicking on the enlarge icon on the Video wall on Vision
- You can create Multi-network video walls on Vision
How can the Vision Portal be accessed?
- 
    By clicking on the Camera Tab -> Monitor -> Vision Portal
- 
    Visiting vision.meraki.com
- 
    Progressive Web Application (once downloaded)
What is a progressive web application?
A progressive web app (PWA) can run on a browser and be downloaded to run like a native app. It includes elements such as the service worker and a pre-caching dB.
What is the advantage of a PWA?
- 
    Reduced wait time for page load
- 
    Resilience in network conditions
- 
    Give a smooth and seamless experience in the browser
- 
    Give a native feeling experience with a small footprint when installed
What is the update button used for?
If the "Update" button isn't manually clicked, the app will eventually update to the latest version after a relaunch of the interface (assuming no other Meraki Vision applications are open). The button exists for those who wish for the update as soon as it is available.
How do I ensure my Vision Portal is on the latest release?
Ensure the application is upgraded to the latest version:
- 
    Restart the application to load the latest version
- 
    Command + shift + R on the keyboard should help reload it
- 
    One can check the currently running version by pressing the What's New panel on the top of the UI.
Can Cisco Meraki Support Engineers view my Vision Portal?
By Default, Cisco Meraki Support Engineers cannot view video or hear the audio in the Vision Portal. You may choose to allow temporary access to the Portal by following this document.
What devices will be supported?
No support within the Application Store for iOS or Android.
- 
    iPad’s (CANNOT be installed but can be pinned from Dashboard)
- 
    PC’s
- 
    Laptop
What browsers will be supported?
- The Vision Portal will always open in your default web browser when navigating from Dashboard
- The Web Application will be supported on the latest versions of Chrome / Safari and Firefox (recommended: Chrome). In most cases, compatibility will extend one version behind the most recent release.
I keep getting logged out of the Vision Portal. Help!
Today the Vision Portal uses the Organization Timeout that has been set on the Dashboard. If you have nothing set but you still keep getting logged out, please create a support ticket.
MV Sense FAQs
What is MV Sense?
It is a set of APIs (both RESTful and MQTT-based) that allow our customers to integrate the edge-computing capabilities of the MV into third-party business solutions. It is best not to call it an “open” API because it requires a license.
What is an API?
An API is a set of routines, protocols, and tools that allows applications to communicate with each other. Essentially, an API is a messenger that will deliver requests and responses from the provider you want to talk to. For example, you can request a list of zones from a specific camera from one of the MV Sense APIs, and the API will return this list to you. This greatly simplifies application development requiring information from external sources.
I don’t see the option for “Sense” on my camera’s settings. How do I use MV Sense?
Ensure the camera is a second-generation (MV*2) or later and on MV 3.22 firmware or later.
How does MV Sense work?
Several APIs are available through MV Sense which is outlined in this article. You can obtain three types of data insights through the MV Sense APIs about the people detection metadata generated through the MI edge analytics running on a second-generation Meraki camera. These APIs can be summarized as follows:
- 
    Historical Aggregate: How many people were here at X time?
- 
    Current Snapshot: How many people are here now?
- 
    Real-time Feed: A sub-second feed of people and their locations.
For more information, see the page on MV Sense on the Meraki Developer Portal.
Are there specific applications we recommend to integrate with MV Sense?
No. We provide a suite of APIs that customers can build solutions on top of. For more information, see the page for MV Sense on the Meraki Developer Portal. Some examples of current solutions using MV Sense can be viewed at the Meraki Marketplace. Furthermore, we are working with Cisco DevNet and other partners to develop more turnkey solutions.
How does licensing work?
MV Sense licensing works on a per-camera basis (it does not co-terminate). Each organization (new or old) with second-generation cameras (trials included) should already have ten free MV Sense licenses. These licenses do not expire.
Can I use MV Sense to funnel video to another application?
No. It is a way to leverage the MV’s people detection capabilities for additional analytics, triggers, or other desired customer applications.
MV Cloud Archive FAQs
How does it work?
Refer to this article on MV Cloud Archive. When enabled, the dashboard will automatically pull the footage from the onboard storage (if available) or the cloud archive without any customer intervention.
Can we assume the same secured access and encryption with Cloud Archive as with onboard storage?
Yes. Video uploaded to Cloud Archive is over an SSL/TLS connection. Stored video is also encrypted at rest. All rigorous security standards for the dashboard outlined here apply for Cloud Archive as we take the privacy of customer video very seriously.
Does the cloud archive store the analytics as well?
Yes.
Does this mean I can bulk offload video in private servers or third party software?
No. The interface for the customer will be almost exactly the same as without cloud archive. The user will still be able to do 1-hour video exports, but no bulk offloading.
How does licensing work?
It is available in 30-day, 90-day, 180-day, and 365-day storage license options, with 1-year, 3-year, and 5-year variants, applied on a per-camera basis (licenses do not co-terminate). The licensing is independent of the camera type and remains consistent across all fixed dome, varifocal, and fisheye cameras.
Do we provide licenses for trial?
No.
MV Audio Detection FAQs
How does it work?
Software on the camera processes the input audio stream, using windowed sub-sampling. These samples are then converted into visual representations of the input audio signal and processed by a model trained for specific audio classes. These audio detections can be streamed for detailed analysis and storage via MQTT. This functionality requires a MV Sense license to be applied to the camera. Our audio detection is driven by computer vision and machine learning.
What do you mean by "machine learning"?
Meraki smart cameras use deep learning, a type of machine learning at the forefront of artificial intelligence research, to drive our computer vision object detection. The smart camera development teams continually show a computer thousands of examples of what objects look like and it "learns" how to identify them more and more accurately over time. The model improves as we provide it with additional training data.
The smart camera analytics models are trained on data that is legally owned or licensed by Cisco Meraki, and only uses data from customers who have explicitly opted-in to continually improve our models.
Does this take any extra bandwidth? Do I need another server?
All audio processing is performed directly on the camera's processor. As with all Meraki products, the hardware transmits small amounts of metadata to the dashboard for further processing and storage. Additionally, MQTT messaging data can be sent to any MQTT broker resource of your choice.
MV Encoding Improvements: Smart Codec FAQs
What is a codec?
A codec is a software tool that encodes or decodes a data stream, such as video. Meraki MV smart cameras utilize the H.264 codec. Starting with firmware version MV4.15, MV cameras will begin using Smart Codec.
How do we deliver more video retention?
Smart Codec analyzes a scene in real-time and dynamically optimizes video retention and playback. It observes various levels of motions and other parameters to apply corresponding levels of compression logic to the video files by intelligently determining which scenes require more compression and which scenes do not.
How much can video retention improvement be expected?
As the Smart Codec applies various compression algorithms based on the amount of motion detected, the video retention noticed on the cameras will also depend on the motion detected. For most cameras, the results show double the improvement in video retention as of November 2021.
How is this going to affect the video quality?
The Smart Codec will perform various video compressions while maintaining perceivable video quality.
Which MV cameras will leverage the new Smart Codec?
All second-generation MV cameras (MV12, MV2, MV22(x), MV32, MV52, MV72(x)) on MV4.15+ will support Smart Codec.
Firmware MV4.18.1 does not support Smart Codec, however firmware MV4.18 does.
Third-generation MV cameras (MV13, MV33, MV63(x), MV93(x) will use Smart Codec as the default encoding.
Will Gen1 Cameras support Smart Codec?
Gen 1 cameras will not support Smart Codec.
MV Intelligence Training FAQs
How does it work?
Every Meraki camera transmits metadata related to object detection, motion, and light levels. Once a camera is registered into Intelligence Training, software on the Meraki Backend monitors for interesting events from cameras using only this metadata. This software will selectively collect a few frames for each interesting event and send this to our secure cloud storage for automatic annotation and training. Data is encrypted at all stages of the process, and strict access policies are in place across all resources. For information on our datacenters, storage architecture, regulatory compliance, and otherwise, please see our Trust page at https://meraki.cisco.com/trust.
Can I stop sharing data from a camera?
You can choose to stop sharing data from a camera or delete previously shared data at any time.
How do I opt in to Intelligence Training?
You can opt in to Intelligence Training by following the steps outlined here
How do I opt out to Intelligence Training?
If you prefer not to participate, you can opt out and disabling Intelligence Training at any time by following the steps process here
What do you mean by "machine learning"?
Meraki smart cameras use deep learning, a type of machine learning at the forefront of artificial intelligence research, to drive our computer vision object detection. The smart camera development teams continually show a computer thousands of examples of what objects look like, and it "learns" how to identify them more accurately over time. The model improves as we provide it with additional training data.
The smart camera analytics models are trained on data that is legally owned or licensed by Cisco Meraki, and only use data from customers who have explicitly opted-in to continually improve our models.
Does this take any extra bandwidth?
Data collection for MV Intelligence Training will use additional bandwidth beyond the normal camera operations. This process is designed to collect up to 3 GB of data per day. Once a diverse set of images has been gathered, data collection from the cameras will cease.
What is changing about Camera Intelligence Training?
To keep pace with advancements in AI and to continue delivering the highest quality models for our customers, we’re updating our data collection limits. Starting July 18th, the daily limit for MV Intelligence Training data collection will increase from 200MB to 3GB per day. This new limit represents a maximum threshold and is not one we anticipate reaching regularly. Our commitment remains to responsible data use while ensuring our AI solutions evolve to best serve your needs.
Vision Portal: Event Search FAQs
How do I change my timezone?
Contact your network admin to update the network timezone
What if I notice a lot of False positives?
For false positives on people or vehicle filters, note that our camera intelligence modeling improves with each new generation of our cameras. The best performance can be observed on our latest Gen 3 models, including the MV63 and MV93. We are actively working to enhance this experience! (Object Detection Documentation here).
Participate in our intelligent training to help make this even more accurate.
How can you sign up for the Intelligence Training?
You can sign up through the banner that appears on your network or by navigating to Configure -> Intelligence Training.
Third Generation MV Cameras: Overview and Specifications FAQs
How do I know if the MV is using a wireless connection?
The Status LED on every MV model will display a solid Blue LED if the device is connected using a wireless connection and a solid Green LED when using a hardwired connection.
How do I know if the MV is recording Audio?
Any MV that has audio recording enabled will always display a solid Purple LED to indicate it is recording audio, regardless of the connection type used by the device.
MV People Detection FAQs
How does it work?
Software on the camera analyzes images multiple times per second and identifies where people are located. The camera then tracks the location of these people over time to understand when they came, stayed, and left. The camera rolls up its findings and reports them to the dashboard, where you can view the data in a summary form. Our object detection is driven by computer vision and machine learning.
What do you mean by "machine learning"?
Meraki smart cameras use deep learning, a type of machine learning at the forefront of artificial intelligence research, to drive our computer vision object detection. The smart camera development teams continually show a computer thousands of examples of what people look like and it "learns" how to identify them more and more accurately over time. The model improves as we provide it with additional training data.
Right now the detector is trained on data from a small number of cameras. The smart camera development team expects to deploy a lot more of these cameras, and plans to use data from opted-in customers and all of their diverse deployment scenarios to continually make the model better.
Does this do person identification or facial recognition?
No. Meraki smart cameras don't identify or track specific individuals, they just detect them.
Does this take any extra bandwidth? Do I need another server?
No! All of the image processing is done right on the camera's processor. Like all Meraki products, the hardware sends small amounts of metadata back to the dashboard for further processing and storage.
Why am I not getting the numbers I would expect?
Once a camera is physically installed in a proper location and adjusted for the correct FoV, the MV will automatically start to gather analytics data. For more information on installing a MV camera to optimize people detection performance, see the deployment guidelines below. In any deployment, you should use the data comparatively for observing trends and anomalies, as opposed to using it as absolute measurement. Refer to the People Detection Features > Entrances section above for some more explanation.
How can I troubleshoot MV people detection?
There are no public facing logs to troubleshoot the analytics tools. You can use the "Show People” tool when viewing historical footage to get a rough idea of what the MV people detection model is detecting.
MV Object Detection FAQs
How does it work?
Software on the camera analyzes images multiple times per second and identifies where objects are located. The camera then tracks the location of these objects over time to understand when they entered, where they went within view, and when they left. The camera rolls up its findings and reports them to the dashboard, where you can view the data in a summary form. These sub-second detections can also be streamed for detailed analysis and storage via MQTT. This functionality requires a MV Sense license to be applied to the camera. Our object detection is driven by computer vision and machine learning.
What do you mean by "machine learning"?
Meraki smart cameras use deep learning, a type of machine learning at the forefront of artificial intelligence research, to drive our computer vision object detection. The smart camera development teams continually show a computer thousands of examples of what objects look like and it "learns" how to identify them more and more accurately over time. The model improves as we provide it with additional training data.
The smart camera analytics models are trained on data that is legally owned or licensed by Cisco Meraki, and only uses data from customers who have explicitly opted-in to continually improve our models.
Does this do person identification or facial recognition?
No! Meraki smart cameras do not identify or track specific individuals. Persons are detected as objects (if the analytics model supports it) and tracked in the scene until the object leaves the camera's field of view.
Does this take any extra bandwidth? Do I need another server?
No! All of the image processing is done right on the camera's processor. Like all Meraki products, the hardware sends small amounts of metadata back to the dashboard for further processing and storage.
Why am I not getting the numbers I would expect?
Once a camera is physically installed in a proper location and adjusted for the correct FoV, the MV will automatically start to gather analytics data. For more information on installing an MV camera to optimize object detection performance, see the deployment guidelines below. In any deployment, you should use the data comparatively for observing trends and anomalies, as opposed to using it as an absolute measurement. Refer to the Object Detection Features > Entrances section above for some more explanation.
How can I troubleshoot MV object detection?
New in June 2020, you can now view the detailed output of the running object detection model. This new capability is instrumental to advanced troubleshooting and debugging of applications consuming this data via either MQTT or Dashboard API calls.
An MV Sense license is required to be applied to a MV camera to access the advanced analytics debug mode.
In order to access the advanced analytics debug mode, navigate to the "Show People” tool when viewing historical footage. You should now see an additional toggle for enabling/disabling the debug mode overlay. The debug mode will render each detected object's Object ID #, confidence %, and bounding coordinates (X0,Y0),(X1,Y1).
If there are no detections on the camera and no MQTT output for a camera, please ensure the camera is receiving the correct PoE power as stated in its datasheet. To see if you're hitting this issue, search the Event Log for Event Type "PoE power error. Incorrect PoE standard detected." which will be logged when the camera boots.
External RTSP
Will 3rd Parties Be Able to Store Video?
Once the RTSP stream leaves our ecosystem, the 3rd party system can handle the video however it would like.
What Type of Stream Delay Can Be Expected?
Since RTSP is not using HTTP Live Streaming (HLS), which is what is used on the dashboard, the delay will be significantly reduced. Enabling RTSP will not affect the dashboard delay for video streaming.
How Do I Secure the RTSP Stream?
While the External RTSP stream itself may not be secured or encrypted, there are various configurations that can secure access to the cameras. Some examples of these configurations are:
- Placing the cameras on a separate VLAN
- Configuring port isolation
- ACLs on the switches or edge firewall to allow/deny traffic to the cameras
Configuring Camera FOVs on a Floor Plan FAQs
My camera network is already set up with cameras placed on our floorplan. How will this update affect my cameras?
- A: If your cameras have already been placed on a floorplan, they will have their default FOV cones represented facing South:
 
 
 You can simply rotate them to the appropriate coverage angle and save your configuration changes.
 
First Year on Us FAQ
Sales Team FAQs
How do I add the offer in CCW?
This promotion will not appear in CCW and will be applied automatically. There is no change from your current deal registration process on any Meraki SKUs in CCW for this promotion.
Is this offer stackable with other Cisco partner promotions?
Yes, this promotion is stackable with all existing Cisco partner programs.
General MV FAQs
Where is video stored?
Video is stored on the camera itself. You also have the option of using Cloud Archive for continuous 24/7 recording for 30, 90, 180 or 365 days. Read the datasheet and this article to learn more.
How much footage can I store?
Retention day estimates based on quality settings can be found in this article. You can also read about different ways to extend your storage on that page.
What if I need longer storage?
You have the option to use Cloud Archive for continuous 24/7 recording, with storage durations of 30, 90, 180, or 365 days. To learn more, please refer to the datasheet and this article.
For customers in Europe or regions with strict storage limitations, the dashboard includes a setting to automatically discard footage after a specified number of days.
Lastly, if the recording options on the dashboard or Cloud Archive do not meet your needs, we also offer External RTSP, which allows footage to be sent to a third-party system. Read more here.
Is my video stored in the cloud?
Standard streaming video is only stored in the cloud when using Cloud Archive. In standard operations, streaming video is not stored in the cloud, but exported video clips are stored in the Meraki cloud ecosystem for a year. Metadata for video thumbnails and motion indexing are also stored in the cloud. Video will proxy through the cloud in the event of remote streaming and is cached in order to retrieve footage more quickly, but during standard recording operations, MV cameras will only save video locally.
Can I choose to backup my video in the cloud?
Yes. You have the option of using Cloud Archive for continuous 24/7 recording for 30, 90 , 180 or 365 days. Due to bandwidth considerations of storing video in the cloud, we recommend only using Cloud Archive on critical cameras. Read the datasheet and this article to learn more.
What resolution do these cameras record in? What is the frame rate?
Tables of resolutions, frame rates, and respective estimated retention days for each MV camera can be found in this article.
What happens if somebody steals my camera? What if it’s damaged or there’s a fire?
As with all security camera systems, care should be taken during installation to ensure cameras are out of reach of potential thieves or vandals. Cameras are rarely stolen and should be positioned out of easy reach. If a camera is stolen, its footage can be retrieved from anywhere when it comes back online thanks to the cloud proxy. This way you can easily retrieve the footage of it being stolen while also watching the live feed of the person using the camera.
Our IP67 rated MV72 should be used in areas where there is a high risk of theft/vandalism. Using motion alerts and/or the snapshot API when reasonable, in conjunction with other access control measures can help mitigate risk. Cloud Archive can be used also provide additional assurance for sensitive or at-risk cameras.
Like most other systems, in the event of a fire, if cameras become damaged to the point of inoperability, footage will be lost. All security camera systems carry some amount of risk, but unlike a deployment with a recording device, there is no single point of vulnerability in an MV deployment since each camera stores its own video.
Is stored video encrypted on the camera?
Yes, MV features full disk encryption. Additionally, cameras automatically purchase and provision their own publicly-signed SSL certificates upon initial boot up. Management data is encrypted as well. All encryption is turned on by default, and cannot be turned off.
Does my SSL certificates renew automatically?
Yes, MV cameras will automatically renew and provision their own publicly-signed SSL certificates, without any manual intervention.
What happens when a camera is removed from a network?
Removing a camera from a network deletes all the footage on the camera.
What if a camera loses access to the network?
MV cameras will continue to record, even if they lose access to the network, as long as they have power.
What will happen if the camera loses power?
Cameras will not operate without power. A switch with redundant power supplies is recommended if there is a concern about power loss.
Is there any way to back video up onto an NVR, DVR, NAS, or other centralized storage device?
The MV is designed to operate without an external recording solution. However, for certain use cases we also have External RTSP, which allows the footage to be sent to a 3rd party system. Read more here.
What if my bandwidth is limited?
The Meraki dashboard will intelligently determine if the viewing computer is in the same local network as the cameras. If this is the case, video traffic will stream directly over the LAN, saving WAN bandwidth. If video is viewed remotely, the dashboard will proxy it through the cloud. It’s recommended that customers with limited WAN bandwidth planning on doing frequent remote viewing should select the Standard Quality video setting.
What CODECs does the MV support?
H.264.
When I export video clips, what format are they exported in?
Video clips are exported as MP4 encoded with H.264.
Can the memory on my camera be upgraded or replaced?
No, the solid state memory on these cameras cannot be removed or replaced.
Are these cameras PTZ capable?
Meraki does not offer a PTZ camera. To see an area from multiple angles, we recommend the use of our MV32 fisheye camera which allows you to see a full 360 degree view of the surrounding area below the camera.
MV21 and MV71 are fixed varifocal dome cameras, so zoom, focus, and aperture settings can be adjusted remotely through the dashboard, but lens angle and positioning must be adjusted during installation.
MV12 and MV32 cameras use a completely fixed lens with autofocus.
Does MV offer analytics capabilities?
MV’s powerful motion search plus motion heat maps capability comes standard with all MV hardware plus a license.
All second generation cameras (MV12, MV22, MV72 and MV32) feature a Qualcomm Snapdragon processor, enabling machine-learning-based computer vision, including detecting people as objects.
With the release of the MV32, Motion Search 2.0 and Motion Recap were announced. These new features enhance the information gathered via motion search, enabling the scene of interest to be observed at a glance.
Are MV cameras NDAA compliant?
All second and third generation MV cameras are NDAA compliant.
Are these cameras wireless?
MV21 and MV71 require a standard Ethernet cable and PoE (for MV21) or PoE+ (for MV71) to operate.
All second generation cameras (MV12, MV22, MV72 and MV32) can be used with standard Ethernet and PoE, but they also feature 802.11ac wireless.
Meraki also has power injectors available for users who do not have a PoE enabled switch available.
Can you zoom in on historical footage?
Video clips are exported in the standard MP4 format, allowing you to use the zoom feature available in most standard video players during playback.
When video is exported from the MV32 in a hemispherical view, the zoom feature from standard video players will not dewarp the image.
What do the LED colors mean?
- Rainbow (solid, rotating through colors) - MV is booting up.
- Flashing Blue - MV is searching for WiFi network(s).
- Flashing Green - MV is upgrading or initializing for the first time.
- Solid Green - MV is connected via Ethernet.
- Solid Blue - MV is connected via WiFi.
- Solid Violet - MV has audio recording enabled.
- Solid Amber - An MV may show a solid amber LED on startup for less than 30 seconds. If the amber LED stays lit, the MV has an issue and may need replacement.
Meraki Display Introduction FAQ
This section provides a concise FAQ overview of Meraki Display, covering software capabilities like camera-only admin access and hardware requirements such as supported Apple TV models. It clarifies compatibility, streaming limitations, and support policies for users.
Software
Does Meraki Display support Camera-Only Admin access?
Yes. You can authenticate as an organization, network, or camera-only admin.
Can I stream 3rd party camera hardware through Meraki Display?
No. We only stream MVs to offer the most secure and best possible experience.
Hardware
Can I use older Apple TV hardware to access Meraki Display?
We are recommending the latest hardware due to its improved performance and functionalities.
Only Apple TVs 4th generation (HD) or later have access to App store for downloading Meraki Display. Apple TV (3rd generation) and prior do not support App Store; Meraki Display cannot be downloaded in those models. You can identify your Apple TV models with this guide.
Does Meraki repair or replace Apple TVs?
No. Meraki does not offer services to Apple TVs. Please refer to official Apple services for help.