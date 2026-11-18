Versions this guide is based on:
| EVE Image Foldername | Downloaded image | Version | vCPU | RAM | HDD Format | Console | Interfaces | 
|---|---|---|---|---|---|---|---|
| huaweiusg6kv-5.1.6 | USG6000v-hda.7z | 5.1.6 | 2 | 4096 | hda | vnc | virtio 6 | 
| Instructions | 
|---|
| Other versions should also be supported following bellow’s procedure. This how to is tested for image version USG6000 5.1.6 1. Open or unzip USG6000v-hda.7z file to obtain USG6000v-hda.qcow2 source file. 2. SSH to EVE and login as root, from cli and create image directory: mkdir /opt/unetlab/addons/qemu/huaweiusg6kv-5.1.6 3. Upload the USG6000v-hda.qcow2 image to the EVE /opt/unetlab/addons/qemu/huaweiusg6kv-5.1.6/ using, for example, FileZilla or WinSCP. 4. From cli go to created directory: cd /opt/unetlab/addons/qemu/huaweiusg6kv-5.1.6/ 5. Rename source original filename to hda.qcow2: mv USG6000v-hda.qcow2 hda.qcow26. Fix permissions /opt/unetlab/wrappers/unl_wrapper -a fixpermissions |