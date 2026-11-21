---
id: collect-261121-veille-ia/gemini-google/mikrotik-cloud-router
title: "Mikrotik Cloud router -"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["mikrotik", "image", "chr-6", "cloud", "router", "versions", "mikrotik-6", "unetlab"]
source: https://www.eve-ng.net/index.php/documentation/howtos/howto-add-mikrotik-cloud-router/
source_anchor: ""
source_lines: [1, 18]
section: "Gemini_Google"
---
# *Versions this guide is based on:*

| EVE Image Foldername | Downloaded Filename | Version | vCPUs | RAM | Console | 
|---|---|---|---|---|---|
| mikrotik-6.40 | chr-6.40.4.img.zip | 6.4.0 | 1 | 256 | Telnet | 

NIC Order tested and working for versions:

**6.44.5, 6.48.6, 7.2.3, 7.3.1, 7.4.1, 7.5.0, 7.6.0**

*NON working version 6.49.7 and 7.1.X Issues with different structure*

| Instructions | 
|---|
| Other versions should also be supported following bellow’s procedure. 1. Download Mikrotik Cloud router image chr-6.40.4.img.zip from: https://mikrotik.com/download 2. Using any archivator program unzip it to get image file chr-6.40.4.img Accordingly our image naming table: http://www.eve-ng.net/index.php/documentation/images-table 3. SSH to your EVE and create directory for Mikrotik node. `mkdir /opt/unetlab/addons/qemu/mikrotik-6.40.4/`4. Upload the chr-6.40.4.img image to the created directory using for example FileZilla or WinSCP. 5. Go to newly created folder and convert the disk to the qcow2 format: ``` cd /opt/unetlab/addons/qemu/mikrotik-6.40.4/
 mv chr-6.40.4.img hda.qcow2
  ``` 6. Fix permissions: ``` /opt/unetlab/wrappers/unl_wrapper -a fixpermissions
  ``` Default username is **admin** with no password**.** |
