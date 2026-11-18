UniFi Protect - Manage Camera Zones
Use UniFi Protect 'Zones' to specify camera view regions that trigger AI detections, or that should not be recorded due to sensitive material.
To configure a Zone, navigate within the Protect application to Devices > select a camera > Recording Mode > Detection & Privacy Zones.
Motion Zones
Motion within this region will create a detection which can be found in the Find Anything tab of your UniFi Protect application. By default, any motion within the camera's field of view will trigger a motion event. Learn more about recordings and detections.
Smart Detection Zones
Similar to Motion Zones, this specifies a region that will generate AI-powered detections for compatible G4, G5, and AI series cameras. By default, any motion within the camera's field of view will trigger a motion event. Learn more about recordings and detections.
Enhanced Bitrate Zones
Specifies a region where the camera will allocate increased encoding bitrate to preserve higher image detail. This prioritizes important areas of the scene. This is often used to improve image quality for entry points, registers, driveways, or other areas where fine detail is critical. Excluding regions with constant motion, such as moving foliage or water, helps ensure bitrate is primarily allocated to areas of interest.
Exclusion Zones
Specifies a region within a Smart Detection Zone that will not generate AI-powered detections. Activity in this area will be ignored for Smart Detection events. This is often used to reduce false positives from high-traffic areas, moving foliage, screens, or reflections within the camera’s field of view.
Privacy Zones
Specifies a region that will not be recorded, but will instead appear as a blacked-out image. This is often used to prevent recording sensitive material, or to comply with local laws and regulations.
When setting up zones, you can also adjust the zone sensitivity. Setting a higher value will make your camera more likely to detect and log subtle motions (i.e., moving branches).
For more information on recordings, click here.