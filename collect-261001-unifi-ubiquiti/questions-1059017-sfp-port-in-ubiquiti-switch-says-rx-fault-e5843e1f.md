guys, so I recently bought an SFP cable to hook up to my server and my switch (Ubiquiti), but today, when I tried to hook everything up in the unifi controller in the port, I was connecting the SFP, I got an error saying RX Fault.
I saw some posts saying to check the bios, and the hardware is being initialized as far as I know. After looking into the bios settings, I reset the partitions to defaults, and the rest of the settings looked like it defaulted too (First time using SFP, so I don't really know what to look for).
Controller:
Proxmox:
In proxmox, it's not detecting the connection, but after inspecting the hardware, I saw a green light blink every now, and then so I guess something is being detected, and in proxmox, it detects the hardware/ports.
eno1 and eno2 are from the SFP controller hardware.