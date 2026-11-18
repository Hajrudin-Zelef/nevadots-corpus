I have a Mikrotik RouterOS v6.36 with my network as follows (I am a newbie to networking in general and have been able to get stuff working half way. I just don't know what to look for.)
- Router:
  - Ether01: ISP Connection(Internet/WAN)
  - Ether02: SIP Trunk(SIP Trunk - No internet)
  - Ether03: Null
  - Ether04: LAN
I have a PBX server on the LocalLan that needs to connect out via Ether01 to authenticate licenses etc. This works fine with no problem.
I now need to connect Ether02 which is a SIP trunk from our ISP to our PBX server.
I am able to get the Router to mark routes and send them out via Ether02 but I am not able to get that data to respond back through Ether02
Please could somebody give me guidance and let me know what data is required to help troubleshoot this.