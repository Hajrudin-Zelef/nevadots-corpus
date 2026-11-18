Click 日本語 for Japanese

## Overview

This article provides a list of most common syslog event types, description of each event, and a sample output of each log.



### Meraki MX Security Appliance


| **Event type** | **Description** | **Sample Syslog Message** | 
| events (Auto VPN) | vpn connectivity change | 1380664922.583851938 MX84 events type=vpn_connectivity_change vpn_type='site-to-site' peer_contact='98.68.191.209:51856' peer_ident='2814ee002c075181bb1b7478ee073860' connectivity='false' | 
| events (Auto VPN) | vpn connectivity change | 1380664994.337961231 MX84 events type=vpn_connectivity_change vpn_type='site-to-site' peer_contact='98.68.191.209:51856' peer_ident='2814ee002c075181bb1b7478ee073860' connectivity='true' | 
| events | uplink connectivity change | Dec 6 08:46:12 192.168.1.1 1 1386337584.254756845 MX84 events Cellular connection down | 
| events | uplink connectivity change | Dec 6 08:45:24 192.168.1.1 1 1386337535.803931423 MX84 events failover to wan1 | 
| events | uplink connectivity change | Dec 6 08:43:43 192.168.1.1 1 1386337435.108107268 MX84 events failover to cellular | 
| events | uplink connectivity change | Dec 6 08:41:44 192.168.1.1 1 1386337316.207232138 MX84 events Cellular connection up | 
| events | dhcp no offers | Sep 11 16:12:41 192.168.10.1 1 1599865961.535491111 MX84 events dhcp no offers for mac A4:83:E7:XX:XX:XX host = 192.168.10.1 | 
| events | dhcp lease | Sep 11 16:05:15 192.168.10.1 1 1599865515.687171503 MX84 events dhcp lease of ip 192.168.10.68 from server mac E0:CB:BC:0F:XX:XX for client mac 8C:16:45:XX:XX:XX from router 192.168.10.1 on subnet 255.255.255.0 with dns 8.8.8.8, 8.8.4.4 | 
| urls | HTTP GET requests | 1374543213.342705328 MX84 urls src=192.168.1.186:63735 dst=69.58.188.40:80 mac=58:1F:AA:CE:61:F2 request: GET https://... | 
| Events | Content filtering block | 1787058176.597410862 MX67 events content_filtering_block url='https://www.facebook.com/...' category0='User-defined Blacklist' server='157.240.225.35:443' client_mac='XX:XX:XX:XX:XX:XX' | 
| L7_firewall | l7_firewall | 1787058162.285901262 MX67 l7_firewall src=10.10.10.3 dst=104.21.4.210 protocol=tcp sport=59526 dport=443 decision=blocked | 
| flows *(deprecated, this has been updated in MX18.101 and newer to "firewall")*  | L3 FW rule matched | 1374543986.038687615 MX84 flows src=192.168.1.186 dst=8.8.8.8 mac=58:1F:AA:CE:61:F2 protocol=udp sport=55719 dport=53 pattern: allow all | 
| firewall cellular_firewall vpn_firewall | L3 FW rule matched | 1374543986.038687615 MX84 firewall src=192.168.1.186 dst=8.8.8.8 mac=58:1F:AA:CE:61:F2 protocol=udp sport=55719 dport=53 pattern: allow all | 
| ids-alerts | ids signature matched | 1377449842.514782056 MX84 ids-alerts signature=129:4:1 priority=3 timestamp=1377449842.512569 direction=ingress protocol=tcp/ip src=74.125.140.132:80 | 
| ids-alerts | ids signature matched | 1377448470.246576346 MX84 ids-alerts signature=119:15:1 priority=2 timestamp=1377448470.238064 direction=egress protocol=tcp/ip src=192.168.111.254:56240 | 
| security_event ids_alerted | ids signature matched | signature=1:28423:1 priority=1 timestamp=1468531589.810079 dhost=98:5A:EB:E1:81:2F direction=ingress protocol=tcp/ip src=151.101.52.238:80 dst=192.168.128.2:53023 decision=blocked action=rst message: EXPLOIT-KIT Multiple exploit kit single digit exe detection | 
| security_event security_filtering_file_scanned | Malicious file blocked by amp | url=http://www.eicar.org/download/eicar.com.txt src=192.168.128.2:53150 dst=188.40.238.250:80 mac=98:5A:EB:E1:81:2F name='EICAR:EICAR_Test_file_not_a_virus-tpd' sha256=275a021bbfb6489e54d471899f7db9d1663fc695ec2fe2a2c4538aabf651fd0f disposition=malicious action=block | 
| security_event security_filtering_disposition_change | File issued retrospective malicious disposition | name=EICAR:EICAR_Test_file_not_a_virus-tpd sha256=275a021bbfb6489e54d471899f7db9d1663fc695ec2fe2a2c4538aabf651fd0f disposition=malicious action=allow | 
| events (post MX 15.12) | Establishing Phase 1 (IKE_SA) tunnel | VPN: <remote-peer-2\|12> IKE_SA remote-peer-2[12] established between 192.168.13.5[192.168.13.5]...192.168.13.2[192.168.13.2] | 
| events (post MX 15.12) | Establishing Phase 2 (Child_SA) tunnel | VPN: <remote-peer-2\|12> CHILD_SA net-2{1478} established with SPIs cd94e190(inbound) c2b06071(outbound) and TS 192.168.12.0/24 === 192.168.13.0/24 | 
| events (post MX 15.12) | Destroying Phase 1 (IKE_SA) tunnel | VPN: <remote-peer-2\|12> deleting IKE_SA remote-peer-2[12] between 192.168.13.5[192.168.13.5]...192.168.13.2[192.168.13.2] | 
| events (post MX 15.12) | Destroying Phase 2 (Child_SA) tunnel | VPN: <remote-peer-2\|12> closing CHILD_SA net-2{1478} with SPIs cd94e190(inbound) (0 bytes) c2b06071(outbound) (0 bytes) and TS 192.168.12.0/24 === 192.168.13.0/24 | 
| events | AnyConnect VPN general (various msgs) | 1720051390.733639600 labs_appliance events type=anyconnect_vpn_general msg= 'AnyConnect server is started. ' | 
| events | AnyConnect VPN authentication success | 1720045578.339796505 labs_appliance events type=anyconnect_vpn_auth_success msg= 'Peer IP=192.168.0.1 Peer port=57096 AAA[7]: AAA authentication successful ' | 
| events | AnyConnect VPN authentication failure | 1720051237.124589040 labs_appliance events type=anyconnect_vpn_auth_failure msg= 'Peer IP=192.168.0.1Peer port[8748] AAA[8]: AAA authenticate failed retval=7 - Authentication failure ' | 
| events | AnyConnect VPN session manager (various msgs) | 1720045578.340434385 labs_appliance events type=anyconnect_vpn_session_manager msg= 'Sess-ID[7] Peer IP=192.168.0.1 User[miles@meraki.net]: Session connected. Session Type: TLS ' | 
| events | AnyConnect VPN Connect | 1720045578.495767745 labs_appliance events anyconnect_vpn_connect user id 'miles@meraki.net' local ip 192.168.5.224 connected from 192.168.0.1 | 
| events | AnyConnect VPN Disconnect | 1720045578.515109505 labs_appliance events anyconnect_vpn_disconnect user id 'miles@meraki.net' local ip 192.168.5.135 connected from 192.168.0.1 | 
| events (pre MX 15.12) | purging ISAKMP-SA | 1578424543.894083034 labs_appliance events Site-to-site VPN: purging ISAKMP-SA spi=9d1bb66d7ddc5cf0:d98cd0ed59e82f13 | 
| events (pre MX 15.12) | ISAKMP-SA deleted | 1578424543.918665436 labs_appliance events Site-to-site VPN: ISAKMP-SA deleted 172.24.23.6[4500]-172.24.23.10[4500] spi:9d1bb66d7ddc5cf0:d98cd0ed59e82f13 | 
| events (pre MX 15.12) | IPsec-SA request queued due to no phase 1 found | 1578424549.917669303 labs_appliance events Site-to-site VPN: IPsec-SA request for 172.24.23.10 queued due to no phase1 found | 
| events (pre MX 15.12) | failed to get sainfo | 1578426208.829677788 labs_Z1 events Site-to-site VPN: failed to get sainfo | 
| events (pre MX 15.12) | failed to pre-process ph2 packet | 1578426208.915091184 labs_Z1 events Site-to-site VPN: failed to pre-process ph2 packet (side: 1, status: 1) | 
| events (pre MX 15.12) | phase2 negotiation failed due to time up waiting for phase1 | 1578424408.321445408 labs_appliance events Site-to-site VPN: phase2 negotiation failed due to time up waiting for phase1. ESP 172.24.23.10[0]->172.24.23.6[0] | 
| events (pre MX 15.12) | initiate new phase 1 negotiation | 1578424549.931720602 labs_appliance events Site-to-site VPN: initiate new phase 1 negotiation: 172.24.23.6[500]<=>172.24.23.10[500] | 
| events (pre MX 15.12) | ISAKMP-SA established | 1578424550.965202127 labs_appliance events Site-to-site VPN: ISAKMP-SA established 172.24.23.6[4500]-172.24.23.10[4500] spi:fb903f191f1c7566:4dc90bd31c7884c1 | 
| events (pre MX 15.12) | initiate new phase 2 negotiation | 1578424550.975495647 labs_appliance events Site-to-site VPN: initiate new phase 2 negotiation: 172.24.23.6[4500]<=>172.24.23.10[4500] | 
| events (pre MX 15.12) | IPsec-SA established | 1578424551.120459981 labs_appliance events Site-to-site VPN: IPsec-SA established: ESP/Tunnel 172.24.23.6[4500]->172.24.23.10[4500] spi=241280704(0xe61a6c0) | 

The **priority** score is based on Snort values. The priorities are as follows:

1 - high priority alert

2 - medium priority alert

3 - low priority alert

4 - very low priority alert

Some values under the Sample Syslog Message are variables (i.e. hostname of the devices, timestamps, etc.) and will be different to Syslog messages generated by another device.

For the urls event type, the URL in the request part of the message will be truncated at 500 characters.

Content filtering events are sent to Syslog. The ‘URLs’ role has to be added to the Syslog server config for them to be sent.

### Meraki MS Switches

| **Event type** | **Description** | **Sample Syslog Message** | 
| events | port status change | 1379967288.409907239 MS220_8P events port 3 status changed from 100fdx to down | 
| events | port status change | 1379967295.290863061 MS220_8P events port 3 status changed from down to 100fdx | 
| events | spanning-tree guard state change | 1379970281.577982192 MS220_8P events Port 5 received an STP BPDU from 78:FE:3D:90:7F:43 so the port was blocked | 
| events | spanning-tree interface role change | 1379970476.195563376 MS220_8P events Port 5 changed STP role from designated to alternate | 
| events | spanning-tree interface role change | 1379969188.448725072 MS220_8P events Port 1 changed STP role from root to designated | 
| events | spanning-tree interface role change | 1379970772.184373058 MS220_8P events Port 5 changed STP role from alternate to root | 
| events | spanning-tree interface role change | 1379972501.619445657 MS220_8P events Port 1 changed STP role from disabled to designated | 
| events | blocked DHCP server response | 1379988354.643337272 MS220_8P events Blocked DHCP server response from 78:FE:3D:90:7F:48 on VLAN 100 | 
| events | 802.1X deauthentication | 1380653487.002002676 MS220_8P events type=8021x_deauth port='' identity='employee@ikarem.com' | 
| events | 802.1X eap success | 1380653443.857790533 MS220_8P events type=8021x_eap_success port='' identity='employee@ikarem.com' | 
| events | 802.1X authentication | 1380653443.868786613 MS220_8P events type=8021x_auth port='3' identity='employee@ikarem.com' | 
| events | 802.1X client deauthentication | 1380653486.994003049 MS220_8P events type=8021x_client_deauth port='3' identity='employee@ikarem.com' | 
| events | Virtual router collision | 1379988354.643337272 MS320_24P events Received VRRP packet for virtual router 1 from a.a.a.a on VLAN x with incompatible configuration | 
| events | VRRP transition | 1379988354.643337272 MS320_24P events changed from VRRP passive to VRRP active because it has not received packets from the active | 
| events | Power supply inserted | 1379988354.643337272 MS320_24P events Power supply xxxx-xxxx-xxxx was inserted into slot 1 | 
| events | OSPF | future enhancement | 
| events | DHCP Server | future enhancement | 

### Meraki MR Access Points

| **Event type** | **Event description** | **Sample Syslog Message** | 
| events | 802.11 association | 1380653443.857790533 MR18 events type=association radio='0' vap='1' channel='6' rssi='23' aid='1813578850' | 
| events | 802.11 disassociation | 1380653443.857790533 MR18 events type=disassociation radio='0' vap='1' channel='6' reason='8' instigator='2' duration='11979.728000' auth_neg_dur='1380653443.85779053324000' last_auth_ago='5.074000' is_wpa='1' full_conn='1.597000' ip_resp='1.597000' ip_src='192.168.111.251' arp_resp='1.265000' arp_src='192.168.111.251' dns_server='192.168.111.1' dns_req_rtt='1380653443.85779053335000' dns_resp='1.316000' aid='1813578850' | 
| events | WPA authentication | 1380653443.857790533 MR18 events type=wpa_auth radio='0' vap='1' aid='1813578850' | 
| events | WPA deauthentication | 1380653443.857790533 MR18 events type=wpa_deauth radio='0' vap='1' aid='1813578850' | 
| events | WPA failed authentication attempt | 1380653443.857790533 MR18 events type=disassociation radio='0' vap='3' channel='6' reason='2' instigator='3' duration='6.003000' auth_neg_failed='1' is_wpa='1' aid='113930199' | 
| events | 802.1X failed authentication attempt | 1380653443.857790533 MR18 events type=8021x_eap_failure radio='0' vap='3' identity='woody8@gmail.com' aid='1701992265' | 
| events | 802.1X deauthentication | 1380653443.857790533 MR18 events type=8021x_deauth radio='0' vap='3' identity='woody8@gmail.com' aid='1701992265' | 
| events | 802.1X authentication | 1380653443.857790533 MR18 events type=8021x_eap_success radio='0' vap='3' identity='woody8@gmail.com' aid='1849280097' | 
| events | splash authentication | 1380653443.857790533 MR18 events type=splash_auth ip='10.87.195.250 [More Information] ' duration='3600' vap='2' download='5242880bps' upload='5242880bps' | 
| events | wireless packet flood detected | 1380653443.857790533 MR18 events type=device_packet_flood packet='deauth' device='00:18:0A:27:43:80' radio='0' state='start' alarm_id='4' dos_count='25' inter_arrival='10000' | 
| events | wireless packet flood end | 1380653443.857790533 MR18 events type=device_packet_flood radio='0' state='end' alarm_id='4' reason='left_channel' | 
| events | rogue SSID detected* | airmarshal_events type= rogue_ssid_detected ssid='' bssid='02:18:5A:AE:56:00' src='02:18:5A:AE:56:00' dst='02:18:6A:13:09:D0' wired_mac='00:18:0A:AE:56:00' vlan_id='0' channel='157' rssi='21' fc_type='0' fc_subtype='5' | 
|  | SSID spoofing detected* | airmarshal_events type= ssid_spoofing_detected ssid='t-nebojsa_devel1' vap='2' bssid='02:18:5A:14:04:E2' src='02:18:5A:14:04:E2' dst='FF:FF:FF:FF:FF:FF' channel='48' rssi='39' fc_type='0' fc_subtype='8' | 
| urls** | HTTP GET requests | Dec 6 08:46:12 192.168.1.1 1 1386337584.254756845 MX84 events Cellular connection down1380653443.857790533 MR18 urls src=192.168.111.253:50215 dst=204.154.94.81:443 mac=F8:1E:DF:E2:EF:F1 request: UNKNOWN https://www.evernote.com/... | 
| flows | flow allowed by Layer 3 firewall | 1380653443.857790533 MR18 flows allow src=192.168.111.253 dst=192.168.111.5 mac=F8:1E:DF:E2:EF:F1 protocol=tcp sport=54252 dport=80 | 
| flows | flow denied by Layer 3 firewall | 1380653443.857790533 MR18 flows deny src=10.20.213.144 dst=192.168.111.5 mac=00:F4:B9:78:58:01 protocol=tcp sport=52421 dport=80 | 


* r*ogue_ssid_detected* and *ssid_spoofing_detected* have been removed in MR29+  firmware

**urls

*request: UNKNOWN*is likely because the URL is encrypted