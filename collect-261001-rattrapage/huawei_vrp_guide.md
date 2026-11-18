# VRP — Le système d'exploitation transversal Huawei

**Guide technique ultra-complet** — Versatile Routing Platform (VRP)
Public : Zelef, chef de service systèmes & énergies
Équipements couverts : routeurs AR720, switchs S310, pare-feu USG6000
Date de rédaction : 2026-09-27 — Langue : français
Objectif : faire de VRP un réflexe transversal, quel que soit le boîtier.

> Convention de notation dans tout ce guide :
> - `<AR720>` = invite de la **user view** (prompt commençant par `<` et se terminant par `>`).
> - `[AR720]` = invite de la **system view** (prompt entre crochets).
> - `[AR720-GigabitEthernet0/0/0]` = invite d'une **interface view**.
> - Les commandes sont à taper sans les chevrons ni crochets.
> - Le nom d'hôte varie (`<S310>`, `<USG6000>`) mais la logique reste identique.

---

## 1. VRP : qu'est-ce que c'est ?

VRP (Versatile Routing Platform) est le système d'exploitation commun à
quasiment tout le portefeuille réseau Huawei : routeurs (séries AR, NE),
switchs (séries S, CloudEngine), pare-feu (séries USG), contrôleurs WLAN (AC)
et passerelles eKit. Un seul CLI à apprendre, des dizaines de boîtiers
pilotés. C'est l'équivalent de Cisco IOS/IOS-XE côté Huawei.

Pourquoi c'est important pour Zelef : l'AR720 (routage WAN), le S310
(commutation d'accès) et l'USG6000 (sécurité périmétrique) partagent 90 % des
commandes. Apprendre une fois = exploiter trois familles.

## 2. Architecture logicielle de VRP

VRP est un OS temps réel modulaire :

- **Noyau temps réel** : ordonnancement préemptif, gestion mémoire protégée.
- **Processus par fonction** : chaque protocole (OSPF, BGP, SNMP...) tourne
  dans son processus ; un plantage d'un processus ne fait pas tomber le
  système entier.
- **Base de configuration** : la configuration active vit en RAM ; elle doit
  être explicitement sauvegardée en flash (`save`), sinon elle est perdue au
  reboot. C'est LA différence culturelle majeure avec certains OS qui
  sauvegardent en continu.
- **Fichiers système** : le logiciel système est un fichier `.cc` (ex.
  `AR720-V300R022C00SPC500.cc`) ; la configuration est un fichier `.zip`
  (par défaut `vrpcfg.zip`) ; les patchs sont des `.pat`.

## 3. Les grandes versions : VRP5 vs VRP8

| Point | VRP5 (classique) | VRP8 (nouvelle génération) |
|---|---|---|
| Modèle de config | Direct : chaque commande s'applique immédiatement | **Candidate database** : les commandes vont dans une base candidate, il faut `commit` |
| Prompt si non commité | n/a | `[~Nom]` : le tilde `~` signale des changements non commités |
| Commande de validation | `save` | `commit` puis `save` |
| Annulation | `undo <commande>` | `rollback configuration` / `commit` annulé avant validation |
| Équipements | AR, S (anciennes versions), USG6000 | CloudEngine récents, NE, versions VRP8 des AR/S |
| Compatibilité syntaxe | Référence historique | ~95 % identique à VRP5 pour le quotidien |

Règle d'or : regardez toujours `display version` AVANT de travailler.
Si le prompt affiche `[~...]`, vous êtes en VRP8 avec des modifications en
attente — pensez `commit` ou `rollback`.

## 4. Quels équipements utilisent VRP ?

| Famille | Exemples | Rôle typique | VRP |
|---|---|---|---|
| Routeurs d'accès | AR720, AR2200, AR1000V | Routage WAN, VPN, NAT | VRP5/VRP8 |
| Switchs campus | S310, S5735, S6730 | Commutation L2/L3 | VRP5/VRP8 |
| Pare-feu | USG6000, USG6600 | Filtrage, NAT, VPN IPsec | VRP5 |
| Contrôleurs WLAN | AC6508, AC6805 | Gestion des AP | VRP5 |
| Routeurs cœur | NE40E, NE9000 | Cœur de réseau opérateur | VRP8 |
| eKit | Switchs/AP eKit | PME | VRP simplifié |

Dans ce guide, les exemples utilisent l'AR720 comme référence, avec des
notes « S310 » et « USG6000 » quand le comportement diffère.

## 5. Premier contact : la console

Paramètres série (PuTTY, Tera Term, minicom) :

- Débit : **9600 bauds**
- Bits de données : 8
- Bit de stop : 1
- Parité : aucune
- Contrôle de flux : aucun

```text
Câble console (RJ45 ou USB) -> port CONSOLE de l'équipement
PuTTY : Connection type = Serial, Speed = 9600
```

À la première mise sous tension, VRP propose souvent l'**Auto-Config**
(découverte automatique via DHCP). En production, répondez `n` (non) puis
configurez manuellement, sinon les réglages DHCP/VTY seront écrasés.

> ⚠️ Si le message « Warning: Auto-Config is working... Do you want to stop
> Auto-Config? [y/n]: » apparaît, répondez **y** pour reprendre la main.

## 6. Philosophie du CLI : display, undo, quit

Trois verbes gouvernent 80 % du quotidien :

| Verbe | Rôle | Exemple |
|---|---|---|
| `display` | Afficher (jamais `show` !) | `display version` |
| `undo` | Annuler / désactiver (équivalent du `no` Cisco) | `undo shutdown` |
| `quit` | Remonter d'un niveau de vue | `quit` |

```vrp
<S310>display version          # afficher
[S310]undo telnet server enable # désactiver
[S310-GigabitEthernet0/0/1]quit # remonter
```

## 7. La règle du `save` — lisez ceci deux fois

VRP ne sauvegarde **rien** automatiquement (sauf si `autosave` est
configuré, voir section 63). Toute configuration vit en RAM jusqu'au `save`.

```vrp
<AR720>save
  The current configuration will be written to the device.
  Are you sure to continue? (y/n)[n]:y
  Now saving the current configuration to the slot 1.
  Save the configuration successfully.
```

**Incident classique n°1** (voir cas de dépannage n°2) : on configure un
vendredi soir, on oublie `save`, coupure de courant le week-end, tout est
perdu lundi matin. Prenez le réflexe : **chaque session de configuration se
termine par `save` + `display saved-configuration` pour vérifier.**

## 8. Abréviations et complétion

VRP accepte les formes abrégées tant qu'elles sont **uniques** :

```vrp
<AR720>dis ver              # = display version
<AR720>sys                  # = system-view
[AR720]dis cu               # = display current-configuration
[AR720-GigabitEthernet0/0/1]dis th   # = display this
```

Tabulation = complétion automatique. `?` = aide contextuelle (voir
section 20).

## 9. Sensibilité à la casse et espaces

- Les mots-clés sont **insensibles à la casse** (`Display` = `display`),
  mais prenez l'habitude du minuscule.
- Les noms d'objets (sysname, descriptions, noms de VLAN) conservent la
  casse telle que saisie.
- Un espace sépare mot-clé et paramètre : `display?` (collé) affiche l'aide
  sur `display`, `display ?` (espace) liste les sous-options.

## 10. Les invites (prompts) par vue — mémo visuel

```text
<AR720>                              USER VIEW
[AR720]                              SYSTEM VIEW
[AR720-GigabitEthernet0/0/1]         INTERFACE VIEW
[AR720-aaa]                          AAA VIEW
[AR720-ospf-1]                       OSPF VIEW
[AR720-bgp]                          BGP VIEW
[AR720-ui-vty0-4]                    VTY USER-INTERFACE VIEW
[AR720-ui-console0]                  CONSOLE USER-INTERFACE VIEW
[AR720-vlan10]                       VLAN VIEW
[AR720-wlan-view]                    WLAN VIEW (contrôleur/AR avec WLAN)
[AR720-zone-trust]                   SECURITY ZONE VIEW (USG6000)
[AR720-policy-security]              SECURITY POLICY VIEW (USG6000)
[~AR720]                             SYSTEM VIEW avec modifs non commitées (VRP8)
```

## 11. Niveaux d'utilisateurs 0 à 3 et privilèges 0 à 15

VRP distingue **niveau d'utilisateur** (0-3, grandes familles de droits) et
**niveau de privilège de commande** (0-15, granularité fine).

| Niveau utilisateur | Nom | Commandes accessibles |
|---|---|---|
| 0 | Visit | Diagnostic réseau de base : `ping`, `tracert`, `telnet` client, quelques `display` |
| 1 | Monitoring | Niveau 0 + maintenance système : tous les `display` |
| 2 | Configuration | Niveaux 0-1 + configuration des services (routage, IP, VLAN...) |
| 3 | Management | Niveaux 0-2 + tout : fichiers, FTP/TFTP, utilisateurs, `debugging`, `reboot` |

Chaque commande possède un niveau de privilège par défaut (0-15) ; un
utilisateur de niveau N exécute les commandes de niveau ≤ N. On peut
réassigner une commande avec `command-privilege` (voir section 55).

## 12. Où trouver la documentation officielle

- `display ?` et `?` dans chaque vue : la documentation embarquée, toujours
  à jour pour VOTRE version.
- Fiches EDOC sur support.huawei.com (nécessite un compte) : « Command
  Reference » par produit et version — la source de vérité pour la syntaxe
  exacte.
- Les guides CLI PDF (ex. AR720, S310, USG6000 Command Reference) : Zelef
  les a déjà en partie via ses collectes (voir mémoire du projet RAG).

---

## 13. La user view — le point d'entrée

```text
<AR720>
```

C'est la vue d'accueil après login. On y fait de la **supervision** et du
**diagnostic**, pas de la configuration :

```vrp
<AR720>display version
<AR720>display current-configuration
<AR720>ping 8.8.8.8
<AR720>tracert 8.8.8.8
<AR720>display interface brief
<AR720>save
<AR720>reboot
```

Toutes les commandes `display`, `ping`, `tracert`, `debugging`, `save`,
`reboot`, `dir`, `tftp`, `ftp` vivent ici (ou y sont accessibles).

## 14. Entrer dans la system view

```vrp
<AR720>system-view
Enter system view, return user view with Ctrl+Z.
[AR720]
```

Le message est explicite : **Ctrl+Z** ramène directement à la user view,
d'où que vous soyez. C'est le réflexe panique à connaître.

## 15. La system view — le centre de configuration

```text
[AR720]
```

Toutes les configurations globales partent d'ici : `sysname`, `vlan`,
`interface`, `aaa`, `ospf`, `bgp`, `acl`, `ip route-static`, etc.

```vrp
[AR720]sysname AR720-SIEGE
[AR720-SIEGE]vlan 10
[AR720-SIEGE-vlan10]description USERS
[AR720-SIEGE-vlan10]quit
[AR720-SIEGE]quit
<AR720-SIEGE>
```

Notez comment le prompt suit le sysname : après `sysname AR720-SIEGE`, le
prompt devient `[AR720-SIEGE]`.

## 16. Les vues d'interface

```vrp
[AR720]interface GigabitEthernet 0/0/0
[AR720-GigabitEthernet0/0/0]ip address 192.168.1.1 24
[AR720-GigabitEthernet0/0/0]description LIEN_VERS_S310
[AR720-GigabitEthernet0/0/0]undo shutdown
[AR720-GigabitEthernet0/0/0]quit
[AR720]
```

Types d'interfaces rencontrés :

| Famille | Nommage typique | Exemple |
|---|---|---|
| AR720 | `GigabitEthernet0/0/X`, `Eth-Trunk`, `Dialer`, `Tunnel`, `Vlanif` (si switching) | `GE0/0/0` |
| S310 | `GigabitEthernet0/0/X`, `XGigabitEthernet0/0/X`, `Vlanif`, `Eth-Trunk`, `NULL0` | `GE0/0/1` |
| USG6000 | `GigabitEthernet0/0/X`, `XGigabitEthernet`, `Vlanif`, `Tunnel`, `Eth-Trunk` | `GE0/0/0` |

Astuce : `interface ?` en system view liste tous les types supportés par
VOTRE boîtier. Les abréviations `gi`, `xgi`, `Eth-Trunk`/`Eth-Trunk` sont
acceptées si uniques.

## 17. Les vues de protocoles (protocol views)

Chaque protocole de routage a sa vue dédiée :

```vrp
[AR720]ospf 1 router-id 1.1.1.1
[AR720-ospf-1]area 0
[AR720-ospf-1-area-0.0.0.0]network 192.168.1.0 0.0.0.255
[AR720-ospf-1-area-0.0.0.0]quit
[AR720-ospf-1]quit
[AR720]bgp 65001
[AR720-bgp]router-id 1.1.1.1
[AR720-bgp]peer 10.0.0.2 as-number 65002
[AR720-bgp]quit
[AR720]isis 1
[AR720-isis-1]network-entity 49.0001.0010.0100.1001.00
[AR720-isis-1]quit
[AR720]rip 1
[AR720-rip-1]version 2
[AR720-rip-1]network 192.168.1.0
[AR720-rip-1]quit
```

Autres vues de protocoles/services : `[AR720-aaa]`, `[AR720-acl-basic-2000]`,
`[AR720-ip-pool-POOL1]` (DHCP), `[AR720-nqa-admin-test]`.

## 18. Les vues spécifiques USG6000 : zones et politiques de sécurité

Sur l'USG6000, le pare-feu organise tout autour des **zones de sécurité** :

```vrp
[USG6000]firewall zone trust
[USG6000-zone-trust]add interface GigabitEthernet 0/0/1
[USG6000-zone-trust]quit
[USG6000]firewall zone untrust
[USG6000-zone-untrust]add interface GigabitEthernet 0/0/0
[USG6000-zone-untrust]quit
[USG6000]security-policy
[USG6000-policy-security]rule name ALLOW_LAN_TO_WAN
[USG6000-policy-security-rule-ALLOW_LAN_TO_WAN]source-zone trust
[USG6000-policy-security-rule-ALLOW_LAN_TO_WAN]destination-zone untrust
[USG6000-policy-security-rule-ALLOW_LAN_TO_WAN]action permit
[USG6000-policy-security-rule-ALLOW_LAN_TO_WAN]quit
```

Rappel fondamental USG : **sans règle de security-policy, tout est refusé
entre zones** (deny par défaut). Une interface sans zone = inutilisable.

## 19. La vue WLAN (AR avec fonction AC / contrôleurs)

```vrp
[AR720]wlan
[AR720-wlan-view]ap-group name AP-GROUP-SIEGE
[AR720-wlan-ap-group-AP-GROUP-SIEGE]quit
[AR720-wlan-view]regulatory-domain-profile name DOMAIN-FR
[AR720-wlan-view]quit
```

> Sur l'AR720, la fonction WLAN n'existe que si le modèle/licence le permet
> (à vérifier avec `display version` / `display license`).

## 20. Navigation complète : quit, return, Ctrl+Z

| Commande / touche | Effet |
|---|---|
| `quit` | Remonte d'**un** niveau (ou quitte la session en user view) |
| `return` | Retour direct à la **user view**, d'où que vous soyez |
| `Ctrl+Z` | Idem `return` (ne fonctionne que depuis le clavier, pas dans un script) |
| `Ctrl+C` | Interrompt la commande en cours (équivalent Ctrl+C Cisco) |

```vrp
[AR720-ospf-1-area-0.0.0.0]quit      # -> [AR720-ospf-1]
[AR720-ospf-1]quit                   # -> [AR720]
[AR720]interface GE 0/0/0
[AR720-GigabitEthernet0/0/0]return   # -> <AR720> directement
```

## 21. Exécuter une commande display depuis la system view : `run`

Pas besoin de faire `quit` pour un simple `display` :

```vrp
[AR720-ospf-1]run display ospf peer
[AR720-GigabitEthernet0/0/0]run display this
```

Équivalent Cisco : `do show ...` en mode configuration.

## 22. `display this` — afficher la config de la vue courante

```vrp
[AR720-GigabitEthernet0/0/0]display this
#
interface GigabitEthernet0/0/0
 description LIEN_VERS_S310
 ip address 192.168.1.1 255.255.255.0
#
return
```

Indispensable pour vérifier ce qu'on vient de taper sans relire toute la
config. Fonctionne dans **toutes** les vues.

## 23. L'aide en ligne : `?` (aide complète et partielle)

```vrp
<AR720>?
  User view commands:
    arp-ping         ARP-ping
    autosave         Autosave command group
    backup           Backup information
    ...
    display          Display information
    ...
<AR720>display ?
  ...  current-configuration  Current configuration
       cpu-usage              Cpu usage information
       ...
<AR720>display c?
  clock        System clock
  cpu-defend   CPU defend
  cpu-usage    Cpu usage information
  current-configuration  Current configuration
```

- `?` seul : toutes les commandes de la vue.
- `mot ?` : aide sur ce mot-clé.
- `début?` (collé) : tous les mots-clés commençant par ces lettres.

## 24. Raccourcis clavier — tableau de référence

| Touche | Effet |
|---|---|
| `Tab` | Complète la commande si la frappe est unique |
| `?` | Aide contextuelle |
| `↑` / `↓` | Historique des commandes (précédente / suivante) |
| `Ctrl+A` | Curseur au début de la ligne |
| `Ctrl+E` | Curseur en fin de ligne |
| `Ctrl+U` | Efface tout ce qui est à gauche du curseur — à vérifier sur votre version |
| `Ctrl+K` | Interrompt une opération en cours (ex. copie TFTP) |
| `Ctrl+W` | Efface le mot à gauche du curseur |
| `Alt+D` | Efface le mot à droite du curseur |
| `Ctrl+C` | Interrompt la commande en cours |
| `Ctrl+Z` | Retour à la user view |
| `Espace` | Page suivante (quand `--More--` s'affiche) |
| `q` ou `Ctrl+C` | Quitte l'affichage paginé |
| `Entrée` | Ligne suivante dans l'affichage paginé |

> Les combinaisons exactes peuvent varier légèrement selon la version VRP ;
> `Ctrl+A/E/Z/C` sont les plus stables. `display history-command` montre les
> 10 dernières commandes de la session.

## 25. Filtrer les sorties : `| include | begin | exclude`

Comme le `| include` de Cisco :

```vrp
<AR720>display current-configuration | include ospf
<AR720>display current-configuration | begin interface GigabitEthernet0/0/0
<AR720>display logbuffer | exclude INFO
<AR720>display ip routing-table | include 192.168.10.0
```

Options disponibles après `|` : `include`, `exclude`, `begin` (et parfois
`count`). Combinez : `display current-configuration | include vlan`.

## 26. Gérer la pagination : `screen-length`

```vrp
<AR720>screen-length 0 temporary     # désactive la pagination pour la session
<AR720>screen-length 24 temporary    # 24 lignes par page (défaut)
```

Le mot-clé `temporary` limite l'effet à la session courante. En system view,
`screen-length 0` (sans temporary) le rend persistant — utile pour les
scripts de sauvegarde (voir section 93).

## 27. L'historique des commandes

```vrp
<AR720>display history-command
  display version
  display current-configuration
  system-view
  ...
```

Affiche les 10 dernières commandes. Avec `↑`/`↓`, on les rappelle et on les
édite — précieux pour rejouer une commande en changeant un paramètre.

## 28. Les alias de commandes (command alias)

VRP permet de définir des alias pour les admins habitués à Cisco :

```vrp
[AR720]command-alias enable
[AR720]command-alias mapping show display
[AR720]command-alias mapping sh display
```

Après cela, `show version` fonctionne comme `display version`. À utiliser
avec parcimonie en équipe : un alias personnel qui n'existe pas sur les
autres boîtiers crée de la confusion. Documentez-les si vous les utilisez.

## 29. Le mode `diagnose` — zone dangereuse

```vrp
[AR720]diagnose
[AR720-diagnose]display ...
```

La vue `diagnose` donne accès à des commandes de diagnostic bas niveau
(mémoire, processus). **Ne l'utilisez que sur consigne du support Huawei** :
certaines commandes peuvent redémarrer un processus ou l'équipement.

## 30. Résumé navigation — pense-bête

```text
<Nom>  --system-view-->  [Nom]  --interface X-->  [Nom-X]
  ^                         |                         |
  |--- return / Ctrl+Z -----+---- return / Ctrl+Z -----+
  |                         |                         |
  +-- quit (quitte session)-+------ quit -------------+
```

---


## 31. `display version` — la carte d'identité

```vrp
<AR720>display version
Huawei Versatile Routing Platform Software
VRP (R) software, Version 5.170 (AR720 V300R022C00SPC500)
Copyright (C) 2011-2024 Huawei Technologies Co., Ltd.
HUAWEI AR720 uptime is 45 days, 3 hours, 12 minutes
...
```

À relever systématiquement : **version VRP exacte**, **modèle**, **uptime**.
C'est la première commande de tout dépannage et de tout upgrade.

## 32. `display device` et étiquettes électroniques

```vrp
<AR720>display device
<AR720>display device elabel        # numéro de série, version matérielle
<AR720>display device fan           # ventilateurs
<AR720>display device power         # alimentations
<AR720>display device temperature all  # sondes de température
```

`display device elabel` donne le **numéro de série** — indispensable pour
ouvrir un ticket TAC ou vérifier une garantie.

## 33. Interfaces : `display interface` et `display interface brief`

```vrp
<AR720>display interface brief
PHY: Physical
*down: administratively down
(l): loopback
(s): spoofing
(b): BFD down
(e): ETHOAM down
InUti/OutUti: input utility/output utility
Interface                   PHY      Protocol  InUti OutUti   inErrors  outErrors
GigabitEthernet0/0/0        up       up           0%     0%          0          0
GigabitEthernet0/0/1        down     down         0%     0%          0          0
NULL0                       up       up           0%     0%          0          0
```

Deux états à distinguer : **PHY** (couche physique) et **Protocol**
(couche liaison). `up/up` = OK. `down/down` = câble ou distant. `up/down` =
problème de protocole (négociation, encapsulation). `*down` = administrativement
coupé (`shutdown`).

Détail d'une interface :

```vrp
<AR720>display interface GigabitEthernet 0/0/0
GigabitEthernet0/0/0 current state : UP
Line protocol current state : UP
Description:LIEN_VERS_S310
...
    Input:  123456 packets, 78901234 bytes
    Output: 654321 packets, 98765432 bytes
    Input bandwidth utilization  : 0.01%
    ...
```

Surveillez : `input errors`, `CRC`, `collisions`, `ignored` — un compteur
qui grimpe = câble, SFP ou duplex à vérifier.

## 34. Adressage IP : `display ip interface brief`

```vrp
<AR720>display ip interface brief
*down: administratively down
(s): spoofing  (l): loopback
Interface                         IP Address/Mask      Physical   Protocol
GigabitEthernet0/0/0              192.168.1.1/24       up         up
Vlanif10                          192.168.10.1/24      up         up
LoopBack0                         1.1.1.1/32           up         up(s)
NULL0                             unassigned           up         up(s)
```

L'équivalent direct du `show ip interface brief` Cisco. Vérifiez aussi :

```vrp
<AR720>display ip interface GigabitEthernet 0/0/0   # détail d'une interface
```

## 35. Table de routage : `display ip routing-table`

```vrp
<AR720>display ip routing-table
Route Flags: R - relay, D - download to fib
------------------------------------------------------------------------------
Routing Tables: Public
         Destinations : 12       Routes : 12
Destination/Mask    Proto   Pre  Cost      Flags NextHop         Interface
      0.0.0.0/0     Static  60   0           D   192.168.1.254   GigabitEthernet0/0/0
   192.168.1.0/24   Direct  0    0           D   192.168.1.1     GigabitEthernet0/0/0
   192.168.1.1/32   Direct  0    0           D   127.0.0.1       InLoopBack0
```

Colonnes : `Proto` (Static, Direct, OSPF, BGP, ISIS, RIP), `Pre`
(préférence administrative), `Cost` (métrique), `NextHop`, `Interface`.
Filtres utiles :

```vrp
<AR720>display ip routing-table 192.168.20.0 24        # route spécifique
<AR720>display ip routing-table protocol ospf         # que l'OSPF
<AR720>display ip routing-table statistics            # compteurs par protocole
<AR720>display fib                                    # FIB (forwarding)
```

## 36. Voisins OSPF : `display ospf peer` et `display ospf interface`

```vrp
<AR720>display ospf peer
          OSPF Process 1 with Router ID 1.1.1.1
                  Neighbors
 Area 0.0.0.0 interface 192.168.1.1(GigabitEthernet0/0/0)'s neighbors
 Router ID: 2.2.2.2     Address: 192.168.1.2
   State: Full         Mode:Nbr is  Slave  Priority: 1
   DR: 192.168.1.2  BDR: 192.168.1.1  MTU: 0
   ...
<AR720>display ospf peer brief
<AR720>display ospf interface GigabitEthernet 0/0/0
<AR720>display ospf lsdb
<AR720>display ospf routing
<AR720>display ospf error                       # erreurs OSPF (utile en dépannage)
```

État attendu entre voisins : **Full** (ou **2-Way** en DROther). Tout autre
état durable (Init, ExStart, Exchange) = problème (MTU, area-id, hello
timers, authentification).

## 37. BGP : `display bgp peer` et table BGP

```vrp
<AR720>display bgp peer
 BGP local router ID : 1.1.1.1
 Local AS number : 65001
 Total number of peers : 1                 Peers in established state : 1
  Peer            V          AS  MsgRcvd  MsgSent  OutQ  Up/Down       State PrefRcv
  10.0.0.2        4       65002     1234     1230     0  02:15:30 Established      12
<AR720>display bgp routing-table
<AR720>display bgp routing-table 192.168.20.0
```

## 38. ARP : `display arp`

```vrp
<AR720>display arp
IP ADDRESS      MAC ADDRESS     EXPIRE(M) TYPE        INTERFACE   VPN-INSTANCE
                                          VLAN/CEVLAN
------------------------------------------------------------------------------
192.168.1.2     00e0-4c12-3456            I -         GE0/0/0
192.168.1.254   00e0-4c78-9abc  18        D-0         GE0/0/0
------------------------------------------------------------------------------
Total:2         Dynamic:1       Static:0    Interface:1
```

Types : `D` = dynamique, `S` = statique, `I` = interface. Variantes :

```vrp
<AR720>display arp interface GigabitEthernet 0/0/0
<AR720>display arp 192.168.1.2
<AR720>display arp statistics
<AR720>display arp all                      # sur certains modèles : table complète
```

Une entrée ARP manquante pour la passerelle = vérifier le lien L2 et le
VLAN avant tout.

## 39. MAC : `display mac-address`

```vrp
<S310>display mac-address
MAC Address    VLAN/VSI/BD   Learned-From        Type
------------------------------------------------------------------------------
00e0-4c12-3456 10            GE0/0/1             dynamic
00e0-4c78-9abc 10            GE0/0/2             dynamic
------------------------------------------------------------------------------
Total items displayed = 2
<S310>display mac-address interface GigabitEthernet 0/0/1
<S310>display mac-address vlan 10
<S310>display mac-address 00e0-4c12-3456
<S310>display mac-address statistics
```

Sur un switch, c'est LA commande pour savoir **où est branché un
équipement** : on cherche sa MAC, on trouve le port. (Sur l'USG6000, voir
aussi `display firewall session table` pour les sessions.)

## 40. Logs : `display logbuffer` et `display trapbuffer`

```vrp
<AR720>display logbuffer
Logging buffer configuration and contents:enabled
Allowed max buffer size : 1024
Actual buffer size : 256
Channel number : 4 , Channel name : logbuffer
Dropped messages : 0
Overwritten messages : 0
Current messages : 87

Jan 15 2026 14:32:10 AR720 %%01IFNET/4/LINK_STATE(l)[3]:The line protocol IP
on the interface GigabitEthernet0/0/1 has entered the DOWN state.
```

```vrp
<AR720>display logbuffer | include DOWN
<AR720>display logbuffer | begin Jan 15
<AR720>display trapbuffer                    # traps SNMP enregistrés
<AR720>display logbuffer size 512            # (syntaxe à vérifier selon version)
```

Chaque message porte un code `%%01IFNET/4/LINK_STATE` : module / sévérité /
sujet. Sévérités : 0 Emergency → 7 Debug. En dépannage, commencez toujours
par lire les 50 dernières lignes du logbuffer.

## 41. CPU et mémoire : `display cpu-usage`, `display memory`

```vrp
<AR720>display cpu-usage
CPU Usage Stat. Cycle: 60 (Second)
CPU Usage            : 12% Max: 45%@Jan 15 2026 14:00:00
CPU Usage Stat. Time : 2026-01-15 14:35:00
<AR720>display cpu-usage history 60m          # historique (selon modèle)
<AR720>display memory
System Total Memory: 2048 M bytes
Total Memory Used:  612 M bytes
Memory Using Percentage: 29%
<AR720>display memory-usage                   # variante selon version
```

Seuils d'alerte terrain : CPU > 80 % durable = investiguer (`display
cpu-defend`, processus gourmand) ; mémoire > 85 % = risque d'instabilité.

## 42. Informations système : `display clock`, `display users`

```vrp
<AR720>display clock
2026-09-27 01:30:00+02:00 Sunday
Time Zone(DefaultZoneName) : CET
<AR720>display users
  User-Intf    Delay    Type   Network Address     AuthenStatus AuthorcmdStatus
  0   CON 0    00:00:00                                                  pass
  1   VTY 0    00:01:12   SSH   192.168.10.50       pass           no
<AR720>display current-configuration | include sysname
```

`display users` montre qui est connecté (CON = console, VTY = distant) —
utile avant un `reboot` ou un upgrade.

## 43. Diagnostic réseau : `ping` et `tracert`

```vrp
<AR720>ping 192.168.1.254
  PING 192.168.1.254: 56  data bytes, press CTRL_C to break
    Reply from 192.168.1.254: bytes=56 Sequence=1 ttl=64 time=1 ms
    ...
  --- 192.168.1.254 ping statistics ---
    5 packet(s) transmitted
    5 packet(s) received
    0.00% packet loss
    round-trip min/avg/max = 1/1/2 ms
<AR720>ping -a 192.168.1.1 192.168.1.254    # source forcée (-a = source IP)
<AR720>ping -c 100 -s 1400 192.168.1.254    # 100 paquets de 1400 octets
<AR720>tracert 8.8.8.8
<AR720>tracert -a 192.168.1.1 8.8.8.8
```

Options clés : `-a` (adresse source), `-c` (nombre de paquets), `-s`
(taille), `-t` (timeout). Pour tester un MTU / une fragmentation : gros
paquets avec `-s 1500`.

## 44. `debugging` — le debug à la Huawei

```vrp
<AR720>terminal debugging                   # autorise l'affichage des debugs
<AR720>debugging ip icmp
<AR720>debugging ospf packet
PING 192.168.1.2 ...
<AR720>undo debugging all                   # TOUJOURS couper après usage
<AR720>undo terminal debugging
<AR720>display debugging                    # vérifie ce qui est actif
```

Règles d'or :
1. **Fenêtre de maintenance** : un debug peut saturer le CPU.
2. **Cibler** : `debugging ospf packet interface GE0/0/0` plutôt que tout OSPF.
3. **Couper** : `undo debugging all` en fin de session, puis `save` si la
   config `terminal debugging` a été persistée par erreur.
4. Sur console, les debugs s'affichent directement ; en SSH/Telnet il faut
   `terminal monitor` + `terminal debugging`.

## 45. `display diagnostic-information` — le tout-en-un

```vrp
<AR720>display diagnostic-information
```

Génère un **fichier** (ou un long affichage) regroupant version, config,
interfaces, routage, logs, CPU, mémoire... C'est ce que le support Huawei
demande en premier lors d'un ticket. Redirigez vers un fichier pour
l'archiver :

```vrp
<AR720>display diagnostic-information > flash:/diag_2026-09-27.txt
```

(La redirection `>` vers fichier existe sur la plupart des versions VRP —
à vérifier sur votre version exacte.)

## 46. Santé matérielle : alarmes et environnement

```vrp
<AR720>display alarm active                 # alarmes en cours
<AR720>display device alarm hardware        # alarmes matérielles
<AR720>display health                       # état de santé global (selon modèle)
<AR720>display temperature all              # variante : display device temperature all
<AR720>display fan                          # variante : display device fan
<AR720>display power                        # variante : display device power
```

En exploitation, un check hebdomadaire `display alarm active` + température
+ alimentations évite 80 % des pannes « surprises ».

## 47. NTP et horloge

```vrp
<AR720>display ntp-service status
 clock status: synchronized
 clock stratum: 3
 reference clock ID: 192.168.100.1
 nominal frequency: 64.0002 Hz
 ...
<AR720>display ntp-service sessions
```

Sans horloge synchronisée, les logs sont inexploitables en corrélation
d'incidents. Configurez toujours au moins un serveur NTP (voir section 58).

## 48. Sessions et sécurité : qui fait quoi

```vrp
<AR720>display ssh server status            # état du serveur SSH
<AR720>display ssh user-information         # utilisateurs SSH connectés
<AR720>display telnet server status
<AR720>display snmp-agent statistics
<USG6000>display firewall session table     # sessions traversant le pare-feu
<USG6000>display security-policy rule all   # règles effectives
```

## 49. Vérifier la redondance : stacking / cluster

```vrp
<S310>display stack                          # état iStack (S310)
<S310>display stack topology
<S310>display css status                     # CSS sur gros switchs (à vérifier)
<AR720>display vrrp                          # VRRP
<AR720>display vrrp brief
<USG6000>display hrp state                   # HRP (haute dispo USG)
<USG6000>display hrp interface
```

## 50. Tableau récapitulatif des `display` par thème

| Thème | Commandes |
|---|---|
| Système | `display version`, `display device`, `display device elabel`, `display clock`, `display startup` |
| Interfaces | `display interface brief`, `display interface X`, `display ip interface brief` |
| L2 | `display mac-address`, `display vlan`, `display stp brief`, `display eth-trunk` |
| L3 | `display ip routing-table`, `display fib`, `display arp`, `display ip pool` |
| Routage dyn. | `display ospf peer`, `display bgp peer`, `display isis peer` |
| Logs | `display logbuffer`, `display trapbuffer` |
| Perf | `display cpu-usage`, `display memory`, `display health` |
| Sécurité | `display users`, `display ssh server status`, `display acl all` |
| USG | `display firewall session table`, `display security-policy rule all`, `display hrp state` |
| Fichiers | `dir`, `display startup`, `display saved-configuration` |

---


## 51. Configuration minimale d'un équipement neuf

```vrp
<AR720>system-view
[AR720]sysname AR720-SIEGE
[AR720-SIEGE]clock timezone Paris add 02:00:00
[AR720-SIEGE]clock daylight-saving-time Paris repeating 03:00:00 2026-03-29 03:00:00 2026-10-25 one-hour
[AR720-SIEGE]interface GigabitEthernet 0/0/0
[AR720-SIEGE-GigabitEthernet0/0/0]ip address 192.168.1.1 24
[AR720-SIEGE-GigabitEthernet0/0/0]description MGMT
[AR720-SIEGE-GigabitEthernet0/0/0]quit
[AR720-SIEGE]ip route-static 0.0.0.0 0.0.0.0 192.168.1.254
[AR720-SIEGE]quit
<AR720-SIEGE>save
```

## 52. `sysname` et `clock` — les bases

```vrp
[AR720]sysname S310-ETAGE1
[AR720]clock timezone Paris add 01:00:00
[AR720]clock datetime 14:30:00 2026-09-27      # réglage manuel (si pas de NTP)
```

Convention de nommage recommandée (voir section 108) :
`<Type>-<Site>-<Rôle>-<Num>` ex. `AR720-SIEGE-WAN-01`, `S310-ETAGE1-ACC-02`,
`USG6000-SIEGE-FW-01`.

## 53. Utilisateurs locaux et niveaux (AAA)

```vrp
[AR720]aaa
[AR720-aaa]local-user admin password irreversible-cipher MotDePasseFort123!
[AR720-aaa]local-user admin privilege level 15
[AR720-aaa]local-user admin service-type terminal ssh telnet http ftp
[AR720-aaa]local-user operateur password irreversible-cipher AutreMotDePasse456!
[AR720-aaa]local-user operateur privilege level 1
[AR720-aaa]local-user operateur service-type terminal ssh
[AR720-aaa]quit
```

- `irreversible-cipher` : hachage non réversible (préféré à `cipher`).
- `privilege level 15` = droits complets ; `level 1` = supervision seule.
- `service-type` limite les modes d'accès autorisés pour cet utilisateur.

> ⚠️ Ne laissez JAMAIS un compte sans mot de passe ou avec le mot de passe
> par défaut en production. À la première connexion, VRP force souvent la
> définition d'un mot de passe — ne contournez pas cette étape.

## 54. Sécuriser la console et les VTY

```vrp
[AR720]user-interface console 0
[AR720-ui-console0]authentication-mode aaa
[AR720-ui-console0]user privilege level 3
[AR720-ui-console0]idle-timeout 10 0
[AR720-ui-console0]quit
[AR720]user-interface vty 0 4
[AR720-ui-vty0-4]authentication-mode aaa
[AR720-ui-vty0-4]protocol inbound ssh          # SSH uniquement, pas de Telnet
[AR720-ui-vty0-4]user privilege level 3
[AR720-ui-vty0-4]idle-timeout 10 0
[AR720-ui-vty0-4]acl 2000 inbound              # restreint les IP sources (optionnel)
[AR720-ui-vty0-4]quit
```

`idle-timeout 10 0` = déconnexion après 10 minutes d'inactivité (défaut
système : 10 minutes). `authentication-mode aaa` délègue l'authentification
à la base locale (ou RADIUS, voir section 88).

## 55. `command-privilege` — granularité fine

Permet de baisser le niveau requis d'une commande (ex. pour un outil de
sauvegarde qui ne doit pas être admin) :

```vrp
[AR720]command-privilege level 1 view shell display current-configuration
[AR720]command-privilege level 1 view shell screen-length
```

Syntaxe : `command-privilege level <0-15> view <vue> <commande>`.
Après cela, un utilisateur de niveau 1 peut exécuter ces commandes.
Vérifiez avec `display command-privilege` (à vérifier selon version).

## 56. Banner et messages légaux

```vrp
[AR720]header login information "ACCES RESERVE - Service Systemes & Energies"
[AR720]header shell information "Toute connexion est journalisee."
```

Variantes selon version : `banner`, `header login`, `header shell`. Le
banner légal protège juridiquement en cas d'intrusion (mention « accès
réservé »).

## 57. DNS et nom de domaine

```vrp
[AR720]dns resolve
[AR720]dns server 192.168.100.10
[AR720]dns server 192.168.100.11 secondary
[AR720]ip host mon-serveur 192.168.10.50     # entrée statique (à vérifier)
```

## 58. NTP — synchroniser l'horloge

```vrp
[AR720]ntp-service unicast-server 192.168.100.1
[AR720]ntp-service unicast-server 192.168.100.2
[AR720]clock timezone Paris add 01:00:00
[AR720]quit
<AR720>display ntp-service status
```

Sans NTP, impossible de corréler les logs entre AR720, S310 et USG6000.
Pointez tous les équipements vers la même source (serveur interne ou
`pool.ntp.org` si autorisé).

## 59. Le système de fichiers : `dir`, `cd`, `pwd`

```vrp
<AR720>dir
Directory of flash:/
  Idx  Attr     Size(Byte)  Date        Time       FileName
    0  -ro-     84,934,656  Sep 26 2026 22:10:00   AR720-V300R022C00SPC500.cc
    1  -rw-          8,192  Sep 27 2026 01:00:00   vrpcfg.zip
    2  -rw-        123,456  Sep 20 2026 10:00:00   backup_avant_upgrade.zip
    3  drw-              -  Sep 27 2026 00:00:00   logfile
42,123,456 bytes total (28,000,000 bytes free)
<AR720>dir flash:/logfile/
<AR720>pwd
flash:
<AR720>cd logfile/
<AR720>pwd
flash:/logfile
<AR720>cd ..
```

Unités : `flash:` (mémoire interne), `usb0:`/`usbf:` (clé USB si présente),
`sd0:` (carte SD sur certains modèles).

## 60. Lire, copier, renommer, supprimer

```vrp
<AR720>more vrpcfg.zip                         # affiche le contenu (limité pour un .zip)
<AR720>copy flash:/vrpcfg.zip flash:/backup_2026-09-27.zip
<AR720>rename backup_2026-09-27.zip backup_old.zip
<AR720>delete backup_old.zip
Delete flash:/backup_old.zip? [y/n]:y
<AR720>undelete backup_old.zip                 # restaure depuis la corbeille
<AR720>reset recycle-bin                       # vide DÉFINITIVEMENT la corbeille
```

Points clés :
- `delete` envoie dans la **corbeille** (recyclable avec `undelete`).
- `delete /unreserved` ou `reset recycle-bin` = suppression définitive.
- `more` pagine automatiquement le contenu.

## 61. `save` en détail et fichiers de configuration

```vrp
<AR720>save                                     # sauvegarde vers le fichier par défaut
<AR720>save backup_manuelle.zip                # sauvegarde vers un fichier nommé
<AR720>display saved-configuration             # contenu du fichier de démarrage
<AR720>display saved-configuration | include sysname
<AR720>display current-configuration           # config active en RAM
<AR720>display current-configuration interface GigabitEthernet 0/0/0
```

Fichiers en jeu :
- **Fichier de démarrage** : celui chargé au boot (défaut `vrpcfg.zip`,
  modifiable avec `startup saved-configuration`).
- **Config courante** : en RAM, ce que `display current-configuration`
  montre.
- Si les deux diffèrent et qu'on reboot → on perd les modifs non sauvées.

## 62. `compare configuration` — le diff avant/après

```vrp
<AR720>compare configuration
```

Affiche les différences entre la configuration courante (RAM) et la
configuration sauvegardée (fichier de démarrage). À exécuter **avant**
chaque `save` en intervention : on relit ce qu'on s'apprête à figer.

## 63. Sauvegarde automatique : `autosave`

```vrp
[AR720]autosave interval on                    # sauvegarde périodique
[AR720]autosave interval 60                    # toutes les 60 minutes (défaut 1440)
[AR720]autosave time on                        # sauvegarde à heure fixe
[AR720]autosave time 03:00:00                  # tous les jours à 3h00
```

> L'autosave écrit dans le fichier de démarrage configuré. En production,
> préférez une sauvegarde **externalisée** (TFTP/FTP/SFTP, voir section 93)
> plutôt que de compter sur l'autosave local seul.

## 64. Choisir le fichier de démarrage

```vrp
<AR720>startup saved-configuration backup_manuelle.zip
Info: Succeeded in setting the configuration for booting system.
<AR720>display startup
MainBoard:
  Startup system software:        flash:/AR720-V300R022C00SPC500.cc
  Next startup system software:   flash:/AR720-V300R022C00SPC500.cc
  Startup saved-configuration file:       flash:/vrpcfg.zip
  Next startup saved-configuration file:  flash:/backup_manuelle.zip
  ...
```

`display startup` est LE contrôle avant tout reboot/upgrade : il montre ce
qui sera chargé au prochain démarrage (logiciel + config + patch).

## 65. `reset saved-configuration` — repartir de zéro

```vrp
<AR720>reset saved-configuration
  The action will delete the saved configuration in the device.
  Continue? [y/n]:y
<AR720>reboot
  ...
  System will reboot? [y/n]:y      # répondre N à "save" si demandé
```

Séquence de **reset usine** : `reset saved-configuration` puis `reboot`
(en répondant `n` à la sauvegarde proposée). L'équipement redémarre avec la
configuration par défaut.

## 66. `configuration rollback` (VRP8) et restauration manuelle

Sur VRP8 (base candidate) :

```vrp
[~AR720]rollback configuration                 # annule les modifs non commitées
[AR720]display configuration commit list       # historique des commits
[AR720]display configuration commit changes 3  # détail du commit n°3 (à vérifier)
```

Sur VRP5 (sans candidate) : pas de rollback natif — la restauration passe
par le fichier de sauvegarde :

```vrp
<AR720>copy tftp: 192.168.100.50 backup_avant_modif.zip flash:/vrpcfg.zip
<AR720>startup saved-configuration vrpcfg.zip
<AR720>reboot
```

D'où l'importance de la discipline : **sauvegarde externe avant chaque
fenêtre de changement** (voir section 93).

## 67. Espace flash : surveiller et nettoyer

```vrp
<AR720>dir
  42,123,456 bytes total (1,234,567 bytes free)   # <-- presque plein !
<AR720>reset recycle-bin
<AR720>delete /unreserved vieux_fichier.cc
<AR720>format flash:                            # FORMATAGE - EFFACE TOUT, dernier recours
```

Un flash plein bloque le `save` et l'upgrade. En routine : vider la
corbeille, supprimer les vieux `.cc` après validation du nouvel OS, garder
2 générations de config max en local (le reste sur serveur).

## 68. Journaux persistants

```vrp
<AR720>dir flash:/logfile/
<AR720>more flash:/logfile/log.log              # (nom exact à vérifier)
[AR720]info-center logfile enable              # journalisation persistante (à vérifier)
```

Les logs en RAM (`display logbuffer`) disparaissent au reboot ; activez la
journalisation vers fichier et/ou un serveur syslog pour l'historique.

## 69. Syslog distant

```vrp
[AR720]info-center loghost 192.168.100.60
[AR720]info-center loghost 192.168.100.60 facility local7
[AR720]info-center source default channel 2 log state on trap state off debug state off
```

Envoyez les logs vers votre SIEM/supervision (Zabbix, Loki — voir les
guides dédiés de Zelef). C'est non négociable en production.

## 70. Checklist « fin de session de configuration »

```text
[ ] 1. display this                    -> relire chaque vue modifiée
[ ] 2. compare configuration           -> diff RAM vs fichier
[ ] 3. save                            -> figer en flash
[ ] 4. display saved-configuration     -> vérifier le contenu sauvé
[ ] 5. display startup                 -> vérifier le fichier de boot
[ ] 6. Copie externe (TFTP/SFTP)       -> si changement important
```

---


## 71. Upgrade firmware — vue d'ensemble

Un upgrade VRP suit toujours le même cycle :

```text
1. Vérifier la version actuelle et l'espace libre
2. Sauvegarder la configuration (locale + externe)
3. Transférer le nouveau fichier .cc (TFTP/FTP/SFTP/USB)
4. Désigner le fichier pour le prochain boot (startup system-software)
5. Vérifier avec display startup
6. Rebooter en fenêtre de maintenance
7. Vérifier la version et le fonctionnement après reboot
```

Temps typique : 10 à 30 minutes d'indisponibilité par équipement (reboot
inclus). Sur USG6000 en cluster HRP, on upgrade en bascule pour limiter la
coupure (voir section 79).

## 72. Étape 1 : vérifications préalables

```vrp
<AR720>display version                          # version actuelle
<AR720>display startup                          # fichiers de boot actuels
<AR720>display patch-information                # patchs installés (.pat)
<AR720>dir                                      # espace libre : il faut > taille du .cc + marge
<AR720>display device                           # état matériel OK ?
<AR720>display alarm active                     # aucune alarme bloquante ?
```

Checklist pré-upgrade (à cocher) :

```text
[ ] Version cible compatible avec le modèle (notes de version Huawei)
[ ] Chemin d'upgrade supporté (ex. V300R019 -> V300R022 direct ? ou palier ?)
[ ] Fichier .cc téléchargé depuis le site officiel, somme de contrôle vérifiée (MD5/SHA)
[ ] Espace flash suffisant (taille du .cc x 2 idéalement)
[ ] Sauvegarde config : save + copie externe (TFTP/SFTP)
[ ] Fenêtre de maintenance validée, utilisateurs prévenus (display users)
[ ] Plan de rollback écrit (ancien .cc conservé en flash)
[ ] Console accessible (en cas de problème au boot)
```

## 73. Transfert via TFTP (méthode simple)

Côté serveur : un serveur TFTP (tftpd-hpa sous Linux, Tftpd64 sous Windows)
avec le fichier `.cc` dans son répertoire racine.

```vrp
<AR720>ping 192.168.100.50                      # le serveur TFTP doit répondre
<AR720>tftp 192.168.100.50 get AR720-V300R022C00SPC500.cc
  ...
  TFTP: Downloading the file successfully. 84934656 bytes received.
<AR720>dir                                      # vérifier la taille du fichier reçu
```

Limites TFTP : pas d'authentification, UDP (moins fiable sur WAN), limite
historique ~32 Mo par transfert sur certaines implémentations — préférez
FTP/SFTP pour les gros `.cc` (> 100 Mo).

## 74. Transfert via FTP (méthode recommandée en LAN)

```vrp
<AR720>ftp 192.168.100.50
Trying 192.168.100.50 ...
Press CTRL+K to abort
Connected to 192.168.100.50.
220 FTP service ready.
User(192.168.100.50:(none)):ftpuser
331 Password required for ftpuser.
Password:
230 User logged in.
[ftp]get AR720-V300R022C00SPC500.cc
[ftp]bye
221 Server closing.
<AR720>dir
```

En une ligne (si supporté) :

```vrp
<AR720>copy ftp://ftpuser:motdepasse@192.168.100.50/AR720-V300R022C00SPC500.cc flash:/
```

> ⚠️ Le mot de passe en clair dans la commande reste dans l'historique :
> préférez la session FTP interactive, ou SFTP.

## 75. Transfert via SFTP / SCP (méthode sécurisée)

```vrp
[AR720]sftp server enable                       # si l'équipement est serveur (dépôt)
```

Pour **pousser** le fichier depuis un poste Linux vers l'équipement
(l'équipement en serveur SFTP) :

```bash
# Sur le poste Linux :
sftp admin@192.168.1.1
sftp> put AR720-V300R022C00SPC500.cc flash:/
sftp> ls -l
sftp> bye
```

Prérequis côté VRP : utilisateur avec `service-type ssh sftp`, `sftp
server enable`, clés RSA générées (`rsa local-key-pair create`). Vérifiez
la taille après transfert avec `dir`.

## 76. Transfert via clé USB

```vrp
<AR720>dir usb0:/
<AR720>copy usb0:/AR720-V300R022C00SPC500.cc flash:/
```

Pratique quand le réseau est indisponible ou pour un site distant. Formatez
la clé en FAT32 ; certains modèles n'acceptent que des clés ≤ 32 Go
(à vérifier selon modèle).

## 77. Désigner le logiciel de démarrage et rebooter

```vrp
<AR720>startup system-software flash:/AR720-V300R022C00SPC500.cc
Info: Succeeded in setting the software for booting system.
<AR720>display startup
MainBoard:
  Startup system software:        flash:/AR720-V300R019C00SPC200.cc
  Next startup system software:   flash:/AR720-V300R022C00SPC500.cc
  ...
<AR720>save                                     # fige la config actuelle
<AR720>reboot
  Warning: The system will reboot. Continue? [y/n]:y
```

`display startup` doit montrer l'ancien en « Startup » et le nouveau en
« Next startup ». Si ce n'est pas le cas, NE REBOOTEZ PAS.

## 78. Vérifications post-upgrade

```vrp
<AR720>display version                          # la nouvelle version est active ?
<AR720>display startup                          # Startup == Next startup == nouveau .cc
<AR720>display device                           # tout le matériel reconnu ?
<AR720>display interface brief                  # interfaces up ?
<AR720>display ip routing-table                 # routes présentes ?
<AR720>display logbuffer | include -i error     # erreurs au boot ?
<AR720>display patch-information                # patchs réappliqués si besoin
```

Checklist post-upgrade :

```text
[ ] display version = version cible
[ ] Uptime reparti de zéro (normal)
[ ] Config présente (display current-configuration | include sysname)
[ ] Interfaces et protocoles montés (ping passerelle, display ospf peer)
[ ] Supervision OK (SNMP, syslog, Zabbix)
[ ] Ancien .cc conservé en flash pour rollback (ou archivé sur serveur)
[ ] Nettoyage : suppression de l'ancien .cc après 1-2 semaines de stabilité
```

## 79. Patchs logiciels (`.pat`)

Les patchs corrigent des bugs sans changer de version majeure :

```vrp
<AR720>tftp 192.168.100.50 get AR720-V300R022SPH010.pat
<AR720>startup patch flash:/AR720-V300R022SPH010.pat
<AR720>display patch-information
<AR720>reboot                                   # certains patchs exigent un reboot
```

Rollback de patch :

```vrp
<AR720>undo startup patch                       # (syntaxe à vérifier selon version)
<AR720>reboot
```

## 80. Rollback en cas d'échec d'upgrade

Scénario : le nouveau `.cc` ne boote pas ou le comportement est anormal.

**Cas A — l'équipement boote encore (accès CLI OK) :**

```vrp
<AR720>startup system-software flash:/AR720-V300R019C00SPC200.cc   # ancien .cc
<AR720>display startup                          # vérifier
<AR720>reboot
```

**Cas B — l'équipement ne boote plus (BootROM/BootLoad) :**
1. Console, reboot, `Ctrl+B` au prompt « Press Ctrl+B to break auto
   startup ».
2. Mot de passe BootROM (défaut selon version : voir section 83).
3. Menu « Enter ethernet submenu » ou « Enter serial submenu » pour
   recharger un `.cc` via TFTP/FTP en BootROM.
4. « Boot with default mode » avec l'ancien fichier.

**Cas C — la config est incompatible :** restaurez la sauvegarde externe
(`copy tftp: ... vrpcfg.zip` + `startup saved-configuration` + `reboot`).

> Règle : ne supprimez JAMAIS l'ancien `.cc` avant d'avoir validé le
> nouveau en production pendant au moins une semaine.

## 81. Upgrade d'un stack (S310) — principes

1. Uploadez le `.cc` sur le **master** ; il se propage aux membres
   (selon modèle/version — à vérifier : `display stack`).
2. `startup system-software` sur le master.
3. `reboot` du stack (tous les membres redémarrent).
4. Vérifiez que tous les membres sont en `Ready` avec la bonne version :
   `display stack`, `display version` (détail par slot).

Si un membre ne remonte pas : console sur le membre, BootROM, rechargement
manuel du `.cc`.

## 82. Password recovery — principes généraux

Deux situations :
- **On a encore un accès admin** (SSH/Telnet/console avec un autre compte)
  → on change le mot de passe en ligne (méthode douce, sans coupure).
- **On a perdu TOUS les accès** → BootROM/BootLoad/Uboot via la console
  (coupure de service, fenêtre de maintenance).

Méthode douce (accès admin existant) :

```vrp
[AR720]aaa
[AR720-aaa]local-user admin password irreversible-cipher NouveauMotDePasse123!
[AR720-aaa]quit
[AR720]quit
<AR720>save
```

Ou pour la console :

```vrp
[AR720]user-interface console 0
[AR720-ui-console0]set authentication password cipher NouveauMotDePasse123!
[AR720-ui-console0]quit
```

## 83. Password recovery via BootROM/BootLoad (AR720, S310)

1. Reliez-vous en **console** (9600 8N1).
2. `reboot` (ou power cycle).
3. Au message `Press Ctrl+B to break auto startup ...`, pressez **Ctrl+B**
   (certains switchs : **Ctrl+E**).
4. Saisissez le mot de passe BootROM/BootLoad. Valeurs par défaut selon
   version (à vérifier dans le manuel de votre version exacte) :
   - V200R003 et antérieur : `huawei`
   - V200R005 et ultérieur : `Admin@huawei`
5. Dans le menu, choisissez **« Clear password for console user »**.
6. **Important** : choisissez ensuite **« Boot with default mode »**
   (option 1) — NE choisissez PAS « Reboot », sinon l'effacement ne
   s'applique pas.
7. Au démarrage, connectez-vous en console **sans mot de passe**.
8. Redéfinissez immédiatement un mot de passe console + `save` :

```vrp
<AR720>system-view
[AR720]user-interface console 0
[AR720-ui-console0]authentication-mode password
[AR720-ui-console0]set authentication password cipher NouveauMotDePasse123!
[AR720-ui-console0]quit
[AR720]quit
<AR720>save
```

> ⚠️ Tant que vous n'avez pas redéfini de mot de passe, un `reboot`
> referme la porte : refaites la manip. Et changez le mot de passe
> BootROM par défaut (`Modify BootROM password` dans le menu) !

## 84. Password recovery sur USG6000 — principes

Même logique que l'AR720 : console + BootROM/BootLoad + « Clear password
for console user ». Particularités USG :
- L'USG peut avoir un **mot de passe BootLoad** différent (voir la plaque
  / documentation livrée avec l'appareil).
- Si l'USG est en cluster HRP, faites l'opération sur les deux membres
  (ou cassez le cluster temporairement).
- Après récupération, pensez à re-synchroniser : `hrp` + `save` sur les
  deux membres.

> Les options exactes du menu BootLoad varient selon les versions USG ;
> suivez les libellés affichés à l'écran (« Clear password... », « Boot
> with default mode »).

## 85. Sécuriser le BootROM après récupération

```text
Menu BootROM/BootLoad -> "Modify BootROM password" (option 7)
Entrez un mot de passe fort, notez-le dans le coffre de l'équipe.
```

Un BootROM avec mot de passe par défaut = quiconque a accès à la console
peut effacer les mots de passe. C'est un vecteur classique d'escalade
physique.

## 86. Tableau Cisco IOS ↔ Huawei VRP — correspondances essentielles

| # | Cisco IOS | Huawei VRP | Notes |
|---|---|---|---|
| 1 | `show running-config` | `display current-configuration` | Config active (RAM) |
| 2 | `show startup-config` | `display saved-configuration` | Config sauvée (flash) |
| 3 | `show version` | `display version` | Version, uptime |
| 4 | `show ip interface brief` | `display ip interface brief` | État IP des interfaces |
| 5 | `show interfaces status` | `display interface brief` | État PHY/Protocol |
| 6 | `show interfaces Gi0/0` | `display interface GigabitEthernet 0/0/0` | Détail interface |
| 7 | `show ip route` | `display ip routing-table` | Table de routage |
| 8 | `show arp` | `display arp` | Table ARP |
| 9 | `show mac address-table` | `display mac-address` | Table MAC (switch) |
| 10 | `show vlan brief` | `display vlan` | VLANs |
| 11 | `show spanning-tree` | `display stp brief` | STP |
| 12 | `show ip ospf neighbor` | `display ospf peer` | Voisins OSPF |
| 13 | `show ip bgp summary` | `display bgp peer` | Pairs BGP |
| 14 | `show logging` | `display logbuffer` | Logs |
| 15 | `show processes cpu` | `display cpu-usage` | CPU |
| 16 | `show memory` / `show processes memory` | `display memory` | Mémoire |
| 17 | `show users` | `display users` | Sessions connectées |
| 18 | `show clock` | `display clock` | Horloge |
| 19 | `show ntp status` | `display ntp-service status` | NTP |
| 20 | `show inventory` | `display device elabel` | N° de série |
| 21 | `show file systems` / `dir` | `dir` | Contenu flash |
| 22 | `configure terminal` | `system-view` | Mode configuration |
| 23 | `end` / `Ctrl+Z` | `return` / `Ctrl+Z` | Retour user view |
| 24 | `exit` | `quit` | Remonter d'un niveau |
| 25 | `do show ...` | `run display ...` | Display depuis config |
| 26 | `interface Gi0/0` | `interface GigabitEthernet 0/0/0` | Vue interface |
| 27 | `ip address 1.1.1.1 255.255.255.0` | `ip address 1.1.1.1 24` | Masque en CIDR |
| 28 | `no shutdown` | `undo shutdown` | Activer l'interface |
| 29 | `shutdown` | `shutdown` | Couper l'interface |
| 30 | `description ...` | `description ...` | Identique |
| 31 | `hostname X` | `sysname X` | Nom d'hôte |
| 32 | `copy running-config startup-config` | `save` | Sauvegarder |
| 33 | `reload` | `reboot` | Redémarrer |
| 34 | `no <commande>` | `undo <commande>` | Annuler |
| 35 | `username X privilege 15 secret Y` | `local-user X password irreversible-cipher Y` + `local-user X privilege level 15` (vue aaa) | Utilisateur local |
| 36 | `line vty 0 4` | `user-interface vty 0 4` | Lignes VTY |
| 37 | `transport input ssh` | `protocol inbound ssh` (vue vty) | Protocole entrant |
| 38 | `ip ssh version 2` + `crypto key generate rsa` | `rsa local-key-pair create` + `stelnet server enable` | Activer SSH |
| 39 | `snmp-server community X RO` | `snmp-agent community read cipher X` (+ `snmp-agent`) | SNMP v2c |
| 40 | `ntp server 1.2.3.4` | `ntp-service unicast-server 1.2.3.4` | NTP |
| 41 | `ip route 0.0.0.0 0.0.0.0 1.2.3.4` | `ip route-static 0.0.0.0 0.0.0.0 1.2.3.4` | Route statique |
| 42 | `router ospf 1` | `ospf 1` (+ `router-id`) | OSPF |
| 43 | `network 1.0.0.0 0.255.255.255 area 0` | `area 0` puis `network 1.0.0.0 0.255.255.255` (vue area) | Annonce OSPF |
| 44 | `traceroute` | `tracert` | Traceroute |
| 45 | `debug ...` | `debugging ...` (+ `terminal debugging`) | Debug |
| 46 | `undebug all` | `undo debugging all` | Couper debugs |
| 47 | `show | include X` | `display ... \| include X` | Filtre include |
| 48 | `show | begin X` | `display ... \| begin X` | Filtre begin |
| 49 | `terminal length 0` | `screen-length 0 temporary` | Pas de pagination |
| 50 | `write erase` + `reload` | `reset saved-configuration` + `reboot` | Reset usine |

## 87. Pièges de conversion IOS → VRP

1. **`show` n'existe pas** : `show version` → erreur. Réflexe `display`.
   (Ou `command-alias mapping show display`, section 28.)
2. **Le masque en CIDR** : `ip address 192.168.1.1 24`, pas
   `255.255.255.0`.
3. **`save` obligatoire** : IOS `copy run start` est explicite ; en VRP
   l'oubli est encore plus traître car tout *semble* fonctionner jusqu'au
   reboot.
4. **Numérotation des interfaces** : `GigabitEthernet0/0/0` (slot/sous-carte/
   port), pas `Gi0/0`.
5. **OSPF** : le `network` se fait dans la **vue d'area**, pas dans la vue
   du processus.
6. **VLAN d'interface** : `port link-type access` + `port default vlan 10`
   (pas `switchport mode access` / `switchport access vlan 10`).
7. **Trunk** : `port link-type trunk` + `port trunk allow-pass vlan 10 20`.
8. **Agrégation** : `interface Eth-Trunk 1` + `trunkport GE0/0/1` (pas
   `channel-group`).
9. **Niveaux 0-15** : un `privilege level 15` VRP ≈ `privilege 15` IOS,
   mais la granularité par commande (`command-privilege`) n'a pas
   d'équivalent direct simple en IOS.

## 88. Fiche mémo VRP pour un cisconien pressé

```text
display ...        = show ...
system-view        = conf t
quit               = exit
return / Ctrl+Z    = end
undo X             = no X
save               = copy run start
reboot             = reload
sysname            = hostname
user-interface vty = line vty
```

---


## 89. SSH (STelnet) — configuration complète pas à pas

```vrp
[AR720]rsa local-key-pair create
  The key name will be: AR720_Host
  The range of public key size is (512 ~ 2048).
  NOTES: If the key modulus is greater than 512,
         it will take a few minutes.
  Input the bits in the modulus[default = 2048]:2048
  Generating keys...
  ..........................++++++
  ..........................++++++
  .........++++++++
  .........++++++++
[AR720]display rsa local-key-pair public      # vérifie la clé
[AR720]aaa
[AR720-aaa]local-user admin password irreversible-cipher MotDePasseFort123!
[AR720-aaa]local-user admin privilege level 15
[AR720-aaa]local-user admin service-type ssh
[AR720-aaa]quit
[AR720]ssh user admin authentication-type password
[AR720]ssh user admin service-type stelnet
[AR720]stelnet server enable
[AR720]ssh server-source -i Vlanif 10          # restreint l'interface d'écoute (optionnel)
[AR720]user-interface vty 0 4
[AR720-ui-vty0-4]authentication-mode aaa
[AR720-ui-vty0-4]protocol inbound ssh
[AR720-ui-vty0-4]user privilege level 3
[AR720-ui-vty0-4]idle-timeout 10 0
[AR720-ui-vty0-4]quit
[AR720]quit
<AR720>save
<AR720>display ssh server status              # vérification
```

Depuis le poste admin :

```bash
ssh admin@192.168.10.1
# Première connexion : vérifier l'empreinte de la clé !
```

> Si la clé RSA n'existe pas, `stelnet server enable` échoue ou SSH refuse
> les connexions : **toujours générer la clé d'abord**.

## 90. Telnet — à éviter, mais à connaître

```vrp
[AR720]telnet server enable
[AR720]user-interface vty 0 4
[AR720-ui-vty0-4]protocol inbound telnet
[AR720-ui-vty0-4]authentication-mode aaa
[AR720-ui-vty0-4]quit
```

Telnet transmet tout en clair (mots de passe inclus). En production :
**SSH uniquement** (`protocol inbound ssh`). Ne gardez Telnet que pour du
lab isolé ou un équipement ancien sans SSH.

## 91. SNMP v2c — supervision de base

```vrp
[AR720]snmp-agent
[AR720]snmp-agent community read cipher CommunauteLecture123
[AR720]snmp-agent community write cipher CommunauteEcriture456
[AR720]snmp-agent sys-info contact "Zelef - Service Systemes & Energies"
[AR720]snmp-agent sys-info location "Siege - Baie A1"
[AR720]snmp-agent sys-info version v2c
[AR720]snmp-agent target-host trap-hostname ZABBIX trap address udp-domain 192.168.100.60 params securityname CommunauteLecture123 v2c
[AR720]snmp-agent trap source Vlanif 10
```

Vérification depuis le serveur de supervision :

```bash
snmpwalk -v2c -c CommunauteLecture123 192.168.10.1 sysDescr
snmpwalk -v2c -c CommunauteLecture123 192.168.10.1 1.3.6.1.2.1.2.2.1.8  # ifOperStatus
```

## 92. SNMP v3 — la version sécurisée (recommandée)

```vrp
[AR720]snmp-agent
[AR720]snmp-agent sys-info version v3
[AR720]snmp-agent group v3 group-secure privacy
[AR720]snmp-agent usm-user v3 user-secure group-secure
[AR720]snmp-agent usm-user v3 user-secure authentication-mode sha AuthPass123! privacy-mode aes128 PrivPass456!
[AR720]snmp-agent target-host trap-hostname ZABBIX trap address udp-domain 192.168.100.60 params securityname user-secure v3 privacy
```

SNMPv3 = authentification + chiffrement. En 2026, c'est le standard à
viser pour toute supervision en production.

## 93. Accès web (HTTP/HTTPS)

```vrp
[AR720]http server enable
[AR720]http secure-server enable               # HTTPS (recommandé)
[AR720]http server-source -i Vlanif 10
[AR720]aaa
[AR720-aaa]local-user admin service-type http
[AR720-aaa]quit
```

URL : `https://192.168.10.1:8443` (le port exact varie selon modèle —
à vérifier avec `display http server` / `display http secure-server`).
L'interface web est utile pour la visualisation, mais le CLI reste la
référence pour l'exploitation sérieuse et l'automatisation.

## 94. AAA et RADIUS — centraliser l'authentification

```vrp
[AR720]radius-server template RADIUS_SIEGE
[AR720-radius-RADIUS_SIEGE]radius-server shared-key cipher ClePartageeRADIUS123
[AR720-radius-RADIUS_SIEGE]radius-server authentication 192.168.100.70 1812
[AR720-radius-RADIUS_SIEGE]radius-server accounting 192.168.100.70 1813
[AR720-radius-RADIUS_SIEGE]quit
[AR720]aaa
[AR720-aaa]authentication-scheme AUTH_RADIUS
[AR720-aaa-authen-AUTH_RADIUS]authentication-mode radius local
[AR720-aaa-authen-AUTH_RADIUS]quit
[AR720-aaa]domain default
[AR720-aaa-domain-default]authentication-scheme AUTH_RADIUS
[AR720-aaa-domain-default]radius-server RADIUS_SIEGE
[AR720-aaa-domain-default]quit
[AR720-aaa]quit
[AR720]user-interface vty 0 4
[AR720-ui-vty0-4]authentication-mode aaa
[AR720-ui-vty0-4]quit
```

Le `local` en secours (`authentication-mode radius local`) garantit l'accès
si le serveur RADIUS tombe — **ne l'oubliez jamais**, sinon une panne
RADIUS = porte fermée partout.

## 95. Sécuriser les accès distants — durcissement

```vrp
[AR720]acl number 2000
[AR720-acl-basic-2000]rule 5 permit source 192.168.100.0 0.0.0.255
[AR720-acl-basic-2000]rule 10 deny
[AR720-acl-basic-2000]quit
[AR720]user-interface vty 0 4
[AR720-ui-vty0-4]acl 2000 inbound
[AR720-ui-vty0-4]protocol inbound ssh
[AR720-ui-vty0-4]quit
[AR720]undo telnet server enable
[AR720]undo http server enable                  # si le web n'est pas utilisé
[AR720]undo ftp server enable                   # idem FTP
```

Checklist durcissement accès :

```text
[ ] Telnet désactivé (SSH uniquement)
[ ] ACL sur les VTY (plage admin uniquement)
[ ] Mots de passe en irreversible-cipher
[ ] idle-timeout configuré (10 min)
[ ] Banner légal configuré
[ ] Comptes nominatifs (pas de compte générique partagé si possible)
[ ] SNMP v3 ou communauté v2c forte, traps vers supervision
[ ] RADIUS avec secours local
```

## 96. Python + Netmiko sur VRP — prise en main

Netmiko supporte les équipements Huawei via `device_type="huawei"`.

```bash
pip install netmiko
```

```python
from netmiko import ConnectHandler

device = {
    "device_type": "huawei",
    "host": "192.168.10.1",
    "username": "admin",
    "password": "MotDePasseFort123!",   # en prod : variable d'environnement / coffre
    "port": 22,
}

conn = ConnectHandler(**device)
print(conn.send_command("display version"))
print(conn.send_command("display ip interface brief"))
conn.disconnect()
```

Pour la configuration (mode `system-view` géré automatiquement) :

```python
commands = [
    "sysname S310-ETAGE1",
    "vlan 20",
    "description SERVEURS",
]
output = conn.send_config_set(commands)
print(output)
conn.send_command("save")   # NE PAS OUBLIER
conn.disconnect()
```

> ⚠️ Netmiko ne fait pas le `save` à votre place : terminez toujours par
> `send_command("save")` + lecture de la confirmation.

## 97. Envoi de commandes en masse (multi-équipements)

```python
from netmiko import ConnectHandler
from getpass import getpass

PASSWORD = getpass("Mot de passe admin : ")

devices = [
    {"device_type": "huawei", "host": "192.168.10.1", "username": "admin",
     "password": PASSWORD},   # AR720-SIEGE
    {"device_type": "huawei", "host": "192.168.10.2", "username": "admin",
     "password": PASSWORD},   # S310-ETAGE1
    {"device_type": "huawei", "host": "192.168.10.3", "username": "admin",
     "password": PASSWORD},   # USG6000-SIEGE
]

COMMANDS = [
    "ntp-service unicast-server 192.168.100.1",
    "info-center loghost 192.168.100.60",
]

for dev in devices:
    try:
        conn = ConnectHandler(**dev)
        out = conn.send_config_set(COMMANDS)
        conn.send_command("save")
        print(f"[OK] {dev['host']}")
        with open(f"change_{dev['host']}.log", "w") as f:
            f.write(out)
        conn.disconnect()
    except Exception as e:
        print(f"[ERREUR] {dev['host']} : {e}")
```

Bonnes pratiques : un appareil à la fois en dry-run d'abord (`send_command`
de lecture), journaliser chaque sortie, timeout généreux, et **toujours**
un plan de rollback.

## 98. Sauvegarde automatisée des configurations

Script de sauvegarde quotidienne (à mettre en cron sur le serveur admin) :

```python
from netmiko import ConnectHandler
from datetime import date
from getpass import getpass
import os

PASSWORD = getpass()
BACKUP_DIR = "/srv/backups/vrp"
os.makedirs(BACKUP_DIR, exist_ok=True)

devices = {
    "AR720-SIEGE": "192.168.10.1",
    "S310-ETAGE1": "192.168.10.2",
    "USG6000-SIEGE": "192.168.10.3",
}

today = date.today().isoformat()
for name, host in devices.items():
    conn = ConnectHandler(device_type="huawei", host=host,
                         username="admin", password=PASSWORD)
    conn.send_command("screen-length 0 temporary")
    cfg = conn.send_command("display current-configuration")
    path = f"{BACKUP_DIR}/{name}_{today}.cfg"
    with open(path, "w") as f:
        f.write(cfg)
    print(f"[OK] {name} -> {path}")
    conn.disconnect()
```

```cron
# /etc/cron.d/vrp-backup — sauvegarde quotidienne à 2h00
0 2 * * * backup /usr/bin/python3 /opt/scripts/vrp_backup.py >> /var/log/vrp_backup.log 2>&1
```

Rétention : 30 jours glissants + 12 mensuels (règle 3-2-1 : voir le guide
Debian/Ubuntu de Zelef pour la stratégie de sauvegarde globale).

## 99. Ansible et VRP — principes

Ansible n'a pas de module natif Huawei officiel maintenu comme pour
d'autres vendeurs, mais deux approches fonctionnent :

**Approche 1 : `ansible.netcommon.cli_command` / `cli_config`**
(transport `ansible_network_os=huawei` via la collection communautaire
`community.network` — disponibilité à vérifier selon votre version
d'Ansible) :

```yaml
---
- name: Configurer NTP sur les VRP
  hosts: vrp
  gather_facts: false
  vars:
    ansible_connection: network_cli
    ansible_network_os: huawei
    ansible_user: admin
  tasks:
    - name: Appliquer la config NTP
      ansible.netcommon.cli_config:
        config: |
          ntp-service unicast-server 192.168.100.1
          info-center loghost 192.168.100.60
    - name: Sauvegarder
      ansible.netcommon.cli_command:
        command: save
        prompt: "(y/n)"
        answer: "y"
```

**Approche 2 : template + Netmiko via un module custom** — souvent plus
fiable : générez les configs avec Jinja2 côté Ansible, poussez avec un
script Python Netmiko (section 97).

## 100. Inventaire et faits — `display` utiles aux scripts

```bash
# Faits minimaux à collecter par équipement (pour CMDB / inventaire)
display version              # modèle + version VRP + uptime
display device elabel        # numéro de série
display current-configuration # config complète
display startup              # fichiers de boot
display interface brief      # état des ports
```

Astuce parsing : les sorties VRP sont stables d'une version à l'autre pour
les commandes de base — parsez avec des regex simples (`^sysname\s+(\S+)`,
`^(\S+) +up +up` dans `display interface brief`), ou utilisez TextFSM/
ntc-templates (le template `huawei_display_version` existe dans
ntc-templates — à vérifier pour votre version exacte).

---


## 101. Méthodologie de dépannage VRP — les 6 réflexes

```text
1. display version        -> quel boîtier, quelle version, quel uptime ?
2. display logbuffer      -> que s'est-il passé juste avant ?
3. display interface brief -> les liens sont-ils up/up ?
4. display ip routing-table -> la route existe-t-elle ?
5. ping / tracert         -> où s'arrête le trafic ?
6. display current-configuration | include <objet> -> la config est-elle là ?
```

Toujours du plus bas vers le plus haut (physique → liaison → IP →
routage → applicatif). 70 % des incidents se résolvent aux étapes 2 et 3.

---

## 102. Cas n°1 — Interface « down/down »

**Symptômes** : `display interface brief` montre `down/down` ; pas de ping.

**Diagnostic** :
```vrp
<AR720>display interface GigabitEthernet 0/0/1
  GigabitEthernet0/0/1 current state : DOWN
  Line protocol current state : DOWN
<AR720>display logbuffer | include GE0/0/1
```
**Causes probables** : câble débranché/cassé, SFP défectueux ou incompatible,
port distant éteint, `shutdown` configuré (`*down`).

**Solution** :
```vrp
[AR720]interface GigabitEthernet 0/0/1
[AR720-GigabitEthernet0/0/1]display this     # vérifier l'absence de "shutdown"
[AR720-GigabitEthernet0/0/1]undo shutdown
```
Vérifier le voyant du port, tester un autre câble/SFP, vérifier le distant.

## 103. Cas n°2 — Configuration perdue après reboot (`save` oublié)

**Symptômes** : après une coupure, l'équipement a « oublié » les modifs de
la veille ; `display current-configuration` ne montre plus les changements.

**Diagnostic** :
```vrp
<AR720>display startup
  Startup saved-configuration file: flash:/vrpcfg.zip
<AR720>display saved-configuration | include <objet-manquant>   # absent = jamais sauvé
```

**Solution** : reconfigurer + `save`. **Prévention** :
- Checklist de fin de session (section 70).
- `autosave interval on` en filet de sécurité.
- Sauvegarde externe automatisée (section 98) : même sans `save`, la config
  du matin est récupérable dans le backup.

## 104. Cas n°3 — SSH refuse la connexion

**Symptômes** : `ssh admin@192.168.10.1` → « Connection refused » ou timeout.

**Diagnostic** :
```vrp
<AR720>display ssh server status              # serveur actif ?
<AR720>display current-configuration | include stelnet
<AR720>display rsa local-key-pair public      # clé présente ?
<AR720>display users                          # VTY saturés (0-4 occupés) ?
<AR720>display current-configuration | include "user-interface vty" -A 6
```

**Causes / solutions** :
| Cause | Solution |
|---|---|
| Clé RSA absente | `rsa local-key-pair create` |
| `stelnet server enable` manquant | l'ajouter + `save` |
| `protocol inbound telnet` seulement | passer à `protocol inbound ssh` |
| Utilisateur sans `service-type ssh` | `local-user X service-type ssh` |
| ACL sur VTY qui bloque votre IP | ajuster la règle ACL |
| Les 5 VTY occupés | `display users` puis libérer / augmenter la plage |

## 105. Cas n°4 — Upgrade qui échoue (fichier rejeté ou boot en échec)

**Symptômes** : `startup system-software` refuse le fichier, ou l'équipement
boucle au boot après `reboot`.

**Diagnostic** :
```vrp
<AR720>dir                    # taille du .cc : complète ? (comparer à la source)
<AR720>display startup        # le bon fichier est-il en "Next startup" ?
```

**Causes / solutions** :
- Fichier tronqué (TFTP interrompu) → retransférer, vérifier la taille à
  l'octet près, idéalement la somme MD5.
- Mauvais fichier (modèle différent) → vérifier la référence exacte
  (`AR720-...cc` vs `S310-...cc` : NON interchangeables).
- Espace flash insuffisant → nettoyer (`reset recycle-bin`, vieux `.cc`).
- Boot en échec → BootROM (Ctrl+B) → recharger l'ancien `.cc` (section 80).

## 106. Cas n°5 — CPU à 100 %

**Symptômes** : lenteurs CLI, pertes de paquets, `display cpu-usage` > 90 %.

**Diagnostic** :
```vrp
<AR720>display cpu-usage
<AR720>display cpu-usage history 60m          # pic récent ou permanent ?
<AR720>display logbuffer | include -i "attack|storm|loop"
<AR720>display interface brief                # un port avec erreurs qui explosent ?
```

**Causes fréquentes** : boucle de switching (broadcast storm), debug oublié
actif (`display debugging`), attaque (SYN flood), processus SNMP qui
scanne trop vite.

**Solution** :
```vrp
<AR720>undo debugging all
[AR720]undo terminal debugging
```
Puis : casser la boucle (débrancher un lien redondant, vérifier STP :
`display stp brief`), ajuster la supervision, activer `cpu-defend`
(`display cpu-defend policy` — à vérifier selon modèle).

## 107. Cas n°6 — Boucle réseau / tempête de broadcast

**Symptômes** : réseau saturé, voyants qui clignotent frénétiquement, CPU haut
sur les switchs.

**Diagnostic** :
```vrp
<S310>display interface brief    # InUti/OutUti proches de 100 % sur plusieurs ports
<S310>display stp brief          # ports en FORWARDING qui devraient être bloqués ?
<S310>display mac-address        # une MAC qui "flappe" entre plusieurs ports ?
<S310>display logbuffer | include -i "loop|storm"
```

**Solution** : identifier le lien fautif (débranchement séquentiel en
fenêtre de maintenance), activer/durcir STP (`stp enable`, `stp mode mstp`),
configurer `loopback-detect` / `storm-control` sur les ports d'accès :

```vrp
[S310]interface GigabitEthernet 0/0/5
[S310-GigabitEthernet0/0/5]storm-control broadcast min-rate 1000 max-rate 2000
[S310-GigabitEthernet0/0/5]quit
```

## 108. Cas n°7 — Voisin OSPF bloqué (pas « Full »)

**Symptômes** : `display ospf peer` montre `Init`, `ExStart` ou `Exchange`
durable, ou aucun voisin.

**Diagnostic** :
```vrp
<AR720>display ospf peer
<AR720>display ospf error
<AR720>display ospf interface GigabitEthernet 0/0/0
<AR720>display logbuffer | include -i ospf
```

**Checklist OSPF** :
```text
[ ] Même area-id des deux côtés
[ ] Même masque / sous-réseau IP
[ ] Hello/Dead timers identiques (défaut 10/40 s)
[ ] MTU cohérente (un classique en ExStart)
[ ] Authentification identique (ou absente des deux côtés)
[ ] Router-ID unique (pas de doublon !)
[ ] network déclaré dans la bonne area (vue area, pas vue processus)
```

## 109. Cas n°8 — Route manquante dans la table

**Symptômes** : `ping` échoue vers un réseau qui « devrait » être connu.

**Diagnostic** :
```vrp
<AR720>display ip routing-table 192.168.20.0 24
<AR720>display ip routing-table protocol ospf | include 192.168.20
<AR720>display current-configuration | include route-static
```

**Causes** : `ip route-static` mal saisie (next-hop injoignable), redistribution
oubliée en OSPF/BGP, route filtrée par une route-policy, VRF/VPN-instance
incorrecte (regarder la bonne table : `display ip routing-table vpn-instance X`).

## 110. Cas n°9 — Pas d'accès Internet derrière l'USG6000

**Symptômes** : le LAN ne sort pas, alors que l'USG a sa route par défaut.

**Diagnostic** :
```vrp
<USG6000>display security-policy rule all     # une règle trust->untrust en permit ?
<USG6000>display firewall session table       # des sessions se créent-elles ?
<USG6000>display nat outbound                 # le NAT sortant est-il configuré ?
<USG6000>display ip routing-table
```

**Causes classiques** : security-policy manquante (deny par défaut !), NAT
`easy-ip` oublié sur l'interface untrust, interface pas dans la bonne zone.

```vrp
[USG6000]nat-policy
[USG6000-policy-nat]rule name NAT_SORTANT
[USG6000-policy-nat-rule-NAT_SORTANT]source-zone trust
[USG6000-policy-nat-rule-NAT_SORTANT]destination-zone untrust
[USG6000-policy-nat-rule-NAT_SORTANT]action source-nat easy-ip
```

## 111. Cas n°10 — DHCP qui ne distribue plus (S310/AR720)

**Symptômes** : les clients n'obtiennent plus d'adresse.

**Diagnostic** :
```vrp
<AR720>display ip pool                        # pools et adresses restantes
<AR720>display ip pool name POOL1 used        # (syntaxe à vérifier)
<AR720>display dhcp server statistics
<AR720>display logbuffer | include -i dhcp
```

**Causes** : pool épuisé (baux trop longs), `dhcp enable` désactivé, mauvaise
interface (`dhcp select global` vs `interface`), relais DHCP manquant entre
VLANs (`dhcp relay server-ip`).

## 112. Cas n°11 — Mot de passe perdu (sans autre accès)

Voir procédure complète sections 82-84. En résumé : console → reboot →
Ctrl+B → mot de passe BootROM → « Clear password for console user » →
« Boot with default mode » → redéfinir + `save`. **Coupure de service
obligatoire** : planifiez une fenêtre.

## 113. Cas n°12 — `save` impossible (flash plein)

**Symptômes** : `save` échoue avec une erreur d'espace.

**Diagnostic / solution** :
```vrp
<AR720>dir                    # "X bytes free" trop faible
<AR720>reset recycle-bin      # vide la corbeille
<AR720>dir
<AR720>delete /unreserved vieux_backup.zip
<AR720>save
```

Prévention : ne garder que 2 générations de `.cc` et 2 configs en flash,
tout le reste sur serveur.

## 114. Cas n°13 — Vitesse/duplex incohérents (négociation)

**Symptômes** : lien up mais débit anormal, erreurs CRC qui montent.

**Diagnostic** :
```vrp
<AR720>display interface GigabitEthernet 0/0/0 | include -i "speed|duplex|error|CRC"
```

**Solution** : forcer des deux côtés identiquement (ou auto des deux côtés,
jamais un mélange) :

```vrp
[AR720]interface GigabitEthernet 0/0/0
[AR720-GigabitEthernet0/0/0]speed 1000
[AR720-GigabitEthernet0/0/0]duplex full
[AR720-GigabitEthernet0/0/0]undo negotiation auto
```

## 115. Cas n°14 — VLAN qui ne passe pas sur un trunk

**Symptômes** : deux switchs reliés, un VLAN injoignable d'un côté.

**Diagnostic** :
```vrp
<S310>display vlan
<S310>display interface GigabitEthernet 0/0/24   # le trunk
<S310>display port vlan GigabitEthernet 0/0/24   # (à vérifier selon version)
```

**Causes** : VLAN non créé d'un côté, `port trunk allow-pass vlan` incomplet,
`port link-type` resté en `access`/`hybrid` par défaut, native VLAN
différente (PVID).

```vrp
[S310]interface GigabitEthernet 0/0/24
[S310-GigabitEthernet0/0/24]port link-type trunk
[S310-GigabitEthernet0/0/24]port trunk allow-pass vlan 10 20 30
```

## 116. Cas n°15 — Sessions VTY saturées

**Symptômes** : « Too many users » à la connexion SSH/Telnet.

**Diagnostic** :
```vrp
<AR720>display users          # VTY 0-4 tous occupés ?
```

**Solution** : demander aux collègues de se déconnecter, ou en console :
```vrp
<AR720>user-interface vty 5 14               # étendre la plage (si supporté)
```
Mieux : `idle-timeout` court (5-10 min) pour libérer les sessions fantômes.

## 117. Cas n°16 — NTP non synchronisé

**Symptômes** : `display ntp-service status` → `clock status: unsynchronized`.

**Diagnostic** :
```vrp
<AR720>display ntp-service sessions
<AR720>ping <ip-serveur-ntp>
<AR720>display current-configuration | include ntp-service
```

**Causes** : serveur injoignable (route/firewall UDP 123), `clock timezone`
incorrect (l'heure semble fausse mais c'est le fuseau), authentification NTP
requise côté serveur.

## 118. Cas n°17 — Après upgrade : patch manquant / comportement régressé

**Symptômes** : tout semble OK mais un bug corrigé par un `.pat` réapparaît.

**Diagnostic** :
```vrp
<AR720>display patch-information    # le patch est-il toujours actif ?
<AR720>display startup              # "Next startup patch package" renseigné ?
```

**Solution** : les patchs sont liés à une version ; après un upgrade majeur,
retéléchargez le patch correspondant à la NOUVELLE version et réappliquez
(`startup patch` + reboot si requis).

## 119. Cas n°18 — Lenteurs applicatives, pas de panne franche

**Symptômes** : « le réseau est lent » mais rien n'est down.

**Diagnostic** :
```vrp
<AR720>display interface brief          # InUti/OutUti : saturation ?
<AR720>display cpu-usage
<AR720>display ip routing-table         # la route prend-elle le bon chemin ?
<AR720>tracert <destination>           # latence par saut
<AR720>ping -c 100 -s 1400 <destination>  # pertes ?
<USG6000>display firewall session table | include <ip-client>  # sessions USG
```

**Pistes** : lien saturé (QoS à mettre en place), asymétrie de routage,
MTU/fragmentation (tester avec gros pings), session firewall qui expire,
boucle partielle.

---


## 120. Bonnes pratiques d'exploitation — nommage

Convention proposée pour le parc de Zelef :

```text
Sysname : <MODELE>-<SITE>-<ROLE>-<NUM>
  Ex. AR720-SIEGE-WAN-01, S310-ETAGE1-ACC-02, USG6000-SIEGE-FW-01

Description d'interface : <DESTINATION>_<USAGE>_<VLAN>
  Ex. description S310-ETAGE1_UPLINK_TRUNK
  Ex. description PC-BUREAU-042_ACC_VLAN10

VLAN : id + nom fonctionnel
  vlan 10 -> description USERS
  vlan 20 -> description SERVEURS
  vlan 99 -> description MGMT

Règles USG : <ACTION>_<SRC>_TO_<DST>_<SERVICE>
  Ex. ALLOW_LAN_TO_WAN_HTTP
```

Un nom doit permettre à l'astreinte de comprendre **sans ouvrir la doc**.

## 121. Documentation du parc — ce qu'il faut tenir à jour

```text
[ ] Plan d'adressage IP (sous-réseaux, passerelles, DHCP)
[ ] Matrice des VLANs (id, nom, usage, équipements concernés)
[ ] Topologie L2/L3 (ports d'interconnexion, trunks, Eth-Trunk)
[ ] Tableau des équipements (modèle, n° série, version VRP, rôle)
[ ] Mots de passe dans un coffre (jamais dans un fichier clair !)
[ ] Procédures : upgrade, password recovery, rollback (ce guide)
[ ] Journal des changements (qui, quoi, quand, pourquoi)
[ ] Sauvegardes de config datées (automatiques, section 98)
```

## 122. Sauvegardes planifiées — politique type

| Quoi | Fréquence | Où | Rétention |
|---|---|---|---|
| `display current-configuration` (script) | Quotidienne 02h00 | Serveur admin `/srv/backups/vrp/` | 30 j + 12 mensuels |
| `save` + copie `vrpcfg.zip` externe | Avant chaque changement | Serveur admin | Liée au changement |
| Fichier `.cc` en production | À chaque upgrade | Serveur admin (versionné) | Tant que l'OS est en prod + 1 |
| `display diagnostic-information` | Mensuelle | Serveur admin | 12 mois |

Testez la restauration **une fois par an** sur un équipement de lab :
une sauvegarde jamais testée n'est pas une sauvegarde.

## 123. Fenêtres de changement — rituel

```text
AVANT :
[ ] display version / display startup (état de départ noté)
[ ] Sauvegarde externe de la config
[ ] Commandes préparées à l'avance (copier-coller, pas d'impro)
[ ] Rollback écrit (les commandes undo / l'ancien .cc identifié)
[ ] Fenêtre validée, utilisateurs prévenus, console accessible

PENDANT :
[ ] Une modification à la fois, vérification après chacune
[ ] display this après chaque vue modifiée

APRÈS :
[ ] compare configuration -> relecture
[ ] save
[ ] display saved-configuration (échantillon)
[ ] Tests de non-régression (ping passerelles, display ospf peer, applis)
[ ] Journal des changements mis à jour
```

## 124. Supervision minimale à mettre en place

```text
[ ] SNMP (v3 de préférence) vers Zabbix/supervision : uptime, CPU, mémoire,
    état des interfaces, température
[ ] Syslog centralisé (tous les équipements -> même serveur, même NTP)
[ ] Traps SNMP pour : link up/down, changement de config, reboot
[ ] Ping de surveillance des IP de management
[ ] Alerte sur : CPU > 80 %, mémoire > 85 %, température, alarme active
[ ] Sauvegarde de config quotidienne + alerte si échec
```

Commandes VRP associées : sections 91-92 (SNMP), 69 (syslog), 58 (NTP).

## 125. Pense-bête de poche — une page

```text
=================== VRP PENSE-BÊTE ===================
VUES
  <H>  user view (supervision)      [H]  system view (config)
  [H-GE0/0/0] interface             [H-ospf-1] / [H-bgp] protocoles
  quit=remonter  return/Ctrl+Z=user view  run display X (depuis config)

AFFICHER (display, JAMAIS show)
  display version | device elabel | startup | clock
  display interface brief | display ip interface brief
  display ip routing-table | display ospf peer | display bgp peer
  display arp | display mac-address | display vlan
  display logbuffer | display trapbuffer
  display cpu-usage | display memory
  display current-configuration | display saved-configuration
  display users | display ssh server status

CONFIGURER
  system-view | sysname X | interface GE0/0/0 | ip address 1.1.1.1 24
  undo shutdown | description TEXTE | quit
  vlan 10 | port link-type access | port default vlan 10
  ip route-static 0.0.0.0 0 192.168.1.254
  ospf 1 router-id 1.1.1.1 | area 0 | network 192.168.1.0 0.0.0.255

SAUVER / FICHIERS
  save  (OBLIGATOIRE !) | compare configuration
  dir | copy | delete | undelete | reset recycle-bin
  startup saved-configuration X.zip | display startup

DIAGNOSTIC
  ping 1.1.1.1 | ping -a SRC DST | tracert DST
  debugging X + terminal debugging | undo debugging all
  display diagnostic-information

ACCÈS
  rsa local-key-pair create | stelnet server enable
  user-interface vty 0 4 | authentication-mode aaa | protocol inbound ssh
  aaa -> local-user X password irreversible-cipher Y | privilege level 15

UPGRADE
  tftp S get F.cc | startup system-software flash:/F.cc
  display startup (vérifier !) | save | reboot

SECOURS
  Ctrl+B au boot -> Clear password for console user -> Boot with default mode
  reset saved-configuration + reboot = reset usine
=====================================================
```

## 126. Glossaire VRP

| Terme | Signification |
|---|---|
| VRP | Versatile Routing Platform — l'OS Huawei |
| User view | Vue de supervision (`<H>`) |
| System view | Vue de configuration globale (`[H]`) |
| `display` | Commande d'affichage (équivalent `show` Cisco) |
| `undo` | Annule une commande (équivalent `no` Cisco) |
| `save` | Sauvegarde la config RAM vers flash |
| vrpcfg.zip | Fichier de configuration de démarrage par défaut |
| `.cc` | Fichier logiciel système VRP |
| `.pat` | Fichier de patch logiciel |
| BootROM/BootLoad | Menu de démarrage bas niveau (Ctrl+B) |
| VTY | Lignes virtuelles pour accès distant (Telnet/SSH) |
| AAA | Authentication, Authorization, Accounting |
| STelnet | SSH côté Huawei (Secure Telnet) |
| Eth-Trunk | Agrégation de liens (équivalent EtherChannel) |
| Vlanif | Interface virtuelle de VLAN (équivalent SVI Cisco) |
| `startup` | Fichiers utilisés au prochain démarrage |
| Candidate database | Base de config en attente de `commit` (VRP8) |
| `commit` | Valide la candidate database (VRP8) |
| `rollback` | Annule les changements non commités (VRP8) |
| HRP | Huawei Redundancy Protocol (HA des USG) |
| iStack / CSS | Empilage / cluster de switchs Huawei |
| PAF | Fichier de licence/fonctionnalité (à vérifier selon version) |
| info-center | Sous-système de journalisation VRP |
| NQA | Network Quality Analyzer (sondes de supervision) |

## 127. Quiz — 10 questions (réponses en section 128)

**Q1.** Dans quelle vue tape-t-on `display version`, et que se passe-t-il si
on la tape en system view ?

**Q2.** Un collègue configure un VLAN un vendredi soir, tout fonctionne. Le
lundi matin après une coupure électrique, le VLAN a disparu. Pourquoi, et
quelle commande aurait évité cela ?

**Q3.** Traduisez en VRP : `show ip interface brief`, `conf t`,
`no shutdown`, `copy run start`, `reload`.

**Q4.** Sur un USG6000, deux interfaces sont dans les zones `trust` et
`untrust` mais aucun trafic ne passe. Quelle est la cause la plus probable ?

**Q5.** En OSPF, où déclare-t-on le `network` : dans la vue du processus
`ospf` ou dans la vue d'area ? Écrivez la séquence complète.

**Q6.** `display interface brief` affiche `up/down` sur une interface. Que
signifie chaque état, et par où commence-t-on l'investigation ?

**Q7.** SSH refuse les connexions alors que `stelnet server enable` est
configuré. Citez deux causes probables et leur diagnostic.

**Q8.** Avant de rebooter pour un upgrade, quelles deux sections de
`display startup` vérifiez-vous impérativement ?

**Q9.** Quelle est la différence entre `quit`, `return` et `Ctrl+Z` ?

**Q10.** Vous êtes en VRP8 et le prompt affiche `[~AR720]`. Que signifie le
tilde `~`, et que devez-vous faire avant de quitter ?

## 128. Quiz — réponses

**R1.** En **user view** (`<H>`). En system view, il faut `run display
version` (ou `quit` pour revenir en user view). Sans `run`, VRP répond que la
commande est inconnue dans cette vue.

**R2.** Il a oublié `save` : la configuration vivait en RAM et a été perdue
au reboot. Réflexe : terminer chaque session par `save` (+ `display
saved-configuration` pour vérifier).

**R3.** `display ip interface brief` ; `system-view` ; `undo shutdown` ;
`save` ; `reboot`.

**R4.** Il manque une **security-policy** autorisant le trafic
(`trust` → `untrust`, `action permit`). Sur USG, le défaut est **deny entre
zones** : sans règle, rien ne passe.

**R5.** Dans la **vue d'area**. Séquence :
```vrp
[AR720]ospf 1 router-id 1.1.1.1
[AR720-ospf-1]area 0
[AR720-ospf-1-area-0.0.0.0]network 192.168.1.0 0.0.0.255
```

**R6.** PHY `up` = signal physique OK ; Protocol `down` = problème de couche
liaison (négociation, encapsulation, keepalive). On commence par `display
interface X` (détail : speed/duplex/erreurs) puis `display logbuffer`.

**R7.** (1) Clé RSA absente → `display rsa local-key-pair public`, puis
`rsa local-key-pair create`. (2) Utilisateur sans `service-type ssh` →
`display current-configuration | include local-user`, corriger en vue aaa.
Autres : `protocol inbound` restreint à telnet, ACL sur VTY, VTY saturés.

**R8.** « Next startup system software » (le nouveau `.cc` doit y figurer)
et « Next startup saved-configuration file » (le bon fichier de config).
Si ce n'est pas le cas : NE PAS REBOOTER.

**R9.** `quit` remonte d'**un** niveau (et quitte la session en user view) ;
`return` et `Ctrl+Z` retournent **directement** à la user view d'où que
l'on soit.

**R10.** Le `~` signale des modifications **non commitées** dans la
candidate database (VRP8). Il faut `commit` (valider) ou `rollback
configuration` (annuler) avant de partir, puis `save`.

## 129. Pour aller plus loin — par famille d'équipement

- **AR720** : voir le guide `huawei_ar720_guide.md` (routage WAN, VPN
  IPsec, NAT, QoS) — la couche 3 avancée au-delà du socle VRP.
- **S310** : voir `huawei_s310_guide.md` (VLAN, STP/MSTP, Eth-Trunk,
  iStack, PoE) — la commutation campus en détail.
- **USG6000** : voir `huawei_usg6000_guide.md` (zones, security-policy,
  NAT, IPsec, HRP) — la sécurité périmétrique.
- **CLI Huawei** : les Command Reference PDF par produit/version
  (EDOC sur support.huawei.com) restent la source de vérité pour la
  syntaxe exacte — Zelef les a en partie dans son corpus RAG.

## 130. Pour aller plus loin — certifications et pratique

- **HCIA-Datacom** : la certification d'entrée Huawei ; son premier module
  est exactement ce socle VRP (vues, display, config de base).
- **eNSP** (Enterprise Network Simulation Platform) : le simulateur
  gratuit Huawei pour pratiquer VRP sans matériel — idéal pour rejouer les
  cas de dépannage de ce guide.
- Pratique recommandée : pour chaque cas de dépannage (sections 102-119),
  provoquez la panne en lab (débranchez, oubliez le save, cassez l'OSPF) et
  déroulez la méthode. On retient en cassant.

## 131. Erreurs fréquentes des débutants VRP — top 10

1. Taper `show` au lieu de `display`.
2. Oublier `save`.
3. Configurer l'OSPF `network` dans la mauvaise vue.
4. Mettre le masque en décimal pointé (`255.255.255.0`) au lieu du CIDR.
5. Laisser Telnet activé en production.
6. Oublier `undo shutdown` (ou laisser `shutdown` par copier-coller).
7. Ne pas vérifier `display startup` avant un reboot d'upgrade.
8. Supprimer l'ancien `.cc` trop tôt après un upgrade.
9. Configurer une interface USG sans zone de sécurité.
10. Faire un `debugging` en production sans fenêtre de maintenance.

## 132. Modèle de fiche d'intervention (à copier)

```text
FICHE D'INTERVENTION VRP
Date/heure : ............    Intervenant : ............
Équipement : ............    Version VRP : ............
Motif : ....................................................
État avant (display version / display startup / display interface brief) :
........................................................................
Sauvegarde config : [ ] locale (save)  [ ] externe (fichier : ............)
Commandes prévues :
  1. ................................................................
  2. ................................................................
Rollback prévu : .....................................................
Vérifications après :
  [ ] display current-configuration (échantillon)
  [ ] save + display saved-configuration
  [ ] Tests : ping ............  display ospf peer ............
  [ ] Supervision OK (Zabbix/syslog)
Journal des changements mis à jour : [ ] oui
```

## 133. Commandes d'urgence — à connaître par cœur

```vrp
<AR720>reboot                                        # redémarrage
<AR720>display diagnostic-information               # tout-en-un pour le support
<AR720>undo debugging all                           # couper les debugs
[AR720-GE0/0/0]shutdown / undo shutdown             # couper/rétablir un lien
<AR720>reset saved-configuration                    # reset usine (puis reboot)
Ctrl+B au boot                                      # BootROM (password recovery)
```

## 134. Note sur les valeurs « à vérifier »

Ce guide privilégie la syntaxe stable entre versions VRP. Quand une
commande varie selon le modèle ou la version (comportement observé sur
certaines déclinaisons uniquement), elle est marquée **« à vérifier »** :
testez-la sur VOTRE version exacte (`display version`) ou dans la Command
Reference EDOC du produit avant de l'utiliser en production. En cas de
doute, `?` dans la vue concernée tranche toujours.

## 135. Conclusion — l'état d'esprit VRP

VRP récompense la méthode : `display` avant d'agir, `display this` après
chaque vue, `compare configuration` avant `save`, `display startup` avant
`reboot`. Trois boîtiers différents (AR720, S310, USG6000), un seul réflexe.
Avec ce socle, chaque guide spécifique (routage, switching, sécurité)
devient une simple couche de vocabulaire métier posée sur les mêmes fondations.

---

*Fin du guide — VRP, le système d'exploitation transversal Huawei.*
*Bon courage, et n'oubliez jamais : `save` !*
