# Can&#39;t see docker ports from external machines when using a veth interface with an OPNSense KVM

*Score : 1 | Source : https://serverfault.com/questions/1049341/cant-see-docker-ports-from-external-machines-when-using-a-veth-interface-with-a*

Quick summary of the setup:
- Ubuntu Server 20.04 with 4 network ports
- OPNsense router running in libvirt KVM
- One port is WAN, three ports are LAN (bridged)
- Router works great
- Server (same one running OPNsense) gets access to LAN and internet by VETH through LAN bridge
- Services run on various ports on the server, and external machines can access them
- PROBLEM: If running a service in Docker, the service ports can be seen by the server, but not from other machines on the LAN (nmap shows them as "filtered")
- This is solved by setting the docker container to run in "host" mode, which is obviously sub-optimal since port-mapping is no longer possible
Why can't external machines see ports exposed by docker in this setup?  I understand it's a complicated networking setup, and there is probably some missing route between docker VLANs and the VETH bridge, but everything I've checked looks fine.  Docker daemon seems to be configured to listen on all interfaces.  I'm at a loss.