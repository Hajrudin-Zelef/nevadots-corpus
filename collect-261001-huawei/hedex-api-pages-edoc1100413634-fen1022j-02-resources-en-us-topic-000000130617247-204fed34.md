Configuring the IP Address of the WAN Interface
Select the Internet access mode according to the information provided by the ISP. Different Internet access modes correspond to different parameters.
Table 1 Internet access mode
| Prerequisites | Access Mode | 
|---|---|
| Your device automatically obtains an IP address from the DHCP server provided by an ISP.  NOTE: By default, the DHCP client function is disabled on the interfaces of an AR. | DHCP | 
| You have obtained a fixed IP address or an IP address segment from an ISP. | Static IP address | 
| You have obtained a user name and a password from an ISP. | PPPoE | 
| A 3G/LTE card has been installed on your device. | 3G/LTE | 
| Your device supports ADSL/VDSL uplinks. | ADSL/VDSL | 
 The default usernames and passwords are available in AR Router Default Usernames and Passwords (enterprise users or carrier users). If you have not obtained the access permission of the document, see the help document on the website to find out how to obtain it. To ensure account security, you are advised to change the password upon the first login.
 
To access the Internet through DHCP:
- Log in to the AR using STelnet or through the console port.
- Run the system-view command to enter the system view.
- Run the interface interface-type interface-number command to enter the uplink interface view.
- Run the ip address dhcp-alloc command to enable the DHCP client function.By default, the DHCP client function is disabled on an interface.
To access the Internet using a static IP address:
- Log in to the AR using STelnet or through the console port.
- Run the system-view command to enter the system view.
- Run the interface interface-type interface-number command to enter the uplink interface view.
- Run the ip address ip-address { mask | mask-length } command to configure the IP address and subnet mask of the cloud AR. The IP address is a public IP address allocated by the carrier.
- Run the ip route-static 0.0.0.0 0.0.0.0 ip-address command to configure a default route. ip-address in this command is the default gateway address provided by the carrier. Ensure that the AR can ping iMaster NCE-Campus successfully after the configuration.
To access the Internet through PPPoE:
- Log in to the AR using STelnet or through the console port.
- Configure a dialer interface.
  - Run the system-view command to enter the system view.
  - (Optional) Configure a dialer ACL. This step is mandatory only when on-demand dial-up is used.
     - Run the dialer-rule command to enter the dialer-rule view.
    - Run the dialer-rule dialer-rule-number { acl { acl-number | name acl-name } | ip { deny | permit } | ipv6 { deny | permit } } command to configure a dialer ACL for a dialer access group.By default, no dialer ACL is configured.
    - Run the quit command to return to the system view.
  - Run the interface dialer number command to create a dialer interface and enter the dialer interface view.
  - Run the dialer user user-name command to enable the RS-DCC function.By default, the RS-DCC function is disabled and the remote user name is not configured.
  - Run the dialer bundle number command to specify a dialer bundle for the dialer interface.
  - (Optional) Run the dialer-group group-number command to configure a dialer access group to which the dialer interface belongs. This step is mandatory only when on-demand dial-up is used. Make sure that the value of group-number in the dialer-group command is the same as that of dialer-rule-number in the dialer-rule command.
- In the dialer interface view, configure the IP address of the dialer interface.
- Configure a PPPoE client as the supplicant.
  - Configure the user name and password for PPP authentication.
    - Configure PAP authentication.
      - Run the ppp pap local-user username password { cipher | simple } password command to configure the user name and password sent from the PPPoE client to the PPPoE server for PAP authentication.The user name and password must be the same as those configured on the PPPoE server. By default, the local device sends a request without the user name and password to the remote device for PAP authentication.
    - Configure CHAP authentication when a user name is configured on the PPPoE server.
    - Configure CHAP authentication when no user name is configured on the PPPoE server.
      - Run the ppp chap user username command to configure the user name for CHAP authentication.
      - Run the ppp chap password { cipher | simple } password command to configure the password for CHAP authentication.
- Enable PPPoE client on an interface.
  - Run the quit command to return to the system view.
  - Run the interface interface-type interface-number command to enter the interface view.
  - Run the pppoe-client dial-bundle-number number [ on-demand ] [ no-hostuniq ] [ ppp-max-payload value ] [ service-name name ] command to specify a dialer bundle for the PPPoE session. 
    - The number of the specified dialer bundle must be the same as that of the dialer bundle configured using the dialer bundle command in 2.e.
    - If on-demand is not specified, the PPPoE dial-up mode is permanently online. If on-demand is specified, the PPPoE dial-up mode is on-demand dial-up. Currently, the on-demand dial-up mode supported by the device is the packet-triggered mode.
    - If no-hostuniq is not specified, a Host-Uniq field is carried in the call initiated by the PPPoE client to associate with a particular host request, making the check strict.
  - Run the quit command to return to the system view.
  - Run the ip route-static 0.0.0.0 0 { nexthop-address | interface-type interface-number } [ preference preference ] command to configure a static route destined for the PPPoE server.
To access the Internet in 3G mode (complying with the WCDMA standard):
- Log in to the AR using STelnet or through the console port.
- Search for and select a PLMN.
  - Run the system-view command to enter the system view.
  - Run the interface cellular interface-number command to enter the 3G cellular interface view.
  - Run the plmn search command to search for a PLMN. After a period of time, the PLMN information will be displayed on the device.
  - Select a PLMN.
    - Run the plmn auto command to configure automatic selection of a PLMN.
    - Run the plmn select manual mcc mnc [ fail-over-auto ] command to configure manual selection of a PLMN.
 By default, the device automatically selects a PLMN.
- Run the mode wcdma { gsm-only | gsm-precedence | wcdma-only | wcdma-precedence } command to configure the WCDMA network connection mode for a 3G modem.
- Configure the Internet dial-up access (single APN scenario).
  - Run the quit command to return to the system view.
  - Create an APN profile.
    - Run the apn profile profile-name command to create an APN profile and enter the APN profile view.By default, no APN profile is created, and the APN is configured dynamically.
    - Run the apn apn-name command to configure an APN.By default, no APN is configured in the APN profile.  
      - To obtain an APN, contact the local network carrier.
      - After an APN is configured, it is permanently recorded in the 3G modem. If the APN changes, reconfigure it.
    - Run the quit command to return to the system view.
  - Enable C-DCC.
    - Run the interface cellular interface-number command to enter the 3G cellular interface view.
    - Run the ip address negotiate command to configure the 3G cellular interface to obtain an IP address dynamically.
    - Run the dialer enable-circular command to enable the C-DCC function.
  - Run the dialer number dial-number [ autodial ] command to configure a dialer number.You can obtain the dialer number from the carrier.
  - Bind the APN profile to the 3G cellular interface.
    - Run the apn-profile profile-name [ track nqa { admin-name test-name } &<1-2> ] command to configure the APN profile bound to the 3G cellular interface.
    - Run the quit command to return to the system view.
  - Run the ip route-static 0.0.0.0 0 cellular interface-number [ preference preference ] command to configure a default route.interface-number indicates the number of a 3G cellular interface.
- Authenticate a PIN.
  - Run the interface cellular interface-number command to enter the 3G cellular interface view.
  - Run the pin verification enable [ auto ] command to enable PIN authentication on a 3G modem.By default, PIN authentication is disabled. To perform this step, you must enter a PIN to enable PIN authentication on the 3G data card.
  - Run the pin verify [ auto ] command to authenticate the PIN.In this step, the PIN is input automatically or manually. When the message PIN has been verified successfully is displayed on the interface after a period of time, the PIN has been authenticated successfully.
To access the Internet in 3G mode (complying with the CDMA2000 standard):
- Log in to the AR using STelnet or through the console port.
- Configure a network connection mode.
  - Run the system-view command to enter the system view.
  - Run the interface cellular interface-number command to enter the 3G cellular interface view.
  - Run the mode cdma { 1xrtt-only | evdo-only | hybrid } command to configure the CDMA2000 network connection mode for a 3G modem.
- Configure the Internet dial-up access.
  - Run the quit command to return to the system view.
  - Enable C-DCC.
    - Run the interface cellular interface-number command to enter the 3G cellular interface view.
    - Run the dialer enable-circular command to enable the C-DCC function.
  - Run the dialer number dial-number [ autodial ] command to configure a dialer number.You can obtain the dialer number from the carrier.
  - Run the quit command to return to the system view.
  - Run the ip route-static 0.0.0.0 0 cellular interface-number [ preference preference ] command to configure a default route.interface-number indicates the number of a 3G cellular interface.
- Authenticate a PIN.
  - Run the interface cellular interface-number command to enter the 3G cellular interface view.
  - Run the pin verification enable [ auto ] command to enable PIN authentication on a 3G modem.By default, PIN authentication is disabled. To perform this step, you must enter a PIN to enable PIN authentication on the 3G data card.
  - Run the pin verify [ auto ] command to authenticate the PIN.In this step, the PIN is input automatically or manually. When the message PIN has been verified successfully is displayed on the interface after a period of time, the PIN has been authenticated successfully.
To access the Internet through LTE:
- Log in to the AR using STelnet or through the console port.
- Search for and select a PLMN.
  - Run the system-view command to enter the system view.
  - Run the interface cellular interface-number command to enter the LTE cellular interface view.
  - Run the plmn search command to search for a PLMN.
  - Select a PLMN.
    - Run the plmn auto command to configure automatic selection of a PLMN.
    - Run the plmn select manual mcc mnc [ fail-over-auto ] command to configure manual selection of a PLMN.
 By default, the device automatically selects a PLMN.
- Run the mode lte { auto | gsm-only | 1xrtt-only | evdo-only | hybrid | lte-only | tdscdma-only | umts-gsm | umts-only | wcdma-gsm | wcdma-only } command to configure the 3G/LTE network connection mode for an LTE modem.By default, the 3G/LTE network connection mode is auto for an LTE modem.
- Configure an APN profile (single SIM card and single APN scenario).
  - Run the quit command to return to the system view.
  - Create an APN profile.
    - Run the apn profile profile-name command to create an APN profile and enter the APN profile view.By default, no APN profile is created, and the APN is configured dynamically.
    - Run the apn apn-name command to configure an APN.By default, no APN is configured in the APN profile.  
      - To obtain an APN, contact the local network carrier.
      - After an APN is configured, it is permanently recorded in the LTE data card. If the APN changes, reconfigure it.
    - Run the quit command to return to the system view.
  - Run the apn-profile profile-name [ track nqa { admin-name test-name } &<1-2> ] command to configure the APN profile bound to an LTE cellular interface.
- Configure the MTU.
  - Run the interface cellular interface-number command to enter the LTE cellular interface view or the LTE channel interface view.
  - Run the mtu mtu command to configure the MTU for the LTE cellular interface or the LTE channel interface.The default MTU of an LTE cellular interface or an LTE channel interface is 1500 bytes.
  - Run the quit command to return to the system view.
- Configure C-DCC for dial-up connection.
  - Configure a dialer ACL.
    - Run the dialer-rule command to enter the dialer-rule view.
    - Run the dialer-rule dialer-rule-number { acl { acl-number | name acl-name } | ip { deny | permit } | ipv6 { deny | permit } } command to configure a dialer ACL for a dialer access group to specify the conditions for initiating DCC calls.
    - Run the quit command to return to the system view.
  - Enable C-DCC.
    - Run the interface cellular interface-number command to enter the LTE cellular interface view or the LTE channel interface view.When the multi-APN function is configured, the LTE channel interface view is displayed. If the multi-APN function is not configured, the LTE cellular interface view is displayed.
    - Run the dialer enable-circular command to enable the C-DCC function.By default, the C-DCC function is disabled on an interface.
    - Run the dialer-group group-number command to configure a dialer access group for the dialer interface.By default, no dialer access group is configured.
  - Obtain an IP address.
    - When a user uses two E392 data cards to connect to the Internet, the LTE link uses the PPP dial-up mode.Run the ip address ppp-negotiate command to configure the interface to obtain an IP address from the remote device through PPP negotiation.
    - In other cases, the WWAN dial-up mode is used.Run the ip address negotiate command to configure an LTE cellular interface or an LTE channel interface to obtain an IP address dynamically.
  - Run the dialer number dial-number [ autodial ] command to configure a dialer number.You can obtain the dialer number from the carrier.
  - Run the quit command to return to the system view.
  - Run the ip route-static 0.0.0.0 0 { nexthop-address | interface-type interface-number } [ preference preference ] command to configure a default route.
- Authenticate a PIN.
  - Run the interface cellular interface-number command to enter the LTE cellular interface view.
  - Run the pin verification enable [ auto ] command to enable PIN authentication on an LTE modem.By default, PIN authentication is disabled. In this step, you must enter a PIN to enable PIN authentication for the LTE modem.
  - Run the pin verify [ auto ] command to authenticate the PIN.In this step, you must enter the PIN. When the message PIN has been verified successfully is displayed on the interface after a period of time, the PIN has been authenticated successfully.
To access the Internet through ADSL:
- Log in to the AR using STelnet or through the console port.
- Deactivate the ADSL interface.
  - Run the system-view command to enter the system view.
  - Run the interface atm interface-number command to enter the ADSL interface view.
  - Run the shutdown command to deactivate the ADSL interface.
- Set uplink parameters for the ADSL interface.
  - Run the adsl standard { adsl2 [ annexm | annexj | annexl ] | adsl2+ [ annexm | annexj ] | auto | gdmt | t1413 } command to configure the transmission standard for the ADSL interface.By default, the transmission standard for an ADSL interface is auto.  The device supports both ADSL-A/M and ADSL-B/J. annexm, annexl, and t1413 are supported only by ADSL-A/M. annexj is supported only by ADSL-B/J.
  - Run the adsl bitswap { off | on } command to enable or disable bit exchange on the ADSL interface.By default, bit exchange is enabled on an ADSL interface.
  - Run the adsl sra { off | on } command to enable or disable seamless rate adaptation on the ADSL interface.By default, seamless rate adaptation is disabled on an ADSL interface.
  - Run the adsl trellis { off | on } command to enable or disable trellis coding on the ADSL interface.By default, trellis coding is enabled on an ADSL interface.
- Run the undo shutdown command to activate the ADSL interface.
To access the Internet through VDSL (in ATM mode):
- Log in to the AR using STelnet or through the console port.
- Configure the VDSL interface to work in ATM mode.
  - Run the system-view command to enter the system view.
  - Run the set workmode slot slot-id vdsl atm command to configure the VDSL interface to work in ATM mode.By default, a VDSL interface works in PTM mode.
- Deactivate the VDSL interface.
  - Run the interface atm interface-number command to enter the view of the VDSL interface working in ATM mode.
  - Run the shutdown command to deactivate the VDSL interface.
- Run the vdsl band v43 { off | on } command to disable or enable the v43 carrier band on the VDSL interface.By default, the v43 carrier band is enabled on a VDSL interface.
- Set uplink parameters for the VDSL interface.
  - Run the adsl standard { adsl2 [ annexa | annexm | annexb | annexj | annexl ] | adsl2+ [ annexa | annexm | annexb | annexj ] | auto | gdmt [ annexa | annexb ] | t1413 } command to configure a transmission standard for the VDSL interface in ATM mode.By default, the transmission standard for a VDSL interface in ATM mode is auto.
  - Run the adsl bitswap { off | on } command to enable or disable bit exchange on the VDSL interface.By default, bit exchange is enabled on a VDSL interface.
  - Run the adsl sra { off | on } command to enable or disable seamless rate adaptation on the VDSL interface.By default, seamless rate adaptation is disabled on a VDSL interface.
  - Run the adsl trellis { off | on } command to enable or disable trellis coding on the VDSL interface.By default, trellis coding is enabled on a VDSL interface.
- Run the undo shutdown command to activate the VDSL interface.
To access the Internet through VDSL (in PTM mode):
- Log in to the AR using STelnet or through the console port.
- Configure the VDSL interface to work in PTM mode.
  - Run the system-view command to enter the system view.
  - Run the set workmode slot slot-id vdsl ptm command to configure the VDSL interface to work in PTM mode.By default, a VDSL interface works in PTM mode.
- Deactivate the VDSL interface.
  - Run the interface ethernet interface-number command to enter the view of the VDSL interface working in PTM mode.
  - Run the shutdown command to deactivate the VDSL interface.
- Set uplink parameters for the VDSL interface.
  - Run the vdsl standard vdsl2 { annexa | annexb } command to configure a transmission standard for the VDSL interface in PTM mode.By default, a VDSL interface in PTM mode automatically uses the transmission standard of the peer interface.
  - Run the adsl bitswap { off | on } command to enable or disable bit exchange on the VDSL interface.By default, bit exchange is enabled on a VDSL interface.
  - Run the adsl sra { off | on } command to enable or disable seamless rate adaptation on the VDSL interface.By default, seamless rate adaptation is disabled on a VDSL interface.
  - Run the adsl trellis { off | on } command to enable or disable trellis coding on the VDSL interface.By default, trellis coding is enabled on a VDSL interface.
- Run the undo shutdown command to activate the VDSL interface.
Registering the AR with iMaster NCE-Campus
- Run the system-view command to enter the system view.
- Run the agile controller host host port port command to configure the IP address/URL and port number information for the AR to register with iMaster NCE-Campus. The value of host in this command can be an IP address or a URL.
- Run the agile controller bootstrap host host port port verifytype [ esn | code [ verifycode verifycode ] ] command to configure the IP address/domain name and port number of a bootstrap server and specify the voucher verification mode.
- Run the quit command to exit the system view.
- Run the save command to save the configuration.