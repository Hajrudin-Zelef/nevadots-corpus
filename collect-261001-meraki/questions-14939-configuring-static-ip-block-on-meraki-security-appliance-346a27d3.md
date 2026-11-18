# Configuring Static IP Block On Meraki Security Appliance

*Score : 4 | Source : https://networkengineering.stackexchange.com/questions/14939/configuring-static-ip-block-on-meraki-security-appliance*

I have two network appliances configured on a network, an older Juniper Firewall and a Meraki Security Appliance.
In the Juniper device I can use the base static IP address, ex: xx.xx.xx.xx and then I can use a /29 prefix and route all of the static IP traffic through said device.
I configure the untrusted uplink as such: xx.xx.xx.xx/29
How do I replicate this on a Meraki or other networking device?

---

### Reponse — score 2

1:1 NAT (under Configure > Firewall) is the right way to route multiple public IP addresses. https://kb.meraki.com/knowledge_base/configuring-11-nat