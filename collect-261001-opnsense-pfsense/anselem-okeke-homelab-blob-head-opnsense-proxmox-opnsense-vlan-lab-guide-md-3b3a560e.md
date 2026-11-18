This lab builds an enterprise-style segmented network using:

- **Proxmox VE** as the virtualization platform
- **OPNsense** as a virtual firewall/router
- **Windows Server** in a server VLAN
- **Windows 11** in a client VLAN
- **FritzBox/home router** as the upstream internet router

The goal is to understand:

- WAN vs LAN
- VLAN tagging
- DHCP per VLAN
- DNS forwarding/resolution
- Inter-VLAN routing
- Firewall rules
- Why enterprise networks separate servers, clients, guests, and management devices

```
Internet
   |
FritzBox / Home Router
192.168.0.1
   |
Proxmox vmbr0
   |
OPNsense WAN
192.168.0.57
   |
OPNsense Firewall / Router
   |
OPNsense LAN trunk on vmbr1
   |
------------------------------------------------
| VLAN 20 - SERVERS | Gateway 192.168.20.1     |
| VLAN 30 - CLIENTS | Gateway 192.168.30.1     |
| VLAN 10 - MGMT    | Gateway 192.168.10.1     |
| VLAN 40 - GUEST   | Gateway 192.168.40.1     |
------------------------------------------------
   |
Windows Server → VLAN 20 → 192.168.20.x
Windows 11     → VLAN 30 → 192.168.30.x
```
WAN means the outside/upstream side of the firewall.

In this lab:

```
OPNsense WAN = connected to vmbr0 = FritzBox/home network
```
Example:

```
OPNsense WAN IP: 192.168.0.57
FritzBox gateway: 192.168.0.1
```
LAN is the inside side of OPNsense.

In this lab:

```
OPNsense LAN = connected to vmbr1
```
The LAN interface carries multiple VLANs:

```
VLAN 20 = SERVERS
VLAN 30 = CLIENTS
VLAN 10 = MGMT
VLAN 40 = GUEST
```
A VLAN separates traffic logically on the same virtual/physical network.

```
No VLAN tag  = untagged LAN
VLAN tag 20 = SERVERS
VLAN tag 30 = CLIENTS
```
Firewall rules answer:

```
Who can talk to who?
```
In OPNsense, rules are usually placed on the interface where traffic **enters** the firewall.

Example:

```
Windows 11 → Internet
```
The packet enters OPNsense through:

```
CLIENTS interface
```
So the firewall rule goes under:

```
Firewall → Rules → CLIENTS
```
Direction is:

```
in
```
Even though the client is going “out” to the internet, from OPNsense’s view the packet first enters through the CLIENTS interface.

`vmbr0` connects to the physical network/FritzBox.

Used by:

```
OPNsense WAN NIC
Proxmox management
Optional direct home-network VMs
```
`vmbr1` is an internal Proxmox bridge.

Used by:

```
OPNsense LAN NIC
Windows Server VLAN 20
Windows 11 VLAN 30
Other lab VMs
```
`vmbr1` does not need a physical port.

Correct `vmbr1` config:

```
auto vmbr1
iface vmbr1 inet manual
        bridge-ports none
        bridge-stp off
        bridge-fd 0
        bridge-vlan-aware yes
        bridge-vids 2-4094
```
Important:

```
vmbr1 must be VLAN-aware
```
Without VLAN-aware enabled, VLAN-tagged VMs will not receive DHCP from OPNsense.

Create a new VM:

| Setting | Value | 
|---|---|
| VM name | `opnsense-fw` | 
| OS | OPNsense ISO | 
| CPU | 2 cores | 
| RAM | 4 GB recommended | 
| Disk | 20 GB | 
| NIC 1 | WAN | 
| NIC 2 | LAN / VLAN trunk | 

| Proxmox NIC | Bridge | VLAN tag | OPNsense name | Purpose | 
|---|---|---|---|---|
| net0 | vmbr0 | empty | vtnet0 | WAN | 
| net1 | vmbr1 | empty | vtnet1 | LAN trunk | 

Important:

```
Do not put VLAN tags on the OPNsense LAN NIC in Proxmox.
```
OPNsense must receive VLANs internally on `vtnet1`.

Correct:

```
OPNsense net0 → vmbr0 → WAN
OPNsense net1 → vmbr1 → LAN trunk
```
Boot the OPNsense ISO.

When asked to import config:

```
Leave blank and press Enter
```
Login to installer:

```
Username: installer
Password: opnsense
```
Choose:

```
Install (UFS)
```
UFS is simpler and lighter for a small lab VM.

Select the VM disk:

```
ada0 / QEMU HARDDISK / 20GB
```
Do not select the CD/DVD device.

When installation is complete:

1. Set the root password.
2. Select `Complete Install` .
3. Remove/detach the ISO from Proxmox.
4. Reboot/start the OPNsense VM from disk.

When OPNsense asks for interface assignment:

```
Do you want to configure LAGGs now? n
Do you want to configure VLANs now? n
```
Assign:

```
WAN = vtnet0
LAN = vtnet1
Optional = blank
```
Final console should show something like:

```
LAN (vtnet1) -> 192.168.1.1/24
WAN (vtnet0) -> DHCP: 192.168.0.57/24
```
Meaning:

```
WAN = home/FritzBox side
LAN = internal lab side
```
From a VM on the untagged LAN:

```
https://192.168.1.1
```
Login:

```
Username: root
Password: your OPNsense password
```
Browser certificate warning is normal in a lab.

Important:

If your machine is on the FritzBox side, for example:

```
192.168.0.x
```
then:

```
https://192.168.1.1
```
will not work.

That is because `192.168.1.1` is on the OPNsense LAN side.

Go to:

```
Interfaces → Devices → VLAN
```
Create VLANs using parent:

```
vtnet1 [LAN]
```
| Field | Value | 
|---|---|
| Parent | `vtnet1 [LAN]` | 
| VLAN tag | `20` | 
| Description | `SERVERS` | 

| Field | Value | 
|---|---|
| Parent | `vtnet1 [LAN]` | 
| VLAN tag | `30` | 
| Description | `CLIENTS` | 

Optional future VLANs:

| VLAN | Name | Gateway | 
|---|---|---|
| 10 | MGMT | `192.168.10.1/24` | 
| 40 | GUEST | `192.168.40.1/24` | 

Click:

```
Save
Apply
```
Go to:

```
Interfaces → Assignments
```
Add the VLAN interfaces.

They may appear as:

```
vlan01
vlan02
vlan03
```
After adding, OPNsense may name them:

```
OPT1
OPT2
OPT3
```
Rename them.

Go to:

```
Interfaces → OPT1
```
Set:

| Field | Value | 
|---|---|
| Enable Interface | checked | 
| Description | `SERVERS` | 
| IPv4 Configuration Type | `Static IPv4` | 
| IPv4 address | `192.168.20.1/24` | 
| IPv6 Configuration Type | `None` | 
| Block private networks | unchecked | 
| Block bogon networks | unchecked | 

Save and apply.

Go to:

```
Interfaces → OPT2
```
Set:

| Field | Value | 
|---|---|
| Enable Interface | checked | 
| Description | `CLIENTS` | 
| IPv4 Configuration Type | `Static IPv4` | 
| IPv4 address | `192.168.30.1/24` | 
| IPv6 Configuration Type | `None` | 
| Block private networks | unchecked | 
| Block bogon networks | unchecked | 

Save and apply.

OPNsense 26.x may use:

```
Services → Dnsmasq DNS & DHCP
```
Go to:

```
Services → Dnsmasq DNS & DHCP → General
```
Check:

```
Enable
```
For interfaces, select:

```
LAN
SERVERS
CLIENTS
```
Do not select WAN.

Save and apply.

Go to:

```
Services → Dnsmasq DNS & DHCP → DHCP ranges
```
Add:

| Field | Value | 
|---|---|
| Interface | `SERVERS` | 
| Start address | `192.168.20.100` | 
| End address | `192.168.20.200` | 
| Subnet mask | automatic | 
| Lease time | default | 

Save and apply.

Add:

| Field | Value | 
|---|---|
| Interface | `CLIENTS` | 
| Start address | `192.168.30.100` | 
| End address | `192.168.30.200` | 
| Subnet mask | automatic | 
| Lease time | default | 

Save and apply.

In Proxmox:

```
Windows Server VM → Hardware → Network Device → Edit
```
Set:

| Field | Value | 
|---|---|
| Bridge | `vmbr1` | 
| VLAN Tag | `20` | 
| Model | `VirtIO` or`Intel E1000` | 
| Firewall | unchecked | 

Important:

```
The Proxmox firewall checkbox caused DHCP problems in this lab.
Keep it unchecked while learning.
```
Inside Windows Server:

```
ipconfig /release
ipconfig /renew
ipconfig /all
```
Expected:

```
IPv4 Address:    192.168.20.x
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1
DHCP Server:     192.168.20.1
DNS Server:      192.168.20.1
```
In Proxmox:

```
Windows 11 VM → Hardware → Network Device → Edit
```
Set:

| Field | Value | 
|---|---|
| Bridge | `vmbr1` | 
| VLAN Tag | `30` | 
| Model | `Intel E1000` or`VirtIO` if driver works | 
| Firewall | unchecked | 

Inside Windows 11:

```
ipconfig /release
ipconfig /renew
ipconfig /all
```
Expected:

```
IPv4 Address:    192.168.30.x
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.30.1
DHCP Server:     192.168.30.1
DNS Server:      192.168.30.1
```
If Windows shows:

```
169.254.x.x
```
that means:

```
Windows asked for DHCP but no DHCP server replied.
```
Possible causes:

- Wrong VLAN tag
- Proxmox bridge not VLAN-aware
- OPNsense DHCP not listening on the VLAN interface
- Missing DHCP range
- Proxmox firewall checkbox enabled
- Wrong NIC is active in Windows
- Multiple adapters are confusing the test

If Windows has multiple adapters, it may have:

```
192.168.0.x  → FritzBox/direct home network
192.168.1.x  → untagged OPNsense LAN
192.168.20.x → SERVERS VLAN
192.168.30.x → CLIENTS VLAN
100.x.x.x    → Tailscale
```
For clean testing, disable extra adapters and keep only the lab adapter active.

PowerShell:

```
Get-NetAdapter
Disable-NetAdapter -Name "Ethernet" -Confirm:$false
```
Use the exact adapter name shown by `Get-NetAdapter`.

New OPNsense interfaces are blocked by default.

That means VLAN interfaces like:

```
SERVERS
CLIENTS
GUEST
MGMT
```
need rules before normal traffic works.

DHCP may work, but ping, browsing, DNS, and Web UI access may fail until rules are added.

This is one of the most important concepts.

If Windows 11 goes to the internet:

```
Windows 11 → OPNsense CLIENTS interface → WAN → Internet
```
The traffic first **enters** OPNsense on the CLIENTS interface.

So the rule is:

```
Interface: CLIENTS
Direction: in
```
Even though the final destination is outside/internet.

Most OPNsense interface rules are created as:

```
Direction: in
```
Start broad first to verify everything works.

Later you can make rules stricter.

Go to:

```
Firewall → Rules → CLIENTS → Add
```
Set:

| Field | Value | 
|---|---|
| Action | `Pass` | 
| Interface | `CLIENTS` | 
| Direction | `in` | 
| TCP/IP Version | `IPv4` | 
| Protocol | `any` | 
| Source | `CLIENTS network` | 
| Destination | `any` | 
| Gateway | `default` | 
| Description | `Allow CLIENTS to any` | 

Save and apply.

Important source choice:

```
CLIENTS network = 192.168.30.0/24
CLIENTS address = only 192.168.30.1
```
Use:

```
CLIENTS network
```
Go to:

```
Firewall → Rules → SERVERS → Add
```
Set:

| Field | Value | 
|---|---|
| Action | `Pass` | 
| Interface | `SERVERS` | 
| Direction | `in` | 
| TCP/IP Version | `IPv4` | 
| Protocol | `any` | 
| Source | `SERVERS network` | 
| Destination | `any` | 
| Gateway | `default` | 
| Description | `Allow SERVERS to any` | 

Save and apply.

```
ipconfig /all
ping 192.168.20.1
ping 8.8.8.8
nslookup google.com
```
Expected:

```
192.168.20.1 ping works
8.8.8.8 ping works
DNS lookup works
```
```
ipconfig /all
ping 192.168.30.1
ping 8.8.8.8
nslookup google.com
```
Expected:

```
192.168.30.1 ping works
8.8.8.8 ping works
DNS lookup works
```
From CLIENTS VLAN:

```
https://192.168.30.1
```
From SERVERS VLAN:

```
https://192.168.20.1
```
From untagged LAN:

```
https://192.168.1.1
```
The firewall rule allows the traffic.

NAT allows private VLAN traffic to go out through WAN.

Default OPNsense usually uses automatic outbound NAT.

Traffic path:

```
Windows 11
192.168.30.x
   |
OPNsense CLIENTS
192.168.30.1
   |
OPNsense WAN
192.168.0.57
   |
FritzBox
192.168.0.1
   |
Internet
```
If:

```
ping 8.8.8.8 works
nslookup google.com fails
```
then routing/NAT works, but DNS is the issue.

If:

```
ping 192.168.30.1 works
ping 8.8.8.8 fails
```
then check:

- CLIENTS firewall rule
- Outbound NAT
- OPNsense WAN gateway
- FritzBox connectivity

Meaning:

```
DHCP failed.
```
Check:

- VM NIC VLAN tag
- VM NIC bridge
- Proxmox firewall checkbox
- `vmbr1` VLAN-aware
- OPNsense VLAN parent
- DHCP range
- Dnsmasq listening interface

Likely cause:

```
vmbr1 is not VLAN-aware
```
Fix:

```
pve01 → System → Network → vmbr1 → VLAN aware checked
```
Or edit:

`nano /etc/network/interfaces`
Ensure:

```
bridge-vlan-aware yes
bridge-vids 2-4094
```
Apply:

`ifreload -a`
If GUI keeps reverting, restart the OPNsense VM or Proxmox networking after confirming config.

Possible causes:

- Change was not applied
- Pending change was reverted
- Wrong bridge was edited
- Proxmox GUI did not persist the setting

Fix manually:

`nano /etc/network/interfaces`
For `vmbr1`:

```
auto vmbr1
iface vmbr1 inet manual
        bridge-ports none
        bridge-stp off
        bridge-fd 0
        bridge-vlan-aware yes
        bridge-vids 2-4094
```
Apply:

`ifreload -a`
Likely cause:

```
OPNsense firewall rule missing
```
New VLAN interfaces are blocked by default.

Add:

```
Firewall → Rules → VLAN_INTERFACE
Action: Pass
Source: VLAN network
Destination: any
```
You may be on the wrong network.

Use the OPNsense gateway IP for your current VLAN:

| Current client IP | OPNsense UI | 
|---|---|
| `192.168.1.x` | `https://192.168.1.1` | 
| `192.168.20.x` | `https://192.168.20.1` | 
| `192.168.30.x` | `https://192.168.30.1` | 
| `192.168.0.x` | WAN side, normally blocked | 

If Windows has:

```
192.168.0.x
Gateway 192.168.0.1
```
then it is using FritzBox directly.

That does not test the OPNsense VLAN lab.

For OPNsense CLIENTS VLAN, Windows should have:

```
192.168.30.x
Gateway 192.168.30.1
```
For OPNsense SERVERS VLAN:

```
192.168.20.x
Gateway 192.168.20.1
```
Release current DHCP lease:

`ipconfig /release`
Request new DHCP lease:

`ipconfig /renew`
Show all network config:

`ipconfig /all`
Flush DNS cache:

`ipconfig /flushdns`
Test gateway:

`ping 192.168.30.1`
Test internet routing:

`ping 8.8.8.8`
Test DNS:

`nslookup google.com`
Test specific port:

`Test-NetConnection 192.168.20.162 -Port 3389`
Show adapters:

`Get-NetAdapter`
Disable adapter:

`Disable-NetAdapter -Name "Ethernet" -Confirm:$false`
Enable adapter:

`Enable-NetAdapter -Name "Ethernet" -Confirm:$false`
Show network config:

`cat /etc/network/interfaces`
Edit network config:

`nano /etc/network/interfaces`
Apply network config without reboot:

`ifreload -a`
Restart a VM only:

```
VM → Shutdown
VM → Start
```
Do not reboot the whole Proxmox host unless necessary.

| NIC | Bridge | VLAN Tag | Purpose | 
|---|---|---|---|
| net0 | vmbr0 | empty | WAN | 
| net1 | vmbr1 | empty | LAN trunk | 

| NIC | Bridge | VLAN Tag | Purpose | 
|---|---|---|---|
| netX | vmbr1 | 20 | SERVERS VLAN | 

| NIC | Bridge | VLAN Tag | Purpose | 
|---|---|---|---|
| netX | vmbr1 | 30 | CLIENTS VLAN | 

For this lab:

```
Unchecked
```
Use OPNsense firewall rules instead.

You can explain the lab like this:

I built a Proxmox-based network lab using OPNsense as a virtual firewall/router. I connected OPNsense with two NICs: one WAN interface to the FritzBox/home network and one LAN trunk interface to an internal VLAN-aware Proxmox bridge. I then created separate VLANs for servers and clients, configured OPNsense interfaces as VLAN gateways, added DHCP scopes per VLAN, and assigned Windows Server to VLAN 20 and Windows 11 to VLAN 30 using Proxmox VLAN tags. After troubleshooting VLAN-aware bridge settings, Proxmox firewall blocking, DHCP timeouts, and Windows adapter routing issues, I implemented OPNsense firewall rules to allow controlled traffic from each VLAN. This gave me a practical understanding of VLAN segmentation, DHCP, DNS, NAT, firewall rule direction, and enterprise-style network isolation.


```
VLAN = logical separation
OPNsense = routing/firewall/DHCP/DNS/NAT
Proxmox = virtual switching and VLAN tagging
vmbr1 must be VLAN-aware
OPNsense LAN NIC is trunk/untagged in Proxmox
VMs receive VLAN tags in Proxmox
New OPNsense VLAN interfaces are blocked by default
Firewall rules are placed where traffic enters the firewall
Direction is usually "in"
DHCP working does not mean firewall traffic is allowed
169.254.x.x means DHCP failed
192.168.0.x means direct FritzBox/home network
192.168.20.x means SERVERS VLAN
192.168.30.x means CLIENTS VLAN
```
After basic rules work, improve security:

Allow:

```
CLIENTS → Internet
CLIENTS → Windows Server only on required ports
```
Block:

```
CLIENTS → MGMT
CLIENTS → other internal networks unless needed
```
Allow:

```
GUEST → Internet
```
Block:

```
GUEST → RFC1918 private networks
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
```
Allow:

```
MGMT → all infrastructure
```
Restrict:

```
SERVERS/CLIENTS/GUEST → MGMT
```
Replace broad rules like:

```
Allow CLIENTS to any
Allow SERVERS to any
```
with stricter rules.

Example for CLIENTS to Windows Server:

```
CLIENTS net → 192.168.20.162 → DNS 53
CLIENTS net → 192.168.20.162 → Kerberos 88
CLIENTS net → 192.168.20.162 → LDAP 389
CLIENTS net → 192.168.20.162 → SMB 445
CLIENTS net → 192.168.20.162 → RDP 3389 if needed
```
For learning, broad rules are okay first. For enterprise practice, tighten rules after verification.

SERVERS VLAN:

```
Gateway: 192.168.20.1
Windows Server: 192.168.20.x
DHCP working
```
CLIENTS VLAN:

```
Gateway: 192.168.30.1
Windows 11: 192.168.30.x
DHCP working
```
Next required step:

```
Add firewall rules:
CLIENTS network → any
SERVERS network → any
```
Then test:

```
ping gateway
ping 8.8.8.8
nslookup google.com
browse internet
access OPNsense UI using the VLAN gateway IP
```