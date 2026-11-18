I am trying to establish a 25 GbE link between Mikrotik CCR2004-1G-12S+2XS router and a server running Windows Server 2022 with Mellanox NIC (MCX512A-ADAT).
For cabling, I tried using the official Mikrotik DAC (XS+DA0003), unofficial FS.com with one side generic and one side "Mellanox-compatible", a pair of SFP28 from FS.com with SMF link and various other SFP/SFP+ modules just for testing.
Each time, the Mikrotik RouterOS shows a connected DAC/SFP incl. its serial number:
However, on the server (Mellanox) side, I only see information about the link being down or the cable unplugged:
I tried changing FEC mode, rate (low/high), flow control and auto-negotiation on Mikrotik, but the link still did not work.
What am I doing wrong?
Edit:
I have also tried setting the FEC and link speed manually:
mlxlink -d mt4121_pciconf0.1 --link_mode_force --speeds 25G
mlxlink -d mt4121_pciconf0.1 -k FC --fec_speed 25G
mlxlink -d mt4121_pciconf0.1 -a TG