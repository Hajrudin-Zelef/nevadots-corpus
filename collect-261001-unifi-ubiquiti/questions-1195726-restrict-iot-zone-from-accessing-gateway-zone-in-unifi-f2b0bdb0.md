# Restrict IoT zone from accessing gateway zone in Unifi

*Score : 1 | Source : https://serverfault.com/questions/1195726/restrict-iot-zone-from-accessing-gateway-zone-in-unifi*

I'm looking to clamp down on my IoT devices, so I've put most of them in an IoT zone and network. Right now the IoT to Gateway policies allow mDNS and all traffic. Looking at the Zone Matrix, the Hotspot to Gateway looks like the policies that I need (minus radius).
Am I on the right path? Are there other ports or concerns by restricting traffic to the Gateway zone from the IoT zone?
Here's my current (default?) Hotspot to Gateway policies.