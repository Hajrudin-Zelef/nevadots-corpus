# OPNsense — Guide ultra-complet de l'interface web

> Tour d'horizon exhaustif de l'interface web d'administration OPNsense :
> chaque menu, à quoi il sert, et comment s'en servir en pratique.
>
> **À jour 2025-2026** : vise OPNsense **24.x / 25.x** (thème par défaut,
> MVC/API, WireGuard intégré au cœur, Unbound, Suricata, Zenarmor en
> plugin). Les chemins de menu sont donnés en anglais tels qu'affichés
> dans l'interface.

---

## 1. Accès et prise en main

- URL par défaut : `https://192.168.1.1/` (LAN). Port HTTPS 443
  (modifiable), HTTP redirigé vers HTTPS par défaut.
- Identifiants par défaut : `root` / `opnsense` — **à changer
  immédiatement** (System > Access > Users).
- L'assistant de démarrage (wizard) au premier login : langue, hostname,
  DNS, fuseau horaire, WAN/LAN, mot de passe root.
- En haut à droite : recherche globale (loupe) — tapez « nat » ou
  « wireguard » pour sauter directement à la page. C'est le moyen le
  plus rapide de naviguer.
- **Bouton « Apply »** : la plupart des pages exigent un clic sur
  Apply / Save pour activer. Une pastille orange signale les changements
  en attente.
- **Dashboard** (Lobby) : widgets configurables (trafic, gateways,
  services, firewall logs, mises à jour). Personnalisez via le « + ».

---

## 2. System — réglages généraux

### System > Settings > General
- Hostname, domaine, fuseau horaire (indispensable : une horloge fausse
  casse IPsec, les certificats et les logs).
- Serveurs DNS (ou « Allow DNS server list to be overridden by DHCP »).
- Langue, thème.

### System > Settings > Administration
- Protocole web UI (HTTPS recommandé), port, interface d'écoute
  (**ne jamais exposer la web UI sur WAN**).
- **Secure Shell** : activer ici l'accès SSH (port, auth par clé).
- Console : mot de passe requis ou non.

### System > Settings > Logging
- Niveaux de log par composant, syslog distant (cible + protocole
  TCP/UDP/TLS), rotation.

### System > Settings > Cron
- Tâches planifiées : mises à jour des alias, renouvellement DHCP,
  backups automatiques, redémarrages planifiés.

### System > Firmware
- **Updates** : vérifier / installer les mises à jour (toujours
  sauvegarder la config avant — voir §15).
- **Plugins** : catalogue (FRR, ACME, WireGuard est au cœur depuis
  21.x, haproxy, crowdsec, etc.). Installer = 2 clics.
- **Packages** : paquets FreeBSD sous-jacents (usage avancé).

### System > Certificates
- **Authorities** : créer votre CA interne.
- **Certificates** : certificats serveur (web UI, OpenVPN, IPsec).
- **Revocation** : CRL.
- Indispensable pour : HTTPS de la web UI avec un vrai certificat,
  OpenVPN, IPsec par certificats, inspection TLS.

### System > Trust
- Autorités et certificats de confiance pour valider les pairs distants.

### System > Gateways
- **Single** : passerelles WAN (IP, monitor IP pour le failover).
- **Group** : groupes de failover / load-balancing multi-WAN
  (Tier 1 = prioritaire, Tier 2 = secours ; trigger : packet loss,
  latency, down).
- Une gateway « down » ici = bascule automatique du groupe.

### System > High Availability
- Synchronisation XMLRPC entre 2 nœuds (config sync : firewall rules,
  aliases, users…), mot de passe de sync, interfaces à synchroniser.
- À combiner avec CARP (voir §4).

### System > Access
- **Users / Groups** : créer des comptes (admin, lecture seule…),
  groupes, privilèges fins (page-level privileges).
- **Servers** : LDAP, RADIUS pour l'authentification centralisée.
- **2FA** : TOTP pour les comptes (recommandé pour root/admin).
- **Sudo** : déléguer des commandes shell à des non-root.
- **Tester** : bouton de test d'authentification.

---

## 3. Interfaces

### Interfaces > Assignments
- Associer chaque port physique (`em0`, `igb0`, `vtnet0`…) à un rôle
  (WAN, LAN, OPT1…). Le cœur du « menu 1 » de la console, en web.

### Interfaces > [WAN] / [LAN] / [OPTn]
- **IPv4/IPv6 Configuration Type** : DHCP, Static, PPPoE, PPTP, L2TP…
- IP statique + passerelle, MTU/MSS, « Block private networks »
  et « Block bogon networks » sur WAN (à laisser cochés).
- **DHCP client options** : hostname, advanced options.

### Interfaces > Other Types
- **VLAN** : créer (parent + tag 802.1Q + description).
- **LAGG** : agrégat LACP (`laggproto lacp`), membres.
- **Bridge** : pont entre interfaces.
- **GRE / GIF** : tunnels.
- **VXLAN** (selon version/plugin).

### Interfaces > Overview
- Vue d'ensemble : état, IP, MAC, trafic, passerelle — le pendant web
  de `ifconfig`.

### Interfaces > Diagnostics
- **ARP Table**, **NDP Table** : qui est joignable en local.
- **Routes** : table de routage lue en direct.

---

## 4. Firewall — règles

### Firewall > Rules > [interface]
- Le cœur du filtrage. Chaque règle : Action (Pass/Block/Reject),
  Disabled, Interface, Direction (in/out), TCP/IP version, Protocol,
  Source, Destination, Destination port range, Log, Description.
- **L'ordre compte** : la première règle qui matche gagne
  (les règles flottantes « Floating » s'évaluent avant, selon
  « quick »).
- **Schedules** : plages horaires (Firewall > Schedules) pour des
  règles actives seulement à certaines heures.
- **Log** : cochez sur les règles sensibles — les hits apparaissent
  dans Firewall > Log Files > Live View.
- Règles automatiques : « Automatically generated rules » (anti-lockout
  LAN, DHCP, etc.) visibles en lecture seule.

### Firewall > Rules > Floating
- Règles transverses (multi-interfaces), idéales pour : bloquer des
  pays/IP partout, QoS, politique sortante globale.

### Firewall > Aliases
- Listes nommées : **Hosts** (IP), **Networks**, **Ports**, **URLs**,
  **URL Tables** (listes téléchargées : blocklists), **GeoIP**
  (pays via MaxMind — nécessite une clé gratuite).
- Utilisées dans Source/Destination des règles et du NAT.
- **URL Table** avec rafraîchissement auto = blocklist dynamique
  (ex. : liste d'IP malveillantes mise à jour chaque jour).

### Firewall > NAT
- **Port Forward** : `Interface WAN → Destination WAN address →
  Redirect target IP → Redirect target port` + règle firewall auto
  (créez-la en même temps, option « Associated filter rule »).
- **Outbound** : Automatic (défaut, PAT sur IP WAN), Hybrid, Manual.
  Manual = contrôle fin (ex. : exclure le trafic VPN du NAT).
- **One-to-One (1:1)** : mapper toute une IP publique vers une IP
  interne (DMZ).
- **NPTv6** : équivalent NAT pour IPv6.

### Firewall > Virtual IPs
- **IP Alias / CARP / Proxy ARP / Other** : IP supplémentaires sur une
  interface (multi-IP publiques du FAI, VIP CARP pour la HA).
- **CARP** : Virtual IP partagée entre 2 nœuds + advskew (priorité).
  Status : Firewall > Virtual IPs > Status.

### Firewall > Shaper
- **Pipes / Queues / Rules** (dummynet) : limiter/garantir la bande
  passante par IP, par service, par interface. Ex. : pipe 100 Mbit
  down / 20 Mbit up sur WAN, queues par VLAN.

### Firewall > Log Files
- **Live View** : le trafic filtré en temps réel (filtres par
  interface, action, IP, port). L'outil n°1 du diagnostic.
- **Plain View** : historique paginé.
- **Summary** : top parleurs / top bloqués.

### Firewall > Diagnostics
- **States** : table des états en direct (recherche par IP, kill).
- **States Summary**, **Statistics** (`pfctl -s info` en web).
- **pfTables** : contenu des tables (bogons, aliases…).
- **pfInfo**, **Sockets**.

---

## 5. DHCP et DNS

### Services > DHCPv4 > [interface]
- **Enable**, Range (plage), passerelle/DNS par défaut ou custom
  (DNS servers, Domain name).
- **Static Mappings** : réservations par MAC (serveurs, imprimantes).
- **Deny unknown clients** : durcissement (avec static mappings).
- **Leases** : baux actifs vus en direct.

### Services > DHCPv6 / Router Advertisements
- RA : Managed / Assisted / Stateless / Disabled selon votre
  stratégie IPv6 (DHCPv6 + RA combinés en général).

### Services > DHCP Relay
- Relayer vers un serveur DHCP central quand plusieurs VLANs.

### Services > Unbound DNS > General
- Activer, interfaces d'écoute, **DNS over TLS** (forward vers
  9.9.9.9 / 1.1.1.1 en DoT : confidentialité des requêtes).
- **Blocklists** : activer les listes (ads, trackers, malware) —
  l'ad-blocking réseau en 2 clics.
- **Advanced** : cache, ECS, qname minimisation.

### Services > Unbound DNS > Overrides
- **Host Overrides** : noms internes (`intranet.lan → 192.168.1.10`).
- **Domain Overrides** : forwarder conditionnel par domaine
  (ex. : `corp.lan` → DNS Active Directory).

### Services > Unbound DNS > Blocklists / Query / Stats
- Gérer les listes, tester une résolution, voir les stats.

---

## 6. VPN

### VPN > IPsec
- **Tunnels** : Phase 1 (IKEv2 recommandé : AES-GCM, DH 14/19/20,
  auth PSK ou certificats) puis Phase 2 (ESP, réseaux locaux/distants).
- **Mobile Clients** : road-warriors (EAP-MSCHAPv2, certificats).
- **Status Overview** : tunnels UP/DOWN en direct.
- **Security Associations** : détail des SA.
- **Pre-Shared Keys / Advanced**.

### VPN > OpenVPN
- **Servers** : mode Remote Access (SSL/TLS + Auth) ou Peer to Peer.
  Assistant (wizard) disponible pour un serveur road-warrior standard.
- **Clients** : tunnels site-à-site ou client vers fournisseur.
- **Client Export** : génère les `.ovpn` prêts à importer (avec
  certificats) — très pratique.
- **Status** : sessions connectées, kill session.

### VPN > WireGuard
- **Instances** : créer (port d'écoute, clés générées en 1 clic).
- **Peers** : endpoint, AllowedIPs, keepalive (25 s derrière un NAT).
- **Status** : handshakes et volumes — le diagnostic en un coup d'œil.

> Rappel valable pour les 3 : une règle **Firewall > Rules**
> (WAN pour IPsec/WireGuard/OpenVPN, + l'interface VPN elle-même)
> est obligatoire — le VPN ne bypass pas le firewall.

---

## 7. IDS/IPS et sécurité avancée

### Services > Intrusion Detection (Suricata)
- **Download** : télécharger les jeux de règles (ET Open, OTX…).
- **Policy** : activer en mode **IDS (détection)** d'abord, analyser
  les alertes, puis basculer en **IPS (blocage)** sur les interfaces
  WAN (ou LAN selon stratégie).
- **Rules** : activer/désactiver par catégorie, seuils.
- **Alerts** : les alertes avec payload — le tuning se fait ici
  (désactiver les faux positifs un par un).

### Plugins de sécurité (à installer si besoin)
- **Zenarmor (Sensei)** : filtrage applicatif/web avancé, contrôle
  par catégories, rapports par utilisateur.
- **CrowdSec** : IPS collaboratif (bannit les IP agressives vues
  par la communauté).
- **ACME Client** : certificats Let's Encrypt automatiques
  (web UI, VPN).

---

## 8. Routage

### System > Routes > Configuration
- Routes statiques IPv4/IPv6 (destination, passerelle).
- « Disable all packet filtering » n'existe pas ici : le firewall
  reste actif, pensez aux règles.

### System > Routes > Status
- Table effective.

### Plugin FRR (à installer)
- OSPF, BGP, RIP via **Routing > FRR** avec VTYSH intégré.
  Pour les réseaux dynamiques multi-sites.

---

## 9. Services réseau

### Services > NTP
- **General** : serveurs (pool.ntp.org par défaut), interfaces d'écoute.
  Une horloge juste = IPsec, logs et 2FA fiables.

### Services > SNMP
- Activer l'agent, communities (v2c) ou **v3** (authPriv recommandé),
  modules MIB-II.

### Services > Captive Portal
- Portail captif (hôtellerie, invités) : zones, page de login,
  vouchers, limites de bande passante.

### Services > Web Proxy (Squid, plugin)
- Proxy transparent ou explicite, cache, filtrage SSL bump
  (avec CA déployée sur les postes), antivirus ICAP (ClamAV).

### Reporting > NetFlow / Insight
- **Insight** : historique du trafic par hôte/service (qui consomme
  quoi) — activez la capture sur les interfaces internes.
- Données précieuses pour le dimensionnement et l'audit.

### Services > Syslog (destinations)
- Déjà vu en §2 : cible distante, TLS.

---

## 10. Utilisateurs, accès, 2FA

- **System > Access > Users** : créer/modifier, groupes, shell,
  clés SSH, **OTP (TOTP)** : scannez le QR code avec votre appli
  d'authentification.
- **Groups** : privilèges par pages (ex. : groupe « support » avec
  accès Diagnostics + Logs seulement).
- **Servers** : brancher LDAP/AD ou RADIUS pour centraliser les logins
  admin et VPN.

---

## 11. Diagnostics (le menu à connaître)

### Interfaces > Diagnostics
- **Ping**, **Traceroute**, **DNS Lookup**, **Test Port** :
  avec choix de l'interface **source** (crucial en multi-WAN :
  tester via WAN1 puis WAN2).
- **Packet Capture** : capture avec filtres (interface, host, port),
  **téléchargeable en .pcap** (ouverture dans Wireshark).
  L'équivalent web de tcpdump, sans SSH.
- **ARP/NDP Tables**, **Routes**.

### Firewall > Diagnostics (§4)
- States, pfTables, Statistics, Sockets.

### System > Diagnostics
- **Activity** (top), **System Activity**, **Disk Usage**.

### VPN > Diagnostics
- Status IPsec/OpenVPN/WireGuard (§6).

---

## 12. Sauvegarde, historique, restauration

### System > Configuration > Backups
- **Download** : télécharger `config.xml` (chiffrable par mot de passe).
- **Restore** : restaurer un fichier.
- **History** : chaque « Apply » crée une révision horodatée avec
  **diff** — cliquez pour voir ce qui a changé, restaurez en 1 clic.
  C'est le « git » de votre firewall : en cas de bêtise, revenez en
  arrière sans stress.
- **Google Drive / Nextcloud** (plugins) : backups automatiques
  chiffrés hors-site.

---

## 13. Mises à jour via l'interface

### System > Firmware > Status / Updates
- **Check for updates** : liste des correctifs (type : security,
  bugfix, major).
- **Update** : installe (redémarre les services, rarement le système
  complet sauf mise à jour majeure).
- Lisez le changelog affiché avant une **mise à jour majeure**
  (ex. 24.x → 25.x) : plugins à vérifier, backup avant.
- **System > Firmware > Audit** : vérifie l'intégrité des fichiers
  (détection de corruption / compromission).

---

## 14. Haute disponibilité via l'interface

1. **Firewall > Virtual IPs** : créer les VIP **CARP** (une par
   interface concernée : WAN, LAN…), même VHID des deux côtés,
   advskew 0 (master) / 100 (backup).
2. **System > High Availability** : activer la sync XMLRPC
   (IP du pair, mot de passe, éléments à synchroniser).
3. **Firewall > Virtual IPs > Status** : vérifier MASTER/BACKUP.
4. Test : débranchez le master → les VIP passent en MASTER sur le
   second en quelques secondes (coupure brève des états pf :
   activez « state synchronization » / pfsync si besoin de continuité
   totale).

---

## 15. Durcissement (hardening) recommandé

- [ ] Mot de passe root fort + **2FA TOTP** (System > Access).
- [ ] Web UI : HTTPS uniquement, écoute sur **LAN uniquement**,
  port non standard (ex. 8443).
- [ ] SSH : clés uniquement (pas de mot de passe), port non standard.
- [ ] WAN : « Block private networks » + « Block bogon networks »
  cochés ; **aucune règle Pass sur WAN** sauf VPN/port-forwards
  nécessaires.
- [ ] Règles LAN : principe du moindre privilège (pas de « allow all »
  permanent si possible) ; log sur les règles sensibles.
- [ ] Mises à jour firmware : vérifier mensuellement.
- [ ] Backups chiffrés automatiques hors-site (History + plugin cloud).
- [ ] Suricata en IPS sur WAN après tuning.
- [ ] DNS over TLS + blocklists (anti-malware au DNS).
- [ ] Désactiver les services inutilisés (chaque service = surface
  d'attaque).

---

## 16. Dépannage via l'interface (méthode)

1. **Dashboard** : gateways UP ? services verts ? mises à jour en
   attente ?
2. **Firewall > Log Files > Live View** : reproduisez le problème et
   regardez ce qui est **bloqué** (action Block, interface, règle).
   80 % des « pannes » se voient ici en 30 secondes.
3. **Interfaces > Diagnostics > Packet Capture** : le paquet arrive-t-il
   ? repart-il ? (sens aller/retour).
4. **Firewall > Diagnostics > States** : l'état existe-t-il ? Tuez-le
   après un changement de règle/NAT.
5. **VPN > … > Status** : tunnel UP ? handshake récent (WireGuard) ?
6. **System > Log Files > General** : erreurs de services.
7. **System > Configuration > History** : « qu'est-ce qui a changé
   juste avant la panne ? » → restaurez la révision précédente.

---

## 17. Raccourcis et productivité

- **Recherche globale** (loupe en haut) : le plus rapide pour trouver
  une page (« alias », « wireguard », « cron »…).
- **Favoris** : épinglez vos pages fréquentes.
- **API REST** : presque tout ce que fait la web UI est scriptable
  (clé API par utilisateur : System > Access > Users > API keys).
  Idéal pour : créer des alias en masse, automatiser les backups,
  superviser via scripts.
- **Thème sombre** disponible (System > Settings > General).
- **Widgets du dashboard** : ajoutez « Firewall Logs », « Gateways »,
  « Services », « Updates » pour un NOC miniature.

---

*Fin du guide. Avec la web UI vous couvrez 100 % des fonctions ;
avec le [guide CLI](opnsense_cli_guide.md) vous faites la même chose
au clavier. Les deux se complètent : la web UI pour découvrir, le CLI
pour automatiser et dépanner vite.*
