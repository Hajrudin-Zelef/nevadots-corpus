I have a meraki VPN mesh which consists of 3 meraki firewalls and 1 OPNSense firewall. There are 3 IKEv2 IPsec connections setup on the OPNSense firewall, one for each meraki. They're all configured identically minus the remote endpoint address and the remote subnets. The tunnels between the OPNSense and meraki firewalls come up and work for a while but will drop offline after some time and need a restart in order to come back up. I figure I have a settings mismatch but I can't find it. I'm not sure if the meraki lifetime is equivalent to the re-auth or rekey time but I've tried both and the behavior doesn't seem to change. What's the best way to get a meraki device connected to OPNSense?
The time it takes to drop is based on the phase 1 re-auth/lifetime of the tunnel. If I set the lifetime(meraki) and re-auth(OPNSense) timers higher then it takes longer for the tunnel to die.
Note about the logs: 12:32 is when I restart the IPsec service on OPNSense to force a tunnel rebuild, 12:30 is when the tunnel died.
On the meraki side I have these logs(IPs have been replaced)
May 9 12:32:58      Non-Meraki VPN  Non-Meraki VPN negotiation  msg: <remote-peer-2|1403> CHILD_SA net-2{2050} established with SPIs cf753c86(inbound) c46fb58a(outbound) and TS 172.16.0.0/24 === 172.17.0.0/24
May 9 12:32:58      Non-Meraki VPN  Non-Meraki VPN negotiation  msg: <remote-peer-2|1403> IKE_SA remote-peer-2[1403] established between 192.0.2.114[192.0.2.114]...192.0.2.13[192.0.2.13]
May 9 12:32:28      Non-Meraki VPN  Non-Meraki VPN negotiation  msg: <remote-peer-2|1400> deleting IKE_SA remote-peer-2[1400] between 192.0.2.114[192.0.2.114]...192.0.2.13[192.0.2.13]
May 9 12:30:12      Non-Meraki VPN  Non-Meraki VPN negotiation  msg: <remote-peer-2|1400> closing CHILD_SA net-2{2046} with SPIs cdb2bcf7(inbound) (30205337 bytes) c8e6aa8f(outbound) (28885177 bytes) and TS 172.16.0.0/24 === 172.17.0.0/24
May 9 12:30:12      Non-Meraki VPN  Non-Meraki VPN negotiation  msg: May 9 19:30:12 10[IKE] <remote-peer-2|1400> outbound CHILD_SA net-2{2048} established with SPIs c075fb18(inbound) ca176942(outbound) and TS 172.16.0.0/24 === 172.17.0.0/24
May 9 12:30:12      Non-Meraki VPN  Non-Meraki VPN negotiation  msg: May 9 19:30:12 10[IKE] <remote-peer-2|1400> inbound CHILD_SA net-2{2048} established with SPIs c075fb18(inbound) ca176942(outbound) and TS 172.16.0.0/24 === 172.17.0.0/24
On the OPNSense I have these for the same time frame
2025-05-09T12:32:58-07:00   Notice  charon   [UPDOWN] received up-client event for reqid 2
2025-05-09T12:32:58-07:00   Notice  charon   [UPDOWN] received up-client event for reqid 2
2025-05-09T12:32:58-07:00   Notice  charon   [UPDOWN] received up-client event for reqid 2
2025-05-09T12:32:58-07:00   Notice  charon   [UPDOWN] received up-client event for reqid 2
2025-05-09T12:32:58-07:00   Informational   charon   09[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> CHILD_SA 4c518c5c-c2f4-4049-a569-60a578ab4fe9{2} established with SPIs c46fb58a_i cf753c86_o and TS 172.17.0.0/24 === 172.16.0.0/24
2025-05-09T12:32:58-07:00   Informational   charon   09[CFG] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> selected proposal: ESP:AES_CBC_256/HMAC_SHA2_256_128/NO_EXT_SEQ
2025-05-09T12:32:58-07:00   Informational   charon   09[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> maximum IKE_SA lifetime 3875s
2025-05-09T12:32:58-07:00   Informational   charon   09[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> scheduling reauthentication in 3515s
2025-05-09T12:32:58-07:00   Informational   charon   09[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> IKE_SA 964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9[2] established between 192.0.2.13[192.0.2.13]...192.0.2.114[192.0.2.114]
2025-05-09T12:32:58-07:00   Informational   charon   09[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> authentication of '192.0.2.114' with pre-shared key successful
2025-05-09T12:32:58-07:00   Informational   charon   09[ENC] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> parsed IKE_AUTH response 1 [ IDr AUTH SA TSi TSr ]
2025-05-09T12:32:58-07:00   Notice  charon   [UPDOWN] received up-client event for reqid 1
2025-05-09T12:32:58-07:00   Notice  charon   [UPDOWN] received up-client event for reqid 1
2025-05-09T12:32:58-07:00   Informational   charon   08[NET] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> received packet: from 192.0.2.239[500] to 192.0.2.13[500] (224 bytes)
2025-05-09T12:32:58-07:00   Informational   charon   09[NET] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> received packet: from 192.0.2.114[500] to 192.0.2.13[500] (272 bytes)
2025-05-09T12:32:58-07:00   Notice  charon   [UPDOWN] received up-client event for reqid 1
2025-05-09T12:32:58-07:00   Informational   charon   13[IKE] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> CHILD_SA 9864a88a-52fa-40e2-bcc0-eea0fb2757c4{1} established with SPIs c8a9de60_i c17ae8d4_o and TS 172.17.0.0/24 === 172.18.0.0/24
2025-05-09T12:32:58-07:00   Informational   charon   13[CFG] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> selected proposal: ESP:AES_CBC_256/HMAC_SHA2_256_128/NO_EXT_SEQ
2025-05-09T12:32:58-07:00   Informational   charon   13[IKE] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> maximum IKE_SA lifetime 3713s
2025-05-09T12:32:58-07:00   Informational   charon   13[IKE] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> scheduling reauthentication in 3353s
2025-05-09T12:32:58-07:00   Informational   charon   13[IKE] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> IKE_SA 8600b45c-2c67-49ed-b27f-593e09665e7a[1] established between 192.0.2.13[192.0.2.13]...192.0.2.226[192.0.2.226]
2025-05-09T12:32:58-07:00   Informational   charon   13[IKE] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> authentication of '192.0.2.226' with pre-shared key successful
2025-05-09T12:32:58-07:00   Informational   charon   13[ENC] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> parsed IKE_AUTH response 1 [ IDr AUTH SA TSi TSr ]
2025-05-09T12:32:58-07:00   Informational   charon   13[NET] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> received packet: from 192.0.2.226[500] to 192.0.2.13[500] (256 bytes)
2025-05-09T12:32:58-07:00   Informational   charon   13[NET] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> sending packet: from 192.0.2.13[500] to 192.0.2.239[500] (256 bytes)
2025-05-09T12:32:58-07:00   Informational   charon   13[ENC] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> generating IKE_AUTH request 1 [ IDi AUTH N(ESP_TFC_PAD_N) SA TSi TSr N(MULT_AUTH) N(EAP_ONLY) N(MSG_ID_SYN_SUP) ]
2025-05-09T12:32:58-07:00   Informational   charon   13[IKE] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> establishing CHILD_SA 8be16212-f51a-41bf-9f73-ae47d3f691f0{3}
2025-05-09T12:32:58-07:00   Informational   charon   13[IKE] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> authentication of '192.0.2.13' (myself) with pre-shared key
2025-05-09T12:32:58-07:00   Informational   charon   13[CFG] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> no IDi configured, fall back on IP address
2025-05-09T12:32:58-07:00   Informational   charon   13[CFG] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> selected proposal: IKE:AES_CBC_256/HMAC_SHA2_256_128/PRF_HMAC_SHA2_256/MODP_2048
2025-05-09T12:32:58-07:00   Informational   charon   13[ENC] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> parsed IKE_SA_INIT response 0 [ SA KE No N(NATD_S_IP) N(NATD_D_IP) N(FRAG_SUP) N(HASH_ALG) N(CHDLESS_SUP) N(MULT_AUTH) ]
2025-05-09T12:32:58-07:00   Informational   charon   13[NET] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> received packet: from 192.0.2.239[500] to 192.0.2.13[500] (472 bytes)
2025-05-09T12:32:58-07:00   Informational   charon   13[NET] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> sending packet: from 192.0.2.13[500] to 192.0.2.114[500] (304 bytes)
2025-05-09T12:32:58-07:00   Informational   charon   13[ENC] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> generating IKE_AUTH request 1 [ IDi AUTH N(ESP_TFC_PAD_N) SA TSi TSr N(MULT_AUTH) N(EAP_ONLY) N(MSG_ID_SYN_SUP) ]
2025-05-09T12:32:58-07:00   Informational   charon   13[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> establishing CHILD_SA 4c518c5c-c2f4-4049-a569-60a578ab4fe9{2}
2025-05-09T12:32:58-07:00   Informational   charon   13[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> authentication of '192.0.2.13' (myself) with pre-shared key
2025-05-09T12:32:58-07:00   Informational   charon   13[CFG] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> no IDi configured, fall back on IP address
2025-05-09T12:32:58-07:00   Informational   charon   13[CFG] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> selected proposal: IKE:AES_CBC_256/HMAC_SHA2_256_128/PRF_HMAC_SHA2_256/MODP_2048
2025-05-09T12:32:58-07:00   Informational   charon   13[ENC] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> parsed IKE_SA_INIT response 0 [ SA KE No N(NATD_S_IP) N(NATD_D_IP) N(FRAG_SUP) N(HASH_ALG) N(CHDLESS_SUP) N(MULT_AUTH) ]
2025-05-09T12:32:58-07:00   Informational   charon   13[NET] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> received packet: from 192.0.2.114[500] to 192.0.2.13[500] (472 bytes)
2025-05-09T12:32:58-07:00   Informational   charon   10[NET] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> sending packet: from 192.0.2.13[500] to 192.0.2.226[500] (288 bytes)
2025-05-09T12:32:58-07:00   Informational   charon   10[ENC] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> generating IKE_AUTH request 1 [ IDi AUTH N(ESP_TFC_PAD_N) SA TSi TSr N(MULT_AUTH) N(EAP_ONLY) N(MSG_ID_SYN_SUP) ]
2025-05-09T12:32:58-07:00   Informational   charon   10[IKE] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> establishing CHILD_SA 9864a88a-52fa-40e2-bcc0-eea0fb2757c4{1}
2025-05-09T12:32:58-07:00   Informational   charon   10[IKE] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> authentication of '192.0.2.13' (myself) with pre-shared key
2025-05-09T12:32:58-07:00   Informational   charon   10[CFG] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> no IDi configured, fall back on IP address
2025-05-09T12:32:58-07:00   Informational   charon   10[CFG] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> selected proposal: IKE:AES_CBC_256/HMAC_SHA2_256_128/PRF_HMAC_SHA2_256/MODP_2048
2025-05-09T12:32:58-07:00   Informational   charon   10[ENC] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> parsed IKE_SA_INIT response 0 [ SA KE No N(NATD_S_IP) N(NATD_D_IP) N(FRAG_SUP) N(HASH_ALG) N(CHDLESS_SUP) N(MULT_AUTH) ]
2025-05-09T12:32:58-07:00   Informational   charon   10[NET] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> received packet: from 192.0.2.226[500] to 192.0.2.13[500] (472 bytes)
2025-05-09T12:32:58-07:00   Informational   charon   11[NET] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> sending packet: from 192.0.2.13[500] to 192.0.2.239[500] (464 bytes)
2025-05-09T12:32:58-07:00   Informational   charon   11[ENC] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> generating IKE_SA_INIT request 0 [ SA KE No N(NATD_S_IP) N(NATD_D_IP) N(FRAG_SUP) N(HASH_ALG) N(REDIR_SUP) ]
2025-05-09T12:32:58-07:00   Informational   charon   11[IKE] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> initiating IKE_SA 302ac6f2-a390-4413-be90-6f58d4db9d1c[3] to 192.0.2.239
2025-05-09T12:32:58-07:00   Informational   charon   11[CFG] initiating '8be16212-f51a-41bf-9f73-ae47d3f691f0'
2025-05-09T12:32:58-07:00   Informational   charon   11[CFG] added vici connection: 302ac6f2-a390-4413-be90-6f58d4db9d1c
2025-05-09T12:32:58-07:00   Informational   charon   13[NET] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> sending packet: from 192.0.2.13[500] to 192.0.2.114[500] (464 bytes)
2025-05-09T12:32:58-07:00   Informational   charon   13[ENC] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> generating IKE_SA_INIT request 0 [ SA KE No N(NATD_S_IP) N(NATD_D_IP) N(FRAG_SUP) N(HASH_ALG) N(REDIR_SUP) ]
2025-05-09T12:32:58-07:00   Informational   charon   13[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> initiating IKE_SA 964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9[2] to 192.0.2.114
2025-05-09T12:32:58-07:00   Informational   charon   13[CFG] initiating '4c518c5c-c2f4-4049-a569-60a578ab4fe9'
2025-05-09T12:32:58-07:00   Informational   charon   13[CFG] added vici connection: 964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9
2025-05-09T12:32:58-07:00   Informational   charon   12[NET] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> sending packet: from 192.0.2.13[500] to 192.0.2.226[500] (464 bytes)
2025-05-09T12:32:58-07:00   Informational   charon   12[ENC] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> generating IKE_SA_INIT request 0 [ SA KE No N(NATD_S_IP) N(NATD_D_IP) N(FRAG_SUP) N(HASH_ALG) N(REDIR_SUP) ]
2025-05-09T12:32:58-07:00   Informational   charon   12[IKE] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> initiating IKE_SA 8600b45c-2c67-49ed-b27f-593e09665e7a[1] to 192.0.2.226
2025-05-09T12:32:58-07:00   Informational   charon   12[CFG] initiating '9864a88a-52fa-40e2-bcc0-eea0fb2757c4'
2025-05-09T12:32:58-07:00   Informational   charon   12[CFG] added vici connection: 8600b45c-2c67-49ed-b27f-593e09665e7a
2025-05-09T12:32:58-07:00   Informational   charon   13[CFG] loaded IKE shared key with id 'ike-d83b554f-e474-44f2-bb12-e65a6ca98dae' for: '192.0.2.13'
2025-05-09T12:32:57-07:00   Informational   charon   00[JOB] spawning 16 worker threads
2025-05-09T12:32:57-07:00   Informational   charon   00[LIB] loaded plugins: charon aes des blowfish rc2 sha2 sha1 md4 md5 random nonce x509 revocation constraints pubkey pkcs1 pkcs7 pkcs12 pgp dnskey sshkey pem openssl pkcs8 fips-prf curve25519 xcbc cmac hmac kdf gcm drbg curl attr kernel-pfkey kernel-pfroute resolve socket-default stroke vici updown eap-identity eap-md5 eap-mschapv2 eap-radius eap-tls eap-ttls eap-peap xauth-generic xauth-eap xauth-pam whitelist addrblock counters
2025-05-09T12:32:57-07:00   Informational   charon   00[CFG] loaded 0 RADIUS server configurations
2025-05-09T12:32:57-07:00   Informational   charon   00[CFG] loading secrets from '/usr/local/etc/ipsec.secrets'
2025-05-09T12:32:57-07:00   Informational   charon   00[CFG] loading crls from '/usr/local/etc/ipsec.d/crls'
2025-05-09T12:32:57-07:00   Informational   charon   00[CFG] loading attribute certificates from '/usr/local/etc/ipsec.d/acerts'
2025-05-09T12:32:57-07:00   Informational   charon   00[CFG] loading ocsp signer certificates from '/usr/local/etc/ipsec.d/ocspcerts'
2025-05-09T12:32:57-07:00   Informational   charon   00[CFG] loading aa certificates from '/usr/local/etc/ipsec.d/aacerts'
2025-05-09T12:32:57-07:00   Informational   charon   00[CFG] loading ca certificates from '/usr/local/etc/ipsec.d/cacerts'
2025-05-09T12:32:57-07:00   Informational   charon   00[CFG] using '/sbin/resolvconf' to install DNS servers
2025-05-09T12:32:57-07:00   Informational   charon   00[LIB] providers loaded by OpenSSL: default legacy
2025-05-09T12:32:57-07:00   Informational   charon   00[DMN] Starting IKE charon daemon (strongSwan 5.9.14, FreeBSD 14.1-RELEASE-p2, amd64)
2025-05-09T12:32:57-07:00   Informational   charon   00[IKE] <7> destroying IKE_SA in state CONNECTING without notification
2025-05-09T12:32:57-07:00   Informational   charon   00[IKE] <8> destroying IKE_SA in state CONNECTING without notification
2025-05-09T12:32:57-07:00   Informational   charon   00[NET] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> sending packet: from 192.0.2.13[500] to 192.0.2.239[500] (80 bytes)
2025-05-09T12:32:57-07:00   Informational   charon   00[ENC] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> generating INFORMATIONAL request 2 [ D ]
2025-05-09T12:32:57-07:00   Informational   charon   00[IKE] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> sending DELETE for IKE_SA 302ac6f2-a390-4413-be90-6f58d4db9d1c[3]
2025-05-09T12:32:57-07:00   Informational   charon   00[IKE] <302ac6f2-a390-4413-be90-6f58d4db9d1c|3> deleting IKE_SA 302ac6f2-a390-4413-be90-6f58d4db9d1c[3] between 192.0.2.13[192.0.2.13]...192.0.2.239[192.0.2.239]
2025-05-09T12:32:57-07:00   Informational   charon   00[DMN] SIGTERM received, shutting down
2025-05-09T12:32:34-07:00   Informational   charon   06[NET] <8> sending packet: from 192.0.2.13[500] to 192.0.2.226[500] (472 bytes)
2025-05-09T12:32:34-07:00   Informational   charon   06[ENC] <8> generating IKE_SA_INIT response 0 [ SA KE No N(NATD_S_IP) N(NATD_D_IP) N(FRAG_SUP) N(HASH_ALG) N(CHDLESS_SUP) N(MULT_AUTH) ]
2025-05-09T12:32:34-07:00   Informational   charon   06[CFG] <8> selected proposal: IKE:AES_CBC_256/HMAC_SHA2_256_128/PRF_HMAC_SHA2_256/MODP_2048
2025-05-09T12:32:34-07:00   Informational   charon   06[IKE] <8> 192.0.2.226 is initiating an IKE_SA
2025-05-09T12:32:34-07:00   Informational   charon   06[ENC] <8> parsed IKE_SA_INIT request 0 [ SA KE No N(NATD_S_IP) N(NATD_D_IP) N(FRAG_SUP) N(HASH_ALG) N(REDIR_SUP) ]
2025-05-09T12:32:34-07:00   Informational   charon   06[NET] <8> received packet: from 192.0.2.226[500] to 192.0.2.13[500] (464 bytes)
2025-05-09T12:32:33-07:00   Informational   charon   06[NET] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> sending packet: from 192.0.2.13[500] to 192.0.2.226[500] (80 bytes)
2025-05-09T12:32:33-07:00   Informational   charon   06[ENC] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> generating INFORMATIONAL response 2 [ ]
2025-05-09T12:32:33-07:00   Informational   charon   06[IKE] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> IKE_SA deleted
2025-05-09T12:32:33-07:00   Informational   charon   06[IKE] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> deleting IKE_SA 8600b45c-2c67-49ed-b27f-593e09665e7a[1] between 192.0.2.13[192.0.2.13]...192.0.2.226[192.0.2.226]
2025-05-09T12:32:33-07:00   Informational   charon   06[IKE] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> received DELETE for IKE_SA 8600b45c-2c67-49ed-b27f-593e09665e7a[1]
2025-05-09T12:32:33-07:00   Informational   charon   06[ENC] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> parsed INFORMATIONAL request 2 [ D ]
2025-05-09T12:32:33-07:00   Informational   charon   06[NET] <8600b45c-2c67-49ed-b27f-593e09665e7a|1> received packet: from 192.0.2.226[500] to 192.0.2.13[500] (80 bytes)
2025-05-09T12:32:28-07:00   Informational   charon   06[NET] <7> sending packet: from 192.0.2.13[500] to 192.0.2.114[500] (472 bytes)
2025-05-09T12:32:28-07:00   Informational   charon   06[ENC] <7> generating IKE_SA_INIT response 0 [ SA KE No N(NATD_S_IP) N(NATD_D_IP) N(FRAG_SUP) N(HASH_ALG) N(CHDLESS_SUP) N(MULT_AUTH) ]
2025-05-09T12:32:28-07:00   Informational   charon   06[CFG] <7> selected proposal: IKE:AES_CBC_256/HMAC_SHA2_256_128/PRF_HMAC_SHA2_256/MODP_2048
2025-05-09T12:32:28-07:00   Informational   charon   06[IKE] <7> 192.0.2.114 is initiating an IKE_SA
2025-05-09T12:32:28-07:00   Informational   charon   06[ENC] <7> parsed IKE_SA_INIT request 0 [ SA KE No N(NATD_S_IP) N(NATD_D_IP) N(FRAG_SUP) N(HASH_ALG) N(REDIR_SUP) ]
2025-05-09T12:32:28-07:00   Informational   charon   06[NET] <7> received packet: from 192.0.2.114[500] to 192.0.2.13[500] (464 bytes)
2025-05-09T12:32:28-07:00   Informational   charon   06[NET] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> sending packet: from 192.0.2.13[500] to 192.0.2.114[500] (80 bytes)
2025-05-09T12:32:28-07:00   Informational   charon   06[ENC] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> generating INFORMATIONAL response 2 [ ]
2025-05-09T12:32:28-07:00   Informational   charon   06[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> IKE_SA deleted
2025-05-09T12:32:28-07:00   Informational   charon   06[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> deleting IKE_SA 964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9[2] between 192.0.2.13[192.0.2.13]...192.0.2.114[192.0.2.114]
2025-05-09T12:32:28-07:00   Informational   charon   06[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> received DELETE for IKE_SA 964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9[2]
2025-05-09T12:32:28-07:00   Informational   charon   06[ENC] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> parsed INFORMATIONAL request 2 [ D ]
2025-05-09T12:32:28-07:00   Informational   charon   06[NET] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> received packet: from 192.0.2.114[500] to 192.0.2.13[500] (80 bytes)
2025-05-09T12:30:12-07:00   Informational   charon   16[NET] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> sending packet: from 192.0.2.13[500] to 192.0.2.114[500] (80 bytes)
2025-05-09T12:30:12-07:00   Informational   charon   16[ENC] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> generating INFORMATIONAL response 1 [ D ]
2025-05-09T12:30:12-07:00   Informational   charon   16[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> outbound CHILD_SA 4c518c5c-c2f4-4049-a569-60a578ab4fe9{6} established with SPIs ca176942_i c075fb18_o and TS 172.17.0.0/24 === 172.16.0.0/24
2025-05-09T12:30:12-07:00   Informational   charon   16[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> CHILD_SA closed
2025-05-09T12:30:12-07:00   Informational   charon   16[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> sending DELETE for ESP CHILD_SA with SPI c8e6aa8f
2025-05-09T12:30:12-07:00   Informational   charon   16[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> closing CHILD_SA 4c518c5c-c2f4-4049-a569-60a578ab4fe9{2} with SPIs c8e6aa8f_i (29201377 bytes) cdb2bcf7_o (33601900 bytes) and TS 172.17.0.0/24 === 172.16.0.0/24
2025-05-09T12:30:12-07:00   Informational   charon   16[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> received DELETE for ESP CHILD_SA with SPI cdb2bcf7
2025-05-09T12:30:12-07:00   Informational   charon   16[ENC] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> parsed INFORMATIONAL request 1 [ D ]
2025-05-09T12:30:12-07:00   Informational   charon   16[NET] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> received packet: from 192.0.2.114[500] to 192.0.2.13[500] (80 bytes)
2025-05-09T12:30:12-07:00   Informational   charon   08[NET] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> sending packet: from 192.0.2.13[500] to 192.0.2.114[500] (528 bytes)
2025-05-09T12:30:12-07:00   Informational   charon   08[ENC] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> generating CREATE_CHILD_SA response 0 [ N(ESP_TFC_PAD_N) SA No KE TSi TSr ]
2025-05-09T12:30:12-07:00   Informational   charon   08[IKE] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> inbound CHILD_SA 4c518c5c-c2f4-4049-a569-60a578ab4fe9{6} established with SPIs ca176942_i c075fb18_o and TS 172.17.0.0/24 === 172.16.0.0/24
2025-05-09T12:30:12-07:00   Informational   charon   08[CFG] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> selected proposal: ESP:AES_CBC_256/HMAC_SHA2_256_128/MODP_2048/NO_EXT_SEQ
2025-05-09T12:30:12-07:00   Informational   charon   08[ENC] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> parsed CREATE_CHILD_SA request 0 [ N(REKEY_SA) SA No KE TSi TSr ]
2025-05-09T12:30:12-07:00   Informational   charon   08[NET] <964c3b6b-7e05-4eaa-8d7b-e8d7de9f06b9|2> received packet: from 192.0.2.114[500] to 192.0.2.13[500] (528 bytes)
On the OPNSense side my tunnel settings are
Phase 1:
Proposals: aes256-sha256-modp2048 [DH14]
Unique: no
Version: IKEv2
Re-auth time: 28800
DPD delay: 10
Phase 2:
Mode: Tunnel
Policies: yes
Start action: start
close action: none
DPD action: trap
ESP proposals: aes256-sha256-modp2048 [DH14]
Rekey time: 3600
On the meraki side
Phase 1:
Encryption: AES256
Authentication: SHA256
Pseudo-random function: Default
Diffie-Hellman group: 14
Lifetime: 28800
Phase 2:
Encryption: AES256
Authentication: SHA256
PFS group: 14
Lifetime: 3600