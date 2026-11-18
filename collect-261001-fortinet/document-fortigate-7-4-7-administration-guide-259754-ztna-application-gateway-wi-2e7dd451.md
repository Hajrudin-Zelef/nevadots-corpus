# ZTNA application gateway with SAML and MFA using FortiAuthenticator example

ZTNA application gateway supports device verification using device certificates that are issued by EMS. To authenticate users, administrators can use either basic or SAML authentication. An advantage of SAML authentication is that multi-factor authentication (MFA) can be provided by the SAML Identity Provider (IdP).

In these examples, a FortiAuthenticator is used as the IdP, and MFA is applied to user authentication for remote users accessing the web, RDP, and SSH resources over the ZTNA application gateway. It is assumed that the FortiGate EMS fabric connector has already been successfully connected.


DNS resolutions:

- 
                                                    webserver.ztnademo.com:9443 -> 10.0.3.10:9443
- 
                                                    fac.ztnademo.com -> 10.0.3.7

The FortiAuthenticator (FAC) integrates with Active Directory (AD) on the Windows Domain Controller, which is also acting as the EMS server. Users are synchronized from the AD to the FAC, and remote users are configured with token-based authentication. SAML authentication is configured on the FortiGate, pointing to the FAC as the SAML IdP. The SAML server is applied to the ZTNA application gateway authentication scheme and rule, to provide the foundation for applying user authentication on individual ZTNA policies.

First configure FortiAuthenticator to synchronize users from AD using LDAP, apply MFA to individual remote users, and be the IdP.

###### To create a remote authentication server pointing to the Windows AD:

1. 
                                                    Go to *Authentication > Remote Auth. Servers > LDAP* and click*Create New* .
2. 
                                                    Configure the following: Name FortiAD Primary server name / IP 10.88.0.1 Port 389 (or another port if using LDAPS) Based distinguished name DC=fortiad,DC=info Bind type Regular Username <user account used for LDAP bind> Password <password of user> Server Type Microsoft Active Directory User object class person (default) Username attribute sAMAccountName (default) Group object class group (default) Obtain group membership from User attribute Group membership attribute memberOf (default) Secure connection Enable if using LDAPS or STARTTLS
3. 
                                                    Click *OK* .
4. 
                                                    Edit the *FortiAD* entry.
5. 
                                                    At the bottom, click *Import users* .
6. 
                                                    On the *Import Remote LDAP User* screen, configure the following settings:
  1. 
                                                            For *Remote LDAP Server* , select*FortiAD* .
  2. 
                                                            For *Action* , select*Import users* .
    1. 
                                                                    Click *Go* .
    2. 
                                                                    Select the users to import.
    3. 
                                                                    Click *OK* .
  3. 
                                                                    
 The screen displays the users that are imported. For more details, see LDAP in the FortiAuthenticator Administration Guide.
7. 
                                                            

###### To configure a remote LDAP user to use MFA:

1. 
                                                    Go to *Authentication > User Management > Remote Users* , and edit a user.
2. 
                                                    Enable *One-Time Password (OTP) authentication* .
  1. 
                                                            Set *Deliver token codes from* .
  2. 
                                                            Set *Deliver token by* .
 For this example, select *FortiToken > Mobile* , select the Token from the drop-down list, and set the*Activation delivery method* to email.
3. 
                                                            
4. 
                                                    In the *User Information* section, add the email address that will be used for the FortiToken activation.
5. 
                                                    Click *OK* .An activation email is sent to the user that they can use to install the token to their FortiToken Mobile app. For more details, see Remote users in the FortiAuthenticator Administration Guide.

###### To configure SAML IdP:

1. 
                                                    Go to *Authentication > SAML IdP > General* and enable*Enable SAML Identity Provider portal* .
2. 
                                                    The *Server address* is the device FQDN or IP address (configured in the System Information widget at*System > Dashboard > Status* ). In this example, it is*fac.ztnademo.com* .
3. 
                                                    Set *Username input format* to*username@realm* .
4. 
                                                    Click *Add a realm* in the*Realms* table:
  1. 
                                                            Set *Realm* to the just created LDAP realm (*FortiAD* ).
  2. 
                                                            Optionally, enable *Filter* and select the required users groups. In this example,*Sales* is configured.
5. 
                                                            
6. 
                                                    Set *Default IdP certificate* to the certificate that will be used in the HTTPS connection to the IdP portal.
7. 
                                                    Click *OK* .
8. 
                                                    Go to *Authentication > SAML IdP > Service Providers* , and click*Create New* to create a service provider (SP) for the FortiGate SP.
9. 
                                                    Configure the following, which must match what will be configured on the FortiGate: SP name saml-service-provider IdP prefix ztna Server certificate Use default setting in *SAML IdP > General* page.SP entity ID http://webserver.ztnademo.com:9443/remote/saml/metadata/ SP ACS (login) URL https://webserver.ztnademo.com:9443/remote/saml/login SP SLS (logout) URL https://webserver.ztnademo.com:9443/remote/saml/logout Participate in single logout Enable Where the *SP entity ID* ,*SP ACS (login) URL* , and*SP SLS (logout) URL* break down as follows:
  - 
                                                            *webserver.ztnademo.com* - The FQDN that resolves to the FortiGate SP.
  - 
                                                            *9443* - The port that is used to map to the FortiGate's SAML SP service.
  - 
                                                            */remote/saml* - The custom, user defined fields.
  - 
                                                            */metadata* ,*/login* , and*/logout* - The standard convention used to identify the SP entity, log in portal, and log out portal.
10. 
                                                            
11. 
                                                    Click *OK* .
12. 
                                                    Edit the just created SP object and, under *Assertion Attribute* , click*Add Assertion Attribute* .
13. 
                                                    Set *SAML attribute* to the username and set*User attribute* to*Username* , then click*OK* .
14. 
                                                    Click *OK* .

For more details, see Configuring SAML settings in the SAML Interoperability Guide.

On FortiGate, a SAML user is used to define the SAML SP and IdP settings. This user is then applied to the ZTNA proxy using an authentication scheme, rule, and settings. A ZTNA server is then created to allow access to the SAML SP server so that end users can reach the FortiGate SP's captive portal. The SAML user must then be added to a ZTNA policy to trigger authentication when accessing the ZTNA application gateway.

###### To create a new SAML user/server from the GUI:

1. 
                                                    On the FortiGate, go to *User & Authentication > Single Sign-On* .
2. 
                                                    Click *Create New* .
3. 
                                                    Set *Name* to*ZTNA-FAC-SAML* .
4. 
                                                    Set *Address* to*webserver.ztnademo.com:9443* .The *Entity ID* ,*Assertion consumer service URL* and*Single logout service URL* will be updated. These should be the same URLs you inputted into FAC.
5. 
                                                    Enable *Certificate* , then select the certificate used for the client.In this example, the *ztna-wildcard* certificate is a local certificate that is used to sign SAML messages that are exchanged between the client and the FortiGate SP.
6. 
                                                    Click *Next* .
7. 
                                                    Use the settings from the Identity Provider to fill the custom *Identity Provider Details* . In this example, we select*Fortinet Product* , and fill in the fields as follows:Address fac.ztnademo.com Prefix ztna IdP certificate REMOTE_Cert_1 
  - 
                                                            Where the `REMOTE_Cert_1` certificate is a remote certificate that is used to identify the IdP; in this example, fac.ztnademo.com.
8. 
                                                            
9. 
                                                    Set *Attribute used to identify users* to*username* . Attributes to identify users and groups are case sensitive.
10. 
                                                    Click *Submit* to save the settings.

###### To create a new SAML user/server from the CLI:

```
config user saml
    edit "ZTNA-FAC-SAML"
        set cert "ztna-wildcard"
        set entity-id "http://webserver.ztnademo.com:9443/remote/saml/metadata/"
        set single-sign-on-url "https://webserver.ztnademo.com:9443/remote/saml/login"
        set single-logout-url "https://webserver.ztnademo.com:9443/remote/saml/logout"
        set idp-entity-id "http://fac.ztnademo.com/saml-idp/ztna/metadata/"
        set idp-single-sign-on-url "https://fac.ztnademo.com/saml-idp/ztna/login/"
        set idp-single-logout-url "https://fac.ztnademo.com/saml-idp/ztna/logout/"
        set idp-cert "REMOTE_Cert_1"
        set user-name "username"
        set digest-method sha1
    next
end
```
                                            ###### To create a user group for the SAML user object:

1. 
                                                    Under *User & Authentication > User Groups* , click*Create New* .
2. 
                                                    Set *Name* to*ztna-saml-users* .
3. 
                                                    Under *Remote Groups* , click*Add* .
4. 
                                                    For *Remote Server* , select*ZTNA-FAC-SAML* .
5. 
                                                    Click *OK* .
6. 
                                                    Click *OK* again to save.

###### To apply the SAML server to proxy authentication from the GUI:

1. 
                                                    Go to *Policy & Objects > Authentication Rules* .
2. 
                                                    Click *Create New > Authentication Scheme* .
3. 
                                                    Set the name to *ZTNA-SAML-scheme* .
4. 
                                                    Set *Method* to*SAML* .
5. 
                                                    Set *SAML SSO server* to*ZTNA-FAC-SAML* .
6. 
                                                    Click *OK* .
7. 
                                                    Go to *Policy & Objects > Authentication Rules* .
8. 
                                                    Click *Create New > Authentication Rule* .
9. 
                                                    Set the *Name* to*ZTNA-SAML-rule* .
10. 
                                                    Set *Source Address* to*all* .
11. 
                                                    Set *Incoming interface* to*port3* .
12. 
                                                    Set *Protocol* to*HTTP* .
13. 
                                                    Enable *Authentication Scheme* and select*ZTNA-SAML-scheme* .
14. 
                                                    Set *IP-based Authentication* to*Disable* .
15. 
                                                    Click *OK* .

###### To apply the SAML server to proxy authentication from the CLI:

```
config authentication scheme
    edit "ZTNA-SAML-scheme"
        set method saml
        set saml-server "ZTNA-FAC-SAML"
    next
end
config authentication rule
    edit "ZTNA-SAML-rule"
        set srcintf "port3"
        set srcaddr "all"
        set ip-based disable
        set active-auth-method "ZTNA-SAML-scheme"
        set web-auth-cookie enable
    next
end
```
                                            Assign an active authentication scheme and captive portal to serve the log in page for the SAML requests.

###### To configure the active authentication scheme and captive portal from the GUI:

1. 
                                                    Go to *User & Authentication > Authentication Settings* .
2. 
                                                    Enable *Authentication scheme* .
3. 
                                                    Select *ZTNA-SAML-scheme* .
4. 
                                                    Set *Captive portal type* to*FQDN* .
5. 
                                                    Enable *Captive Portal* .
6. 
                                                    Select the firewall address *webserver.ztnademo.com* . If this has not be created, create this firewall address.
7. 
                                                    Click *Apply* to save.

###### To configure the active authentication scheme and captive portal from the CLI:

```
config firewall address
    edit "webserver.ztnademo.com"
        set type fqdn
        set fqdn "webserver.ztnademo.com"
    next
end
config authentication setting
    set active-auth-scheme "ZTNA-SAML-scheme"
    set captive-portal "webserver.ztnademo.com"
end
```
                                            ###### To configure a ZTNA application gateway to allow SAML authentication requests to the SP:

1. 
                                                    Configure the ZTNA server: 
  1. 
                                                            Go to *Policy & Objects > ZTNA* , select the*ZTNA Servers* tab, and click*Create New* .
  2. 
                                                            Configure the following: Name ZTNA-access Interface Any IP 10.0.3.10 Port 9443 SAML Enabled SAML SSO Server ZTNA-FAC-SAML Default certificate ztna-wildcard
  3. 
                                                            Click *OK* .
2. 
                                                            
3. 
                                                    Define the full ZTNA policy to allow access to the ZTNA server: 
  1. 
                                                            Go to *Policy & Objects > Proxy policy* and click*Create New* .
  2. 
                                                            Configure the following: Name ZTNA-Rule Type ZTNA Incoming Interface port3 Source (Address) all Source (User) ztna-saml-users Destination all ZTNA Server ZTNA-access Action Accept Log Allowed Traffic All Sessions
  3. 
                                                            Click *OK* .
4. 
                                                            

###### To configure a VIP and a firewall policy to forward IdP authentication traffic to the FortiAuthenticator:

Remote clients connect to the FortiAuthenticator IdP behind the FortiGate using a VIP. In this example, users connect to the FQDN fac.ztnademo.com that resolves to the VIP's external IP address.

1. 
                                                    Configure the VIP to forward traffic to the FortiAuthenticator: 
  1. 
                                                            Go to *Policy & Objects > Virtual IPs* and navigate to the*Virtual IP* tab.
  2. 
                                                            Click *Create new* .
  3. 
                                                            Configure the following: Name FAC-VIP Interface Any External IP address 10.0.3.7 Map to > IPv4 address/range 10.88.0.7 Port Forwarding Enabled Protocol TCP External service port 443 Map to IPv4 port 443
  4. 
                                                            Click *OK* .
2. 
                                                            
3. 
                                                    Configure a firewall policy to allow VIP: 
  1. 
                                                            Go to *Policy & Objects > Firewall Policy* and click*Create New* .
  2. 
                                                            Configure the following: Name WAN_to_FAC Type Standard Incoming Interface port3 Outgoing Interface port2 Source All ZTNA Server FAC-VIP Schedule Always Service HTTP. HTTPS Action Accept NAT disabled
  3. 
                                                            Click *OK* .
4. 
                                                            

In this HTTPS access proxy example, two real servers are implemented with round robin load balancing performed between them. The HTTPS access proxy is configured on the same ZTNA server as was configured in the authentication step. The same ZTNA rule and firewall policy also apply.

###### To configure the ZTNA server for HTTPS access proxy with load balancing:

1. 
                                                    Go to *Policy & Objects > ZTNA* and select the*ZTNA Servers* tab.
2. 
                                                    Edit the ZTNA-access server.
3. 
                                                    In the *Service/server mapping* table, click*Create New* :
  1. 
                                                            Set *Service* to*HTTPS* .
  2. 
                                                            Set *Virtual Host* to*Any Host* .
  3. 
                                                            In the *Server* section, select*IP* :
    1. 
                                                                    Set *IP address* to*10.88.0.3* .
    2. 
                                                                    Set *Port* to*9443* .
    3. 
                                                                    Click *OK* .
  4. 
                                                                    
4. 
                                                            
5. 
                                                    Click *OK* .
6. 
                                                    Use the CLI to configure the second server with IP *10.88.0.4* .config firewall access-proxy edit "ZTNA-access" config api-gateway edit 1 config realservers edit 0 set ip 10.88.0.4 set port <real server 2 port> next end next end next end
7. 
                                                    In the GUI, edit the ZTNA-access server and verify the server mapping. 
  1. In the *Service/server* mapping table, edit the entry. The*Load balancing* option is visible, and additional real servers can be added by clicking*Create new* .
  2. Click *Cancel* .
8. In the 

###### Testing and verification:

From the remote endpoint, user Tom Smith attempts to connect to the web server over ZTNA:

1. 
                                                    On the remote Windows computer, open FortiClient and register to the EMS server. It is not necessary to configure a *ZTNA Destination* on the FortiClient for the HTTPS access proxy use case. In fact, configuring a*ZTNA Destination* rule for the website may interfere with its operation.
2. 
                                                    Open a browser and attempt to connect to the web server at *https://webserver.ztnademo.com:9443* .
3. 
                                                    Device authentication prompts the user for their device certificate. Select the certificate issued by EMS and click *OK* .
4. 
                                                    FortiGate receives the SAML request and redirects the user to the IdP login screen. Enter the username and password for Tom Smith and click *Login* .
5. 
                                                    A second prompt opens asking for the *Token Code* . Enter the code then click*Verify* .
6. 
                                                    The FortiAuthenticator IdP verifies the login, then sends the SAML assertion back to the user.
7. 
                                                    The browser redirects the assertion to the FortiGate SP, which decides if the user is allowed access.
8. 
                                                    On a successful log in, FortiGate redirects the user to the web page that they are trying to access.

###### Logs and debugs:

On the FortiGate, a successful connection can be seen in *Log & Report > Forward Traffic log*, or by using the CLI:

# execute log filter category 0
# execute log filter field subtype srcip 
# execute log display
...
2: date=2023-05-09 time=00:47:56 eventtime=1683618475812847145 tz="-0700" logid="0005000024" type="traffic" subtype="ztna" level="notice" vd="root" **srcip=10.0.3.2** srcport=17201 srcintf="port3" srcintfrole="wan" dstcountry="Reserved" srccountry="Reserved" **dstip=10.88.0.4** dstport=9443 dstintf="port2" dstintfrole="dmz" sessionid=141588 service="tcp/9443" proxyapptype="http" proto=6 action="accept" policyid=1 policytype="proxy-policy" **policyname="ZTNA-Rule"** duration=83 **user="tsmith" group="ztna-saml-users"** authserver="ZTNA-FAC-SAML" gatewayid=1 **realserverid=2** vip="ZTNA-access" accessproxy="ZTNA-access" clientdevicemanageable="manageable" wanin=316227 rcvdbyte=316227 wanout=6477 lanin=14843 sentbyte=14843 lanout=315122 appcat="unscanned"
...
8: date=2023-05-09 time=00:46:09 eventtime=1683618369392379538 tz="-0700" logid="0005000024" type="traffic" subtype="ztna" level="notice" vd="root" **srcip=10.0.3.2** srcport=17177 srcintf="port3" srcintfrole="wan" dstcountry="Reserved" srccountry="Reserved" **dstip=10.88.0.3** dstport=9443 dstintf="port2" dstintfrole="dmz" sessionid=141526 service="tcp/9443" proxyapptype="http" proto=6 action="accept" policyid=1 policytype="proxy-policy" poluuid="08a04362-ee38-51ed-77fa-737e2656f04a" **policyname="ZTNA-Rule"** duration=63 **user="tsmith" group="ztna-saml-users"** authserver="ZTNA-FAC-SAML" gatewayid=1 **realserverid=1** vip="ZTNA-access" accessproxy="ZTNA-access" clientdevicemanageable="manageable" wanin=313997 rcvdbyte=313997 wanout=4894 lanin=14676 sentbyte=14676 lanout=314865 appcat="unscanned"
...
10: date=2023-05-09 time=00:45:26 eventtime=1683618326285366024 tz="-0700" logid="0000000013" type="traffic" subtype="forward" level="notice" vd="root" **srcip=10.0.3.2** srcport=17181 srcintf="port3" srcintfrole="wan" dstip=10.0.3.7 dstport=443 dstintf="port2" dstintfrole="dmz" srccountry="Reserved" dstcountry="Reserved" sessionid=141539 proto=6 action="client-rst" policyid=6 policytype="policy" poluuid="2e840d4e-1184-51ec-63b9-9b805a8b7344" **policyname="WAN_to_FAC"** service="HTTPS" trandisp="dnat" tranip=10.88.0.7 tranport=443 duration=10 sentbyte=3449 rcvdbyte=4372 sentpkt=13 rcvdpkt=13 appcat="unscanned"

Log number ten shows that authentication first passes through the WAN_to_FAC policy. Log numbers two and eight show the traffic allowed through the ZTNA proxy-policy over two successive sessions. Note that they have different destination IP addresses (`dstip`), indicating that ZTNA was performing server load balancing.

Use the following command to show if the FortiGate's WAD process has an active record of the SAML user login:

# diagnose wad user list 
ID: 8, VDOM: root, **IPv4: 10.0.3.2**
  user name   : **tsmith**
  worker      : 1
  duration    : 332
  auth_type   : Session
  auth_method : **SAML**
  pol_id      : 1
  g_id        : 4
  user_based  : 0
  expire      : 390
  LAN:
    bytes_in=29519 bytes_out=629987
  WAN:
    bytes_in=958636 bytes_out=19201

In this TCP forwarding access proxy example, RDP connections are allowed to be forwarded to the Windows/EMS server. Traffic to TCP/3389 is allowed through the ZTNA proxy.

###### To configure the ZTNA server for TCP forwarding on TCP/3389:

1. 
                                                    Create a firewall address for the Windows/EMS server: 
  1. 
                                                            Go to *Policy & Objects > Addresses* and select*Address* .
  2. 
                                                            Click *Create new* .
  3. 
                                                            Configure the following: Name winserver Type Subnet IP/Netmask 10.88.0.1/32 Interface any
  4. 
                                                            Click *OK* .
2. 
                                                            
3. 
                                                    Go to *Policy & Objects > ZTNA* and select the*ZTNA Servers* tab.
4. 
                                                    Edit the ZTNA-access server.
5. 
                                                    In the *Service/server mapping* table, click*Create New* :
  1. 
                                                            Set *Service* to*TCP Forwarding* .
  2. 
                                                            In the *Server* section, set*Address* to*winserver* .
  3. 
                                                            Set *Ports* to*3389* .
  4. 
                                                            Click *OK* .
6. 
                                                            
7. 
                                                    Click *OK* .

###### Testing and verification:

On the remote endpoint, manually configure a ZTNA destination to forward RDP traffic to the ZTNA application gateway. The rules can also be pushed from the EMS server; for details see Provisioning ZTNA TCP forwarding rules via EMS.

Configure the ZTNA Destination:

1. 
                                                    On the remote Windows computer, open FortiClient.
2. 
                                                    Register to the EMS server.
3. 
                                                    On the *ZTNA Destination* tab, click*Add Destination* to add a TCP forwarding rule.
4. 
                                                    Configure the following: Rule Name RDP-server Destination Host 10.88.0.1:3389 Proxy Gateway 10.0.3.10:9443 Mode Transparent Encryption Disabled *Encryption* can be enabled or disabled. When it is disabled, the client to access proxy connection is not encrypted in HTTPS. Because RDP is encrypted by default, disabling*Encryption* does not reduce security.
5. 
                                                    Click *Create* .

Connect over RDP:

1. 
                                                    On the remote PC, open a new RDP connection.
2. 
                                                    Enter the IP address 10.88.0.1. By default, RDP session use port 3389. When the connection to the ZTNA application gateway is established, FortiGate will redirect the SAML login request to the FortiAuthenticator IdP. A FortiClient prompt will open with the FortiAuthenticator login screen.
3. 
                                                    Enter the username and password then click *Login* .
4. 
                                                    A second prompt opens asking for the *Token Code* . Enter the code from your FortiToken app, then click*Verify* .
5. 
                                                    FortiAuthenticator verifies the token code, determines if the login is successful, then sends the SAML assertion back to the client.
6. 
                                                    The client redirects the response back to the FortiGate SP.
7. 
                                                    If the log in was successful, the user can now log on to the RDP session.

###### Logs and debugs:

On the FortiGate, a successful connection can be seen in *Log & Report > Forward Traffic log*, or by using the CLI:

# execute log filter category 0
# execute log filter field srcip 10.0.3.2
# execute log display
...
8: date=2023-05-09 time=01:14:34 eventtime=1683620074907124357 tz="-0700" logid="0005000024" type="traffic" subtype="ztna" level="notice" vd="root" **srcip=10.0.3.2** srcport=17556 srcintf="port3" srcintfrole="wan" dstcountry="Reserved" srccountry="Reserved" **dstip=10.88.0.1** dstport=3389 dstintf="port2" dstintfrole="dmz" sessionid=142359 **service="RDP"** proxyapptype="http" proto=6 action="accept" policyid=1 policytype="proxy-policy" poluuid="08a04362-ee38-51ed-77fa-737e2656f04a" policyname="ZTNA-Rule" duration=85 **user="tsmith" group="ztna-saml-users"** gatewayid=3 vip="ZTNA-access" accessproxy="ZTNA-access" clientdevicemanageable="manageable" wanin=0 rcvdbyte=0 wanout=0 lanin=3639 sentbyte=3639 lanout=3797 appcat="unscanned"
9: date=2023-05-09 time=01:14:16 eventtime=1683620056835075857 tz="-0700" logid="0000000013" type="traffic" subtype="forward" level="notice" vd="root" **srcip=10.0.3.2** srcport=17571 srcintf="port3" srcintfrole="wan" **dstip=10.0.3.7** dstport=443 dstintf="port2" dstintfrole="dmz" srccountry="Reserved" dstcountry="Reserved" sessionid=142389 proto=6 action="client-rst" policyid=6 policytype="policy" poluuid="2e840d4e-1184-51ec-63b9-9b805a8b7344" **policyname="WAN_to_FAC"** service="HTTPS" trandisp="dnat" **tranip=10.88.0.7** tranport=443 duration=10 sentbyte=3368 rcvdbyte=4376 sentpkt=13 rcvdpkt=13 appcat="unscanned"

Use the following command to show if the FortiGate's WAD process has an active record of the SAML user login:

# diagnose wad user list
ID: 9, VDOM: root, **IPv4: 10.0.3.2**
  user name   : **tsmith**
  worker      : 1
  duration    : 164
  auth_type   : Session
  auth_method : **SAML**
  pol_id      : 1
  g_id        : 4
  user_based  : 0
  expire      : no
  LAN:
    bytes_in=80950 bytes_out=233010
  WAN:
    bytes_in=219973 bytes_out=58315

In this SSH access proxy example, SSH connections can be forwarded to the FortiAnalyzer (10.88.0.2).

###### To configure the ZTNA server for SSH access proxy:

1. 
                                                    Create a firewall address for the web server: 
  1. 
                                                            Go to *Policy & Objects > Addresses* and select*Address* .
  2. 
                                                            Click *Create new* .
  3. 
                                                            Configure the following: Name FAZ Type Subnet IP/Netmask 10.88.0.2/32 Interface any
  4. 
                                                            Click *OK* .
2. 
                                                            
3. 
                                                    Go to *Policy & Objects > ZTNA* and select the*ZTNA Servers* tab.
4. 
                                                    Edit the ZTNA-access server.
5. 
                                                    In the *Service/server mapping* table, edit the*TCP Forwarding* entry:
  1. 
                                                            In the *Servers* table click*Create New* :
    1. 
                                                                    Set *Address* to*FAZ* .
    2. 
                                                                    Set *Ports* to*22* .
    3. 
                                                                    Optionally, enable *Additional SSH Options* to configure other SSH options as needed.
    4. 
                                                                    Click *OK* .
  2. 
                                                                    
  3. 
                                                            Click *OK* .
6. 
                                                            
7. 
                                                    Click *OK* .

###### Testing and verification:

On the remote endpoint, manually configure ZTNA destination to forward SSH traffic to the ZTNA access proxy. The destination can also be pushed from the EMS server; for details see Provisioning ZTNA TCP forwarding rules via EMS.

Configure the ZTNA Destination:

1. 
                                                    On the remote Windows computer, open FortiClient.
2. 
                                                    Register to the EMS server.
3. 
                                                    On the *ZTNA Destination* tab, click*Add Destination* to add a TCP forwarding rule.
4. 
                                                    Configure the following: Rule Name SSH-FAZ Destination Host 10.88.0.2:22 Proxy Gateway 10.0.3.10:9443 Mode Transparent Encryption Disabled
5. 
                                                    Click *Create* .

Connect over SSH:

1. 
                                                    On the remote PC, open a new SSH connection.
2. 
                                                    Enter the host admin@10.88.0.2 on port 22. When the connection to the ZTNA application is established, FortiGate will redirect the SAML login request to the FortiAuthenticator IdP. A FortiClient prompt will open with the FortiAuthenticator login screen.
3. 
                                                    Enter the username and password then click *Login* .
4. 
                                                    A second prompt opens asking for the *Token Code* . Enter the code from your FortiToken app, then click*Verify* .
5. 
                                                    FortiAuthenticator verifies the token code, determines if the login is successful, then sends the SAML assertion back to the client.
6. 
                                                    The client redirects the response back to the FortiGate SP.
7. 
                                                    If the log in was successful, the user can now log on to the SSH session.

###### Logs and debugs:

On the FortiGate, a successful connection can be seen in *Log & Report > Forward Traffic log*, or by using the CLI:

# execute log filter category 0
# execute log filter field srcip 10.0.3.2
# execute log display
...
5: date=2023-05-09 time=10:06:04 eventtime=1683651963820802005 tz="-0700" logid="0005000024" type="traffic" subtype="ztna" level="notice" vd="root" **srcip=10.0.3.2** srcport=22536 srcintf="port3" srcintfrole="wan" dstcountry="Reserved" srccountry="Reserved" **dstip=10.88.0.2 dstport=22** dstintf="port2" dstintfrole="dmz" sessionid=151696 **service="SSH"** proxyapptype="http" proto=6 action="accept" policyid=1 policytype="proxy-policy" policyname="ZTNA-Rule" duration=9 **user="tsmith" group="ztna-saml-users"** gatewayid=3 vip="ZTNA-access" accessproxy="ZTNA-access" clientdevicemanageable="manageable" wanin=2981 rcvdbyte=2981 wanout=2753 lanin=4663 sentbyte=4663 lanout=5247 appcat="unscanned"

Use the following command to show if the FortiGate's WAD process has an active record of the SAML user login:

# diagnose wad user list
ID: 10, VDOM: root, **IPv4: 10.0.3.2**
  user name   : **tsmith**
  worker      : 1
  duration    : 387
  auth_type   : Session
  auth_method : **SAML**
  pol_id      : 1
  g_id        : 4
  user_based  : 0
  expire      : no
  LAN:
    bytes_in=23261 bytes_out=14024
  WAN:
    bytes_in=3242 bytes_out=2541