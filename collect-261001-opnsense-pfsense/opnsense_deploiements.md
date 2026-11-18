# OPNsense — 4 déploiements simulés (niveau 1 → hardcore)

> Quatre scénarios complets et réalistes : topologie, plan d'adressage,
> configuration pas à pas (interface web **et** équivalents CLI),
> vérifications et pièges. Chaque niveau réutilise les acquis du
> précédent — lisez dans l'ordre.
>
> **Conventions** : les chemins web sont en anglais tels qu'affichés
> (`Firewall > Rules > LAN`). Les blocs CLI renvoient aux commandes du
> [guide CLI](opnsense_cli_guide.md).

---

# NIVEAU 1 — Petit bureau / domicile (SOHO)

## Objectif
Protéger un petit réseau : 1 box FAI, ~15 postes, WiFi. Internet
fonctionnel en 30 minutes, avec les bases de sécurité.

## Topologie

```
        ┌─────────┐
        │ Box FAI │ 192.168.0.1 (DHCP)
        └────┬────┘
             │ WAN (DHCP)
      ┌──────┴──────┐
      │  OPNsense   │
      └──────┬──────┘
             │ LAN 192.168.1.1/24
      ┌──────┴──────┐
      │  Switch    │── PC, imprimante, borne WiFi (AP mode)
      └────────────┘
```

## Plan d'adressage
| Réseau | Plage | Passerelle |
|---|---|---|
| LAN | 192.168.1.0/24 | 192.168.1.1 (OPNsense) |
| DHCP | 192.168.1.100–200 | — |

## Étapes (web UI)

1. **Installation** : boot sur l'ISO/USB, login `root`/`opnsense`,
   menu console → **1) Assign Interfaces** (em0=WAN, em1=LAN),
   puis **2)** : WAN en DHCP, LAN en `192.168.1.1/24`.
2. **Premier login web** : `https://192.168.1.1`, assistant :
   hostname `fw-maison`, DNS `9.9.9.9`, fuseau horaire, mot de passe
   root fort.
3. **System > Settings > Administration** : HTTPS uniquement,
   activer SSH (clés plus tard).
4. **Services > DHCPv4 > LAN** : Enable, Range `192.168.1.100–200`,
   DNS `192.168.1.1` (l'OPNsense lui-même).
5. **Services > Unbound DNS > General** : Enable, écoute sur LAN,
   **DNS over TLS** coché (forward TLS vers Quad9/Cloudflare),
   **Blocklists** : activer `ads` + `malware`.
6. **Firewall > Rules > LAN** : la règle « Default allow LAN to any »
   existe déjà → la garder pour l'instant (niveau 1).
7. **Firewall > Rules > WAN** : vérifier qu'il n'y a **aucune**
   règle Pass (par défaut : rien ne rentre — parfait).
8. **System > Settings > General** : NTP par défaut OK.
9. **System > Configuration > Backups** : télécharger le premier
   `config.xml`.

## Équivalents CLI

```shell
# Vérifier interfaces et DHCP
ifconfig
cat /var/dhcpd/etc/dhcpd.conf | head -20
# Règles actives
pfctl -sr | head -20
pfctl -sn
# Test DNS chiffré
drill www.example.com @127.0.0.1
# Backup
cp /conf/config.xml /root/niveau1-$(date +%Y%m%d).xml
```

## Vérifications
- [ ] Un PC obtient `192.168.1.x` en DHCP, navigue sur Internet.
- [ ] `drill pub.malware.exemple @192.168.1.1` → bloqué (blocklist).
- [ ] Depuis Internet (4G du téléphone) : `https://<ip-publique>` →
  **aucune réponse** (web UI non exposée).
- [ ] **Firewall > Log Files > Live View** : voir les blocages WAN
  (scans) en direct.

## Pièges niveau 1
- Box FAI en routeur (double NAT) : acceptable au niveau 1, mais
  mettre la box en **mode bridge** si possible (sinon port-forwards
  à faire 2 fois plus tard).
- Oublier le backup après chaque étape qui marche.

---

# NIVEAU 2 — PME (3 VLANs + VPN road-warrior)

## Objectif
Bureau de 40 personnes : séparer **employés**, **invités** et
**serveurs** ; accès VPN pour les télétravailleurs ; un serveur web
publié ; DNS interne.

## Topologie

```
                    ┌────────────┐
                    │    FAI     │ IP publique fixe 203.0.113.10
                    └─────┬──────┘
                          │ WAN (PPPoE ou statique)
                   ┌──────┴──────┐
                   │  OPNsense   │
                   └──┬──┬───┬───┘
                      │  │   │   trunk 802.1Q
              ┌───────┘  │   └───────┐
              │          │           │
        VLAN 10    VLAN 20     VLAN 30
   EMPLOYES 192.168.10.0/24  INVITES 192.168.20.0/24  SERVEURS 192.168.30.0/24
   .1 = OPNsense             .1 = OPNsense            .1 = OPNsense
                                                  .10 = serveur web
                                                  .11 = NAS
```

## Plan d'adressage
| VLAN | Réseau | DHCP | Notes |
|---|---|---|---|
| 10 EMPLOYES | 192.168.10.0/24 | .100–200 | accès Internet + serveurs |
| 20 INVITES | 192.168.20.0/24 | .100–200 | Internet seul, isolé |
| 30 SERVEURS | 192.168.30.0/24 | réservations MAC | pas d'Internet direct* |

\* Les serveurs sortent via un proxy/updates autorisés uniquement
(voir règles).

## Étapes (web UI)

### A. VLANs et interfaces
1. **Interfaces > Other Types > VLAN** : créer VLAN 10, 20, 30 sur
   l'interface physique LAN (parent `em1`).
2. **Interfaces > Assignments** : assigner `VLAN 10 → EMPLOYES`,
   `VLAN 20 → INVITES`, `VLAN 30 → SERVEURS`, Enable + IP statique
   `.1/24` sur chacune.
3. Switch : trunk 802.1Q vers OPNsense, ports access dans les bons
   VLANs (côté switch, hors OPNsense).

### B. DHCP + DNS
4. **Services > DHCPv4** : activer sur les 3 VLANs (ranges adaptées).
   **Static Mappings** sur SERVEURS : `.10` (web), `.11` (NAS).
5. **Services > Unbound DNS > Overrides > Host Overrides** :
   `intranet.pme.lan → 192.168.30.10`, `nas.pme.lan → 192.168.30.11`.

### C. Règles firewall (le cœur du niveau 2)
6. **Firewall > Aliases** : créer `VLAN_EMPLOYES` (192.168.10.0/24),
   `VLAN_INVITES`, `VLAN_SERVEURS`, `SRV_WEB` (192.168.30.10),
   `PORTS_WEB` (80, 443).
7. **Firewall > Rules > INVITES** :
   - Pass INVITES → `! RFC1918` (alias « réseaux privés » : tout
     sauf les réseaux internes) ports 80/443/53 — Internet seul.
   - (implicite : pas d'accès aux autres VLANs.)
8. **Firewall > Rules > EMPLOYES** :
   - Pass EMPLOYES → SERVEURS ports 80/443 (intranet).
   - Pass EMPLOYES → any (Internet).
9. **Firewall > Rules > SERVEURS** :
   - Pass SERVEURS → any port 80/443 (mises à jour uniquement).
   - Block SERVEURS → LAN nets (log).
10. **Firewall > Rules > WAN** : aucune règle Pass pour l'instant.

### D. Publication du serveur web
11. **Firewall > NAT > Port Forward** : WAN, TCP, dest `WAN address`,
    port 443 → `192.168.30.10:443`, cocher « Associated filter rule »
    (crée la règle WAN automatiquement).
12. Tester depuis l'extérieur : `https://203.0.113.10` → site OK.

### E. VPN road-warrior (WireGuard)
13. **VPN > WireGuard > Instances** : Add, port `51820`, générer les clés.
14. **VPN > WireGuard > Peers** : un peer par télétravailleur
    (clé publique du client, AllowedIPs `10.8.0.x/32`, keepalive 25).
15. **Interfaces > Assignments** : assigner `wg0` → interface `WG`
    (pour les règles firewall).
16. **Firewall > Rules > WG** : Pass WG → EMPLOYES + SERVEURS
    (selon profil).
17. **Firewall > Rules > WAN** : Pass UDP 51820 (source any).
18. Distribuer les configs clients (QR code / fichier).

### F. Finitions
19. **System > Access > Users** : créer un compte `admin` (désactiver
    le login web `root` direct si souhaité), activer **2FA TOTP**.
20. **System > Firmware** : vérifier les mises à jour.
21. **System > Configuration > Backups** : télécharger + activer
    l'historique (déjà actif par défaut).

## Équivalents CLI

```shell
# VLANs
ifconfig | grep vlan
# Règles par interface
pfctl -sr | grep -A2 "label"
# NAT port-forward actif
pfctl -sn | grep 443
# WireGuard
wg show
wg show wg0 latest-handshakes
# États d'un invité
pfctl -ss | grep 192.168.20.
# Backup
cp /conf/config.xml /root/niveau2-$(date +%Y%m%d).xml
```

## Vérifications
- [ ] Un invité (VLAN 20) navigue mais `ping 192.168.10.1` → **échec**.
- [ ] Un employé ouvre `https://intranet.pme.lan`.
- [ ] Depuis l'extérieur : `https://203.0.113.10` → site ; `http://`
      → rien (pas de règle 80).
- [ ] Télétravailleur WireGuard : handshake récent (`wg show`),
      accès intranet OK.
- [ ] **Live View** : voir les Block sur INVITES→EMPLOYES quand on teste.

## Pièges niveau 2
- Règle INVITES « Pass → any » au lieu de « Pass → !RFC1918 » :
  les invités voient tout le réseau interne.
- Oublier la règle firewall WAN pour WireGuard (le peer n'a jamais
  de handshake).
- AllowedIPs du peer mal configuré (`0.0.0.0/0` route tout le trafic
  du client via le VPN — voulu ou non ?).
- Le switch : trunk oublié = VLANs morts (toujours tester en
  untagged d'abord).

---

# NIVEAU 3 — Entreprise multi-sites (HA + multi-WAN + IPS)

## Objectif
Siège + 2 agences, 200 utilisateurs : **double FAI avec failover**,
**HA active/passive** (2 firewalls), tunnels **IPsec** vers les
agences, **IPS** actif, supervision.

## Topologie

```
   FAI-1 (fibre)              FAI-2 (4G/ADSL secours)
      │                          │
 ┌────┴─────┐              ┌──────┴─────┐
 │ FW-1     │◄─── sync ───►│ FW-2       │  CARP : .2/.3, VIP .1/.254
 │ (MASTER) │  + pfsync   │ (BACKUP)   │
 └────┬─────┘              └────────────┘
      │ trunk (VLANs 10/20/30/40 comme niveau 2 + VLAN 99 MGMT)
      │
   ┌──┴──┐        IPsec IKEv2         ┌────────┐
   │ LAN │◄──────────────────────────►│ Agence │ (OPNsense ou routeur IPsec)
   └──┬──┘   192.168.0.0/16 chiffré  └────────┘
      │
   (DMZ, serveurs, WiFi corporate…)
```

## Plan d'adressage (siège)
| Élément | IP |
|---|---|
| WAN1 (FAI-1) | 203.0.113.10/30, GW 203.0.113.9 |
| WAN2 (FAI-2) | DHCP (ou 198.51.100.50/29) |
| LAN VIP CARP | 192.168.10.254 (FW-1 .253, FW-2 .252) |
| MGMT VLAN 99 | 192.168.99.0/24 |
| Agence A | 192.168.100.0/24 via IPsec |
| Agence B | 192.168.110.0/24 via IPsec |

## Étapes (web UI)

### A. Haute disponibilité (à faire en premier)
1. Sur FW-1 et FW-2 : **même version OPNsense** (prérequis absolu).
2. **Firewall > Virtual IPs** : créer les VIP **CARP** (WAN, LAN,
   chaque VLAN) — même VHID, advskew `0` sur FW-1, `100` sur FW-2.
3. **System > High Availability** : activer sync XMLRPC vers le pair
   (IP, mot de passe, cocher : rules, aliases, NAT, DHCP, VPN…).
4. **Status** : vérifier MASTER/BACKUP sur les deux nœuds.
5. Tester : débrancher FW-1 → FW-2 passe MASTER (coupure < 10 s).

### B. Multi-WAN avec failover
6. **System > Gateways > Single** : GW_WAN1 (monitor `9.9.9.9`),
   GW_WAN2 (monitor `1.1.1.1`). « Disable Gateway Monitoring » =
   **jamais** en prod.
7. **System > Gateways > Group** : `GW_FAILOVER` — WAN1 Tier 1,
   WAN2 Tier 2, trigger `Packet Loss or High Latency`.
8. **Firewall > NAT > Outbound** : passer en **Hybrid**, règle
   manuelle : LAN → GW_FAILOVER (le NAT suit le groupe).
9. **Firewall > Rules > LAN** : règle Pass avec **Gateway =
   GW_FAILOVER** (sinon le trafic sort toujours par la default GW).
10. Tester : débrancher WAN1 → les sessions basculent sur WAN2
    (vérifier **System > Gateways** : WAN1 « down », groupe sur WAN2).

### C. IPsec vers les agences (IKEv2)
11. **VPN > IPsec > Tunnels** : Phase 1 IKEv2 (AES-256-GCM, DH 19,
    auth **certificats** — créez la CA dans System > Certificates),
    Phase 2 ESP (réseaux `192.168.0.0/16` ↔ `192.168.100.0/24`).
12. **Firewall > Rules > IPsec** : Pass IPsec → LAN (et inversement
    sur LAN → IPsec).
13. **Status Overview** : tunnel UP, trafic chiffré.
14. Répéter pour l'agence B.

### D. IPS (Suricata)
15. **Services > Intrusion Detection** : **Download** des règles
    (ET Open), activer sur **WAN** en mode **IDS** (détection).
16. Laisser tourner 1 semaine, analyser **Alerts**, désactiver les
    faux positifs (applis métiers).
17. Basculer en **IPS** (blocage) sur WAN.
18. Politique : bloquer les catégories `exploit`, `malware`,
    `trojan` ; alerter seulement sur `policy`, `info`.

### E. Supervision et finitions
19. **Reporting > NetFlow** : activer la capture sur LAN/VLANs
    (Insight : qui consomme quoi).
20. **Services > SNMP** : v3 authPriv pour Zabbix/Centreon.
21. **System > Settings > Logging** : syslog distant (serveur SIEM).
22. **System > Access** : 2FA pour tous les admins, comptes nominatifs.
23. **System > Configuration > Backups** : backup chiffré + envoi
    automatique (plugin Nextcloud/Google Drive).
24. **System > Firmware** : planifier la vérification mensuelle
    (System > Settings > Cron).

## Équivalents CLI

```shell
# CARP
ifconfig | grep -A2 carp
# Gateways
configctl gateways status 2>/dev/null || grep -A8 "<gateways>" /conf/config.xml
# IPsec
ipsec status
swanctl --list-sas
# Suricata
configctl suricata status
clog -f /var/log/suricata/eve.json | grep alert
# Sync HA
grep -A10 "<hasync>" /conf/config.xml
```

## Vérifications
- [ ] Failover WAN : coupure WAN1 → Internet maintenu via WAN2.
- [ ] Failover HA : extinction FW-1 → FW-2 MASTER, sessions maintenues
      (avec pfsync).
- [ ] Agences : `ping 192.168.100.1` depuis le siège via IPsec.
- [ ] Suricata : lancer un test (ex. : `curl` vers une URL de test
      EICAR-like) → alerte/block visible.
- [ ] Restauration : restaurer un backup sur FW-2 (lab) → config OK.

## Pièges niveau 3
- Versions différentes entre FW-1/FW-2 → sync silencieusement cassée.
- Règle LAN sans « Gateway = groupe » : le failover WAN ne s'applique
  pas au trafic LAN.
- IPsec + NAT : exclure le trafic IPsec du NAT outbound manuel
  (règle `no-nat` / ne pas NATer vers les réseaux distants).
- Suricata en IPS **avant** tuning = applis métiers coupées un lundi
  matin. Toujours IDS d'abord.
- Monitor IP des gateways = la passerelle FAI elle-même : si le FAI
  route mais perd Internet, le monitor reste « up ». Monitorer une IP
  **Internet** (9.9.9.9).

---

# NIVEAU HARDCORE — Datacenter / opérateur

## Objectif
Héberger des clients en datacenter : **routage BGP**, **IPv6 natif**,
**filtrage applicatif** (Zenarmor), **threat intel** (CrowdSec),
**QoS stricte**, **automatisation API**, durcissement maximal.
C'est le niveau « on ne dort que d'un œil ».

## Topologie (simplifiée)

```
        Transit-1 (BGP)         Transit-2 (BGP)
            │                       │
      ┌─────┴───────┐       ┌───────┴───────┐
      │  FW-A       │◄─────►│  FW-B         │  HA CARP + pfsync
      │  (FRR BGP)  │ sync  │  (FRR BGP)    │
      └─────┬───────┘       └───────┬───────┘
            │ trunk (VLANs clients, MGMT, DMZ, TRANSIT)
   ┌────────┼──────────┬───────────┼─────────┐
   │        │          │           │         │
CLIENT-A  CLIENT-B   DMZ-      MGMT     TRANSIT
VLAN 101  VLAN 102  MUTUALISE  VLAN 99  /30 publics
::/48      ::/48    (hébergé)
/24        /24
```

## Plan d'adressage (extrait)
| Élément | IPv4 | IPv6 |
|---|---|---|
| Transit-1 | 203.0.113.0/30 | 2001:db8:1::/64 |
| AS propre | AS 65001 (ou PI RIPE) | — |
| Client A | 198.51.100.0/24 | 2001:db8:100::/48 |
| Client B | 198.51.101.0/24 | 2001:db8:200::/48 |
| MGMT | 192.168.99.0/24 | — |

## Étapes

### A. Routage BGP (plugin FRR)
1. Installer **os-frr**, activer : **Routing > FRR > General**.
2. **Routing > FRR > BGP** : AS 65001, router-id, neighbors vers les
   deux transits (remote-as du FAI, password MD5, prefix-lists
   d'import/export strictes).
3. **Prefix-lists** : n'annoncer **que** vos préfixes (jamais de
   `0.0.0.0/0` en sortie vers un transit, jamais de full-table
   acceptée sans filtre — risque de devenir transit par accident).
4. **Route-maps** : prepend/communities selon politique.
5. Vérifier : `vtysh -c "show bgp summary"` (FRR), préfixes reçus/
   annoncés conformes.
6. **BFD** si supporté par les transits (détection de panne < 1 s).

### B. IPv6 natif dual-stack
7. **Interfaces > [WAN]** : IPv6 = DHCPv6 ou statique selon transit.
8. **Services > DHCPv6 + Router Advertisements** : RA `Managed` sur
   les VLANs clients (ou SLAAC + stateless DHCPv6 selon client).
9. **Firewall > Rules** : **dupliquer la politique en IPv6**
   (les règles IPv4 ne couvrent PAS IPv6 — erreur classique :
   IPv6 grand ouvert par défaut !).
10. **Firewall > NAT > NPTv6** si besoin (éviter le NAT66, préférer
    le routage pur).
11. Tester : `test-ipv6.com` depuis un client → 10/10.

### C. Filtrage applicatif et threat intel
12. **Zenarmor** (plugin) : déployer sur les interfaces clients —
    politiques par catégorie (réseaux sociaux, streaming, malware),
    rapports par IP/client, **TLS inspection** avec CA déployée
    (clients managés uniquement, avec consentement/charte).
13. **CrowdSec** (plugin) : bannissement collaboratif (les IP qui
    attaquent ailleurs sont bloquées ici avant d'essayer).
14. **Suricata** : IPS sur TRANSIT + règles ET Pro si budget.
15. **Firewall > Aliases > URL Tables** : blocklists (Spamhaus DROP,
    Emerging Threats) rafraîchies toutes les heures.

### D. QoS stricte (Shaper)
16. **Firewall > Shaper** : pipes par client (débit contractuel),
    queues prioritaires (VoIP EF > tout), règle « par VLAN ».
17. Exemple : pipe `CLIENT-A-DOWN` 500 Mbit, `CLIENT-A-UP` 500 Mbit ;
    queue VoIP `priority 7`, queue default `weight 1`.
18. Vérifier sous charge : `ping` stable pendant un speedtest
    (bufferbloat maîtrisé).

### E. Automatisation (API)
19. **System > Access > Users** : créer un compte `api-bot` avec clé
    API (privilèges restreints : aliases, diagnostics).
20. Scripts : ajouter/retirer des IP d'alias en masse, snapshots de
    config avant chaque changement, checks de supervision.
21. Exemple (conceptuel) :
    `POST /api/firewall/alias/addItem` — voir la doc API intégrée
    (`https://<fw>/api/docs` selon version / wiki OPNsense).
22. **Git** : versionner les exports `config.xml` (cron + push).

### F. Durcissement extrême
23. Tout le §15 du guide web UI, **plus** :
    - Web UI + SSH : écoute sur **MGMT/VLAN 99 uniquement**, via VPN
      admin (WireGuard dédié) pour l'accès distant.
    - `root` : login console uniquement ; admins = comptes nominatifs
      + 2FA + sudo ciblé.
    - **System > Firmware > Audit** : vérification d'intégrité
      hebdomadaire (cron).
    - Désactiver **tous** les services non utilisés (proxy, captive
      portal, etc.).
    - Logs : syslog TLS vers SIEM **hors-site**, rétention 1 an.
    - **Backups chiffrés** : local + hors-site, test de restauration
      trimestriel (pas de backup non testé = pas de backup).
    - Plan de **disaster recovery** écrit : RTO/RPO, procédure de
      rebuild from scratch, contacts FAI/transits.

### G. Supervision
24. **SNMPv3** → Zabbix : CPU, mémoire, états pf, trafic par interface,
    statut CARP, tunnels.
25. **NetFlow/Insight** : rétention longue (dimensionnement, facturation
    au 95e percentile si revente de bande passante).
26. Alertes : gateways down, CARP MASTER change, Suricata blocks en
    masse, disque > 80 %, certificats < 30 jours (ACME auto normalement).

## Équivalents CLI

```shell
# BGP (FRR)
vtysh -c "show bgp summary"
vtysh -c "show bgp ipv6 unicast summary"
# IPv6
ifconfig | grep inet6
netstat -rn -f inet6 | head
# États pf sous charge
pfctl -s info | grep -i "state-table\|insert"
pfctl -s memory
# Queues shaper
pfctl -s queue -v
# API (exemple lecture d'alias)
# curl -k -u "key:secret" https://fw-mgmt/api/firewall/alias/get
```

## Vérifications
- [ ] `show bgp summary` : 2 sessions Established, préfixes annoncés
      = vos blocs uniquement.
- [ ] Coupure d'un transit : convergence BGP < 2 min, pas de blackhole.
- [ ] IPv6 : 10/10 sur test-ipv6.com, règles IPv6 = miroir des IPv4.
- [ ] Speedtest client : débit = contrat, latence stable (QoS).
- [ ] Attaque simulée (scan nmap) : CrowdSec/Suricata bannissent,
      alerte reçue.
- [ ] Restore : rebuild complet d'un nœud depuis le backup en < 1 h
      (chronométré en exercice).

## Pièges hardcore
- Devenir **transit accidentel** (réannoncer les routes d'un FAI à
  l'autre) → prefix-lists strictes, toujours.
- IPv6 oublié dans les règles → backdoor grande ouverte.
- TLS inspection sans base légale/charte → problème juridique.
  En entreprise : informer et faire signer.
- API sans restriction de privilèges = RCE déguisé. Compte dédié,
  réseau MGMT uniquement.
- pfsync sur lien saturé → états désynchronisés → coupures au
  failover. Lien sync dédié, jamais sur le trunk de production.

---

# Tableau récapitulatif

| Aspect | Niv. 1 | Niv. 2 | Niv. 3 | Hardcore |
|---|---|---|---|---|
| WAN | 1 (DHCP) | 1 (fixe) | 2 + failover | 2 transits BGP |
| HA | non | non | CARP 2 nœuds | CARP + pfsync |
| VLANs | 1 LAN | 3 | 4+ | N clients |
| VPN | non | WireGuard RA | + IPsec sites | + VPN admin dédié |
| DNS | Unbound + DoT | + overrides internes | idem | idem |
| Filtrage | NAT + base | inter-VLAN strict | + Suricata IPS | + Zenarmor + CrowdSec |
| IPv6 | non | optionnel | recommandé | natif dual-stack |
| Routage | statique | statique | statique | BGP (FRR) |
| QoS | non | non | basique | shaper contractuel |
| Supervision | dashboard | dashboard | SNMP + NetFlow | Zabbix + SIEM |
| Backups | manuel | manuel + historique | auto chiffré | auto + test restore |
| 2FA admin | non | oui | oui | oui + VPN admin |

---

*Fin des scénarios. Chaque niveau est déployable tel quel en suivant
les étapes dans l'ordre — et chaque niveau supérieur suppose les
réflexes du précédent (backup avant, tester après, monitorer toujours).*
