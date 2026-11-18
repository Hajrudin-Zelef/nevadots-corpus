| title | Unifi SSH Guide | 
|---|---|
| date | 2026-03-28 | 
| lastmod | 2026-03-28 | 
| draft | false | 
| author | Andrew Jones | 
| description | SSH deployment methods involving Ubiquity network devices. | 
| categories |  | 
| tags |  | 
| featuredImage | /images/posts/hiclipart.com.png | 
| toc | true | 
| SSH | 
| Unifi | SSH | 
SSH behaviour across the UniFi product family is not consistent. The UNAS Pro runs a full Debian-based Linux environment. UniFi switches run a proprietary firmware with a limited shell. UniFi gateways (UDM, UDM Pro, UXG) run a locked-down Linux environment with their own persistence model. Each requires a different approach and carries different caveats around authentication, key storage, and what survives a reboot or firmware update.
This guide documents the known working methods for each device type as of UniFi Network 9.x.
UniFi devices fall into two distinct tiers when it comes to SSH configuration ownership. Understanding which tier a device belongs to determines how SSH is configured, where credentials are managed, and what survives a reboot or firmware update.
These devices run full Linux environments and own their SSH stack independently of the controller. SSH can be configured directly on the device via sshd_config, authorized_keys, and standard Linux tooling. The controller can push credentials to these devices, but the device retains its own SSH daemon and can be accessed regardless of controller availability.
| Device | OS / Environment | Default SSH Port | Key Auth Support | Persistent Key Storage | 
|---|---|---|---|---|
| UNAS Pro | Debian Linux | 22 | Yes | Yes (rwfs overlay) | 
| UDM Pro / UDM SE | UniFi OS (Debian-based) | 22 | Yes | Partial (lost on firmware update) | 
| UDR / UXG Pro | UniFi OS (Debian-based) | 22 | Yes | Partial (lost on firmware update) | 
These devices have no meaningful independent SSH configuration. SSH credentials are pushed down from the controller at adoption and updated whenever the controller's Device SSH Authentication settings change. There is nothing to configure on the device directly — SSH access is entirely dependent on what the controller has provisioned.
| Device | OS / Environment | Default SSH Port | Key Auth Support | Persistent Key Storage | 
|---|---|---|---|---|
| UniFi Managed Switch (USW) | Proprietary firmware | 22 | No | No | 
| UniFi Access Point (UAP) | BusyBox / OpenWrt-based | 22 | No | No | 
Note: The USW-Ultra is an exception even within Tier 2 — it has no SSH access at all. See the Unadopted Device section for details.
Unadopted devices use factory default credentials. These are well known and should be treated as temporary — they are overwritten the moment a device is adopted by a controller.
| Device Type | Newer Firmware | Older Firmware | 
|---|---|---|
| APs, Switches, Cameras | ui /ui | ubnt /ubnt | 
| Gateways, Consoles (UDM, UXG, CloudKey) | root /ui | root /ubnt | 
If you are unsure which applies, try the newer credentials first. If those fail, try the older set. If both fail, the device has most likely been adopted by another controller and retains that controller's SSH password — a factory reset is the only recovery path in that scenario.
If the device has not received a DHCP lease (e.g. connected directly to a laptop with no DHCP server), it will fall back to a static IP:
| Device Type | Default Fallback IP | 
|---|---|
| Gateways (UDM, UXG, USG) | 192.168.1.1 | 
| APs, Switches, Cameras | 192.168.1.20 | 
Reference: UniFi Default SSH Credentials - UniHosted
# Newer firmware - APs and switches
ssh ui@<DEVICE-IP>
# Newer firmware - gateways and consoles
ssh root@<DEVICE-IP>
# Older firmware - APs and switches
ssh ubnt@<DEVICE-IP>
# Older firmware - gateways and consoles
ssh root@<DEVICE-IP>
# password: ubnt
If a device is on the network but not appearing in the controller, or is pointing at the wrong controller, SSH in using the default credentials and run the set-inform command to point it at your controller manually:
set-inform http://<CONTROLLER-IP>:8080/inform
For a locally hosted controller at 10.2.1.x for example:
set-inform http://10.2.1.10:8080/inform
The device will then appear as pending adoption in the UniFi Network controller. The controller must be reachable from the device on port 8080 for this to work.
An orphaned device is one that was previously adopted by a controller you no longer have access to. It will not respond to default credentials and cannot be pointed at a new controller via SSH without the original controller password. The only recovery path is a physical factory reset:
- Locate the reset pinhole on the device (underside or rear panel)
- Press and hold with a paperclip for approximately 10 seconds until the LED flashes
- Allow the device to reboot to a steady LED state
- Default credentials are restored and the device is ready for fresh adoption
The USW-Ultra series switches do not support SSH at all — neither before nor after adoption. The traditional set-inform adoption workflow is not available on these devices. If DHCP Option 43 is not configured to push the controller inform URL automatically, alternatives include using the UniFi mobile app on the same network segment, or temporarily spinning up a local controller on a laptop connected to the same LAN to adopt the device and then redirect it to the primary controller.
Reference: USW-Ultra and the adoption challenge - Cody Deluisio
The set-inform command is not limited to local network adoption. Devices at remote sites can be pointed at a controller over the internet using either a static WAN IP or a DDNS hostname. This is the standard method for managing remote site devices from a centralised controller without requiring a VPN.
Before attempting remote adoption the following must be in place:
| Requirement | Detail | 
|---|---|
| Port 8080 open inbound | The controller must accept TCP 8080 from the internet or the remote site's WAN IP | 
| Controller reachable by hostname or IP | Either a static WAN IP or a DDNS hostname resolving to the controller's WAN IP | 
| Device on the remote network | Device must have an active internet connection | 
| SSH access to the remote device | Either physically present or via an existing management path | 
If the controller site has a static WAN IP, point the device directly at it:
set-inform http://<CONTROLLER-WAN-IP>:8080/inform
For sites without a static WAN IP, Cloudflare DDNS provides a stable hostname that updates automatically as the WAN IP changes. This is the recommended approach for homelab and SMB deployments.
Assuming Cloudflare DDNS is configured and a hostname such as <DDNS.DOmain> resolves to the controller's current WAN IP:
set-inform http://<FQDN or IP>:8080/inform
The device resolves the hostname via public DNS, connects to the WAN IP on port 8080, and registers with the controller. It will then appear as pending adoption in the UniFi Network controller.
Cloudflare DDNS works by running a client on the controller host (or gateway) that periodically checks the current WAN IP and updates the Cloudflare DNS A record if it has changed. The controller's inform URL therefore remains stable even when the ISP changes the WAN IP.
The update client can be a lightweight script running on a cron job or a systemd timer, calling the Cloudflare API:
curl -s -X PUT "https://api.cloudflare.com/client/v4/zones/<ZONE-ID>/dns_records/<RECORD-ID>" \
  -H "Authorization: Bearer <API-TOKEN>" \
  -H "Content-Type: application/json" \
  --data "{\"type\":\"A\",\"name\":\"<DDNS-HOSTNAME>\",\"content\":\"$(curl -s https://checkip.amazonaws.com)\",\"ttl\":60,\"proxied\":false}"
Set TTL to 60 seconds on the Cloudflare DNS record so IP changes propagate quickly. Do not proxy the record through Cloudflare (orange cloud) — the device must reach the controller directly on port 8080, and Cloudflare's proxy only passes HTTP/HTTPS traffic on standard ports.
Reference: Cloudflare API - Update DNS Record
Port 8080 TCP must be open inbound on the controller site's gateway. If the controller is behind a pfSense or UniFi gateway, add a NAT port forward and corresponding firewall rule:
For pfSense:
Firewall > NAT > Port Forward
Interface: WAN
Protocol: TCP
Destination port: 8080
Redirect target IP: <CONTROLLER-LAN-IP>
Redirect target port: 8080
For UniFi gateway, add a port forward rule under Settings > Routing > Port Forwarding targeting the controller's LAN IP on port 8080.
After running set-inform on the remote device, confirm it is attempting to reach the controller:
# On the remote device
info
The info command on UniFi devices returns the current inform URL and status. Look for the inform URL reflecting the address you set and a status of connected or adopting.
If the device does not appear in the controller after 60-90 seconds, check:
# Confirm DNS resolves from the device
nslookup <FQDN>
# Confirm port 8080 is reachable from the device
nc -zv <FQDN> 8080
If nc fails, port 8080 is not reachable from the remote device — the issue is either a firewall rule on the controller site or the DDNS hostname is not resolving correctly.
Exposing port 8080 to the internet is required for remote adoption but represents an attack surface. Options to reduce exposure:
- Restrict inbound port 8080 to known remote site WAN IP ranges if they are static
- Use a VPN between sites instead of exposing port 8080 publicly, then use the VPN tunnel IP for set-inform
- Close port 8080 after adoption is complete if ongoing remote management uses an alternative path
Reference: UniFi - Inform Communication
SSH credentials must be configured before any of the methods below will work. Ubiquiti has moved this setting multiple times across recent versions. Use the table below to find the correct location for your controller version.
| Version | SSH Settings Location | 
|---|---|
| Pre-9.2.87 | Settings > System > Advanced > Device Authentication | 
| 9.2.87 - 9.5.20 | Devices > Device Updates and Settings (top right corner) | 
| 9.5.21+ | Devices > Device Updates and Settings (bottom left corner) | 
In all versions, the setting is labelled "Device SSH Authentication" and allows configuring a site-wide username and password that applies to all adopted devices. SSH keys can also be added from the same panel.
Note: UniFi OS SSH (the gateway console itself) and UniFi Network device SSH (switches, APs) are managed separately. Enabling one does not enable the other.
Reference: UniFi - Connecting with Debug Tools and SSH
The UNAS Pro runs a full Debian-based Linux environment. SSH behaves as it would on any standard Debian host. Key-based authentication is fully supported and can be made persistent across reboots using the rwfs overlay filesystem.
The UNAS Pro is a Tier 1 device and manages its own SSH daemon independently. Changing Device SSH Authentication credentials in the UniFi Network controller has no effect on the UNAS Pro. SSH access is controlled entirely by the local sshd_config and the local root password on the device itself.
ssh root@<UNAS-IP>
Password is the local root password set on the UNAS Pro itself. This is a Linux account password local to the device. If it was never explicitly set or is unknown, set it from an active SSH session:
passwd root
To enable password authentication if it has been disabled:
sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
systemctl reload ssh
Key-based authentication is the most secure and practical method for regular access. The UNAS Pro supports all standard OpenSSH key types. Ed25519 is preferred over RSA for new keys due to smaller key size and equivalent security at lower computational cost.
Reference: OpenSSH key types comparison - OpenBSD man page
ssh-keygen -t ed25519 -C "orion-unas" -f ~/.ssh/id_ed25519_unas
The -C flag adds a comment for identification. The -f flag specifies the output filename to keep UNAS keys separate from other key pairs.
ssh-copy-id -i ~/.ssh/id_ed25519_unas.pub root@<UNAS-IP>
Or manually append to the authorized_keys file:
cat ~/.ssh/id_ed25519_unas.pub | ssh root@<UNAS-IP> "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"ssh -i ~/.ssh/id_ed25519_unas root@<UNAS-IP>
By default on the UNAS Pro, the root filesystem is partially read-only. Changes written to standard paths may not survive a reboot. The rwfs overlay provides a writable layer that persists across reboots.
Confirm the rwfs mount point:
mount | grep rwfs
The persistent path for SSH authorized keys on the UNAS Pro is typically:
/rwfs/root/.ssh/authorized_keys
Symlink the standard path to the persistent location:
mkdir -p /rwfs/root/.ssh
chmod 700 /rwfs/root/.ssh
cp ~/.ssh/authorized_keys /rwfs/root/.ssh/authorized_keys
chmod 600 /rwfs/root/.ssh/authorized_keys
ln -sf /rwfs/root/.ssh/authorized_keys /root/.ssh/authorized_keys
Verify after reboot that the key is still present:
cat /root/.ssh/authorized_keys
Note: Firmware updates on the UNAS Pro may reset the overlay or overwrite symlinks. Verify key persistence after any firmware update.
Add an entry to ~/.ssh/config on the client machine to avoid specifying the key and user on every connection:
Host unas
    HostName <UNAS-IP>
    User root
    IdentityFile ~/.ssh/id_ed25519_unas
    Port 22
Connect with:
ssh unas
Before relying on any SSH method, verify the daemon configuration is in the expected state:
grep -E "PasswordAuthentication|PubkeyAuthentication|PermitRootLogin" /etc/ssh/sshd_config
Expected output with both methods enabled:
PermitRootLogin yes
PasswordAuthentication yes
PubkeyAuthentication absent from output means it is defaulting to yes — this is normal on the UNAS Pro.
To view the full config:
cat /etc/ssh/sshd_config
The UNAS Pro sshd_config contains an include directive at the top:
Include /etc/ssh/sshd_config.d/*.conf
Any .conf files in this directory are loaded and can override the main config. A directive such as PasswordAuthentication no in an included file will override PasswordAuthentication yes in the main file. Always check this directory if authentication behaves unexpectedly:
ls /etc/ssh/sshd_config.d/
cat /etc/ssh/sshd_config.d/*.conf
The UNAS Pro may contain an AllowGroups directive in /etc/ssh/sshd_config.d/ — for example:
AllowGroups root
This restricts SSH access to users belonging only to the root group. Connecting as root is unaffected, but any other user account cannot SSH in unless added to that group. This is expected and correct for a single-admin homelab NAS.
Always test both methods from a second terminal before closing your active session:
# Test key auth
ssh -i ~/.ssh/id_ed25519_unas root@<UNAS-IP>
# Test password auth explicitly
ssh -o PreferredAuthentications=password -o PubkeyAuthentication=no root@<UNAS-IP>
Use reload rather than restart when applying config changes:
systemctl reload ssh
systemctl status ssh
Check which port SSH is listening on:
ss -tlnp | grep sshd
View active SSH sessions:
who
UniFi gateways run UniFi OS, a Debian-based environment with an additional application layer managing network services. SSH access reaches the UniFi OS shell, from which the UniFi Network application container can also be accessed.
ssh root@<GATEWAY-IP>
Password is the SSH password set in UniFi Network > Settings > System > Advanced. This is the same credential used across all adopted devices.
Key authentication is supported on UniFi gateways but persistence is more limited than on the UNAS Pro.
ssh-copy-id -i ~/.ssh/id_ed25519_unas.pub root@<GATEWAY-IP>
On UniFi gateways, ~/.ssh/authorized_keys is stored in the root filesystem which may be reset on firmware update. There is no officially documented persistent overlay equivalent to the UNAS Pro rwfs path. Keys survive normal reboots but should be re-applied after firmware updates.
Reference: UniFi OS SSH key persistence - community discussion
From the gateway SSH shell, the UniFi Network application runs in a container that can be accessed directly:
# On UDM Pro / UDM SE (podman-based)
unifi-os shell
# On older UDM (docker-based)
docker exec -it unifi-network-application /bin/bashHost udm
    HostName <GATEWAY-IP>
    User root
    IdentityFile ~/.ssh/id_ed25519_unas
    Port 22
UniFi switches run proprietary firmware with a restricted shell environment. Full Linux functionality is not available. SSH access is primarily useful for diagnostics, checking link state, and running limited show commands.
ssh admin@<SWITCH-IP>
Username is admin. Password is the SSH password set in UniFi Network > Settings > System > Advanced.
Note: Some switch firmware versions use ubnt as the default username on factory reset devices that have not yet been adopted.
ssh ubnt@<SWITCH-IP>
# Default password: ubnt
| Command | Function | 
|---|---|
| info | Show system information | 
| show interfaces | Show interface status | 
| show mac-address-table | Show MAC address table | 
| show spanning-tree | Show spanning tree status | 
| show lldp neighbors | Show LLDP neighbours | 
| reboot | Reboot the switch | 
Full Linux commands such as ip, ss, systemctl, or package management are not available on switch firmware.
Key-based authentication is not reliably supported across all USW firmware versions. Password authentication should be treated as the primary method for switch access.
Access points run a BusyBox or OpenWrt-based environment. SSH access is available primarily for diagnostics.
ssh admin@<AP-IP>
Same SSH password as all other adopted devices. Username ubnt with password ubnt applies to factory reset or unadopted devices.
# Show system info
info
# Show wireless interface status
iwconfig
# Show connected clients
cat /proc/net/arp
# Show current channel and frequency
iwlist ath0 channel
# Restart the AP management agent
syswrapper.sh restart
# View logs
logread
Key authentication is not persistently supported on access points. Keys written to ~/.ssh/authorized_keys will be lost on reboot as the AP filesystem is largely volatile. Password authentication is the practical method for AP access.
SSH is not enabled on the device or the device has not been adopted by the controller.
Verify SSH is enabled: UniFi Network > Settings > System > Advanced > Device SSH Authentication.
The password entered does not match the SSH password in the controller. Confirm the password in UniFi Network settings. On factory reset devices, try ubnt / ubnt.
The public key is not present in authorized_keys on the target device, or the file permissions are incorrect.
Verify on the target device:
ls -la ~/.ssh/
cat ~/.ssh/authorized_keys
Correct permissions:
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
The authorized_keys file is not stored on the rwfs overlay. Follow the persistent key storage steps in the UNAS Pro section above.
Expected behaviour on gateways and APs. Re-copy the public key after any firmware update:
ssh-copy-id -i ~/.ssh/id_ed25519_unas.pub root@<DEVICE-IP>
Occurs when a device has been factory reset or replaced but the same IP is reused. Remove the stale entry from the known hosts file:
ssh-keygen -R <DEVICE-IP>ssh -vvv root@<DEVICE-IP>
The triple verbose flag shows the full authentication negotiation and will identify exactly where a connection is failing.
A consolidated ~/.ssh/config block for the ORION environment:
Host unas
    HostName 10.2.60.x
    User root
    IdentityFile ~/.ssh/id_ed25519_unas
    Port 22
Host udm
    HostName 10.2.1.x
    User root
    IdentityFile ~/.ssh/id_ed25519_unas
    Port 22
Host sw-core
    HostName 10.2.1.x
    User admin
    Port 22
Host sw-access
    HostName 10.2.1.x
    User admin
    Port 22
Replace IP addresses with actual device addresses. Key authentication entries can be omitted for switches where password auth is the only reliable method.
---
## References
---
- UniFi Default SSH Credentials: https://www.unihosted.com/blog/default-password-in-unifi-devices
- USW-Ultra Adoption Challenge: https://deluisio.com/networking/unifi/2025/02/05/the-missing-ssh-unifi-ultra-switches-and-the-adoption-challenge/
- Cloudflare API - Update DNS Record: https://developers.cloudflare.com/api/resources/dns/subresources/records/methods/update/
- UniFi - Required Ports Reference: https://help.ui.com/hc/en-us/articles/204976094-UniFi-Network-Required-Ports-Reference
- UniFi - Connecting with Debug Tools and SSH: https://help.ui.com/hc/en-us/articles/204909374-Connecting-to-UniFi-with-Debug-Tools-SSH
- OpenSSH Key Types - OpenBSD man page: https://man.openbsd.org/ssh-keygen
- UniFi OS SSH Key Persistence - Community Discussion: https://community.ui.com/questions/SSH-Key-Authentication/a25be39d-9873-4f3e-9a2b-5870ef399d65