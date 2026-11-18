I'm trying to boot OPNSense (the amd64 VGA image, 21.1, from https://opnsense.org/download/) from USB. The machine is fairly new (to me), but I've seen

- it booting a Linux kernel successfully from a USB stick (so it's probably mostly okay)
- the OPNSense USB stick boots reasonably in qemu on another machine, too.

Nevertheless, OPNSense gets stuck right away, right after boot:

```
>> FreeBSD EFI boot block
   Loader path: /boot/loader.efi
   
   Initializing modules ZFS UFS
   Load Device: PciRoot(0x0)/Pci(0x1d,0x0)/USB(0x1,0x0)/USB(0x4,0x0)/HD(1,GPT,[lots of hex],0x3,0x640)
   BootCurrent: 0004
   BootOrder: 0004[*] 0003 0001 0002
   Probing 5 block devices........* done
    ZFS found no pools
    UFS found 1 partition
```
... I've also dd'd all of it onto the USB stick another time in case it's a few bits of corruption (no).

Any ideas how to start debugging this?

**Update**: it's most likely just a bad UEFI implementation; switching over to MBR boot is a workaround that happened to work well.

DO NOT post images of code, data, error messages, etc.- copy or type the text into the question.