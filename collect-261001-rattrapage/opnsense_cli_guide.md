# OPNsense — Guide d'administration en CLI (sans l'interface web)

> OPNsense est un firewall/routeur basé sur **FreeBSD** (+ `pf` pour le
> filtrage). Contrairement à Cisco ou Huawei, il n'y a pas de « CLI
> propriétaire » : l'administration en ligne de commande passe par
> **le shell FreeBSD**, l'utilitaire **`configctl`** et **`pfctl`**.
> Ce guide montre comment tout faire au clavier : config, firewall,
> NAT, VPN, diagnostic, mises à jour.
>
> **À jour 2025-2026** : vise OPNsense **24.x / 25.x** (base FreeBSD 14,
> pf, WireGuard/OpenVPN/IPsec via strongSwan, Unbound, Suricata).
> Certaines options du menu console peuvent varier légèrement selon
> la version — la logique reste identique.

---

## 1. Philosophie : comment OPNsense est construit

- **Un seul fichier de vérité** : `/conf/config.xml` contient TOUTE la
  configuration (interfaces, règles, NAT, VPN, utilisateurs…).
- L'interface web n'est qu'un éditeur de ce fichier : chaque « Apply »
  régénère les fichiers de conf des services via **configd**
  (templates) puis recharge les services.
- En CLI, vous avez deux approches :
  1. **Utilitaires dédiés** (`configctl`, `easyrule`, `pfctl`…) — propre ;
  2. **Éditer `/conf/config.xml`** à la main puis recharger — puissant,
     à manier avec précaution (toujours sauvegarder avant).
- Le filtrage est fait par **`pf`** (packet filter de FreeBSD) :
  `pfctl` l'interroge et le pilote en direct.

---

## 2. Accéder au shell

```shell
# En console physique / VGA / série : le menu s'affiche après login root.
# En SSH (une fois activé) : choisissez l'option 8 (Shell).

# Activer SSH (à faire une fois, via web UI ou en éditant config.xml) :
# System > Settings > Administration > Enable Secure Shell.
# En CLI pur, éditez /conf/config.xml, section <system><ssh>... (voir §4).

# Une fois dans le shell :
whoami          # root
uname -a        # FreeBSD 14.x
cat /usr/local/opnsense/version/opnsense.version
```

- Le prompt du menu console propose le choix **8) Shell** pour tomber
  sur un vrai shell `sh`/`csh` en root.
- `exit` depuis le shell **revient au menu console**, pas de déconnexion.

---

## 3. Le menu console (référence)

```
0) Logout                       7) Ping host
1) Assign Interfaces            8) Shell
2) Set interface(s) IP address  9) pfTop (ou pfctl)
3) Reset the root password      10) Firewall Logs (vue live)
4) Reset to factory defaults    11) Reload all services
5) Power off system             12) Firmware update
6) Reboot system                13) Restore a backup
```

- **1)** : réassigner les interfaces physiques (em0, igb0…) aux rôles
  LAN/WAN/OPT. Indispensable après changement de carte réseau.
- **2)** : configurer IP/masque/passerelle/DHCP d'une interface, activer
  le serveur DHCP — le « quick setup » d'une fresh install.
- **3)** : réinitialiser le mot de passe root / admin web.
- **4)** : retour usine (efface `/conf/config.xml` !).
- **12)** : mise à jour du firmware sans web UI (voir §18).
- **13)** : restaurer un backup de config depuis un fichier.

---

## 4. `/conf/config.xml` — le cœur du système

```shell
# Toujours sauvegarder AVANT de toucher :
cp /conf/config.xml /conf/config.xml.bak-$(date +%Y%m%d-%H%M)
cp /conf/config.xml /root/config-backup.xml

# Voir la structure (extraits) :
grep -A5 "<interfaces>" /conf/config.xml | head -30
grep -A3 "<filter>" /conf/config.xml | head -20

# Valider que le XML est bien formé après édition :
xmllint --noout /conf/config.xml && echo "XML OK"
# (xmllint peut ne pas être installé : pkg install -y libxml2)
```

**Workflow d'édition manuelle** :
1. `cp /conf/config.xml /conf/config.xml.bak`
2. Éditez avec `vi` / `ee` (sections `<interfaces>`, `<filter>`,
   `<nat>`, `<ipsec>`, `<openvpn>`, `<unbound>`…).
3. Validez le XML (`xmllint`).
4. Appliquez : rechargez le service concerné via `configctl`
   (voir §5) ou `reboot` si vous avez touché aux interfaces.
5. En cas de problème : restaurez le `.bak` et rechargez.

> Les backups automatiques sont dans `/conf/backup/` (rotatifs).
> L'option 13 du menu console restaure l'un d'eux.

---

## 5. `configctl` — le couteau suisse

`configctl` pilote configd : il liste et exécute les actions des services.

```shell
configctl                       # liste TOUTES les actions disponibles
configctl interface list        # interfaces connues du système
configctl interface reconfigure # réapplique la conf réseau
configctl filter reload         # recharge les règles pf
configctl webgui restart        # redémarre l'interface web
configctl unbound restart       # redémarre le DNS Unbound
configctl dhcpd restart         # redémarre le serveur DHCP
configctl ipsec status          # état des tunnels IPsec
configctl ipsec start           # démarre strongSwan
configctl openvpn status
configctl wireguard status
configctl suricata status
configctl firmware status       # état des mises à jour
```

- Astuce : `configctl <service>` sans argument liste souvent les
  sous-actions du service.
- Après édition manuelle de `config.xml`, c'est `configctl` qui
  « applique » sans reboot.

---

## 6. Interfaces réseau en CLI

```shell
ifconfig                        # toutes les interfaces (état, IP, MAC)
ifconfig em0                    # détail d'une interface
ifconfig -a | grep -A1 "status: active"

# Monter / descendre une interface :
ifconfig em1 up
ifconfig em1 down

# IP temporaire (perdue au reboot si pas dans config.xml !) :
ifconfig em1 inet 192.168.2.1 netmask 255.255.255.0

# Statistiques et erreurs :
netstat -i
netstat -I em0 -w 1             # débit live d'une interface

# Renommer / réassigner durablement : option 1 du menu console,
# ou éditer <interfaces> dans config.xml puis :
configctl interface reconfigure
```

- Noms FreeBSD : `em`, `igb`, `ix` (Intel), `re` (Realtek), `vtnet`
  (virtuel/KVM), `vmx` (VMware).
- Le rôle logique (lan/wan/opt1) est mappé dans `<interfaces>` ;
  `ifconfig` montre les noms physiques.

---

## 7. VLAN, LAGG (agrégat), bridges

```shell
# VLAN 10 sur em1 (temporaire ; durable = config.xml) :
ifconfig em1.10 create vlan 10 vlandev em1
ifconfig em1.10 inet 192.168.10.1 netmask 255.255.255.0 up

# LAGG (équivalent EtherChannel) :
ifconfig lagg0 create
ifconfig lagg0 laggproto lacp laggport em2 laggport em3
ifconfig lagg0 up

# Bridge :
ifconfig bridge0 create
ifconfig bridge0 addm em1 addm em2 up

# Voir :
ifconfig lagg0
ifconfig bridge0
```

- En production, créez-les via `<vlans>`, `<laggs>`, `<bridges>` dans
  `config.xml` (ou la web UI une fois), puis `configctl interface
  reconfigure`.

---

## 8. Routage

```shell
netstat -rn                     # table de routage complète
netstat -rn | grep default      # passerelle par défaut
route -n get 8.8.8.8            # quelle route pour cette destination ?

# Route statique temporaire :
route add -net 192.168.5.0/24 192.168.1.254
route delete -net 192.168.5.0/24 192.168.1.254

# Passerelles configurées (multi-WAN) :
grep -A10 "<gateways>" /conf/config.xml

# Groupes de passerelles (failover / load-balancing) :
grep -A15 "<gateway_groups>" /conf/config.xml
```

- Les routes durables vont dans `<staticroutes>` de config.xml.
- Le routage dynamique (OSPF/BGP) passe par le plugin **FRR**
  (`vtysh` pour la CLI Quagga/FRR une fois installé).

---

## 9. Pare-feu `pf` avec `pfctl` — lecture

```shell
pfctl -sr                       # règles de filtrage actives
pfctl -sn                       # règles NAT actives
pfctl -s state                  # alias : -ss, états/connexions actives
pfctl -ss | head -20
pfctl -s info                   # compteurs globaux pf
pfctl -s Tables                 # tables (alias) chargées
pfctl -t bogons -T show | head  # contenu d'une table
pfctl -s queue                  # files de traffic shaping
pfctl -s labels                 # règles avec labels (utile au debug)

# États pour une IP précise :
pfctl -ss | grep 192.168.1.50

# Compter les états (signe de saturation / attaque) :
pfctl -ss | wc -l
pfctl -s memory
```

---

## 10. Pare-feu `pf` avec `pfctl` — action

```shell
# Tuer les états d'un hôte (utile après changement de règle/NAT) :
pfctl -k 192.168.1.50
pfctl -k 192.168.1.50 -k 0.0.0.0/0   # forme complète

# Recharger les règles depuis la conf OPNsense :
configctl filter reload
# (équivaut à : pfctl -f /tmp/rules.debug après régénération)

# Désactiver / réactiver pf — DANGER à distance (vous vous coupez !) :
pfctl -d                        # disable (console physique uniquement !)
pfctl -e                        # enable

# Vider les tables d'un alias :
pfctl -t mon_alias -T flush
# Ajouter une IP à une table :
pfctl -t mon_alias -T add 203.0.113.7
```

---

## 11. Ajouter des règles firewall en CLI : `easyrule`

```shell
# Bloquer une IP sur WAN :
easyrule block wan 203.0.113.7

# Autoriser un flux précis :
easyrule pass lan tcp 192.168.1.50 203.0.113.7 443

# Syntaxe générale :
# easyrule {block|pass} <interface> <proto> <source> <dest> <port>
easyrule --help
```

- `easyrule` écrit dans `config.xml` (<filter>) puis recharge pf :
  c'est la méthode CLI **propre** pour ajouter une règle.
- Pour des règles complexes (schedules, groupes), éditez `<filter>`
  dans config.xml ou utilisez l'API REST.

---

## 12. NAT en CLI

```shell
pfctl -sn                       # voir les règles NAT actives
pfctl -s nat                    # idem

# Traductions en cours :
pfctl -s state | grep -i ">" | head

# Le NAT outbound se configure dans <nat><outbound> de config.xml ;
# le port-forward dans <nat><rule> :
grep -A12 "<nat>" /conf/config.xml | head -40

# Après modif :
configctl filter reload
pfctl -k 192.168.1.50           # tue les vieux états pour forcer le nouveau NAT
```

- Mode outbound : `automatic` (tout le LAN en PAT sur l'IP WAN) par
  défaut ; passez en `hybrid`/`manual` pour des exceptions.
- Un port-forward = règle NAT **+** règle firewall WAN→destination
  (n'oubliez jamais la seconde).

---

## 13. DHCP en CLI

```shell
# Conf générée :
cat /var/dhcpd/etc/dhcpd.conf | head -40

# Baux actifs :
cat /var/dhcpd/var/db/dhcpd.leases

# Baux via la conf OPNsense :
grep -A8 "<dhcpd>" /conf/config.xml | head -30

# Redémarrer après modif :
configctl dhcpd restart

# Voir qui a quelle IP en live :
arp -a | grep 192.168.1.
```

---

## 14. DNS Unbound en CLI

```shell
# Tester la résolution via l'Unbound local :
drill www.google.com @127.0.0.1
dig @127.0.0.1 www.google.com

# Stats et cache :
unbound-control stats
unbound-control dump_cache | head
unbound-control flush_zone example.com

# Conf générée :
grep -v "^#" /var/unbound/unbound.conf | head -40

# Redémarrer :
configctl unbound restart

# Blocklists DNS (blocklist plugin) :
grep -A5 "<unboundplus>" /conf/config.xml | head -20
```

---

## 15. VPN : IPsec (strongSwan)

```shell
ipsec status                    # tunnels et SA actifs
ipsec statusall                 # détail complet
swanctl --list-sas              # SAs IKEv2 (moderne)
swanctl --list-conns            # connexions configurées
setkey -D                       # SAD (associations de sécurité, IKEv1)

# Logs IPsec en direct :
clog -f /var/log/ipsec.log
# ou : tail -f /var/log/charon.log  (selon version)

# Redémarrer :
configctl ipsec restart
ipsec restart

# La conf vient de <ipsec> dans config.xml :
grep -A10 "<ipsec>" /conf/config.xml | head -30
```

---

## 16. VPN : OpenVPN et WireGuard

```shell
# OpenVPN :
configctl openvpn status
cat /var/etc/openvpn/*.conf 2>/dev/null | head -30
# Logs :
clog -f /var/log/openvpn.log

# WireGuard :
wg show                         # pairs, handshakes, transfert
wg show wg0 peers
wg show wg0 latest-handshakes
# Un handshake > 3 min = pair injoignable (réseau ou clé fausse)

# Redémarrer :
configctl wireguard restart
configctl openvpn restart
```

- WireGuard : vérifiez `Endpoint`, `AllowedIPs` et l'horloge (dérive >
  quelques minutes = échec d'authentification).
- OpenVPN : `verb 3` dans la conf pour des logs exploitables.

---

## 17. IDS/IPS Suricata en CLI

```shell
configctl suricata status
suricata --version
# Règles chargées :
ls /usr/local/etc/suricata/rules/
# Logs d'alertes :
clog -f /var/log/suricata/eve.json | grep alert
# ou : tail -f /var/log/suricata/fast.log

# Recharger après changement de règles :
configctl suricata reload
```

---

## 18. Logs : tout voir en CLI

```shell
# Logs circulaires (clog) — les principaux :
clog /var/log/system.log | tail -50       # système général
clog -f /var/log/filter.log               # firewall en direct (live view)
clog /var/log/filter.log | grep 192.168.1.50
clog /var/log/dhcpd.log | tail -30
clog /var/log/unbound.log | tail -30
clog /var/log/ipsec.log | tail -50
clog /var/log/openvpn.log | tail -30

# Boot et matériel :
dmesg | tail -40
dmesg | grep -i "error\|fail"

# Tous les logs disponibles :
ls -lh /var/log/
```

- `clog -f` = `tail -f` pour les logs circulaires (ne jamais `cat`
  bêtement un fichier clog : utilisez `clog` pour le lire).
- Filtrez par IP/port pour isoler un flux bloqué : c'est l'équivalent
  du « live view » de la web UI.

---

## 19. Monitoring et performance

```shell
top -b | head -20               # CPU / mémoire (mode batch)
systat -vmstat 1                # vue système live (Ctrl+C pour quitter)
vmstat 1 5
netstat -m                      # mémoire mbuf (réseau)
pfctl -s memory                 # mémoire utilisée par pf
pfctl -ss | wc -l               # nombre d'états (table pleine ?)
df -h                           # espace disque
dmesg | grep -i temperature     # températures (selon matériel)
```

- États pf par défaut : limite haute (plusieurs centaines de milliers)
  ; si `pfctl -s info` montre des `state-table` insertions failures =
  table pleine → augmentez dans config.xml ou traquez l'attaque.

---

## 20. Mises à jour firmware en CLI

```shell
# Vérifier les mises à jour :
configctl firmware status
opnsense-update -c              # check only

# Mise à jour (équivalent option 12 du menu) :
opnsense-update -t opnsense

# Voir l'historique :
opnsense-update -l | head

# Kernel / base séparément si besoin :
# opnsense-update -t kernel
# opnsense-update -t base
```

- Toujours un **backup de config** avant (`cp /conf/config.xml …`).
- Après une mise à jour majeure, `reboot` et vérifiez
  `cat /usr/local/opnsense/version/opnsense.version`.

---

## 21. Sauvegarde / restauration en CLI

```shell
# Backup manuel :
cp /conf/config.xml /root/opnsense-backup-$(date +%Y%m%d).xml

# Backups automatiques rotatifs :
ls -lt /conf/backup/ | head

# Exporter hors de la machine :
scp /conf/config.xml admin@192.168.1.100:/backups/

# Restaurer :
cp /root/opnsense-backup-20260101.xml /conf/config.xml
configctl configd reload
# ou option 13 du menu console, ou reboot.

# Chiffrement du backup : la web UI propose un mot de passe ;
# en CLI, chiffrez vous-même :
# openssl enc -aes-256-cbc -in /conf/config.xml -out backup.enc
```

---

## 22. Utilisateurs et mots de passe en CLI

```shell
# Changer le mot de passe root :
passwd

# L'admin web = utilisateur « root » par défaut (section <system>
# de config.xml, <user>). Ajouter un utilisateur en CLI = éditer
# <system><user> dans config.xml (mot de passe hashé bcrypt).

# Mot de passe perdu : option 3 du menu console
# (réinitialise le mot de passe root).

# Voir les utilisateurs configurés :
grep -A6 "<user>" /conf/config.xml | head -30

# Clés SSH autorisées :
cat /root/.ssh/authorized_keys
```

---

## 23. HA / CARP en CLI

```shell
# Interfaces CARP :
ifconfig | grep -A3 carp

# État CARP (MASTER / BACKUP) :
ifconfig carp0 | grep carp:

# Paramètres sysctl :
sysctl net.inet.carp.preempt
sysctl net.inet.carp.allow

# Forcer le basculement (maintenance) :
ifconfig carp0 advskew 254    # se rend moins prioritaire
# puis remettre : ifconfig carp0 advskew 0

# Sync XMLRPC : section <hasync> de config.xml
grep -A10 "<hasync>" /conf/config.xml
```

- Les deux nœuds doivent avoir la **même version** d'OPNsense.
- Testez le failover en heures creuses : coupez le MASTER et
  vérifiez que le BACKUP passe MASTER (`ifconfig carp0`).

---

## 24. Dépannage express en CLI

```shell
# === Pas d'Internet depuis le LAN ===
ifconfig                          # interfaces UP ?
netstat -rn | grep default        # passerelle par défaut ?
drill www.google.com @127.0.0.1   # DNS local OK ?
pfctl -sr | head -30              # règles chargées ?
pfctl -sn                         # NAT présent ?
clog -f /var/log/filter.log       # qui est bloqué, en direct ?
# Test depuis OPNsense lui-même :
ping -c3 8.8.8.8
traceroute 8.8.8.8

# === Un port-forward ne marche pas ===
pfctl -sn | grep <port>           # règle NAT présente ?
pfctl -sr | grep <port>           # règle firewall WAN présente ?
pfctl -ss | grep <ip_serveur>     # états créés ?
# Le serveur a-t-il OPNsense comme passerelle ? (arp -a)

# === VPN IPsec qui ne monte pas ===
ipsec status                      # SA visibles ?
clog -f /var/log/ipsec.log        # erreurs phase 1 / phase 2 ?
# PSK identique ? Proposals compatibles ? UDP 500/4500 autorisés ?

# === WireGuard silencieux ===
wg show wg0 latest-handshakes     # handshake récent ?
# Non -> réseau/clé/endpoint. Oui mais pas de trafic -> AllowedIPs / firewall.

# === Web UI inaccessible ===
configctl webgui restart
sockstat -4 | grep :443            # le port écoute ?
# En dernier recours : option 11 (reload services) ou reboot.
```

---

## 25. Fiche réflexe : les commandes à connaître par cœur

| Besoin | Commande |
|---|---|
| Config complète | `cat /conf/config.xml` (+ backup avant modif) |
| Appliquer la conf | `configctl <service> restart` / `configctl filter reload` |
| Règles firewall actives | `pfctl -sr` |
| NAT actif | `pfctl -sn` |
| Connexions actives | `pfctl -ss` |
| Tuer les états d'une IP | `pfctl -k <ip>` |
| Ajouter une règle simple | `easyrule block\|pass ...` |
| Table de routage | `netstat -rn` |
| Interfaces | `ifconfig` |
| DHCP baux | `cat /var/dhcpd/var/db/dhcpd.leases` |
| DNS test | `drill <nom> @127.0.0.1` |
| IPsec | `ipsec status` / `swanctl --list-sas` |
| WireGuard | `wg show` |
| Logs firewall live | `clog -f /var/log/filter.log` |
| Logs système | `clog /var/log/system.log` |
| Mise à jour | `opnsense-update -t opnsense` |
| Version | `cat /usr/local/opnsense/version/opnsense.version` |

---

*Fin du guide. Règle d'or OPNsense en CLI : `config.xml` est la vérité,
`configctl` applique, `pfctl` vérifie. Sauvegardez avant chaque
modification manuelle, et ne désactivez jamais `pf` à distance.*
