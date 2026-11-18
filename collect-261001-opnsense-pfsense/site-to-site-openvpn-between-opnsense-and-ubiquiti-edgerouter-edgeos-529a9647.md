After much troubleshooting, I was able to successfully establish OpenVPN “Site-to-Site” VPN tunnel between my primary OPNsense firewall and an edge Ubiquiti EdgeRouter (ERLite-3). This guide will show you how to setup the OPNsense side using the new “instances” configuration and how to setup your EdgeRouter OpenVPN profile manually.
The Problem
There are three main challenges at play here: 1) preshared keys are being depreciated and 2) finding interoperability between major versions of OpenVPN ciphers and 3) the Ubiquiti documentation, guides and knowledge around setting up site-to-site VPNs all focus on preshared keys and there are limitation in the EdgeOS gui which does not pass validation thus we must pivot to a traditional OpenVPN profile configuration on the EdgeRouter.
In summary, the EdgeRouter interface does not validate/support the options we need to connect to a newer version of OpenVPN.
Configuration
Assumptions
This article assumes you have your own:
- Root CA
- Certificate & key for Site A acting as the OpenVPN server
- Certificate & key for Site B acting as the OpenVPN client
- Both Site A & Site B certificates are signed by your Root CA
This article will not help you create a Root CA, issuing and signing certificates. I will say there is a nifty tool called XCA which I highly recommend for helping you do this and keep track of all your certificates. Here is a XCA How-to Tutorial. You’ll want to use the TLS_server profile for Site A and TLS_client Profile for Site B within XCA.
Additionally, I assume you’ve generated a TLS-CRPYT key. You can learn to do that here: EasyRSA3 OpenVPN Howto. This example uses a TLS-CRYPT key not the TLS-CRYPT-v2 key. TLS-CRYPT-v2 is not supported in OpenVPN 2.4 (EdgeRouter). This (TLS-CRYPT-v2) was introduced in OpenVPN 2.5.
Contextual Notes
This tutorial is focused around OpenVPN operating via UDP and not TCP. You can change everything to be TCP if you wish, just make sure the ports and protocols are consistent throughout your configuration. I also opt to operate on higher port ranges to along with firewall inbound source address filtering. Defining your source (Site B) IP address or through an Alias you can define a DDNS FQDN to limit the scope of who can talk inbound to that port. This is good for the security-minded folk. If you don’t care that Russia or China knocks on your OpenVPN front door make Step 9, Source: any!
Topology
OPNsense “Site A”
- VPN -> OpenVPN -> Instances [new] -> Static Keys (tab) -> Click “+”
- Add your generated TLS-CRYPT key:
  - Description: OpenVPN TLS-CRYPT Key
  - Mode: crypt (Encrypt and authenticate all control channel packets)
  - Static Key: <copy/paste key here with —–BEGIN OpenVPN Static key V1—– and —–END OpenVPN Static key V1—–>
- Press “Save.
- VPN -> OpenVPN -> Instances [new] -> Click “+”
- Toggle “Advance Mode”
  - Role: Server
  - Description: OpenVPN Site-to-Site
  - Protocol: UDP (IPv4)
  - Port: <23222>
  - Bind Address: <leave blank>
  - Type: tun
  - (Optional) Verbosity: 4 (Normal)
  - (Optional) Keep alive interval: 60
  - (Optional) Keep alive timeout: 300
  - Server (IPv4): <10.255.0.0/24>
  - Topology: subnet
  - Trust: <Site B Certificate>
  - Certificate Authority: <Root CA>
  - Verify Client Certificate: required
  - Certificate Depth: Two (Client+Intermediate+Server)
  - TLS static key: <select OpenVPN key from step 1>
  - Auth: SHA256 (256-bit)
  - Data Ciphers: AES-256-GCM, AES-128-GCM
  - Data Ciphers Fallback: Nothing selected
  - Authentication: Nothing selected
  - Local Network: <10.1.0.0/24>
  - Remote Network: <10.2.0.0/24>
  - .. Leave the rest of the settings default …
- Press “Save”
- VPN -> OpenVPN -> Client Specific Overrides -> Click “+”
  - Enabled: Checked
  - Servers: <select the one you just created in step 2>
  - Common name: <name of your Site B certificate CN>
  - Local Network: <10.1.0.0/24>
  - Remote Network: <10.2.0.0/24>
- Press “Save”
- Firewall -> Rules -> WAN-> Click “+”
  - Action: Pass
  - Interface: WAN
  - Direction: in
  - TCP/IP Version: IPv4
  - Protocol: UDP
  - Source: <source ip address or fqdn/ddns> -or- <any>
  - Destination: WAN address
  - Destination port range: from (other) <23222> to (other) <23222>
  - Description: OpenVPN Tunnel
- Press “Save”
- Press “Apply changes”
An Important Note About “Client Specific Overrides”
In short, they (Client Specific Overrides) are required for site-to-site deployments. They serve a very specific purpose and it’s tied to routing tables within OpenVPN. They are called “iroutes” which is different than OPNsense system’s routing table. You may see the routes in the OPNsense system but that information doesn’t correlate the SSL certificate that was authenticated and which routes that unique client (in this case) have on the other end of the wire.
I had a scenario where I had the VPN tunnel up and I was able to ping the 10.255.0.2 (which means the tunnel is up, passing traffic but I couldn’t ping or connect to anything on the 10.2.0.0/24 network. It was tied to a misconfiguration on the Client Specific Overrides.
It’s worthy noting, iroutes within OpenVPN only apply when your tunnel network is larger than /30 network meaning you have the potential of more than one site-to-site client in your tunnel network. Learn more about iroutes, here.
Ubiquiti EdgeRouter “Site B”
To setup the EdgeRouter this will be done via terminal. You can’t do it through the web interface, it’s simply not supported.
- Connect via SSH to your EdgeRouter
- bash
- sudo su –
- mkdir /var/log/openvpn
- chown root:vyattacfg /var/log/openvpn
- chmod 777 /var/log/openvpn
- vi /config/site-to-site.ovpn
- Modify the configuration and put it into vim (right mouse click). Click here for the configuration file
  - remote <ddns.noip.org> (Remote Address) – Update to your fqdn/ip
  - rport <23222> – (Remote Port) Update to Site A’s port number
  - lport <24222> – (Local Port) Update to a higher port number locally at random
  - writepid </var/run/openvpn-vtun1.pid> – (PID file) Change filename to reflect the correct vtun device on the EdgeRouter
  - status <status /var/run/openvpn/status/vtun1.status 30> – (Status file) Change filename to reflect the correct vtun device on the EdgeRouter
  - Insert your certificates into the <tls-crypt> <ca> <cert> and <key> sections.
- Save by pressing colon “:” typing “wq” and pressing enter
- chown root:vyattacfg /config/site-to-site.ovpn
- chmod 744 /config/site-to-site.ovpn
- exit
- configure
- set interfaces openvpn vtun1 config-file /config/site-to-site.ovpn
- commit
- save
This configuration on the EdgeRouter side, since it’s role is a client will receive routing information that will get pushed from the OPNsense instance. Make sure you’re local and remote routes (specifically subnets) are correct on the OPNsense OpenVPN Instance. If you push bad routes, you’ll never route traffic.
Troubleshooting
If you run into an issue on either side, please check the OpenVPN logs, they are very telling when you have ‘verb 4’ / verbose 4 set. Generally speaking, you don’t need to get more aggressive with logging unless you’ve hit a software bug that needs more detailed dump logging. If you encounter any issues on the EdgeRouter side you’ll need to disable/renable the vtun interface. You can do this by issuing the following command:
configure
set interfaces openvpn vtun1 disable
commit
delete interfaces openvpn vtun1 disable
commit
save
Lastly, check your firewall rules. You’re routes maybe correct but if you have a deny policy or don’t have an open firewall acl, again you won’t route traffic.
Technical Dumps
EdgeRouter OpenVPN Version Information
root@edgerouter:/var/log# openvpn --version
OpenVPN 2.4.7 mips-unknown-linux-gnu [SSL (OpenSSL)] [LZO] [LZ4] [EPOLL] [PKCS11] [MH/PKTINFO] [AEAD] built on Apr 22 2022
library versions: OpenSSL 1.0.2u  20 Dec 2019, LZO 2.08
Originally developed by James Yonan
Copyright (C) 2002-2018 OpenVPN Inc <sales@openvpn.net>
Compile time defines: enable_async_push=no enable_comp_stub=no enable_crypto=yes enable_crypto_ofb_cfb=yes enable_debug=yes enable_def_auth=yes enable_dependency_tracking=no enable_dlopen=unknown enable_dlopen_self=unknown enable_dlopen_self_static=unknown enable_fast_install=needless enable_fragment=yes enable_iproute2=yes enable_libtool_lock=yes enable_lz4=yes enable_lzo=yes enable_maintainer_mode=no enable_management=yes enable_multihome=yes enable_pam_dlopen=no enable_pedantic=no enable_pf=yes enable_pkcs11=yes enable_plugin_auth_pam=yes enable_plugin_down_root=yes enable_plugins=yes enable_port_share=yes enable_selinux=no enable_server=yes enable_shared=yes enable_shared_with_static_runtimes=no enable_silent_rules=no enable_small=no enable_static=yes enable_strict=no enable_strict_options=no enable_systemd=yes enable_werror=no enable_win32_dll=yes enable_x509_alt_username=yes with_aix_soname=aix with_crypto_library=openssl with_gnu_ld=yes with_mem_check=no with_sysroot=no
EdgeRouter OpenVPN Cipher Information
root@edgerouter:/usr/sbin#.\openvpn --show-ciphers
The following ciphers and cipher modes are available for use
with OpenVPN.  Each cipher shown below may be use as a
parameter to the --cipher option.  The default key size is
shown as well as whether or not it can be changed with the
--keysize directive.  Using a CBC or GCM mode is recommended.
In static key mode only CBC mode is allowed.
AES-128-CBC  (128 bit key, 128 bit block)
AES-128-CFB  (128 bit key, 128 bit block, TLS client/server mode only)
AES-128-CFB1  (128 bit key, 128 bit block, TLS client/server mode only)
AES-128-CFB8  (128 bit key, 128 bit block, TLS client/server mode only)
AES-128-GCM  (128 bit key, 128 bit block, TLS client/server mode only)
AES-128-OFB  (128 bit key, 128 bit block, TLS client/server mode only)
AES-192-CBC  (192 bit key, 128 bit block)
AES-192-CFB  (192 bit key, 128 bit block, TLS client/server mode only)
AES-192-CFB1  (192 bit key, 128 bit block, TLS client/server mode only)
AES-192-CFB8  (192 bit key, 128 bit block, TLS client/server mode only)
AES-192-GCM  (192 bit key, 128 bit block, TLS client/server mode only)
AES-192-OFB  (192 bit key, 128 bit block, TLS client/server mode only)
AES-256-CBC  (256 bit key, 128 bit block)
AES-256-CFB  (256 bit key, 128 bit block, TLS client/server mode only)
AES-256-CFB1  (256 bit key, 128 bit block, TLS client/server mode only)
AES-256-CFB8  (256 bit key, 128 bit block, TLS client/server mode only)
AES-256-GCM  (256 bit key, 128 bit block, TLS client/server mode only)
AES-256-OFB  (256 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-128-CBC  (128 bit key, 128 bit block)
CAMELLIA-128-CFB  (128 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-128-CFB1  (128 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-128-CFB8  (128 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-128-OFB  (128 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-192-CBC  (192 bit key, 128 bit block)
CAMELLIA-192-CFB  (192 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-192-CFB1  (192 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-192-CFB8  (192 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-192-OFB  (192 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-256-CBC  (256 bit key, 128 bit block)
CAMELLIA-256-CFB  (256 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-256-CFB1  (256 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-256-CFB8  (256 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-256-OFB  (256 bit key, 128 bit block, TLS client/server mode only)
SEED-CBC  (128 bit key, 128 bit block)
SEED-CFB  (128 bit key, 128 bit block, TLS client/server mode only)
SEED-OFB  (128 bit key, 128 bit block, TLS client/server mode only)
The following ciphers have a block size of less than 128 bits,
and are therefore deprecated.  Do not use unless you have to.
BF-CBC  (128 bit key by default, 64 bit block)
BF-CFB  (128 bit key by default, 64 bit block, TLS client/server mode only)
BF-OFB  (128 bit key by default, 64 bit block, TLS client/server mode only)
...
EdgeRouter OpenVPN Configuration
Back to EdgeRoute “Site B” Configuration
client
tls-client
dev-type tun
dev vtun1
rport 23222
remote ddns.noip.org
remote-cert-tls server
lport 24222
tun-mtu 1500
auth SHA256
cipher AES-256-GCM
keysize 256
explicit-exit-notify
persist-tun
persist-key
fast-io
auth-nocache
float
resolv-retry 3
persist-tun
verb 4
writepid /var/run/openvpn-vtun1.pid
status /var/run/openvpn/status/vtun1.status 30
log /var/log/openvpn/ovpn.log
ping 10
ping-restart 60
<tls-crypt>
-----BEGIN OpenVPN Static key V1-----
..
-----END OpenVPN Static key V1-----
</tls-crypt>
<ca>
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
</ca>
<cert>
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
</cert>
<key>
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
</key>
EdgeRouter OpenVPN Connection Log File
sudo cat /var/log/openvpn/ovpn.logThu Jan 25 11:47:28 2024 us=120846 OpenVPN 2.4.7 mips-unknown-linux-gnu [SSL (OpenSSL)] [LZO] [LZ4] [EPOLL] [PKCS11] [MH/PKTINFO] [AEAD] built on Apr 22 2022
Thu Jan 25 11:47:28 2024 us=121133 library versions: OpenSSL 1.0.2u  20 Dec 2019, LZO 2.08
Thu Jan 25 11:47:28 2024 us=135172 Outgoing Control Channel Encryption: Cipher 'AES-256-CTR' initialized with 256 bit key
Thu Jan 25 11:47:28 2024 us=135560 Outgoing Control Channel Encryption: Using 256 bit message hash 'SHA256' for HMAC authentication
Thu Jan 25 11:47:28 2024 us=135839 Incoming Control Channel Encryption: Cipher 'AES-256-CTR' initialized with 256 bit key
Thu Jan 25 11:47:28 2024 us=136156 Incoming Control Channel Encryption: Using 256 bit message hash 'SHA256' for HMAC authentication
Thu Jan 25 11:47:28 2024 us=137063 Control Channel MTU parms [ L:1621 D:1156 EF:94 EB:0 ET:0 EL:3 ]
Thu Jan 25 11:47:28 2024 us=464874 Data Channel MTU parms [ L:1621 D:1450 EF:121 EB:406 ET:0 EL:3 ]
Thu Jan 25 11:47:28 2024 us=465237 Local Options String (VER=V4): 'V4,dev-type tun,link-mtu 1549,tun-mtu 1500,proto UDPv4,cipher AES-256-GCM,auth [null-digest],keysize 256,key-method 2,tls-client'
Thu Jan 25 11:47:28 2024 us=465414 Expected Remote Options String (VER=V4): 'V4,dev-type tun,link-mtu 1549,tun-mtu 1500,proto UDPv4,cipher AES-256-GCM,auth [null-digest],keysize 256,key-method 2,tls-server'
Thu Jan 25 11:47:28 2024 us=465625 TCP/UDP: Preserving recently used remote address: [AF_INET]<ip obscured>:23222
Thu Jan 25 11:47:28 2024 us=465841 Socket Buffers: R=[294912->294912] S=[294912->294912]
Thu Jan 25 11:47:28 2024 us=466054 UDP link local (bound): [AF_INET][undef]:24222
Thu Jan 25 11:47:28 2024 us=466237 UDP link remote: [AF_INET]<ip obscured>:23222
Thu Jan 25 11:47:28 2024 us=513089 TLS: Initial packet from [AF_INET]<ip obscured>:23222, sid=36f84160 d53e60ae
Thu Jan 25 11:47:28 2024 us=606744 VERIFY OK: depth=1, CN=<ca obscured>
Thu Jan 25 11:47:28 2024 us=626181 VERIFY KU OK
Thu Jan 25 11:47:28 2024 us=626486 Validating certificate extended key usage
Thu Jan 25 11:47:28 2024 us=626735 ++ Certificate has EKU (str) TLS Web Server Authentication, expects TLS Web Server Authentication
Thu Jan 25 11:47:28 2024 us=626953 VERIFY EKU OK
Thu Jan 25 11:47:28 2024 us=627157 VERIFY OK: depth=0, CN=<name obscured> 
Thu Jan 25 11:47:31 2024 us=333020 WARNING: 'link-mtu' is used inconsistently, local='link-mtu 1549', remote='link-mtu 1553'
Thu Jan 25 11:47:31 2024 us=333346 WARNING: 'cipher' is present in local config but missing in remote config, local='cipher AES-256-GCM'
Thu Jan 25 11:47:31 2024 us=333567 WARNING: 'auth' is used inconsistently, local='auth [null-digest]', remote='auth SHA256'
Thu Jan 25 11:47:31 2024 us=333878 WARNING: 'keysize' is used inconsistently, local='keysize 256', remote='keysize 128'
Thu Jan 25 11:47:31 2024 us=334874 Control Channel: TLSv1.2, cipher TLSv1/SSLv3 ECDHE-RSA-AES256-GCM-SHA384, 4096 bit RSA
Thu Jan 25 11:47:31 2024 us=335202 [<name obscured>] Peer Connection Initiated with [AF_INET]<ip obscured>:23222
Thu Jan 25 11:47:32 2024 us=409756 SENT CONTROL [<name obscured>]: 'PUSH_REQUEST' (status=1)
Thu Jan 25 11:47:32 2024 us=463398 PUSH: Received control message: 'PUSH_REPLY,route 10.1.0.0 255.255.0.0,route-gateway 10.255.0.1,topology subnet,ping 60,ping-restart 300,ifconfig 10.255.0.2 255.255.255.0,peer-id 0,cipher AES-256-GCM'
Thu Jan 25 11:47:32 2024 us=464096 OPTIONS IMPORT: timers and/or timeouts modified
Thu Jan 25 11:47:32 2024 us=464310 OPTIONS IMPORT: --ifconfig/up options modified
Thu Jan 25 11:47:32 2024 us=464450 OPTIONS IMPORT: route options modified
Thu Jan 25 11:47:32 2024 us=464577 OPTIONS IMPORT: route-related options modified
Thu Jan 25 11:47:32 2024 us=464700 OPTIONS IMPORT: peer-id set
Thu Jan 25 11:47:32 2024 us=464825 OPTIONS IMPORT: adjusting link_mtu to 1624
Thu Jan 25 11:47:32 2024 us=464947 OPTIONS IMPORT: data channel crypto options modified
Thu Jan 25 11:47:32 2024 us=465135 Data Channel MTU parms [ L:1552 D:1450 EF:52 EB:406 ET:0 EL:3 ]
Thu Jan 25 11:47:32 2024 us=466086 Outgoing Data Channel: Cipher 'AES-256-GCM' initialized with 256 bit key
Thu Jan 25 11:47:32 2024 us=466345 Incoming Data Channel: Cipher 'AES-256-GCM' initialized with 256 bit key
Thu Jan 25 11:47:32 2024 us=467418 ROUTE_GATEWAY <ip obscured>/<netmask obscured> IFACE=eth0 HWADDR=04:18:d6:f1:23:25
Thu Jan 25 11:47:32 2024 us=473094 TUN/TAP device vtun1 opened
Thu Jan 25 11:47:32 2024 us=473396 TUN/TAP TX queue length set to 100
Thu Jan 25 11:47:32 2024 us=473806 do_ifconfig, tt->did_ifconfig_ipv6_setup=0
Thu Jan 25 11:47:32 2024 us=474133 /sbin/ip link set dev vtun1 up mtu 1500
Thu Jan 25 11:47:32 2024 us=491786 /sbin/ip addr add dev vtun1 10.255.0.2/24 broadcast 10.255.0.255
Thu Jan 25 11:47:32 2024 us=507385 /sbin/ip route add 10.1.0.0/16 via 10.225.0.1
Thu Jan 25 11:47:32 2024 us=518722 Initialization Sequence Completed
OPNsense OpenVPN Version Information
root@opnsense:~ $ openvpn --version
OpenVPN 2.6.8 amd64-portbld-freebsd13.2 [SSL (OpenSSL)] [LZO] [LZ4] [PKCS11] [MH/RECVDA] [AEAD]
library versions: OpenSSL 1.1.1w  11 Sep 2023, LZO 2.10
Originally developed by James Yonan
Copyright (C) 2002-2023 OpenVPN Inc <sales@openvpn.net>
Compile time defines: enable_async_push=no enable_comp_stub=no enable_crypto_ofb_cfb=yes enable_dco=no enable_debug=yes enable_dlopen=unknown enable_dlopen_self=unknown enable_dlopen_self_static=unknown enable_fast_install=needless enable_fragment=yes enable_iproute2=no enable_libtool_lock=yes enable_lz4=yes enable_lzo=yes enable_management=yes enable_pam_dlopen=no enable_pedantic=no enable_pkcs11=yes enable_plugin_auth_pam=yes enable_plugin_down_root=yes enable_plugins=yes enable_port_share=yes enable_selinux=no enable_shared=yes enable_shared_with_static_runtimes=no enable_silent_rules=no enable_small=no enable_static=yes enable_strict=yes enable_strict_options=no enable_systemd=no enable_unit_tests=no enable_werror=no enable_win32_dll=yes enable_wolfssl_options_h=yes enable_x509_alt_username=no with_aix_soname=aix with_crypto_library=openssl with_gnu_ld=yes with_mem_check=no with_openssl_engine=auto with_sysroot=no
OPNsense OpenVPN Cipher Information
root@opnsense:~ $ openvpn --show-ciphers
The following ciphers and cipher modes are available for use
with OpenVPN.  Each cipher shown below may be used as a
parameter to the --data-ciphers (or --cipher) option. In static
key mode only CBC mode is allowed.
See also openssl list -cipher-algorithms
AES-128-CBC  (128 bit key, 128 bit block)
AES-128-CFB  (128 bit key, 128 bit block, TLS client/server mode only)
AES-128-CFB1  (128 bit key, 128 bit block, TLS client/server mode only)
AES-128-CFB8  (128 bit key, 128 bit block, TLS client/server mode only)
AES-128-OFB  (128 bit key, 128 bit block, TLS client/server mode only)
AES-192-CBC  (192 bit key, 128 bit block)
AES-192-CFB  (192 bit key, 128 bit block, TLS client/server mode only)
AES-192-CFB1  (192 bit key, 128 bit block, TLS client/server mode only)
AES-192-CFB8  (192 bit key, 128 bit block, TLS client/server mode only)
AES-192-OFB  (192 bit key, 128 bit block, TLS client/server mode only)
AES-256-CBC  (256 bit key, 128 bit block)
AES-256-CFB  (256 bit key, 128 bit block, TLS client/server mode only)
AES-256-CFB1  (256 bit key, 128 bit block, TLS client/server mode only)
AES-256-CFB8  (256 bit key, 128 bit block, TLS client/server mode only)
AES-256-OFB  (256 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-128-CBC  (128 bit key, 128 bit block)
CAMELLIA-128-CFB  (128 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-128-CFB1  (128 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-128-CFB8  (128 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-128-OFB  (128 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-192-CBC  (192 bit key, 128 bit block)
CAMELLIA-192-CFB  (192 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-192-CFB1  (192 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-192-CFB8  (192 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-192-OFB  (192 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-256-CBC  (256 bit key, 128 bit block)
CAMELLIA-256-CFB  (256 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-256-CFB1  (256 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-256-CFB8  (256 bit key, 128 bit block, TLS client/server mode only)
CAMELLIA-256-OFB  (256 bit key, 128 bit block, TLS client/server mode only)
CHACHA20-POLY1305  (256 bit key, stream cipher, TLS client/server mode only)
SEED-CBC  (128 bit key, 128 bit block)
SEED-CFB  (128 bit key, 128 bit block, TLS client/server mode only)
SEED-OFB  (128 bit key, 128 bit block, TLS client/server mode only)
AES-128-GCM  (128 bit key, 128 bit block, TLS client/server mode only)
AES-192-GCM  (192 bit key, 128 bit block, TLS client/server mode only)
AES-256-GCM  (256 bit key, 128 bit block, TLS client/server mode only)
The following ciphers have a block size of less than 128 bits,
and are therefore deprecated.  Do not use unless you have to.
BF-CBC  (128 bit key, 64 bit block)
BF-CFB  (128 bit key, 64 bit block, TLS client/server mode only)
BF-OFB  (128 bit key, 64 bit block, TLS client/server mode only)
...
References
- https://community.openvpn.net/openvpn/wiki/CipherNegotiation
- https://hohnstaedt.de/xca
- https://hohnstaedt.de/xca/index.php/documentation/stepbystep
- https://docs.netgate.com/pfsense/en/latest/recipes/openvpn-s2s-tls.html
- https://docs.netgate.com/pfsense/en/latest/vpn/openvpn/configure-overrides.html
- https://docs.netgate.com/pfsense/en/latest/troubleshooting/openvpn-iroute.html