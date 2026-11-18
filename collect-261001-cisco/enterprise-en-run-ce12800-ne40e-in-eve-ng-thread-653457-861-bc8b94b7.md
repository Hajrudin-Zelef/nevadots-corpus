Hello guys,
I'll show you a way to integrate the Huawei eNSP images into the EVE-NG.
Download the CE12800 images and configuration file, also the CE12800 icon. And extract the download image.
Login to the EVE-NG. It's recommended to use the Mobaxterm.
Upload the configuration(huaweice12800.yml) into the EVE-NG path: /opt/unetlab/html/templates/intel/ If you are using the AMD CPU, the accordingly path is /opt/unetlab/html/templates/amd/
Upload the ce icon file(ce.png) into the EVE-NG path: /opt/unetlab/html/images/icons/
Upload the CE12800 image into the EVE-NG path: /opt/unetlab/addons/qemu/
Fix the permission using command: /opt/unetlab/wrappers/unl_wrapper -a fixpermissions
Enable the CE12800 in the EVE-NG web UI.
As I received massive messages about how to use the images. I'll show you guys how to extract the image files.
Download all the parts and extract them accordingly. You'll get the seven files, named from "CE12800.part1.txt" to "CE12800.part7.txt".
Modify the filename extension from .txt to .rar, for example, the file "CE12800.part1.txt" changed to "CE12800.part1.rar". Finish all the files renaming before step 3.
Fold all the renamed files in the same folder, and extract the files again. It will require all seven parts to finish the extracting. After that, you'll get the necessary files.
If you are interested in using eNSP, you can download eNSP in my post Download the eNSP.