WPA3 Encryption and Configuration Guide
Click 日本語 for Japanese
WPA3 Overview
WPA3 is the latest and third iteration of the Wi-Fi Protected Access (WPA) standard, developed by the Wi-Fi Alliance, and serves as the successor to WPA2. The WPA standard was originally created by the Wi-Fi Alliance Security Technical Task Group, chaired by Cisco’s Stephen Orr, with the goal of standardizing wireless security.
WPA3 introduces advanced features for enterprise, personal, and open networks by enhancing cryptographic strength and enabling a more secure authentication process across all WPA3-supported devices.
It is designed to:
- Strengthen wireless security
- Simplify secure connectivity for users
- Provide strong protection even with weak passwords
Enhance security for public and open Wi-Fi networks
The WPA3 Enterprise form extends the solid foundation provided by WPA2 Enterprise by making it mandatory to use Protected Management Frames (PMF) on all connections. This security feature protects against such dangerous attacks as Denial of Service (DoS), honeypots, and eavesdropping.
Supported WPA3 Modes
● WPA3-Enterprise, for 802.1X security networks. This leverages IEEE 802.1X with SHA-256 as the Authentication and Key Management (AKM).
● WPA3-Personal, which uses the Simultaneous Authentication of Equals (SAE) method for personal security networks. There are two sub-methods to derive the Password Element in SAE:
o Hunting and Pecking (HnP)
o Hash-to-Element (H2E)
Note: Wi-Fi 6E (6 GHz) and Wi-Fi 7 requires Hash-to-Element as mandatory, as HnP is prone to brute force dictionary attacks.
● WPA3 Transition Mode (WPA2+WPA3 security-based WLANs for both personal and enterprise).
● Opportunistic Wireless Encryption (OWE) for open security networks.
WPA3 Requirements for 6 GHz operations and Wi-Fi 7
Wi-Fi Alliance mandated WPA3 for 6 GHz band and Wi-Fi 7 to ensure modern security and protect from vulnerabilities and provide a secure foundation for new device ecosystems.
WPA3 Requirements in Wi-Fi 6E (6 GHz) :
· WPA3 is mandatory for all Wi-Fi 6E devices operating in 6 GHz band.
· WPA3-Personal (SAE) with H2E for home/personal use.
· WPA3-Enterprise (802.1X, with optional 192-bit security suite) for enterprise deployments.
· Enhanced Open (OWE) for open networks requiring encryption without passwords
· Protected Management Frames (PMF) is mandatory in 6 GHz.
· WPA2 is not permitted in 6 GHz operation.
Note: As per the WPA3 v3.4 specifications (Section 11.2), Enhanced Open transition mode is not supported with 6 GHz.
There are no new specific ciphers or algorithm requirements for WPA3-Enterprise, apart from 802.11w/Protected Management Frame (PMF) enforcement. Many vendors, including Cisco, consider 802.1X-SHA256 or "FT + 802.1X" (which actually is 802.1X with SHA256 and Fast Transition on top) only to be WPA3 compliant and plain 802.1X (which uses SHA1) is considered part of WPA2, therefore not fit/supported for 6 GHz.
WPA3 Requirements in Wi-Fi 7:
· WPA3 is mandatory for all Wi-Fi 7 devices for features like Multi Link Operation and 802.11be data rates.
· WPA3-Personal (SAE) with GCMP256 as Cipher and SAE-EXT-KEY or the FT equivalent of it FT-SAE-EXT-KEY as AKMs.
· WPA3-Enterprise with AES (CCMP128) and 802.1X-SHA256 or the FT equivalent of it FT+802.1X (which still uses SHA256, though it’s not explicit in naming) as AKM.
Note: Cipher requirement of GCMP256 is required for WPA3-Enterprise. However, it’s not strictly enforced in the Access Point and Wireless clients.
· Enhanced Open (OWE) with GCMP256 as Cipher for open networks requiring encryption without passwords
· Protected Management Frames (PMF) is mandatory.
· Beacon Protection is mandatory.
Note: Similar to Wi-Fi 6E, Enhanced Transition Mode is not supported for Wi-Fi 7 operation in 6 GHz band. It is recommended to configure a pure OWE only WLAN for Wi-Fi 7 operation in 6 GHz band.
Note: Because Wi-Fi 7 is still a recent certification at the time of this writing, with an as early as possible release, many vendors did not enforce all these security requirements from the beginning.
The table below provides the security requirements for different Wi-Fi standards.
Note: AKMs and Cipher highlighted in “red” are mandatory for Wi-Fi 7.
Note: Standard requires GCMP256 as cipher for WPA3 Enterprise with 802.1x-SHA256, but most clients in the market today are capable of Wi-Fi 7 functionality with AES (CCMP128)
Migration Considerations
In many existing networks, WLANs continue to operate on Wi-Fi 6 or earlier standards, secured with WPA2 or older protocols. This is primarily due to legacy Access Point (AP) infrastructure—such as Wave-1 APs—that do not support WPA3, as well as a significant number of client devices lacking WPA3 capability.
With the introduction of Wi-Fi 6E (6 GHz) and Wi-Fi 7, the Wi-Fi Alliance has mandated WPA3 as a requirement for operating in the 6 GHz spectrum and for enabling the full feature set of Wi-Fi 7. This creates practical challenges when migrating to modern AP platforms while simultaneously upgrading all WLANs to WPA3.
Organizations have three practical design and deployment options to address these challenges:
1. Migrate all WLANs to WPA3/Enhanced Open
- 
    Provides a fully secure WLAN environment.
- 
    However, it poses challenges in maintaining coexistence with legacy WLANs and clients.
- 
    Best suited if: 
  - 
        All APs support WPA3 (i.e., Wave-2 or newer).
  - 
        No legacy clients remain that lack WPA3 support.
- 
        
2. Redesign or introduce new WLANs with WPA3/Enhanced Open
- 
    Retain existing WLANs to continue supporting legacy clients.
- 
    Deploy new WLANs using WPA3/Enhanced Open to: 
  - 
        Meet Wi-Fi 6E and Wi-Fi 7 requirements.
  - 
        Support modern clients with WPA3 capability.
- 
        
- 
    This hybrid model offers flexibility but requires managing two sets of SSIDs, which increases operational overhead.
3. Use Transition Modes to support multiple security standards
- 
    Convert WPA2 WLANs to WPA3 Transition Mode (also known as WPA2+WPA3 Mixed Mode).
- 
    In this mode, APs broadcast both WPA2 and WPA3 capabilities: 
  - 
        WPA2-only clients can continue connecting.
  - 
        WPA3-capable clients connect using WPA3.
- 
        
- 
    This approach: 
  - 
        Preserves existing SSID names.
  - 
        Provides a smooth migration path with minimal disruption for legacy devices.
- 
        
Network preparation before enabling Wi-Fi 7
Below is a quick snapshot of how existing SSIDs can be transitioned to ensure Wi-Fi 6E or Wi-Fi 7 compliance.
| Use Case | WLAN Security Today | Migrate to Wi-Fi 7 compliant SSID | 
| Guest Access | Open | OWE | 
| Corporate SSID with Radius Auth | WPA2 | WPA3 (or) WPA3 Transition (Enterprise) | 
| IoT/Guest (PSK based) | WPA2 | WPA3 (or) WPA3 Transiton (Personal) | 
| Corporate Secure | SuiteB 192 Bit | SuiteB 192 Bit | 
For more information you can refer to the migration guide
Configuration Workflow
The WPA3 configuration workflow consists of two main steps:
- 
    Create a WLAN using WPA3-Enterprise, WPA3-Personal, or OWE (Opportunistic Wireless Encryption).
- 
    (Optional) Enable SSIDs for Wi-Fi 7, if the network includes Wi-Fi 7 access points. This step is required due to the additional security requirements introduced with Wi-Fi 7.
Note: Wi-Fi 6E and Wi-Fi 7 introduced MBSSID (Multiple SSID), which allows groups of four SSIDs to be advertised within a single Beacon or Probe Response frame in the 6 GHz band. The dashboard supports four MBSSID groups, each containing four SSIDs. For an access point to advertise a group’s Wi-Fi 7 capability, all SSIDs in that group must be Wi-Fi 7 compliant. If any SSID in the group is not compliant, the group’s capability is reduced to Wi-Fi 6E.
Note: Per-group SSID configuration is available starting in MR 32.1.4. In earlier releases (MR 31.1.x through MR 32.1.3), all SSIDs transmitted by the AP were required to be Wi-Fi 7 compliant. If even one SSID was not security-compliant with Wi-Fi 7, the AP’s capability was restricted to Wi-Fi 6E.
WPA3 Enterprise
WPA3-Enterprise builds upon the foundation of WPA2-Enterprise with the additional requirement of using Protected Management Frames on all WPA3 connections with 802.1X for user authentication with a RADIUS server. By default, WPA3 uses 128-bit encryption, but it also introduces an optionally configurable SuiteB-192 bit cryptographic strength encryption using GMCP-256, which gives additional protection to any network transmitting sensitive data. The WPA3-Enterprise is highly preferred and recommended to be used and commonly seen in enterprises, financial institutions, government, and other market sectors where network security is most critical.
WPA3 Enterprise has three modes of operation available on dashboard to meet the network requirements as needed. They are
1. WPA3 Only
2. WPA3 192-bit security
3. WPA3 Transition Mode
WPA3 Only
To have a WPA3 Enterprise only WLAN, follow these steps:
1. Navigate to Wireless --> Access control -->security
2. Select Enterprise with my Radius server
3. Set the WPA encryption selection as WPA3 Only.
4. If the network has Wi-Fi 7 APs, select GCMP256 in the Advanced WPA3 Settings.
5. Configure the Radius server.
WPA3 192-bit Security
This mode utilizes 192-bit security while still using the 802.1X standard to provide a secure wireless network for enterprise use. This provides a superior encryption method to better protect any kind of data. The security suite is aligned with the recommendations from the Commercial National Security Algorithm (CNSA) suite and is commonly placed in high-security Wi-Fi networks such as in government, defense, finance, and other industries.
WPA3 192-bit security will be exclusive for EAP-TLS, which will require certificates on both the supplicant and RADIUS server. Also, to use WPA3 192-bit enterprise, the RADIUS servers must use one of the permitted EAP ciphers:
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
Note: SuiteB - 192 Bit SSID is compliant with Wi-Fi 7, so no changes are required within the SSID's configuration in Dashboard, but the SSID must be re-configured using higher number SSIDs.
Note: When Wi-Fi 7 is enabled for All SSIDs sub tab it should be noted that SuiteB - 192 Bit SSID will only be supported on SSIDs #13 - #15. This is to ensure that the correct MBSSID grouping is enabled for all types of SSIDs on the network. If a network has SuiteB - 192 Bit SSID configured on a lower number SSID, Dashboard will show an alert stating Wi-Fi 7 cannot be enabled because the following SSIDs do not meet the security requirements.
Note: If SuiteB - 192 Bit SSID is already configured between SSIDs #13 - #15. no changes are needed.
To have only a WPA3 192-bit security only WLAN, follow these steps:
1. Navigate to Wireless --> Access control -->security
2. Select Enterprise with my Radius server
3. Set the WPA encryption selection as WPA3 192-bit security from the drop down menu.
4. Configure the Radius server.
Enterprise WPA3 Transition Mode
WPA3 Transition mode for 802.1X enables clients to connect to a single SSID with dynamic encryption. This is done by using WPA2 for 2.4 GHz and 5 GHz, while using WPA3 for 6 GHz radio. This allows Wi-Fi 5, 6 and 6E clients to connect to the same broadcasting SSID configured for RADIUS-based authentication. With WPA3 Transition Mode, clients can roam between WPA2 enterprise and WPA3 enterprise SSIDs. When a client roams from a WPA2 to WPA3 SSID reauthentication will take place, but with minimal disruption to connectivity.
Note: This feature is available from MR 31.1.x and above firmware versions.
To have only a WPA3 Transition security WLAN, follow these steps:
1. Navigate to Wireless --> Access control--> security
2. Select Enterprise with my Radius server
3. Set the WPA encryption selection as WPA3 Transition security from the drop down menu.
4. If the network has Wi-Fi 7 APs, select GCMP256 in the Advanced WPA3 Settings.
5. Configure the Radius server.
Note: It’s recommended to set 802.11w to Enabled, which will allow WPA2 clients that do not have support for PMF to associate.
Note: GCMP256 cipher is needed to be enabled on the Access Point for Wi-Fi 7, but, in reality many clients in the market today can do a Wi-Fi 7 features like MLO, 11be rates with AES (CCMP128).
Note: With MR 31.1.7, Wi-Fi 6E Access Points configured with WPA3 Transition mode will enable the 6 GHz radio.
WPA3-Personal
WPA3-Personal uses 128-bit cryptographic-strength encryption with a password-based authentication method through SAE for user authentication purposes. In addition, unlike WPA2-Personal, WPA3-Personal heightens network security against offline dictionary attacks by limiting password guesses and requiring users to interact with a live network every time they do so. This requirement makes hacking into a network much more time-consuming and dissuades attempts at a brute force attack.
WPA3-Personal provides the following key advantages:
● Creates a shared secret that is different for each SAE authentication
● Protects against brute force “dictionary” attacks and passive attacks
● Provides forward secrecy
WPA3 Personal has two modes of operation available on dashboard to meet the network requirements as needed. They are
1. WPA3 Only
2. WPA3 Transition Mode
WPA3 Only
To have a Personal WPA3 only WLAN, follow these steps:
1. Navigate to Wireless > Configure > Access control > Security
2. Select Password option and enter the password.
3. Set the WPA encryption selection as WPA3 Only.
4. If the network has Wi-Fi 7 APs, expand the Advanced WPA3 settings for cipher and AKM
a. GCMP256 as Cipher
b. SAE and SAE-EXT-KEY as the AKM.
5. If the network has Wi-Fi 6/6E APs and no Wi-Fi 7 APs, expand the Advanced WPA3 settings for cipher and AKM
a. Select SAE as the AKM.
Note: Wi-Fi 6/6E standard does not enforce GCMP256 and SAE-EXT.
Personal WPA3 Transition Mode
To have Personal WPA3 Transition Mode WLAN, follow these steps:
1. Navigate to Wireless > Access control > security
2. Select Password option and enter the password.
3. Set the WPA encryption selection as WPA3 Transition Mode.
4. If the network has Wi-Fi 7 APs, expand the Advanced WPA3 settings for cipher and AKM
a. GCMP256 as Cipher
b. SAE and SAE-EXT-KEY as the AKM.
5. If the network has Wi-Fi 6/6E APs and no Wi-Fi 7 APs, then SAE as AKM alone would suffice. No need of GCMP-256 as the Cipher and SAE-EXT as the AKM.
a. Select SAE as the AKM.
Note: It’s recommended to set 802.11w to Enabled, which will allow WPA2 clients that do not have support for PMF to associate.
Enhanced Open (OWE - Opportunistic Wireless Encryption)
OWE is a security method paired with an open-security wireless network to provide it with encryption to protect the network from eavesdroppers. With OWE, the client and AP perform a Diffie-Hellman key exchange during the endpoint association packet exchange and use the resulting PMK to conduct the 4-way handshake. Being associated with open-security wireless networks, OWE can be used with regular open networks as well as those associated with captive portals.
OWE has two modes of operation available on dashboard to meet the network requirements as needed. They are
1. OWE
2. OWE Transition Mode
Note: OWE Transition Mode requires the network to be running MR 32+
The WPA3 Spec v3.4, Section 11.3, states that “The AP's BSS Configuration shall not allow Wi-Fi Enhanced Open Transition Mode (i.e., where the OWETransition Mode element is included in Beacons and Probe responses)” Hence OWE Transition is not valid with 6 GHz and Wi-Fi 7
OWE
To have a OWE only WLAN, follow these steps
1. Navigate to Wireless-->Configure -->Access Control -->Security
2. Select Opportunistic Wireless Encryption (OWE)
3. Select the WPA Encryption as WPA3 only.
4. If the network has Wi-Fi 7 APs, expand the Advanced WPA3 settings for cipher and AKM
a. GCMP256 as Cipher
OWE Transition Mode
The Opportunistic Wireless Encryption (OWE) transition mode enables OWE and non-OWE STAs to connect to the same SSID simultaneously. OWE transition mode enables a seamless transition from Open unencrypted WLANs to OWE WLANs without impacting the wireless connection.
- 
    Both the open WLAN and the OWE WLAN transmit beacon frames. Beacon and probe response frames from the OWE WLAN include the Wi-Fi Alliance vendor IE to encapsulate the BSSID and SSID of the open WLAN, and similarly, the open WLAN also includes for OWE WLAN.
- 
    An OWE STA only displays to the user in the list of available networks the SSID of the Open BSS of an OWE AP operating in OWE Transition Mode, and hides the OWE BSSID of that OWE AP.
- 
    WPA3-capable clients associate with the OWE SSID via the Open SSID.
- 
    Non-WPA3 clients associate directly with the Open SSID.
To have a OWE Transition WLAN, follow these steps.
1. Navigate to Wireless --> Configure--> Acces Control -->Security.
2. First, create a Open WLAN.
3. Create a OWE WLAN.
4. Set the WPA encryption selection as WPA3 Transition Mode.
5. Point the Open WLAN.
Limitations for OWE Transiton Mode
OWE Transition mode is not supported by Wi-Fi 7. OWE transition relies on a pair of SSIDs where one is an Open SSID. Open SSIDs are not compliant with Wi-Fi 7. This is not the case with PSK Transition and Enterprise Transition SSIDs.
|  | 2.4/5 GHz | 6 GHz | 
| Wi-Fi 7 | OWE Only | OWE Only | 
| Wi-Fi 6/6E | Open, OWE Transition and OWE | OWE Only | 
802.11be SSID Group Configuration
The Per SSID toggle for 802.11be (Wi-Fi 7) feature allows WiFi 7 access points to operate some SSIDs in 802.11ax mode while others operate in 802.11be mode. This is also called "ax/be mixed mode", "per-SSID 11be".
If the network has Wi-Fi 7 APs, then 802.11be has to be enabled per group. As stated earlier, all the SSIDs in a group of four has to be security compliant with Wi-Fi 7 requirements. The dashboard has four SSID groups.
Group 1 – SSID 1 to 4
Group 2 – SSID 5 to 8
Group 3 – SSID 9 to 12
Group 4 – SSID 13 to 15
Hence, the users are required to re-arrange the SSIDs that complies to the security within a group. As an example, all SSIDs that comply to Wi-Fi 7 requirement can be in Group 1, the SSIDs like WPA2 or Open that do not comply to Wi-Fi 7 can be in Group 2 and so on.
To enable 11be Per SSID Group,
- Navigate to Wireless > Configuration > Radio Settings > RF Profiles
- Edit the profile that is of interest.
- Navigate to 802.11be section in the General Tab.
- Toggle to Per SSID Group.
- Ensure all SSIDs within a group are Wi-Fi 7 compliant and the 80.11be knob is enabled.
- For 8011.be SSID groups that has non-compliant the SSIDs will be disabled.
Note: Please ensure proper security settings in a group. If security settings are changed after enabling, it impacts the entire MBSSID group. The MBSSID will be recomputed after the config change.