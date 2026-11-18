Versions this guide is based on:
| EVE Image Foldername | Downloaded image | Version | vCPU | RAM | HDD Format | Console | Interfaces | 
|---|---|---|---|---|---|---|---|
| huaweiar1k-5.170 | var_allinone.img | 5.170 | 1 | 2048 | hda | vnc | virtio 6 | 
| Instructions | 
|---|
| Other versions should also be supported following bellow’s procedure. This how to is tested for image version Huawei AR1000v-5.170 1. SSH to EVE and login as root, from cli and create image directory: mkdir /opt/unetlab/addons/qemu/huaweiar1k-5.170 2. Upload the var_allinone.img image to the EVE /opt/unetlab/addons/qemu/huaweiar1k-5.170/ using, for example, FileZilla or WinSCP. 3. From cli go to created directory: cd /opt/unetlab/addons/qemu/huaweiar1k-5.170/ 4. Rename source original filename to virtioa.qcow2: mv var_allinone.img hda.qcow25. Fix permissions /opt/unetlab/wrappers/unl_wrapper -a fixpermissions |