UniFi Protect is the video security system from Ubiquiti. Just like the Unifi Access Point and Network components, are the UniFi Camera and Protect system really easy to use. They integrate seamlessly together, but to be clear, you can use the Unifi Protect also on its own.
If we are talking about Unifi Protect, then we are talking about the security system behind the cameras. Protect is the NVR for recording the video streams, it sends out the motion alerts and allows you to control and manage your camera’s.
In this article
In this review, we are going to take a look at UniFi Protect, how to install it, and how to configure your camera’s.
Unifi Protect Overview
UniFi Protect does a great job of finding the perfect balance between flexibility and ease-of-use. You can choose between a wide variety of cameras, which you can all install with PoE (Power-over-Ethernet) and get it up and running in a couple of minutes.
The biggest advantage of UniFi Protect compared to other systems is that all the recordings are stored locally but can be viewed from anywhere in the world through the Unifi Protect App.
And another important different is that Protect is completely subscription-free, even for the motion-alerts or the cloud access is no subscription needed.
The product range of UniFi Protect has extend significantly the last couple years. When I wrote the first review of Protect, only a few cameras existed. Right now, you can choose from ~30 different cameras, connect different doorbells, and extend the system with sensors, floodlights and siren.
Advanced Features
Also the range of features have expended over time. Where it started with smart detection, the system can now read license plates and recognize faces.
Smart Detections
Motion detections are always hard to get right. Basic motion detection will result in a lot of false positives, due to light reflections or shadows for example. And fine-tuning the motion detection takes a lot of time.
Smart Detection, however, allows cameras to recognize familiar objects, like persons, vehicles, animals or even packages. All current UniFi Protect cameras support smart detections these days. But some camera are of course a bit better in it then others or are capable of detecting more.
You can find a complete overview of the difference here in the UniFi documentation.
License Plate Recognition
As the name says, camera with license plate recognition are capable of reading license plates from cars. This allows you to fully track a car through different camera views or quickly search recordings based on the license plate.
Face Recognition
The newest cameras also support Face Recognition. Now good to know is that the face recognition algoritme is running locally, so no data is send to the cloud for processing.
The feature allows you to track persons through difference cameras feeds, and can also be used to unluck doors when combining it with UniFi Access.
AI Recognition
The AI Recognition helps you with finding the right footage, also called the “Find anything” feature. You can for example search through your footage to find all cars that are red, or find all persons with a backpack.
Again, this logic behind this runs locally, so nothing is send to the cloud, which is great. But the number of cameras that support this is limited, and in most cases you will need to use the AI Key or AI Port to enhance the footage.
Unifi Protect Cameras
As mentioned in the beginning, you can currently choose between roughly 30 difference cameras, from ceiling mounted dome and turrets to full PTZ cameras. Each camera only needs a PoE connection to get it up and running.
Protect works best with UniFi Cameras, but you can also connect ONVIF-compatible third-party cameras. When using third-party cameras, features like motion detection, or PTZ are not supported. But this allows you to transition easily from an existing environment to a full UniFi Protect environment.
Now which camera you should choose really depends on the location you want to place it and the intend usages.
Getting started with Unifi Protect
To use UniFi Protect you first need an NVR or an Protect compatible Cloud Gateway. These days there are quite a few options, but the most commonly used ones are:
|  | Network Video Recorder Instant | Cloud Gateway Max | Network Video Recorder | 
|---|---|---|---|
| Number of cameras | 15 HD or 6 4K | 15 HD or 5 4K | 60 HD or 18 4k | 
| Storage | 1TB – to 24TB | 512 GB – to 2TB | 8TB to 4x 24TB | 
| PoE ports | 6 GbE Ports | No | No | 
| Price excl storage | $199 | $199 | $299 | 
The Cloud Gateway Max is interesting when you want to use UniFi Network as well. You can then use it as gateway (router) as well. The downside is that you will need an PoE capable switch to power your cameras.
If you only want to use UniFi Protect, then the the Network Video Recorder Instant is the best entry option. It gives you plenty of storage options, and comes with a built in PoE switch.
For enterprise grade installation, the Network Video Recorder is the way to go. It comes with 4 drive bays, allowing you to expand the storage capacity to 4x 24TB.
Other good options are the Dream Machine product lines for the larger office installations.
Installing UniFi Protect
Depending on the console you are using, Protect will be installed straight from the beginning or you will need to install the Protection application after you have set up UniFi Network.
You can install the application under Control Plane > Updates. Make sure it’s updated to the latest version as well.
After you have installed Protect, the first thing you need to do is adopt your cameras. In Protect, click on Devices. All cameras connected to your network will be listed. Simply click Adopt to add the camera to Protect.
With the cameras connected we can start with settings up the motion zones, fine-tuning the system, and setting up the alerts.
Configuration is done per camera, so for each camera we defined the recording zones, when to record, in which quality, etc.
Recording Settings
In the device page, click on an camera to open the recording setting. The first step is to decide when the camera should record. There are two settings that influence when the camera starts recording:
- When to Record
- Recording mode
Important here is to keep your storage in mind, how much storage do you have and how long do you want to retain it. Having two cameras record 24/7 on 2K or 4K will require signifying more storage compared to motion only recording.
I prefer to set the cameras to record Always, and the recording mode to Adaptive. This means that the camera will record in high quality when motion is detected, and otherwise in lower quality.
Next we can configure which AI and Audio Events we want to detect, I recommend enabling them all. It will make searching the footage later a lot easier.
But a setting more importantly is the Record Motion Events. Make sure to enable this, because this will highlight motion events in your recordings. And we need to defined the number of seconds before and after the event we want to record.
Now I prefer to set this to at least 2 seconds before and after. More then often I have seen a motion detection kicking in to late, or in sometimes you just want to now what happened before the recording.
The default settings for the Recording Quality are often good enough. If storage is an issue, and quality less important, you can lower the recording quality and/or FPS (Frame Per Second).
Under the Recording Quality you also have an option to set the Encoding. Now standard is perfectly fine for most cases, but if you look at the info description, then the Enhanced option will give you better video quality and it reduces storage size. Sound like a win, right? There is actual an important difference that you need to know.
Standard uses the H.264 codec, which is the most compatible option. It works in all major browser. The enhanced setting uses H.265/HVEC, this codec is indeed more efficient and uses less storage. But it’s only well supported on Apple devices.
So if you only have Apple devices, then you can safely enable the Enhanced encoding, but in other cases, it’s recommended to use the Standard encoding.
Zones and Lines
In the Zones and Lines tab (1), we can add Motion Zones, Smart Zones, Crossing Lines, and Privacy zones. Privacy zones are areas that you want to block (black) out from the recordings, for example, your neighbors garden.
You can create multiple zones for each camera, and fined per zone the sensitivity and what you wan to detect (Smart zones). Now the sensitivity is always a bit of trial and error. In general the further away the object is, the higher you need to set it, in relation to the object size you want to detect.
As you can see in the screenshot above, I have added multiple zones to this camera. The reasons for this is that I will need to use different sensitivity settings depending on how far away the object is, in order capture persons and not birds.
If you click Add Motion Zone you can draw an area in which you want to detect the motion. A zones can only have 8 points, so you are a bit limit to the shape you can draw. If you need more, simply split the zones in two motion zones.
Smart Detection Zones
Smart Detections zones work pretty much the same way as motion zones. The only difference is that you can select what it should detect, person, vehicle, animal, or multiple. Again you will need to play with the sensitivity settings to find what works for your situations or even use multiple zones.
What you can detect als depends on the camera model you have. You can also create Exclusion zones that ignore certain area wrom detection.
Crossing Lines
Lines crossing is another way to triggering alert. It allows you to draw a line and select what kind of detection should trigger an event. Line crossing are a great way to detect if somebody passes and entry gate for example.
Settings
The last tab, Settings, allow you to fine tune the image quality, night vision and audio recording of the camera. Most cameras have built in microphone. It’s always a good idea the enable the Noise reduction to clear up the audio recording. You can also change the sensitivity of the microphone.
For the night vision, by default it’s set to auto, but you can also change this to custom, allowing you to set the lux level when it should switch over. It depends a bit on the ambient light when you want to switch it over.
In night vision mode, the recordings are of course monochrome. But if you have some outdoor lights, then you might want to switch it over later, so you record longer in color mode, allowing you to see the color of a car for example.
Another setting I want to point out is the status light. A lot of people turn the status light off, because they want to make the camera less noticeable. But by leaving it on, people tend to look up to the camera, curious what the light is, giving you a good shot of there face. So I prefer to leave it on.
Under Advanced setting you can change the shutter exposure. If the recordings are a bit to dark or to bright, then you can fine tune it here.
Alarm Manager
With the camera configured and the zones created, the next step is to defined / fine-tune the alerts. Now as always with systems that send notifications / alerts, you should minimize to only the notifications that really matter, otherwise people get notification fatigue, where they start ignoring alerts.
Ubiquiti has expanded the capabilities of the alarm manager quite significantly the last years, so you can completely fine-tune it to your liking.
You can edit existing alarms or create new alarms. For each alarm you can defined when it needs to send the notification, based on the object detect or system event. You can even combine triggers, like a line crossing followed by a motion detection x seconds later.
In the scope section, you can defined the areas (zones/cameras) that are included in the detection. You can select multiple cameras and for each camera the zones you want to use.
And the last step is of course to define the Action. The actions that you can use depend on the systems you have. If you, for example, have a chime installed, you can let it play a sound when somebody crosses a line.
A good tip is to use schedules where possible. For example, during business hours, certain areas don’t need to trigger a motion alert. So using schedules allow you to minimize the noise.
Downloading and Looking back Events
The playback page allows you to view back recordings. At the bottom you have a couple of options to quickly find the correct recording, based on date and detected events.
We can also use this page to download recordings. To do this, first click on Download (1), this allows you to select the timeframe you want to export, using the sliders in the vertical timeline, or by entering the date/time in the lower part of the screen.
When done, click on Download to this Device, and select where you want to store the recording.
Two other great features are the heatmap and grid search. The heatmap button will generate an overlay, showing you where most activity was. The grid search on the other hand, allows you to quickly find all recordings with motion in the select grid(s).
Now when it comes to finding recordings (events), we also have the Find Anything page, which allows you to go through your recordings based on different filters.
The options I have a bit limited, because I don’t have any AI capable cameras, but with the newer generation cameras, you can also filter the recording on things like car color, license plate, persons with backpacks, etc.
This can really help with quickly finding the right events when you have a lot of footage to go through.
Storage Settings
The last setting I want to highlight are the storage settings. Here you can enable Enhanced Retention which will reduce the recording quality after the specified days. This allows you to extend how long you can retain recordings.
The other options is Continues Archiving. You can link another storage account to UniFi protect, like a OneDrive, Google Drive or NAS storage, and let Protect automatically archive footage to your second storage device.
Now I would recommend to use a cloud based (or at least remote) storage option, so in case something happens with your UniFi installation, you can at least still review the latest footage.
You can limit how much storage you want to use for this, as you can see in the screenshot above, I only allocated 1GB, and set it to only archive Person detection events.
Frequently Asked Questions
A couple of answer on some common questions when it comes to Unifi Protect.
Does Unifi Protect work with other cameras?
 Yes, you can now connect third-partry cameras to UniFi Protect. But you are limited to recording only.
How much storage does Unifi Protect has?
The amount of storage depends on the system and hard disk you choose.
Can I run UniFi Protect in Docker
No, UniFi Protect can only be used in combination with one of the supported UniFi Protect NVRs or Cloud Gateways
Can I install Unifi Protect on a Raspberry PI
No, unfortunatelly thas is also not possible.
Conclusion
I am really happy with the way UniFi Protect is going. I started many years ago with UniFi Video and have transitions along to the way to Protect and currently the G5 cameras. The feature set and capabilities keep expanding which is great.
I hope you liked this UniFi Protect Review, if you have any questions, just drop a comment below.
Hi Rudy, could you update this article with the latest features in 2025?
I understand they now support 3rd party cameras albeit without smart detections (now “AI” detections as everything has to be labelled AI these days ). And then they sell a separate device to enable detections on 3rd party cameras – worth it?
I also can’t find anywhere if UniFi Protect iOS app supports PIP, I only see things about PIP notifications on Apple TV using HomeKit or Scrypted.
Sorry, took 6 months, but it’s updated finally.
I’m looking for a solution which uploads the video to a cloud storage in case of defined motion alerts within near real time. Is there any way to achive this with Unifi Protect? Anything which can be automated by coding would be ok as well.
Unifi Protect can record on movement, that is not enough? You could try to place the Unifi NVR offsite, with a site-to-site VPN connection for example. But offloading the recording with a script is not possible. You can’t even extract the video files from the record and play them on something else.
I wrote a tool for exactly this: https://github.com/ep1cman/unifi-protect-backup
Is there any way to allow a remote Ubiquiti camera on a different LAN to be part of my Unifi Protect environment on my LAN/Cloud Key Plus?
You will need to create a Site-to-Site VPN tunnel between the two sites.
Nice article. Can you do one as well for Unifi Access and how it can intergrate with other applications like a garage door. I think this may interest a lot of people out there
Is Ubiquiti going to add the option to use Windows 10 as a NVR to integrate with UniFi Protect?
You had the option for UniFi video……why take away the option? I would like to use a normal blade server for my NVR and still have remote access with Protect since UniFi Video has reached EOL.
Thanks
No probably not. No docker images either.
Hey Ruud,
I switched from UniFi Video on a Intel NUC to UniFi Protect on a CloudKey Gen2 Plus. The 2 things I miss the most, or maybe I’m looking wrong, is the option to make scheduled motion events for recording and the possibility to record and/or move the recordings to external storage like a NAS or cloud solution. Do you perhaps know if there are in Protect as well? Cause I cannot find them :S
Both are not possible, unfortunately. You can however replace the disk in your Cloudkey Gen2 plus with a larger one up to 5TB.
Hi Rudy,
The problem with local storage is when the CK is gone/stolen then so are the files with the perps on it. I hope Ubiquiti wises up. Those 2 features we’re in Unifi Video so why it’s not in Protect is a mystery to me….
What you can do is use rclone and make a backup of the folder /srv/unifi-protect/video. The file format is .ubv, so it might be a challenge to view the files. Maybe it’s possible to restore them back to another Cloudkey, but I haven’t tested that (yet).
Thanks for the useful review. Just to confirm my understanding, I’m currently running UniFi Video with the NVR software running on my Win10 PC. It looks like UniFi Protect does not allow you to use your Win10 PC as the NVR, instead it requires one of their hardware devices, is that correct?
Hi Tim,
That is correct. You will need a Cloudkey Gen2 Plus, Dream Machine Pro, or Univi NVR.
I have a basic unifi system in my home (USG, cloud key gen 2+, us-8-60, ap pro, ap lite) and am considering switching from Arlo cameras to unifi. What is the best option for me as far as additional equipment needed. I’m going to use the g3 flex cameras. I will probably have a max of 6 cameras. Should I just buy another us-8-60? I have a shop that is about a 200ft run that is wired with cat 6 where I am using the ap lite. What would I need to add to also be able to install a camera or two in my shop?
Great article. Thanks!
I would place an us-8-60 indeed in the shop, for the AC and flex (I have exactly the same setup for my garage). For the other camera’s you will need to see if it’s worth buying an extra us-8-60 or just use PoE adapters.
Great article! I am having an issue with the location/geofencing in that my status is always ‘Away’. I have gone into the UnifiProtect web interface and reset my location address and radius, restarted my app, but no change. Any other ideas of how to get it to know that I am home?
Did you set the correct permissions for the Protect app on your phone?
I’m looking to add this to my unifi network and was curious if you can have role based user logins. For example, if I want to give a friend a logon I just want them to be able to look at video feeds.
I have updated the article a bit with the roles. But in short, yes you can create (custom) role with view only and/or edit permissions per camera.