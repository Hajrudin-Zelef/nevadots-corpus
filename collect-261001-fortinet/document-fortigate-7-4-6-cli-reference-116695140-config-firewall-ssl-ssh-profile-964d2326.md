config firewall ssl-ssh-profile
config firewall ssl-ssh-profile
Configure SSL/SSH protocol options.
config firewall ssl-ssh-profile
    Description: Configure SSL/SSH protocol options.
    edit <name>
        set allowlist [enable|disable]
        set block-blocklisted-certificates [disable|enable]
        set caname {string}
        set comment {var-string}
        config dot
            Description: Configure DNS over TLS options.
            set cert-validation-failure [allow|block|...]
            set cert-validation-timeout [allow|block|...]
            set client-certificate [bypass|inspect|...]
            set expired-server-cert [allow|block|...]
            set proxy-after-tcp-handshake [enable|disable]
            set quic [inspect|bypass|...]
            set revoked-server-cert [allow|block|...]
            set sni-server-cert-check [enable|strict|...]
            set status [disable|deep-inspection]
            set unsupported-ssl-cipher [allow|block]
            set unsupported-ssl-negotiation [allow|block]
            set unsupported-ssl-version [allow|block]
            set untrusted-server-cert [allow|block|...]
        end
        config ech-outer-sni
            Description: ClientHelloOuter SNIs to be blocked.
            edit <name>
                set sni {string}
            next
        end
        config ftps
            Description: Configure FTPS options.
            set cert-validation-failure [allow|block|...]
            set cert-validation-timeout [allow|block|...]
            set client-certificate [bypass|inspect|...]
            set expired-server-cert [allow|block|...]
            set min-allowed-ssl-version [ssl-3.0|tls-1.0|...]
            set ports {integer}
            set revoked-server-cert [allow|block|...]
            set sni-server-cert-check [enable|strict|...]
            set status [disable|deep-inspection]
            set unsupported-ssl-cipher [allow|block]
            set unsupported-ssl-negotiation [allow|block]
            set unsupported-ssl-version [allow|block]
            set untrusted-server-cert [allow|block|...]
        end
        config https
            Description: Configure HTTPS options.
            set cert-probe-failure [allow|block]
            set cert-validation-failure [allow|block|...]
            set cert-validation-timeout [allow|block|...]
            set client-certificate [bypass|inspect|...]
            set encrypted-client-hello [allow|block]
            set expired-server-cert [allow|block|...]
            set min-allowed-ssl-version [ssl-3.0|tls-1.0|...]
            set ports {integer}
            set proxy-after-tcp-handshake [enable|disable]
            set quic [inspect|bypass|...]
            set revoked-server-cert [allow|block|...]
            set sni-server-cert-check [enable|strict|...]
            set status [disable|certificate-inspection|...]
            set unsupported-ssl-cipher [allow|block]
            set unsupported-ssl-negotiation [allow|block]
            set unsupported-ssl-version [allow|block]
            set untrusted-server-cert [allow|block|...]
        end
        config imaps
            Description: Configure IMAPS options.
            set cert-validation-failure [allow|block|...]
            set cert-validation-timeout [allow|block|...]
            set client-certificate [bypass|inspect|...]
            set expired-server-cert [allow|block|...]
            set ports {integer}
            set proxy-after-tcp-handshake [enable|disable]
            set revoked-server-cert [allow|block|...]
            set sni-server-cert-check [enable|strict|...]
            set status [disable|deep-inspection]
            set unsupported-ssl-cipher [allow|block]
            set unsupported-ssl-negotiation [allow|block]
            set unsupported-ssl-version [allow|block]
            set untrusted-server-cert [allow|block|...]
        end
        set mapi-over-https [enable|disable]
        config pop3s
            Description: Configure POP3S options.
            set cert-validation-failure [allow|block|...]
            set cert-validation-timeout [allow|block|...]
            set client-certificate [bypass|inspect|...]
            set expired-server-cert [allow|block|...]
            set ports {integer}
            set proxy-after-tcp-handshake [enable|disable]
            set revoked-server-cert [allow|block|...]
            set sni-server-cert-check [enable|strict|...]
            set status [disable|deep-inspection]
            set unsupported-ssl-cipher [allow|block]
            set unsupported-ssl-negotiation [allow|block]
            set unsupported-ssl-version [allow|block]
            set untrusted-server-cert [allow|block|...]
        end
        set rpc-over-https [enable|disable]
        set server-cert <name1>, <name2>, ...
        set server-cert-mode [re-sign|replace]
        config smtps
            Description: Configure SMTPS options.
            set cert-validation-failure [allow|block|...]
            set cert-validation-timeout [allow|block|...]
            set client-certificate [bypass|inspect|...]
            set expired-server-cert [allow|block|...]
            set ports {integer}
            set proxy-after-tcp-handshake [enable|disable]
            set revoked-server-cert [allow|block|...]
            set sni-server-cert-check [enable|strict|...]
            set status [disable|deep-inspection]
            set unsupported-ssl-cipher [allow|block]
            set unsupported-ssl-negotiation [allow|block]
            set unsupported-ssl-version [allow|block]
            set untrusted-server-cert [allow|block|...]
        end
        config ssh
            Description: Configure SSH options.
            set inspect-all [disable|deep-inspection]
            set ports {integer}
            set proxy-after-tcp-handshake [enable|disable]
            set ssh-algorithm [compatible|high-encryption]
            set ssh-tun-policy-check [disable|enable]
            set status [disable|deep-inspection]
            set unsupported-version [bypass|block]
        end
        config ssl
            Description: Configure SSL options.
            set cert-probe-failure [allow|block]
            set cert-validation-failure [allow|block|...]
            set cert-validation-timeout [allow|block|...]
            set client-certificate [bypass|inspect|...]
            set encrypted-client-hello [allow|block]
            set expired-server-cert [allow|block|...]
            set inspect-all [disable|certificate-inspection|...]
            set min-allowed-ssl-version [ssl-3.0|tls-1.0|...]
            set revoked-server-cert [allow|block|...]
            set sni-server-cert-check [enable|strict|...]
            set unsupported-ssl-cipher [allow|block]
            set unsupported-ssl-negotiation [allow|block]
            set unsupported-ssl-version [allow|block]
            set untrusted-server-cert [allow|block|...]
        end
        set ssl-anomaly-log [disable|enable]
        config ssl-exempt
            Description: Servers to exempt from SSL inspection.
            edit <id>
                set address {string}
                set address6 {string}
                set fortiguard-category {integer}
                set regex {string}
                set type [fortiguard-category|address|...]
                set wildcard-fqdn {string}
            next
        end
        set ssl-exemption-ip-rating [enable|disable]
        set ssl-exemption-log [disable|enable]
        set ssl-handshake-log [disable|enable]
        set ssl-negotiation-log [disable|enable]
        config ssl-server
            Description: SSL server settings used for client certificate request.
            edit <id>
                set ftps-client-certificate [bypass|inspect|...]
                set https-client-certificate [bypass|inspect|...]
                set imaps-client-certificate [bypass|inspect|...]
                set ip {ipv4-address-any}
                set pop3s-client-certificate [bypass|inspect|...]
                set smtps-client-certificate [bypass|inspect|...]
                set ssl-other-client-certificate [bypass|inspect|...]
            next
        end
        set ssl-server-cert-log [disable|enable]
        set supported-alpn [http1-1|http2|...]
        set untrusted-caname {string}
        set use-ssl-server [disable|enable]
    next
end
                                            config firewall ssl-ssh-profile
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| allowlist | Enable/disable exempting servers by FortiGuard allowlist. | option | - | disable | 
|  |  |  |  |  | 
| block-blocklisted-certificates | Enable/disable blocking SSL-based botnet communication by FortiGuard certificate blocklist. | option | - | enable | 
|  |  |  |  |  | 
| caname | CA certificate used by SSL Inspection. | string | Maximum length: 35 | Fortinet_CA_SSL | 
| comment | Optional comments. | var-string | Maximum length: 255 |  | 
| mapi-over-https | Enable/disable inspection of MAPI over HTTPS. | option | - | disable | 
|  |  |  |  |  | 
| name | Name. | string | Maximum length: 35 |  | 
| rpc-over-https | Enable/disable inspection of RPC over HTTPS. | option | - | disable | 
|  |  |  |  |  | 
| server-cert <name> | Certificate used by SSL Inspection to replace server certificate. Certificate list. | string | Maximum length: 79 |  | 
| server-cert-mode | Re-sign or replace the server's certificate. | option | - | re-sign | 
|  |  |  |  |  | 
| ssl-anomaly-log | Enable/disable logging of SSL anomalies. | option | - | enable | 
|  |  |  |  |  | 
| ssl-exemption-ip-rating | Enable/disable IP based URL rating. | option | - | enable | 
|  |  |  |  |  | 
| ssl-exemption-log | Enable/disable logging of SSL exemptions. | option | - | disable | 
|  |  |  |  |  | 
| ssl-handshake-log | Enable/disable logging of TLS handshakes. | option | - | disable | 
|  |  |  |  |  | 
| ssl-negotiation-log | Enable/disable logging of SSL negotiation events. | option | - | enable | 
|  |  |  |  |  | 
| ssl-server-cert-log | Enable/disable logging of server certificate information. | option | - | disable | 
|  |  |  |  |  | 
| supported-alpn | Configure ALPN option. | option | - | all | 
|  |  |  |  |  | 
| untrusted-caname | Untrusted CA certificate used by SSL Inspection. | string | Maximum length: 35 | Fortinet_CA_Untrusted | 
| use-ssl-server | Enable/disable the use of SSL server table for SSL offloading. | option | - | disable | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| disable | Disable FortiGuard certificate blocklist. | 
| enable | Enable FortiGuard certificate blocklist. | 
| Option | Description | 
|---|---|
| enable | Enable inspection of MAPI over HTTPS. | 
| disable | Disable inspection of MAPI over HTTPS. | 
| Option | Description | 
|---|---|
| enable | Enable inspection of RPC over HTTPS. | 
| disable | Disable inspection of RPC over HTTPS. | 
| Option | Description | 
|---|---|
| re-sign | Multiple clients connecting to multiple servers. | 
| replace | Protect an SSL server. | 
| Option | Description | 
|---|---|
| disable | Disable logging of SSL anomalies. | 
| enable | Enable logging of SSL anomalies. | 
| Option | Description | 
|---|---|
| enable | Enable IP based URL rating. | 
| disable | Disable IP based URL rating. | 
| Option | Description | 
|---|---|
| disable | Disable logging of SSL exemptions. | 
| enable | Enable logging of SSL exemptions. | 
| Option | Description | 
|---|---|
| disable | Disable logging of TLS handshakes. | 
| enable | Enable logging of TLS handshakes. | 
| Option | Description | 
|---|---|
| disable | Disable logging of SSL negotiation events. | 
| enable | Enable logging of SSL negotiation events. | 
| Option | Description | 
|---|---|
| disable | Disable logging of server certificate information. | 
| enable | Enable logging of server certificate information. | 
| Option | Description | 
|---|---|
| http1-1 | Enable all ALPN including HTTP1.1 except HTTP2 and SPDY. | 
| http2 | Enable all ALPN including HTTP2 except HTTP1.1 and SPDY. | 
| all | Allow all ALPN extensions except SPDY. | 
| none | Do not use ALPN. | 
| Option | Description | 
|---|---|
| disable | Don't use SSL server configuration. | 
| enable | Use SSL server configuration. | 
config dot
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| cert-validation-failure | Action based on certificate validation failure. | option | - | block | 
|  |  |  |  |  | 
| cert-validation-timeout | Action based on certificate validation timeout. | option | - | allow | 
|  |  |  |  |  | 
| client-certificate | Action based on received client certificate. | option | - | bypass | 
|  |  |  |  |  | 
| expired-server-cert | Action based on server certificate is expired. | option | - | block | 
|  |  |  |  |  | 
| proxy-after-tcp-handshake | Proxy traffic after the TCP 3-way handshake has been established (not before). | option | - | disable | 
|  |  |  |  |  | 
| quic | QUIC inspection status. | option | - | inspect | 
|  |  |  |  |  | 
| revoked-server-cert | Action based on server certificate is revoked. | option | - | block | 
|  |  |  |  |  | 
| sni-server-cert-check | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | option | - | enable | 
|  |  |  |  |  | 
| status | Configure protocol inspection status. | option | - | disable | 
|  |  |  |  |  | 
| unsupported-ssl-cipher | Action based on the SSL cipher used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-negotiation | Action based on the SSL negotiation used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-version | Action based on the SSL version used being unsupported. | option | - | block | 
|  |  |  |  |  | 
| untrusted-server-cert | Action based on server certificate is not issued by a trusted CA. | option | - | allow | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| bypass | Bypass the session. | 
| inspect | Inspect the session. | 
| block | Block the session. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| inspect | Inspect QUIC traffic. | 
| bypass | Bypass QUIC traffic. | 
| block | Block QUIC traffic. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| enable | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, use the CN in the server certificate to do URL filtering. | 
| strict | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, close the connection. | 
| disable | Do not check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | 
| Option | Description | 
|---|---|
| disable | Disable. | 
| deep-inspection | Full SSL inspection. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the cipher is not supported. | 
| block | Block the session when the cipher is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the negotiation is not supported. | 
| block | Block the session when the negotiation is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the version is not supported. | 
| block | Block the session when the version is not supported. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
config ech-outer-sni
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| name | ClientHelloOuter SNI name. | string | Maximum length: 79 |  | 
| sni | ClientHelloOuter SNI to be blocked. | string | Maximum length: 255 |  | 
config ftps
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| cert-validation-failure | Action based on certificate validation failure. | option | - | block | 
|  |  |  |  |  | 
| cert-validation-timeout | Action based on certificate validation timeout. | option | - | allow | 
|  |  |  |  |  | 
| client-certificate | Action based on received client certificate. | option | - | bypass | 
|  |  |  |  |  | 
| expired-server-cert | Action based on server certificate is expired. | option | - | block | 
|  |  |  |  |  | 
| min-allowed-ssl-version | Minimum SSL version to be allowed. Flow-based inspection does not support SSL version control. | option | - | tls-1.1 | 
|  |  |  |  |  | 
| ports | Ports to use for scanning. | integer | Minimum value: 1 Maximum value: 65535 |  | 
| revoked-server-cert | Action based on server certificate is revoked. | option | - | block | 
|  |  |  |  |  | 
| sni-server-cert-check | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | option | - | enable | 
|  |  |  |  |  | 
| status | Configure protocol inspection status. | option | - | deep-inspection | 
|  |  |  |  |  | 
| unsupported-ssl-cipher | Action based on the SSL cipher used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-negotiation | Action based on the SSL negotiation used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-version | Action based on the SSL version used being unsupported. | option | - | block | 
|  |  |  |  |  | 
| untrusted-server-cert | Action based on server certificate is not issued by a trusted CA. | option | - | allow | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| bypass | Bypass the session. | 
| inspect | Inspect the session. | 
| block | Block the session. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| ssl-3.0 | SSL 3.0. | 
| tls-1.0 | TLS 1.0. | 
| tls-1.1 | TLS 1.1. | 
| tls-1.2 | TLS 1.2. | 
| tls-1.3 | TLS 1.3. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| enable | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, use the CN in the server certificate to do URL filtering. | 
| strict | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, close the connection. | 
| disable | Do not check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | 
| Option | Description | 
|---|---|
| disable | Disable. | 
| deep-inspection | Full SSL inspection. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the cipher is not supported. | 
| block | Block the session when the cipher is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the negotiation is not supported. | 
| block | Block the session when the negotiation is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the version is not supported. | 
| block | Block the session when the version is not supported. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
config https
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| cert-probe-failure | Action based on certificate probe failure. | option | - | block | 
|  |  |  |  |  | 
| cert-validation-failure | Action based on certificate validation failure. | option | - | block | 
|  |  |  |  |  | 
| cert-validation-timeout | Action based on certificate validation timeout. | option | - | allow | 
|  |  |  |  |  | 
| client-certificate | Action based on received client certificate. | option | - | bypass | 
|  |  |  |  |  | 
| encrypted-client-hello | Block/allow session based on existence of encrypted-client-hello. | option | - | block | 
|  |  |  |  |  | 
| expired-server-cert | Action based on server certificate is expired. | option | - | block | 
|  |  |  |  |  | 
| min-allowed-ssl-version | Minimum SSL version to be allowed. Flow-based inspection does not support SSL version control. | option | - | tls-1.1 | 
|  |  |  |  |  | 
| ports | Ports to use for scanning. | integer | Minimum value: 1 Maximum value: 65535 |  | 
| proxy-after-tcp-handshake | Proxy traffic after the TCP 3-way handshake has been established (not before). | option | - | disable | 
|  |  |  |  |  | 
| quic | QUIC inspection status. | option | - | inspect | 
|  |  |  |  |  | 
| revoked-server-cert | Action based on server certificate is revoked. | option | - | block | 
|  |  |  |  |  | 
| sni-server-cert-check | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | option | - | enable | 
|  |  |  |  |  | 
| status | Configure protocol inspection status. | option | - | deep-inspection | 
|  |  |  |  |  | 
| unsupported-ssl-cipher | Action based on the SSL cipher used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-negotiation | Action based on the SSL negotiation used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-version | Action based on the SSL version used being unsupported. | option | - | block | 
|  |  |  |  |  | 
| untrusted-server-cert | Action based on server certificate is not issued by a trusted CA. | option | - | allow | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| allow | Bypass the session when unable to retrieve server's certificate for inspection. | 
| block | Block the session when unable to retrieve server's certificate for inspection. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| bypass | Bypass the session. | 
| inspect | Inspect the session. | 
| block | Block the session. | 
| Option | Description | 
|---|---|
| allow | Pass the session when encrypted-client-hello exists. | 
| block | Block the session when encrypted-client-hello exists. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| ssl-3.0 | SSL 3.0. | 
| tls-1.0 | TLS 1.0. | 
| tls-1.1 | TLS 1.1. | 
| tls-1.2 | TLS 1.2. | 
| tls-1.3 | TLS 1.3. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| inspect | Inspect QUIC traffic. | 
| bypass | Bypass QUIC traffic. | 
| block | Block QUIC traffic. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| enable | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, use the CN in the server certificate to do URL filtering. | 
| strict | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, close the connection. | 
| disable | Do not check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | 
| Option | Description | 
|---|---|
| disable | Disable. | 
| certificate-inspection | Inspect SSL handshake only. | 
| deep-inspection | Full SSL inspection. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the cipher is not supported. | 
| block | Block the session when the cipher is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the negotiation is not supported. | 
| block | Block the session when the negotiation is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the version is not supported. | 
| block | Block the session when the version is not supported. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
config imaps
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| cert-validation-failure | Action based on certificate validation failure. | option | - | block | 
|  |  |  |  |  | 
| cert-validation-timeout | Action based on certificate validation timeout. | option | - | allow | 
|  |  |  |  |  | 
| client-certificate | Action based on received client certificate. | option | - | inspect | 
|  |  |  |  |  | 
| expired-server-cert | Action based on server certificate is expired. | option | - | block | 
|  |  |  |  |  | 
| ports | Ports to use for scanning. | integer | Minimum value: 1 Maximum value: 65535 |  | 
| proxy-after-tcp-handshake | Proxy traffic after the TCP 3-way handshake has been established (not before). | option | - | disable | 
|  |  |  |  |  | 
| revoked-server-cert | Action based on server certificate is revoked. | option | - | block | 
|  |  |  |  |  | 
| sni-server-cert-check | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | option | - | enable | 
|  |  |  |  |  | 
| status | Configure protocol inspection status. | option | - | deep-inspection | 
|  |  |  |  |  | 
| unsupported-ssl-cipher | Action based on the SSL cipher used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-negotiation | Action based on the SSL negotiation used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-version | Action based on the SSL version used being unsupported. | option | - | block | 
|  |  |  |  |  | 
| untrusted-server-cert | Action based on server certificate is not issued by a trusted CA. | option | - | allow | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| bypass | Bypass the session. | 
| inspect | Inspect the session. | 
| block | Block the session. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| enable | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, use the CN in the server certificate to do URL filtering. | 
| strict | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, close the connection. | 
| disable | Do not check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | 
| Option | Description | 
|---|---|
| disable | Disable. | 
| deep-inspection | Full SSL inspection. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the cipher is not supported. | 
| block | Block the session when the cipher is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the negotiation is not supported. | 
| block | Block the session when the negotiation is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the version is not supported. | 
| block | Block the session when the version is not supported. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
config pop3s
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| cert-validation-failure | Action based on certificate validation failure. | option | - | block | 
|  |  |  |  |  | 
| cert-validation-timeout | Action based on certificate validation timeout. | option | - | allow | 
|  |  |  |  |  | 
| client-certificate | Action based on received client certificate. | option | - | inspect | 
|  |  |  |  |  | 
| expired-server-cert | Action based on server certificate is expired. | option | - | block | 
|  |  |  |  |  | 
| ports | Ports to use for scanning. | integer | Minimum value: 1 Maximum value: 65535 |  | 
| proxy-after-tcp-handshake | Proxy traffic after the TCP 3-way handshake has been established (not before). | option | - | disable | 
|  |  |  |  |  | 
| revoked-server-cert | Action based on server certificate is revoked. | option | - | block | 
|  |  |  |  |  | 
| sni-server-cert-check | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | option | - | enable | 
|  |  |  |  |  | 
| status | Configure protocol inspection status. | option | - | deep-inspection | 
|  |  |  |  |  | 
| unsupported-ssl-cipher | Action based on the SSL cipher used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-negotiation | Action based on the SSL negotiation used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-version | Action based on the SSL version used being unsupported. | option | - | block | 
|  |  |  |  |  | 
| untrusted-server-cert | Action based on server certificate is not issued by a trusted CA. | option | - | allow | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| bypass | Bypass the session. | 
| inspect | Inspect the session. | 
| block | Block the session. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| enable | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, use the CN in the server certificate to do URL filtering. | 
| strict | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, close the connection. | 
| disable | Do not check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | 
| Option | Description | 
|---|---|
| disable | Disable. | 
| deep-inspection | Full SSL inspection. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the cipher is not supported. | 
| block | Block the session when the cipher is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the negotiation is not supported. | 
| block | Block the session when the negotiation is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the version is not supported. | 
| block | Block the session when the version is not supported. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
config smtps
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| cert-validation-failure | Action based on certificate validation failure. | option | - | block | 
|  |  |  |  |  | 
| cert-validation-timeout | Action based on certificate validation timeout. | option | - | allow | 
|  |  |  |  |  | 
| client-certificate | Action based on received client certificate. | option | - | inspect | 
|  |  |  |  |  | 
| expired-server-cert | Action based on server certificate is expired. | option | - | block | 
|  |  |  |  |  | 
| ports | Ports to use for scanning. | integer | Minimum value: 1 Maximum value: 65535 |  | 
| proxy-after-tcp-handshake | Proxy traffic after the TCP 3-way handshake has been established (not before). | option | - | disable | 
|  |  |  |  |  | 
| revoked-server-cert | Action based on server certificate is revoked. | option | - | block | 
|  |  |  |  |  | 
| sni-server-cert-check | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | option | - | enable | 
|  |  |  |  |  | 
| status | Configure protocol inspection status. | option | - | deep-inspection | 
|  |  |  |  |  | 
| unsupported-ssl-cipher | Action based on the SSL cipher used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-negotiation | Action based on the SSL negotiation used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-version | Action based on the SSL version used being unsupported. | option | - | block | 
|  |  |  |  |  | 
| untrusted-server-cert | Action based on server certificate is not issued by a trusted CA. | option | - | allow | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| bypass | Bypass the session. | 
| inspect | Inspect the session. | 
| block | Block the session. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| enable | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, use the CN in the server certificate to do URL filtering. | 
| strict | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, close the connection. | 
| disable | Do not check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | 
| Option | Description | 
|---|---|
| disable | Disable. | 
| deep-inspection | Full SSL inspection. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the cipher is not supported. | 
| block | Block the session when the cipher is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the negotiation is not supported. | 
| block | Block the session when the negotiation is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the version is not supported. | 
| block | Block the session when the version is not supported. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
config ssh
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| inspect-all | Level of SSL inspection. | option | - | disable | 
|  |  |  |  |  | 
| ports | Ports to use for scanning. | integer | Minimum value: 1 Maximum value: 65535 |  | 
| proxy-after-tcp-handshake | Proxy traffic after the TCP 3-way handshake has been established (not before). | option | - | disable | 
|  |  |  |  |  | 
| ssh-algorithm | Relative strength of encryption algorithms accepted during negotiation. | option | - | compatible | 
|  |  |  |  |  | 
| ssh-tun-policy-check | Enable/disable SSH tunnel policy check. | option | - | disable | 
|  |  |  |  |  | 
| status | Configure protocol inspection status. | option | - | disable | 
|  |  |  |  |  | 
| unsupported-version | Action based on SSH version being unsupported. | option | - | bypass | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| disable | Disable. | 
| deep-inspection | Full SSL inspection. | 
| Option | Description | 
|---|---|
| enable | Enable setting. | 
| disable | Disable setting. | 
| Option | Description | 
|---|---|
| compatible | Allow a broader set of encryption algorithms for best compatibility. | 
| high-encryption | Allow only AES-CTR, AES-GCM ciphers and high encryption algorithms. | 
| Option | Description | 
|---|---|
| disable | Disable SSH tunnel policy check. | 
| enable | Enable SSH tunnel policy check. | 
| Option | Description | 
|---|---|
| disable | Disable. | 
| deep-inspection | Full SSL inspection. | 
| Option | Description | 
|---|---|
| bypass | Bypass the session. | 
| block | Block the session. | 
config ssl
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| cert-probe-failure | Action based on certificate probe failure. | option | - | block | 
|  |  |  |  |  | 
| cert-validation-failure | Action based on certificate validation failure. | option | - | block | 
|  |  |  |  |  | 
| cert-validation-timeout | Action based on certificate validation timeout. | option | - | allow | 
|  |  |  |  |  | 
| client-certificate | Action based on received client certificate. | option | - | bypass | 
|  |  |  |  |  | 
| encrypted-client-hello | Block/allow session based on existence of encrypted-client-hello. | option | - | block | 
|  |  |  |  |  | 
| expired-server-cert | Action based on server certificate is expired. | option | - | block | 
|  |  |  |  |  | 
| inspect-all | Level of SSL inspection. | option | - | disable | 
|  |  |  |  |  | 
| min-allowed-ssl-version | Minimum SSL version to be allowed. Flow-based inspection does not support SSL version control. | option | - | tls-1.1 | 
|  |  |  |  |  | 
| revoked-server-cert | Action based on server certificate is revoked. | option | - | block | 
|  |  |  |  |  | 
| sni-server-cert-check | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | option | - | enable | 
|  |  |  |  |  | 
| unsupported-ssl-cipher | Action based on the SSL cipher used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-negotiation | Action based on the SSL negotiation used being unsupported. | option | - | allow | 
|  |  |  |  |  | 
| unsupported-ssl-version | Action based on the SSL version used being unsupported. | option | - | block | 
|  |  |  |  |  | 
| untrusted-server-cert | Action based on server certificate is not issued by a trusted CA. | option | - | allow | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| allow | Bypass the session when unable to retrieve server's certificate for inspection. | 
| block | Block the session when unable to retrieve server's certificate for inspection. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| bypass | Bypass the session. | 
| inspect | Inspect the session. | 
| block | Block the session. | 
| Option | Description | 
|---|---|
| allow | Pass the session when encrypted-client-hello exists. | 
| block | Block the session when encrypted-client-hello exists. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| disable | Disable. | 
| certificate-inspection | Inspect SSL handshake only. | 
| deep-inspection | Full SSL inspection. | 
| Option | Description | 
|---|---|
| ssl-3.0 | SSL 3.0. | 
| tls-1.0 | TLS 1.0. | 
| tls-1.1 | TLS 1.1. | 
| tls-1.2 | TLS 1.2. | 
| tls-1.3 | TLS 1.3. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
| Option | Description | 
|---|---|
| enable | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, use the CN in the server certificate to do URL filtering. | 
| strict | Check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. If mismatched, close the connection. | 
| disable | Do not check the SNI in the client hello message with the CN or SAN fields in the returned server certificate. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the cipher is not supported. | 
| block | Block the session when the cipher is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the negotiation is not supported. | 
| block | Block the session when the negotiation is not supported. | 
| Option | Description | 
|---|---|
| allow | Bypass the session when the version is not supported. | 
| block | Block the session when the version is not supported. | 
| Option | Description | 
|---|---|
| allow | Allow the server certificate. | 
| block | Block the session. | 
| ignore | Re-sign the server certificate as trusted. | 
config ssl-exempt
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| address | IPv4 address object. | string | Maximum length: 79 |  | 
| address6 | IPv6 address object. | string | Maximum length: 79 |  | 
| fortiguard-category | FortiGuard category ID. | integer | Minimum value: 0 Maximum value: 255 | 0 | 
| id | ID number. | integer | Minimum value: 0 Maximum value: 512 | 0 | 
| regex | Exempt servers by regular expression. | string | Maximum length: 255 |  | 
| type | Type of address object (IPv4 or IPv6) or FortiGuard category. | option | - | fortiguard-category | 
|  |  |  |  |  | 
| wildcard-fqdn | Exempt servers by wildcard FQDN. | string | Maximum length: 79 |  | 
| Option | Description | 
|---|---|
| fortiguard-category | FortiGuard category. | 
| address | Firewall IPv4 address. | 
| address6 | Firewall IPv6 address. | 
| wildcard-fqdn | Fully Qualified Domain Name with wildcard characters. | 
| regex | Regular expression FQDN. | 
config ssl-server
| Parameter | Description | Type | Size | Default | 
|---|---|---|---|---|
| ftps-client-certificate | Action based on received client certificate during the FTPS handshake. | option | - | bypass | 
|  |  |  |  |  | 
| https-client-certificate | Action based on received client certificate during the HTTPS handshake. | option | - | bypass | 
|  |  |  |  |  | 
| id | SSL server ID. | integer | Minimum value: 0 Maximum value: 4294967295 | 0 | 
| imaps-client-certificate | Action based on received client certificate during the IMAPS handshake. | option | - | bypass | 
|  |  |  |  |  | 
| ip | IPv4 address of the SSL server. | ipv4-address-any | Not Specified | 0.0.0.0 | 
| pop3s-client-certificate | Action based on received client certificate during the POP3S handshake. | option | - | bypass | 
|  |  |  |  |  | 
| smtps-client-certificate | Action based on received client certificate during the SMTPS handshake. | option | - | bypass | 
|  |  |  |  |  | 
| ssl-other-client-certificate | Action based on received client certificate during an SSL protocol handshake. | option | - | bypass | 
|  |  |  |  |  | 
| Option | Description | 
|---|---|
| bypass | Bypass the session. | 
| inspect | Inspect the session. | 
| block | Block the session. |