---
id: collect-261121-veille-ia/gemini-google/arborescence-des-pages-75
title: "Arborescence des pages"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["certificate", "string", "name", "issuer", "default", "used", "certificates", "template"]
source: https://help.mikrotik.com/docs/display/ROS/Certificates
source_anchor: ""
source_lines: [1, 188]
section: "Gemini_Google"
---
# Overview

The general menu is used to manage certificates, add templates, issue certificates, and manage CRL and SCEP Clients.

## Certificate Template

Certificate templates are used to prepare a desired certificate for signing.

Certificate template is deleted right after a certificate is signed or a certificate request command is executed

To print out certificates:

### Certificate template properties

During the certificate template creation process, it is possible define and configure multiple parameters to meet specific requirements.

| Property | Description | 
|---|---|
| **common-name** (*string* ) | Certificate common name | 
| **copy-from**  (*name* ) | Certificate name from which to copy general settings | 
| **country** (*string* ) | Certificate issuer country | 
| **days-valid**  (days Default: 365) | Days certificate will be valid after signing | 
| **digest-algorithm** (*md5 \| sha1 \| sha256 \| sha384 \| sha512* Default:**sha256** ) | Certificate public key algorithm | 
| **key-size** (1024 \| 1536 \| 2048 \| 4096 \| 8192 \| prime256v1 \| secp384r1 \| secp521r1 Default:**2048** ) | Certificate public key size | 
| **key-usage** (code-sign \| crl-sign \| decipher-only \| dvcs \| encipher-only     key-cert-sign \| ocsp-sign \| tls-client \| content-commitment \| data-encipherment \| digital-signature \| email-protect \| key-agreement \| key-encipherment \| timestamp \| tls-server Default: **digital-signature,key-encipherment,data-encipherment,key-cert-sign,crl-sign,tls-server,tls-client)** | Certificate usage | 
| **locality** (*string* ) | Certificate issuer locality | 
| **name** (*string* ) | Certificate name | 
| **organization** (*string* ) | Certificate issuer organization | 
| **state** (*string* ) | Certificate issuer state | 
| **subject-alt-name** (*DNS: \| IP: \| email:* ) | Certificate subject alternative name | 
| **trusted** (*no \| yes*  ) | Wherever to trust certificate. If *yes,*  certificate will be used for host certificate verification. | 
| **trust-store**  (*all* \|*capsman* \|*dns* \|*email* \|*ipsec* \|*mqtt* \|*openflow* \|*radius* \|*sstp* \|*userman* \|*www* \|*api* \|*container* \|*dot1x* \|*fetch* \|*lora* \|*netwatch* \|*ovpn* \|*tr069* \|*wpa-eap* Default:**all** ) | Specify service which can use a specific certificate for certificate verification or trust-chain creation (www, sstp). | 
| **unit** (*string* ) | Certificate issuer organizational unit | 

## Certificate properties

For a signed certificate, most properties are read-only, with the exception of *name, trusted*, and *trust-store*.

| Property | Description | 
|---|---|
| **acme-status***(string)* | ACME client status | 
| **common-name** (*string* ) | Certificate common name | 
| **copy-from**  (*name* ) | Certificate name from which to copy general settings | 
| **country** (*string* ) | Certificate issuer country | 
| **days-valid**  (days) | Days certificate will be valid after signing | 
| **digest-algorithm** (*md5 \| sha1 \| sha256 \| sha384 \| sha512*  ) | Certificate public key algorithm | 
| **directory-url** *(string)* | ACME client directory URL | 
| **domain-names** *(string)* | ACME client used domain names | 
| **key-size** (1024 \| 1536 \| 2048 \| 4096 \| 8192 \| prime256v1 \| secp384r1 \| secp521r1) | Certificate public key size | 
| **key-usage** (code-sign \| crl-sign \| decipher-only \| dvcs \| encipher-only     key-cert-sign \| ocsp-sign \| tls-client \| content-commitment \| data-encipherment \| digital-signature \| email-protect \| key-agreement \| key-encipherment \| timestamp \| tls-server) | Certificate usage | 
| **locality** (*string* ) | Certificate issuer locality | 
| **organization** (*string* ) | Certificate issuer organization | 
| **revoked** *(date)* | Certificate revoke time (only for certificates that are signed and revoked in specific device) | 
| **state** (*string* ) | Certificate issuer state | 
| **subject-alt-name** (*DNS \| IP \| email* ) | Certificate subject alternative name | 
| **trusted** (*no \| yes* ) | Wherever to trust certificate. If *yes,*  certificate will be used for host certificate verification. | 
| **trust-store**  (*all* \|*capsman* \|*dns* \|*email* \|*ipsec* \|*mqtt* \|*openflow* \|*radius*  \|*sstp* \|*userman* \|*www* \|*api* \|*container* \|*dot1x* \|*fetch* \|*lora* \|*netwatch* \|*ovpn* \|*tr069* \|*wpa-eap* ) | Specify service which can use a specific certificate for certificate verification or trust-chain creation (www, sstp). | 
| **unit** (*string* ) | Certificate issuer organizational unit | 
| **serial-number** (*string* ) | Certificate serial number | 
| **fingerprint** (*string* ) | Certificate fingerprint | 
| **akid** (*string* ) | Certificate authority ID | 
| **skid** (*string* ) | Certificate subject ID | 
| **issuer** (*string* ) | Certificate Authority | 
| **invalid-before***(date)* | Date and time before which a certificate expired | 
| **invalid-after** *(date)* | Date and time after which a certificate expired | 
| **expires-after** *(time)* | Time left before expiration | 
| **key-type** (string) | Private key ype | 
| **ca** *(string)* | CA certificate name (shown only for certificates that are signed in specific device) | 

If the CA certificate is removed, all issued certificates in the chain are also removed.

## Sign Certificate

Certificates should be signed. In the following example, we will sign certificates and add CRL URL for the server certificate:

Let`s check is the certificates are signed:

For a video example click here.

The time of the key signing process depends on the key size of a specific certificate. With values of 4k and higher, it might take a substantial time to sign this specific certificate on less powerful CPU-based devices.

## Export Certificate

It is possible to export client certificates with keys and CA certificates in two formats - PEM or PCKS12.

| Property | Description | 
|---|---|
| **export-passphrase** (*string* Default: none)*sensitive* | Passphrase that will be used for exported certificate private key encryption. | 
| **file-name** (*string*  Default: cert_export_[Certificate name].crt/key/pkcs12) | Exported certificate file name. | 
| **type** (*pem \| pkcs12* Default: pem) | Exported certificate type. In case of PEM, certificate will be exported with CRT extension, if export-passphrase is specified, also encrypted private KEY file will be exported. In case of PKCS12, certificate will be exported with P12 extension, if export-passphrase is specified, exported certificate will contain encryted private key. | 

Exported certificates are available under the */file* section:

Exporting certificates requires "sensitive" user policy.

## Import Certificate

To import certificates, certificates must be uploaded to a device using one of the file upload methods.

Certificates must be imported as a file.

Supported are PEM, DER, CRT, PKCS12 formats.

| Property | Description | 
|---|---|
| **name** (*string* Default: file-name_number) | A certificate name that will be shown in the certificate manager | 
| **file-name** (*string* ) | A file name that will be imported | 
| **passphrase** (*string* Default: none)*sensitive* | File passphrase if there is such | 
| **trusted** (*yes \| no* Default: yes) | Adds *trusted* flag for imported certificate | 
| **trust-store**  (*all* \|*capsman* \|*dns* \|*email* \|*ipsec* \|*mqtt* \|*openflow* \|*radius* \| \|*sstp* \|*userman* \|*www* \|*api* \|*container* \|*dot1x* \|*fetch* \|*lora* \|*netwatch* \|*ovpn* \|*tr069* \|*wpa-eap* Default:**all** ) | Specify service which can use a specific certificate for certificate verification or trust-chain creation (www, sstp). | 

## Settings

*/certificate settings* allows configuring Certificate Revocation List (CRL) settings.

By default, CRL is not utilized, and certificates are not verified for revocation status.

| Property | Description | 
|---|---|
| **builtin-trust-store** (*all* \|*default* \|*capsman* \|*dns* \|*email* \|*ipsec* \|*mqtt* \|*openflow* \|*radius* \| \|*sstp* \|*userman* \|*www* \|*api* \|*container* \|*dot1x* \|*fetch* \|*lora* \|*netwatch* \|*ovpn* \|*tr069* \|*wpa-eap \|**untrusted* Default:***default*** ) | Services that can use built-in trust store authorities for certificate verification. The current defaults:  | 
| **crl-download** (*yes \| no* Default:**no** ) | Whether to automatically download/update CRL | 
| **crl-store** (*ram \| sytem* Default:**ram** ) | Where to store downloaded CRL information CRL will be automatically renewed every hour for certificates which have "trusted=yes" using http protocol (ldap and ftp is currently unsupported) | 
| **crl-use** (*yes \| no* Default: no) | Whether to use CRL | 

If */certificate/settings/set crl-use* is set to *yes,* RouterOS will check CRL for each certificate in a certificate chain, therefore, an entire certificate chain should be installed into a device - starting from Root CA, intermediate CA (if there are such), and certificate that is used for specific service.

An example on importing a root certificate.

# ACME client

The ACME client automates the acquisition and renewal of multiple TLS certificates via ACME.

To add a new ACME client via CLI, use the command /certificate add-acme.

Existing ACME clients appear in the Certificates view and are marked with the *a* (acme-manage) flag.

Domain names must resolve to the router, and TCP port 80 must be accessible from the WAN (HTTP-01 challange is used). For example.sn.mynetname.net domain name, DNS-01 challange is used.

Certificates are automatically renewed when 80% of their validity period has elapsed.

If the certificate is not retrieved during the initial setup, a new ACME client must be added.

## Properties

| Property | Description | 
|---|---|
| **directory-url** (*string* ) | ACME directory URL | 
| **domain-names** (*string* ) | comma separated list of domain names | 
| **eab-hmac-key** (*string* ) | HMAC key for ACME External Account Binding | 
| **eab-kid** (*string* ) | Key identifier | 
| **name** (*string* ) | ACME client name | 

## Let's Encrypt certificate

To retrieve Let's Encrypt certificate with automatic certificate renewal, must manually provide ACME directory URL (https://acme-v02.api.letsencrypt.org/directory) and domain-name.

To generate Let's Encrypt certificate for /*ip cloud* name (ie. example.sn.mynetname.net), as domain-name provide *dns-name* from */ip/cloud* menu or use "*[/ip/cloud/get dns-name]*"

# SCEP

SCEP is using HTTP protocol and base64 encoded GET requests. Most of the requests are without authentication and cipher, however, important ones can be protected if necessary (ciphered or signed using a received public key).

SCEP client in RouterOS will:

- get CA certificate from CA server or RA (if used);
- user should compare the fingerprint of the CA certificate or if it comes from the right server;
- generate a self-signed certificate with a temporary key;
- send a certificate request to the server;
- if the server responds with status x, then the client keeps requesting until the server sends an error or approval.

The SCEP server supports the issuance of one certificate only. RouterOS supports also renew and next-ca options:

- renew - the possibility to renew the old certificate automatically with the same CA.
- next-ca - possibility to change the current CA certificate to the new one.

The client polls the server for any changes, if the server advertises that the next-ca is available, then the client may request the next CA or wait until CA almost expires and then request the next-ca.

The RouterOS client by default will try to use POST, AES, and SHA256 if the server advertises that. If the above algorithms are not supported, then the client will try to use 3DES, DES and SHA1, MD5.

SCEP certificates are renewed when 3/4 of their validity time has passed.

RouterOS contains list of built-in root certificate authorities that specific services can use for host certificate verification.

List of services that can use built-in root certificate authorities can be found here.

It is possible to use DoH, download Adlist from URL or use fetch tool with certificate validation without the need to manually import the relevant root certificate.

The list of built-in root certificate authorities is accessible in System → Certificates → Built In CA
