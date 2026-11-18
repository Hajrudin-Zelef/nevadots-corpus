# Huawei USG — Guide CLI complet (USG6000 series, V500R005 / V600)

> Synthèse rédigée à partir de la documentation officielle Huawei (Command
> Reference & Configuration Guide USG6000). Couvre l'essentiel du CLI VRP
> appliqué aux pare-feu USG : installation, zones, politiques, NAT, VPN,
> UTM, HA, supervision et maintenance. Vérifiez les variantes selon votre
> version logicielle exacte (`display version`).

---

## 1. Gammes et concepts

- **USG6300 / 6500 / 6600 / 6700** : pare-feu « NGFW » (Next-Generation
  Firewall), OS = VRP (même base que routeurs/switches Huawei).
- Le concept central du USG est la **zone de sécurité** : tout le trafic
  est contrôlé *entre zones*, jamais directement entre interfaces.
- Zones prédéfinies : `local` (100), `trust` (85), `dmz` (50),
  `untrust` (5). Le chiffre = priorité (plus haut = plus de confiance).
- Par défaut, **tout trafic interzone est refusé** : il faut des règles
  explicites pour autoriser.
- Deux moteurs de politique coexistent :
  - `security-policy` (mode NGFW moderne, recommandé) ;
  - `policy interzone` (mode legacy).
- Les profils UTM (IPS, antivirus, filtrage URL…) s'appliquent **dans**
  les règles `security-policy` via `profile`.

---

## 2. Premier accès

- Console : 9600 bauds, 8N1. Identifiants par défaut :
  `admin` / `Admin@123` (changement obligatoire à la première connexion).
- Interface de management : `GigabitEthernet0/0/0` = **192.168.0.1/24**.
- Web UI : `https://192.168.0.1:8443` (activée par défaut).
- SSH activé après configuration (voir §13).

```shell
# Passer en mode configuration
system-view
[USG] sysname USG-SIEGE

# Voir la version et le matériel
display version
display device
display device fan
display device power
```

---

## 3. Bases du CLI VRP

```shell
system-view              # user view <USG> -> system view [USG]
quit                     # remonte d'un niveau
return                   # retour direct en user view (Ctrl+Z)
display current-configuration   # config active
display saved-configuration     # config sauvegardée
save                     # sauvegarde (toujours après modifs !)
compare configuration    # diff entre active et sauvegardée
display this             # config du niveau courant
?                        # aide contextuelle partout
display history-command   # historique
```

- `display` = afficher (jamais `show`), `undo` = annuler une commande.
- Les commandes s'abrègent : `dis cur`, `sys`, etc.
- Niveaux de commande : 0 (visite) à 3 (management) ; `super` pour monter.

---

## 4. Interfaces

```shell
system-view
[USG] interface GigabitEthernet 0/0/1
[USG-GigabitEthernet0/0/1] ip address 192.168.1.1 24
[USG-GigabitEthernet0/0/1] description LAN_SIEGE
[USG-GigabitEthernet0/0/1] quit

# Sous-interface VLAN (trunk 802.1Q)
[USG] interface GigabitEthernet 0/0/1.10
[USG-GigabitEthernet0/0/1.10] vlan-type dot1q 10
[USG-GigabitEthernet0/0/1.10] ip address 192.168.10.1 24
[USG-GigabitEthernet0/0/1.10] quit

# Agrégat de liens
[USG] interface Eth-Trunk 1
[USG-Eth-Trunk1] mode lacp-static
[USG-Eth-Trunk1] trunkport GigabitEthernet 0/0/3 to 0/0/4
[USG-Eth-Trunk1] ip address 10.0.0.1 30
[USG-Eth-Trunk1] quit

# Vérifications
display interface brief
display ip interface brief
display interface GigabitEthernet 0/0/1
```

### PPPoE (fibre/ADSL grand public)

```shell
[USG] interface Dialer 1
[USG-Dialer1] dialer user u1
[USG-Dialer1] dialer-group 1
[USG-Dialer1] dialer bundle 1
[USG-Dialer1] ppp chap user monlogin password cipher MonPass
[USG-Dialer1] ip address ppp-negotiate
[USG-Dialer1] quit
[USG] interface GigabitEthernet 0/0/2
[USG-GigabitEthernet0/0/2] pppoe-client dial-bundle-number 1
[USG-GigabitEthernet0/0/2] quit
[USG] dialer-rule
[USG-dialer-rule] dialer-rule 1 ip permit
```

---

## 5. Zones de sécurité

```shell
system-view
# Zones prédéfinies : ajouter les interfaces
[USG] firewall zone trust
[USG-zone-trust] set priority 85
[USG-zone-trust] add interface GigabitEthernet 0/0/1
[USG-zone-trust] quit

[USG] firewall zone untrust
[USG-zone-untrust] set priority 5
[USG-zone-untrust] add interface GigabitEthernet 0/0/2
[USG-zone-untrust] quit

[USG] firewall zone dmz
[USG-zone-dmz] set priority 50
[USG-zone-dmz] add interface GigabitEthernet 0/0/3
[USG-zone-dmz] quit

# Zone personnalisée (ex : invités)
[USG] firewall zone name GUEST
[USG-zone-GUEST] set priority 60
[USG-zone-GUEST] add interface GigabitEthernet 0/0/1.20
[USG-zone-GUEST] quit

display zone
display firewall zone trust
```

- La zone `local` = le pare-feu lui-même (ping, SSH, web vers le USG).
- Une interface ne peut appartenir qu'à **une seule** zone.

---

## 6. Security-policy (cœur du USG)

```shell
system-view
[USG] security-policy
[USG-policy-security] rule name LAN_TO_WAN
[USG-policy-security-rule-LAN_TO_WAN] source-zone trust
[USG-policy-security-rule-LAN_TO_WAN] destination-zone untrust
[USG-policy-security-rule-LAN_TO_WAN] source-address 192.168.1.0 24
[USG-policy-security-rule-LAN_TO_WAN] destination-address any
[USG-policy-security-rule-LAN_TO_WAN] action permit
[USG-policy-security-rule-LAN_TO_WAN] quit

# Autoriser le ping / l'admin vers le pare-feu lui-même
[USG-policy-security] rule name ADMIN_TO_FW
[USG-policy-security-rule-ADMIN_TO_FW] source-zone trust
[USG-policy-security-rule-ADMIN_TO_FW] destination-zone local
[USG-policy-security-rule-ADMIN_TO_FW] service ping ssh https
[USG-policy-security-rule-ADMIN_TO_FW] action permit
[USG-policy-security-rule-ADMIN_TO_FW] quit
[USG-policy-security] quit

# Vérifications
display security-policy rule all
display security-policy rule name LAN_TO_WAN
```

- Les règles se lisent **dans l'ordre** ; la première qui matche gagne.
- `action deny` = refuser (loggé si `logging` activé sur la règle).
- Sans règle correspondante : trafic **refusé** par défaut.
- Le trafic *retour* d'une session autorisée est automatiquement accepté
  (pare-feu stateful) : inutile de créer la règle inverse.

---

## 7. Objets : adresses, services, plannings

```shell
system-view
# Groupe d'adresses
[USG] ip address-set SERVEURS_DMZ
[USG-address-set-SERVEURS_DMZ] address 0 192.168.50.0 mask 24
[USG-address-set-SERVEURS_DMZ] quit

# Groupe de services
[USG] ip service-set WEB
[USG-service-set-WEB] service 0 protocol tcp destination-port 80
[USG-service-set-WEB] service 1 protocol tcp destination-port 443
[USG-service-set-WEB] quit

# Plage horaire (ex : heures ouvrées)
[USG] time-range BUREAU 08:00 to 18:00 daily

# Utilisation dans une règle
[USG] security-policy
[USG-policy-security] rule name WAN_TO_DMZ_WEB
[USG-policy-security-rule-WAN_TO_DMZ_WEB] source-zone untrust
[USG-policy-security-rule-WAN_TO_DMZ_WEB] destination-zone dmz
[USG-policy-security-rule-WAN_TO_DMZ_WEB] destination-address address-set SERVEURS_DMZ
[USG-policy-security-rule-WAN_TO_DMZ_WEB] service service-set WEB
[USG-policy-security-rule-WAN_TO_DMZ_WEB] schedule time-range BUREAU
[USG-policy-security-rule-WAN_TO_DMZ_WEB] action permit
```

---

## 8. NAT source (accès Internet)

```shell
system-view
# Pool d'adresses publiques (PAT)
[USG] nat address-group SNAT_POOL
[USG-address-group-SNAT_POOL] mode pat
[USG-address-group-SNAT_POOL] section 0 202.10.10.10 202.10.10.20
[USG-address-group-SNAT_POOL] quit

[USG] nat-policy
[USG-policy-nat] rule name SNAT_LAN
[USG-policy-nat-rule-SNAT_LAN] source-zone trust
[USG-policy-nat-rule-SNAT_LAN] destination-zone untrust
[USG-policy-nat-rule-SNAT_LAN] source-address 192.168.1.0 24
[USG-policy-nat-rule-SNAT_LAN] action source-nat address-group SNAT_POOL
[USG-policy-nat-rule-SNAT_LAN] quit
[USG-policy-nat] quit

# Variante Easy-IP : utilise l'IP de l'interface de sortie
[USG-policy-nat] rule name SNAT_EASY
[USG-policy-nat-rule-SNAT_EASY] source-zone trust
[USG-policy-nat-rule-SNAT_EASY] destination-zone untrust
[USG-policy-nat-rule-SNAT_EASY] source-address 192.168.1.0 24
[USG-policy-nat-rule-SNAT_EASY] action source-nat easy-ip
```

- Le NAT se configure **entre zones**, comme la security-policy.
- Vérifier : `display nat-policy rule all`, `display nat session all`.

---

## 9. NAT server (publication de serveurs)

```shell
system-view
# Publie le serveur interne 192.168.50.10:80 sur l'IP publique :8080
[USG] nat server protocol tcp global 202.10.10.10 8080 inside 192.168.50.10 80

# Ne pas oublier la règle security-policy untrust -> dmz (voir §6/§7) !

# Vérifications
display nat server
display firewall session table destination-ip 192.168.50.10
```

- `nat server` sans nom = global ; forme nommée possible avec zones.
- Pensez au *hairpin* : pour accès interne via l'IP publique, ajouter une
  règle NAT adaptée ou utiliser le DNS interne (split-DNS).

---

## 10. Routage

```shell
system-view
# Route statique par défaut
[USG] ip route-static 0.0.0.0 0.0.0.0 202.10.10.1

# OSPF
[USG] ospf 1
[USG-ospf-1] area 0
[USG-ospf-1-area-0.0.0.0] network 192.168.1.0 0.0.0.255
[USG-ospf-1-area-0.0.0.0] quit
[USG-ospf-1] quit

# BGP simple
[USG] bgp 65001
[USG-bgp] peer 10.0.0.2 as-number 65002
[USG-bgp] network 192.168.1.0 24
[USG-bgp] quit

# Routage par politique (PBR)
[USG] policy-based-route
[USG-policy-pbr] rule name PBR1
[USG-policy-pbr-rule-PBR1] ingress-interface GigabitEthernet 0/0/1
[USG-policy-pbr-rule-PBR1] action next-hop 10.0.0.254
```

- Vérifier : `display ip routing-table`, `display ospf peer`,
  `display bgp peer`.

---

## 11. DHCP, DNS, NTP

```shell
system-view
[USG] dhcp enable
[USG] ip pool LAN_POOL
[USG-ip-pool-LAN_POOL] network 192.168.1.0 mask 24
[USG-ip-pool-LAN_POOL] gateway-list 192.168.1.1
[USG-ip-pool-LAN_POOL] dns-list 8.8.8.8 1.1.1.1
[USG-ip-pool-LAN_POOL] lease day 3 hour 0 minute 0
[USG-ip-pool-LAN_POOL] quit
[USG] interface GigabitEthernet 0/0/1
[USG-GigabitEthernet0/0/1] dhcp select global
[USG-GigabitEthernet0/0/1] quit

[USG] dns resolve
[USG] dns server 8.8.8.8
[USG] ntp-service unicast-server 192.168.1.100

display ip pool name LAN_POOL
```

---

## 12. AAA et comptes admin

```shell
system-view
[USG] aaa
[USG-aaa] local-user operateur password irreversible-cipher Huawei@123
[USG-aaa] local-user operateur privilege level 1
[USG-aaa] local-user operateur service-type ssh web
[USG-aaa] local-user admin2 password irreversible-cipher Huawei@123
[USG-aaa] local-user admin2 privilege level 3
[USG-aaa] local-user admin2 service-type ssh web
[USG-aaa] quit

# Voir les utilisateurs en ligne
display access-user
display local-user
```

- Niveau 3 = administrateur complet, 0 = visite.
- Pour l'auth 802.1X / portail captif : `authentication-scheme`,
  `portal` (souvent configuré via web UI).

---

## 13. SSH et accès distant sécurisé

```shell
system-view
[USG] stelnet server enable
[USG] ssh user admin
[USG] ssh user admin authentication-type password
[USG] ssh user admin service-type stelnet
[USG] ssh server port 2222   # optionnel : changer le port
[USG] user-interface vty 0 4
[USG-ui-vty0-4] authentication-mode aaa
[USG-ui-vty0-4] protocol inbound ssh
[USG-ui-vty0-4] quit
```

---

## 14. VPN IPsec site-à-site

```shell
system-view
# 1. ACL = trafic à chiffrer
[USG] acl number 3000
[USG-acl-adv-3000] rule 5 permit ip source 192.168.1.0 0.0.0.255 destination 192.168.2.0 0.0.0.255
[USG-acl-adv-3000] quit

# 2. Phase 1 (IKE)
[USG] ike proposal 10
[USG-ike-proposal-10] encryption-algorithm aes-256
[USG-ike-proposal-10] dh group14
[USG-ike-proposal-10] authentication-algorithm sha2-256
[USG-ike-proposal-10] quit
[USG] ike peer PEER_PARIS
[USG-ike-peer-PEER_PARIS] pre-shared-key Huawei@123
[USG-ike-peer-PEER_PARIS] ike-proposal 10
[USG-ike-peer-PEER_PARIS] remote-address 202.20.20.2
[USG-ike-peer-PEER_PARIS] quit

# 3. Phase 2 (IPsec)
[USG] ipsec proposal IPSEC1
[USG-ipsec-proposal-IPSEC1] esp authentication-algorithm sha2-256
[USG-ipsec-proposal-IPSEC1] esp encryption-algorithm aes-256
[USG-ipsec-proposal-IPSEC1] quit
[USG] ipsec policy POLICY1 10 isakmp
[USG-ipsec-policy-isakmp-POLICY1-10] security acl 3000
[USG-ipsec-policy-isakmp-POLICY1-10] ike-peer PEER_PARIS
[USG-ipsec-policy-isakmp-POLICY1-10] proposal IPSEC1
[USG-ipsec-policy-isakmp-POLICY1-10] quit

# 4. Application sur l'interface WAN
[USG] interface GigabitEthernet 0/0/2
[USG-GigabitEthernet0/0/2] ipsec policy POLICY1
[USG-GigabitEthernet0/0/2] quit

# Ne pas oublier : security-policy local/untrust pour IKE (UDP 500/4500)
# et ESP entre les peers !

# Vérifications
display ike sa
display ipsec sa
display ike proposal
```

- Pour du *route-based* : utilisez `ipsec policy-template` + interface
  Tunnel avec `tunnel-protocol ipsec`.
- NAT-T automatique si un NAT est détecté sur le chemin.

---

## 15. GRE, L2TP, SSL VPN

```shell
# GRE simple
system-view
[USG] interface Tunnel 0/0/1
[USG-Tunnel0/0/1] tunnel-protocol gre
[USG-Tunnel0/0/1] source 202.10.10.10
[USG-Tunnel0/0/1] destination 202.20.20.2
[USG-Tunnel0/0/1] ip address 172.16.0.1 30
[USG-Tunnel0/0/1] quit
```

- **L2TP** (accès distant) : serveur L2TP + groupe d'adresses + AAA ;
  configuration typique via l'assistant web UI.
- **SSL VPN** : passerelle virtuelle (Web proxy, partage de fichiers,
  redirection de ports, extension réseau). Se configure essentiellement
  via la web UI (`SSL VPN > Gateway`), avec certificats et utilisateurs
  AAA. Idéal pour le télétravail sans client lourd.

---

## 16. UTM : IPS, antivirus, filtrage URL

```shell
system-view
# Profils (les contenus exacts se règlent en web UI)
[USG] profile type ips name IPS_STRICT
[USG-profile-ips-IPS_STRICT] quit
[USG] profile type av name AV_DEFAULT
[USG-profile-av-AV_DEFAULT] quit
[USG] profile type url-filter name URL_DEFAULT
[USG-profile-url-filter-URL_DEFAULT] quit

# Application dans une règle security-policy
[USG] security-policy
[USG-policy-security] rule name LAN_TO_WAN
[USG-policy-security-rule-LAN_TO_WAN] profile ips IPS_STRICT
[USG-policy-security-rule-LAN_TO_WAN] profile av AV_DEFAULT
[USG-policy-security-rule-LAN_TO_WAN] profile url-filter URL_DEFAULT
[USG-policy-security-rule-LAN_TO_WAN] quit
[USG-policy-security] quit

display utm profile all
display ips signature-database
```

- Autres moteurs : `file-blocking`, `content-filter` (mots-clés),
  `antispam`, `apt` (sandbox), `dns-filter`.
- Les bases de signatures se mettent à jour via l'*Update Center*
  (web UI) ou en planifié ; licence UTM requise.
- Pour HTTPS : inspection SSL (certificat CA à déployer) via
  `ssl-encrypted-traffic-detection`.

---

## 17. Défense anti-attaque et listes

```shell
system-view
# Listes noires / blanches
[USG] firewall blacklist 203.0.113.5
[USG] firewall whitelist 198.51.100.10
display firewall blacklist
display firewall whitelist

# Protections : flood SYN/UDP/ICMP, IP sweep, port scan...
# (réglages fins en web UI : Security > Attack Defense)
```

- Le USG protège aussi le plan de contrôle (zone `local`) contre les
  floods par défaut (CPCAR).
- Journalisez les attaques : `display firewall defend statistics`.

---

## 18. QoS / gestion de bande passante

```shell
system-view
# Politique de trafic : classifier + behavior
[USG] traffic classifier VOIP
[USG-classifier-VOIP] if-match dscp ef
[USG-classifier-VOIP] quit
[USG] traffic behavior PRIORITAIRE
[USG-behavior-PRIORITAIRE] queue af bandwidth pct 30
[USG-behavior-PRIORITAIRE] quit
[USG] traffic policy QOS1
[USG-trafficpolicy-QOS1] classifier VOIP behavior PRIORITAIRE
[USG-trafficpolicy-QOS1] quit

# Limitation de débit par règle (bandwidth) : voir web UI
# Security Policy > règle > Bandwidth
```

---

## 19. Haute disponibilité (HRP / VRRP)

```shell
system-view
# Hot Standby (actif/passif) entre deux USG
[USG] hrp enable
[USG] hrp interface GigabitEthernet 0/0/5 remote 10.10.10.2
[USG] hrp auto-sync config       # synchro auto de la config
[USG] hrp preempt                # préemption du primaire

# VRRP sur une interface (VIP partagée)
[USG] interface GigabitEthernet 0/0/1
[USG-GigabitEthernet0/0/1] vrrp vrid 1 virtual-ip 192.168.1.254
[USG-GigabitEthernet0/0/1] vrrp vrid 1 priority 120
[USG-GigabitEthernet0/0/1] vrrp vrid 1 preempt-mode timer delay 60
[USG-GigabitEthernet0/0/1] quit

display hrp state verbose
display vrrp
```

- Le lien heartbeat HRP doit être direct et dédié.
- VGMP : les groupes VRRP basculent ensemble avec le HRP.

---

## 20. Logs et supervision

```shell
# Journalisation locale
display logbuffer
display trapbuffer
display logbuffer level warnings

# Envoi vers un syslog distant
system-view
[USG] info-center enable
[USG] info-center loghost 192.168.1.100
[USG] info-center source default channel 2 log state on

# Sessions en cours (très utile en dépannage)
display firewall session table
display firewall session table source-ip 192.168.1.50
display firewall statistics

# SNMP
[USG] snmp-agent
[USG] snmp-agent community read cipher Public@123
[USG] snmp-agent sys-info version v2c
[USG] snmp-agent target-host trap address udp-domain 192.168.1.100 params securityname public v2c
```

- Les logs de sessions/politiques se configurent par règle
  (`session-logging` dans la règle security-policy).
- NetStream disponible pour l'analyse de flux (`ip netstream`).

---

## 21. Diagnostic

```shell
display version
display device
display cpu-usage
display memory-usage
display fan
display power
display interface brief
display ip routing-table
display arp all
display mac-address
display security-policy rule all
display nat-policy rule all
display ike sa
display ipsec sa
display hrp state verbose
ping -a 192.168.1.1 8.8.8.8
tracert 8.8.8.8

# Debug temps réel (attention en production !)
terminal monitor
terminal debugging
debugging ip packet
undo debugging all
```

---

## 22. Maintenance : fichiers, upgrade, licence

```shell
dir                         # contenu flash:
tftp 192.168.1.100 get USG6600-V600R007.bin
startup system-software flash:/USG6600-V600R007.bin
display startup
reboot                      # redémarre (confirmer)

# Sauvegarde / restauration de config
save
display saved-configuration
reset saved-configuration   # efface la config sauvegardée
# puis reboot -> retour config d'usine

# Licence
display license
# activation via web UI (fichier .dat) ou commande license active

# Mot de passe console perdu : BootWare au démarrage (Ctrl+B),
# menu « Clear password for console user ».
```

---

## 23. Exemple complet : USG en passerelle d'entreprise

```shell
system-view
[USG] sysname USG-SIEGE
# --- Interfaces & zones
[USG] interface GigabitEthernet 0/0/1
[USG-GigabitEthernet0/0/1] ip address 192.168.1.1 24
[USG-GigabitEthernet0/0/1] quit
[USG] interface GigabitEthernet 0/0/2
[USG-GigabitEthernet0/0/2] ip address 202.10.10.10 30
[USG-GigabitEthernet0/0/2] quit
[USG] firewall zone trust
[USG-zone-trust] add interface GigabitEthernet 0/0/1
[USG-zone-trust] quit
[USG] firewall zone untrust
[USG-zone-untrust] add interface GigabitEthernet 0/0/2
[USG-zone-untrust] quit
# --- Routage & DNS
[USG] ip route-static 0.0.0.0 0.0.0.0 202.10.10.9
[USG] dns resolve
[USG] dns server 8.8.8.8
# --- NAT Internet
[USG] nat-policy
[USG-policy-nat] rule name SNAT
[USG-policy-nat-rule-SNAT] source-zone trust
[USG-policy-nat-rule-SNAT] destination-zone untrust
[USG-policy-nat-rule-SNAT] source-address 192.168.1.0 24
[USG-policy-nat-rule-SNAT] action source-nat easy-ip
[USG-policy-nat-rule-SNAT] quit
[USG-policy-nat] quit
# --- Politique : LAN->WAN + UTM, admin vers local
[USG] security-policy
[USG-policy-security] rule name LAN_TO_WAN
[USG-policy-security-rule-LAN_TO_WAN] source-zone trust
[USG-policy-security-rule-LAN_TO_WAN] destination-zone untrust
[USG-policy-security-rule-LAN_TO_WAN] source-address 192.168.1.0 24
[USG-policy-security-rule-LAN_TO_WAN] profile av AV_DEFAULT
[USG-policy-security-rule-LAN_TO_WAN] action permit
[USG-policy-security-rule-LAN_TO_WAN] quit
[USG-policy-security] rule name ADMIN_TO_FW
[USG-policy-security-rule-ADMIN_TO_FW] source-zone trust
[USG-policy-security-rule-ADMIN_TO_FW] destination-zone local
[USG-policy-security-rule-ADMIN_TO_FW] action permit
[USG-policy-security-rule-ADMIN_TO_FW] quit
[USG-policy-security] quit
[USG] quit
save
```

---

*Fin du guide — ~400 lignes. Pour aller plus loin : les Command Reference
officiels par produit sont la référence exacte de chaque commande.*
