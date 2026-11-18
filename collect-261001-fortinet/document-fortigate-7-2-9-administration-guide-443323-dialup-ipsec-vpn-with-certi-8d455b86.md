# Dialup IPsec VPN with certificate authentication

# Dialup IPsec VPN with certificate authentication

In a dialup IPsec VPN setup, a company may choose to use X.509 certificates as their authentication solution for remote users. This method includes the option to verify the remote user using a user certificate, instead of a username and password. This method can be simpler for end users.

Administrators need to issue unique user certificates to each user for remote access management. The user certificate can be verified by the subject field, common name, or the principal name in the Subject Alternative Name (SAN) field.

## Subject field verification

This is the basic method that verifies the subject string defined in the PKI user setting matches a substring in the subject field of the user certificate. For example:

```
config user peer
    edit "tgerber"
        set ca "CA_Cert_2"
        set subject "CN=tgerber"
    next
end
```
                                            
                                            In this method, administrators can define the CN string to match the common name (CN) in the subject field of the certificate. For example:

```
config user peer
    edit "tgerber"
        set ca "CA_Cert_2"
        set cn "tgerber"
    next
end
```
                                            The matching certificate looks like the following:


A PKI user must be created on the FortiGate for each remote user that connects to the VPN with a unique user certificate.

## Principal name with LDAP integration

In this method, the PKI user setting references an LDAP server. When `ldap-mode` is set to `principal-name`, the UPN in the user certificate's SAN field is used to look up the user in the LDAP directory. If a match is found, then authentication succeeds. For example:

```
config user peer
    edit "ldap-peer"
        set ca "CA_Cert_2"
        set ldap-server "WIN2K16-KLHOME-LDAPS"
        set ldap-mode principal-name
    next
end
```
                                            The matching certificate looks like the following:


This method is more scalable because only one PKI user needs to be created on the FortiGate. Remote users connect with their unique user certificate that are matched against users in the LDAP server.

## Certificate management

Dialup IPsec VPN with certificate authentication requires careful certificate management planning. Assuming that a company's private certificate authority (CA) is used to generate and sign all the certificates, the following certificates are needed:

| Certificate type | Description | 
|---|---|
| Server certificate | The server certificate is used to identify the FortiGate IPsec dialup gateway. A CSR can be generated on the FortiGate and signed by the CA, or the CA can generate the private and public keys and export the certificate package to the FortiGate. | 
| User certificate | The user certificate is generated and signed by the CA with unique CNs in the subject field and/or unique Principal Names in the SAN field. They are used to identify the user that is connecting to the VPN. User certificates must be installed on client machines. | 
| CA certificate | The root CA certificate, and any subordinate CA that signed the actual user and server certificates, must be imported into the FortiGate and client machines. The CA certificate is used to verify the certificate chain of the server and user certificates. | 

## Example

In this example, a dialup IPsec VPN tunnel is configured with certificate authentication using the subject field verification method and the LDAP integration method.


The company CA, named root CA, signs all the server and user certificates. The user, tgerber@klhome.local, has a user certificate signed by root CA installed on their endpoint. The corresponding user account is also present under the company's Active Directory.

There are five major steps to configure this example:

The server certificate and CA certificate need to be imported into the FortiGate.

###### To import the server certificate:

1. Go to *System > Certificates* and select*Import > Local Certificate* .
2. For *Type* , select*PKCS #12 Certificate* .
3. Upload the key file exported from the CA and enter the password.
4. Click *OK* . The certificate now appears in the*Local Certificate* section.

###### To import the CA certificate:

1. Go to *System > Certificates* and select*Import > CA Certificate* .
2. For *Type* , select*File* .
3. Upload the CA certificate (usually a .CRT file). This certificate only contains the public key.
4. Click *OK* . The certificate now appears in the*Remote CA Certificate* section.

|  | If any subordinate CA is involved in signing the certificates, you need to import its certificate. | 

FortiGate PKI users do not appear in the GUI until at least one PKI user has been created in the CLI. The following instructions create the PKI users in the CLI.

###### To configure PKI users for subject field verification:

1. Create the PKI user and choose the CA certificate that was imported (if the certificate was signed by a subordinate CA, choose the subordinate CA's certificate):```
config user peer
    edit "tgerber"
        set ca "CA_Cert_2"
        set subject "CN=tgerber"
    next
end
```
For an example of CN field matching, see Common name verification.
2. Create additional users as needed.
3. Place the users into a peer group:```
config user peergrp
    edit "pki-users"
        set member "tgerber" <user> ... <user>
    next
end
```

###### To configure PKI users for LDAP integration:

1. Configure the LDAP server that users connect to for authentication:```
config user ldap
    edit "WIN2K16-KLHOME-LDAPS"
        set server "192.168.20.6"
        set cnid "sAMAccountName"
        set dn "dc=KLHOME,dc=local"
        set type regular
        set username "KLHOME\\Administrator"
        set password ************
        set secure ldaps
        set ca-cert "CA_Cert_1"
        set port 636
    next
end
```
2. Configure the PKI user to reference the LDAP server using the CA certificate that was imported:```
config user peer
    edit "ldap-peer"
        set ca "CA_Cert_2"
        set ldap-server "WIN2K16-KLHOME-LDAPS"
        set ldap-mode principal-name
    next
end
```
3. Place the user into a peer group:```
config user peergrp
    edit "pki-ldap"
        set member "ldap-peer"
    next
end
```

To configure the VPN, the address objects must be defined first so they can be used in the VPN and policy configurations. In this example, the VPN is configured in custom mode to define the authentication settings.

###### To configure the address objects:

1. Create the address range for the dialup clients:
  1. Go to *Policy & Objects > Addresses* and click*Create New > Address* .
  2. For *Name* , enter*remote-user-range* .
  3. For *Type* , select*IP Range* and enter*172.18.200.10-172.18.200.99* in the*IP Range* field.
  4. Click *OK* .
2. Go to 
3. Create the address subnet for the destination 192.168.20.0/24:
  1. Click *Create New > Address* .
  2. For *Name* , enter*192.168.20.0* .
  3. For *Type* , select*Subnet* and enter*192.168.20.0/24* in the*IP/Netmask* field.
  4. Click *OK* .
4. Click 

###### To configure the IPsec dialup tunnel:

1. Go to *VPN > IPsec Tunnels* and click*Create New > IPsec Tunnel* .
2. Enter a name for the tunnel, *Dialup-cert_0* .
3. For *Template type* , select*Custom* then click*Next* .
4. In the *Network* section, enter the following:Remote Gateway *Dialup User*Interface *port1*Mode Config Enable Assign IP From *Range*IPv4 mode config > Client Address Range *172.18.200.10-172.18.200.99*Enable IPv4 Split Tunnel Enable Accessible Networks *192.168.20.0*
5. In the *Authentication* section, enter the following:Method *Signature*Certificate Name Select the server certificate that was imported. Mode *Aggressive*Peer Options > Accept Types *Peer certificate group*Peer Options > Peer certificate group Select the group based on the preferred method: 
  - For subject verification, select *pki-users* .
  - For LDAP integration, select *pki-ldap* .
 When IKEv1 is used, aggressive mode should be selected so that the connecting endpoint will provide its peer ID in the first message of the IKE exchange. The peer identifier allows the FortiGate to match the correct tunnel when multiple dialup tunnels are defined.
6. For subject verification, select 
7. For *Phase 2 Selectors* , leave the local and remote selectors as*0.0.0.0/0.0.0.0* .
8. Click *OK* .

###### To configure the firewall policy:

1. Go to *Policy & Objects > Firewall Policy* and click*Create New* .
2. Configure the following:Name Enter a policy name. Incoming interface *Dialup-cert_0*Outgoing Interface *port3*Source *remote-user-range*Destination *192.168.20.0*Schedule *always*Service *ALL*Action *ACCEPT*
3. Configure the other settings as needed.
4. Click *OK* .

The following example is configured on a Windows PC with FortiClient 7.0.0. Other configurations may differ slightly.

The user certificate and CA certificate must be installed on the endpoint device. They may be pushed by the administrator through group policies or another method. This example assumes that the user certificate and CA certificate are already installed on the endpoint.

###### To verify the user and CA certificates:

1. Open the Windows certificate manager (certmgr):
  1. In the Control Panel, type *Manage user certificate* in the search box.
  2. Click the result, *Manage user certificates* .
2. In the Control Panel, type 
3. Go to *Personal > Certificate* . The user certificate should be listed.
4. Go to *Trusted Root Certification Authorities > Certificates* . The company CA certificate should be listed.

###### To configure the FortiClient endpoint settings:

1. In FortiClient, click the *Remote Access* tab and add a new connection:
  1. If there are no existing connections, click *Configure VPN* .
  2. If there are existing connections, click the menu icon and select *Add a new connection* .
2. If there are no existing connections, click 
3. Configure the following:VPN *IPsec VPN*Connection Name *Dialup-cert_0*Remote Gateway *192.168.2.5*Authentication Method *X.509 Certificate*Select the user certificate, *tgerber/root CA* , from the dropdown.Authentication (XAuth) *Disable*
4. Click *Save* .

1. On the client PC, open FortiClient and click the *Remote Access* tab.
2. Select the VPN tunnel, *Dialup-cert_0* , and click*Connect* .If the connection is successful, a FortiClient pop-up will appear briefly indicating that the IKE negotiation succeeded. The *Remote Access* window now displays*VPN Connected* and the associated VPN tunnel details.
3. On the FortiGate, go to *Dashboard > Network* and locate the*IPsec* widget to view the VPN tunnel monitor. Click the widget to expand to full view.The widget displays tunnel information, including the *Peer ID* containing the subject field of the user certificate.
4. Go to *Log & Report > System Events* and select the*VPN Events* card. Several tunnel related logs are recorded.
5.  The same logs can be viewed in the CLI:# execute log filter category 1 # execute log filter field subtype vpn # execute log display 7: date=2021-08-23 time=15:53:08 eventtime=1629759188862005740 tz="-0700" logid="0101037138" type="event" subtype="vpn" level="notice" vd="root" logdesc="IPsec connection status changed" msg="IPsec connection status change" **action="tunnel-up"** remip=192.168.2.1 locip=192.168.2.5 remport=64916 locport=4500 outintf="port1" cookies="19f05ebc8c2f7a0d/7716190005538db5"**user="C = CA, ST = British Columbia, L = Burnaby, O = FortiKeith, OU = TAC, CN = tgerber" group="pki-ldap"** useralt="C = CA, ST = British Columbia, L = Burnaby, O = FortiKeith, OU = TAC, CN = tgerber" xauthuser="N/A" xauthgroup="N/A" assignip=172.18.200.10 vpntunnel="Dialup-cert_0" tunnelip=172.18.200.10 tunnelid=3418215253 tunneltype="ipsec" duration=0 sentbyte=0 rcvdbyte=0 nextstat=0
6. If any issues arise during the connection, run the following debug commands to troubleshoot the issue:# diagnose debug application ike -1 # diagnose debug application fnbamd -1 # diagnose debug enable