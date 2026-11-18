# Labs eNSP pour former son équipe — Travaux pratiques Huawei corrigés

> **Public cible** : équipe systèmes & énergies manipulant en production des AP361, AP761, switches S310, routeurs AR720 et pare-feu USG6000.
> **Objectif pédagogique** : faire monter l'équipe en compétence sur la CLI VRP Huawei (concepts identiques sur AR/S/USG), le switching, le routage, le NAT, la sécurité et le WLAN, sans toucher au réseau de production.
> **Outil** : Huawei eNSP (Enterprise Network Simulation Platform), simulateur gratuit. Non maintenu officiellement par Huawei, mais reste l'outil standard de lab pour les équipements de campus/PME.
> **Langue** : français. Les commandes VRP sont identiques quel que soit le modèle.

---

## Sommaire

- [1. Installation d'eNSP sur Windows](#1-installation-densp-sur-windows)
- [2. Prise en main d'eNSP](#2-prise-en-main-densp)
- [3. TP1 — VLAN de base sur S3700](#3-tp1--vlan-de-base-sur-s3700)
- [4. TP2 — Trunk 802.1Q entre deux switches](#4-tp2--trunk-8021q-entre-deux-switches)
- [5. TP3 — STP/RSTP : boucles et élection du root](#5-tp3--stprstp--boucles-et-élection-du-root)
- [6. TP4 — Serveur DHCP sur routeur AR](#6-tp4--serveur-dhcp-sur-routeur-ar)
- [7. TP5 — Routage statique et floating static](#7-tp5--routage-statique-et-floating-static)
- [8. TP6 — OSPF mono-zone](#8-tp6--ospf-mono-zone)
- [9. TP7 — NAT : NAPT et port mapping](#9-tp7--nat--napt-et-port-mapping)
- [10. TP8 — PPPoE client et serveur](#10-tp8--pppoe-client-et-serveur)
- [11. TP9 — IPSec site-à-site](#11-tp9--ipsec-site-à-site)
- [12. TP10 — WLAN : AC + AP en mode Fit](#12-tp10--wlan--ac--ap-en-mode-fit)
- [13. TP11 — USG : zones, politiques, NAT sortant](#13-tp11--usg--zones-politiques-nat-sortant)
- [14. TP12 — Dépannage guidé : la topologie cassée](#14-tp12--dépannage-guidé--la-topologie-cassée)
- [15. Quiz final (10 questions corrigées)](#15-quiz-final-10-questions-corrigées)
- [16. Glossaire](#16-glossaire)
- [17. Pour aller plus loin](#17-pour-aller-plus-loin)
- [Annexe A — Aide-mémoire des commandes VRP](#annexe-a--aide-mémoire-des-commandes-vrp)
- [Annexe B — Modèle de fiche de lab vierge](#annexe-b--modèle-de-fiche-de-lab-vierge)

**Conventions utilisées dans ce guide :**

- Les blocs ` ``` ` contiennent des commandes VRP à saisir sur l'équipement indiqué.
- `<Huawei>` = vue utilisateur (user view), `[Huawei]` = vue système (system view), `[Huawei-GigabitEthernet0/0/1]` = vue d'interface.
- `[R1]`, `[SW1]`, `[FW1]`, `[AC1]` indiquent l'équipement concerné.
- `#` en début de ligne = commentaire explicatif, jamais une commande.
- Les adresses IP utilisées suivent le plan : `192.168.x.0/24` par site, avec `.254` = passerelle (routeur).

---

## 1. Installation d'eNSP sur Windows

### 1.1. Ce qu'est eNSP (et ce qu'il n'est pas)

eNSP (Enterprise Network Simulation Platform) est le simulateur réseau gratuit de Huawei, équivalent de Cisco Packet Tracer / GNS3. Il émule de vrais firmwares VRP :

- **Routeurs** : AR201, AR1220, AR2220, AR2240, AR3260 (modèles ARxxxx, proches dans l'esprit de vos AR720).
- **Switches** : S3700 (L2), S5700 (L3, routage inter-VLAN), S9300, CE6800, CE12800.
- **Sécurité** : USG5500, USG6000V (logique identique à vos USG6000, commandes interzone).
- **WLAN** : AC6005/AC6605 + points d'accès AP (AP6010DN, AP2050...) — logique Fit AP identique à vos AP361/AP761.
- **Terminaux** : PC, Laptop, Server, Client, STA (station Wi-Fi), Cloud (pont vers le réseau réel de la machine hôte).

**Limites à connaître avant de commencer :**

- eNSP **n'est plus officiellement maintenu** par Huawei : la dernière version courante est la **V1R2.00.510** (aussi appelée 1.3.00.100). Il n'y aura pas de correctif futur.
- Il **fonctionne uniquement sous Windows** (7/8/10/11).
- Il repose sur **VirtualBox** pour la virtualisation et **WinPcap** pour la capture.
- Les équipements émulés correspondent à des gammes des années 2010 : les concepts (VLAN, OSPF, NAT, zones USG, WLAN) sont **identiques** à vos équipements récents, mais les interfaces physiques et certains détails (noms de vues, limites) diffèrent.
- eNSP ne supporte pas : le PoE réel, le Wi-Fi 6/7 (les AP émulés sont du Wi-Fi 4/5), le stacking iStack complet, EVPN, SD-WAN.

### 1.2. Prérequis matériels et logiciels

| Élément | Recommandation | Commentaire |
|---|---|---|
| OS | Windows 10 ou 11 (64 bits) | Windows 7/8 fonctionnent aussi |
| CPU | 4 cœurs minimum, virtualisation VT-x/AMD-V activée dans le BIOS | Vérifiable avec `systeminfo` (rechercher "Virtualisation activée") |
| RAM | 8 Go minimum, 16 Go conseillés | Chaque équipement émulé consomme 200 à 500 Mo |
| Disque | 5 Go libres (SSD conseillé) | Les images sont incluses dans le package |
| VirtualBox | **5.2.x** (ex. 5.2.44) | Les versions 6.x/7.x provoquent des erreurs de démarrage des nœuds ; eNSP vérifie la version et refuse parfois les versions "trop basses" ou "trop hautes" |
| WinPcap | **4.1.3** | Indispensable pour la capture Wireshark ; Npcap *peut* fonctionner en mode "compatibilité WinPcap" mais WinPcap 4.1.3 reste la valeur sûre |
| Wireshark | Dernière version stable | Optionnel mais très utile pour les TP (capture sur les liens) |
| Droits | Compte administrateur | Installation + exécution |

> **Point d'attention 2025-2026 (vérifié)** : la communauté contourne les problèmes d'installation de deux façons :
> 1. **Couple éprouvé** : Windows 10/11 + VirtualBox 5.2.x + WinPcap 4.1.3 + eNSP V1R2.00.510, le tout lancé **en administrateur**. C'est la combinaison la plus fiable.
> 2. **ISO pré-configurée communautaire** : certains formateurs diffusent une VM VirtualBox contenant eNSP déjà installé (à importer en `.ova`). Pratique pour déployer vite sur plusieurs PC d'équipe, mais à n'utiliser que depuis une source de confiance.
> Les symptômes classiques d'une mauvaise installation : équipements affichés `???` dans la topologie, nœuds qui restent rouges, erreur "VirtualBox version too low" (solution : réinstaller la 5.2.x, chemin d'installation sans caractères accentués ni espaces exotiques).

### 1.3. Conflit Hyper-V / WSL2 : à désactiver impérativement

VirtualBox 5.2.x **ne cohabite pas** avec l'hyperviseur natif de Windows (Hyper-V, WSL2, Credential Guard, Device Guard, isolation du noyau). Symptôme : les équipements eNSP démarrent puis s'arrêtent, ou VirtualBox affiche des erreurs VT-x.

**Procédure (en administrateur) :**

```
# 1. Désactiver les fonctionnalités Windows (GUI) :
# Panneau de configuration > Programmes > Activer/désactiver des fonctionnalités Windows
# Décocher : Hyper-V, Plateforme d'hyperviseur Windows, Plateforme de machine virtuelle,
#            Sous-système Windows pour Linux (si WSL2 n'est pas nécessaire)

# 2. En invite de commandes administrateur, forcer la désactivation de l'hyperviseur :
bcdedit /set hypervisorlaunchtype off

# 3. Redémarrer le PC.
```

> Si vous avez besoin de WSL2/Docker sur la même machine, sachez que c'est **l'un ou l'autre** avec VirtualBox 5.2. Pour un poste de formation dédié eNSP, désactivez tout : c'est le plus simple.

### 1.4. Installation pas à pas

1. **Installer VirtualBox 5.2.x** (exécuter le setup en administrateur). Laisser les options par défaut, y compris l'interface réseau hôte (Host-Only) — elle sert au "Cloud" d'eNSP.
2. **Installer WinPcap 4.1.3** (setup administrateur). Si un antivirus bloque l'installation du pilote, le désactiver temporairement.
3. **Installer eNSP V1R2.00.510** (setup administrateur). Choisir un chemin simple, ex. `C:\eNSP` (éviter les accents et les chemins trop longs).
4. **Installer Wireshark** (optionnel mais recommandé).
5. **Redémarrer** le PC après l'ensemble.

### 1.5. Premier lancement et vérifications

1. Clic droit sur l'icône eNSP > **Exécuter en tant qu'administrateur** (toujours).
2. Menu **Menu > Outils > Options** : vérifier que l'onglet VirtualBox détecte bien la version installée.
3. Créer un nouveau projet : la palette d'équipements à gauche doit afficher routeurs, switches, pare-feu, WLAN, etc.
4. Si un équipement affiche `???` après démarrage : problème d'image VirtualBox — réinstaller eNSP en administrateur, ou vérifier qu'aucun antivirus ne bloque les VM.

### 1.6. Les images : rien à ajouter (c'est inclus)

Contrairement à GNS3, **eNSP embarque déjà les images** des équipements dans son installeur : AR1220/2220/3260, S3700, S5700, USG5500, AC6605, AP... Il n'y a **aucun import d'image à faire**. C'est une des forces d'eNSP pour une formation d'équipe : zéro manipulation de firmware.

### 1.7. Hello World : deux routeurs, un ping

**Objectif** : valider que l'installation fonctionne de bout en bout.

**Topologie :**

```
[R1 : AR2220] GE0/0/0 (192.168.1.1/24) ---- câble Copper ---- GE0/0/0 (192.168.1.2/24) [R2 : AR2220]
```

**Procédure :**

1. Glisser-déposer deux routeurs **AR2220** sur le canevas.
2. Sélectionner l'outil **Copper** (câble), cliquer sur R1 > choisir **GE0/0/0**, cliquer sur R2 > choisir **GE0/0/0**.
3. Bouton **Démarrer** (triangle vert) : les deux routeurs passent au vert après 1 à 2 minutes (le premier boot est long).
4. Clic droit sur R1 > **CLI** : une console s'ouvre.

**Configuration R1 :**

```
<Huawei>system-view
Enter system view, return user view with Ctrl+Z.
[Huawei]sysname R1
[R1]interface GigabitEthernet 0/0/0
[R1-GigabitEthernet0/0/0]ip address 192.168.1.1 24
[R1-GigabitEthernet0/0/0]quit
```

**Configuration R2 :**

```
<Huawei>system-view
[Huawei]sysname R2
[R2]interface GigabitEthernet 0/0/0
[R2-GigabitEthernet0/0/0]ip address 192.168.1.2 24
[R2-GigabitEthernet0/0/0]quit
```

**Test :**

```
[R2]ping 192.168.1.1
  PING 192.168.1.1: 56  data bytes, press CTRL_C to break
    Reply from 192.168.1.1: bytes=56 Sequence=1 ttl=255 time=10 ms
    ...
```

> Si le ping échoue : vérifier que les interfaces sont **up** (`display interface brief`), que les câbles sont bien branchés sur les bonnes interfaces, et que les deux équipements sont démarrés (icône verte).

**Sauvegarder** : Menu > **Fichier > Enregistrer** : le fichier `.topo` contient la topologie *et* les configurations (eNSP sauvegarde les configs dans le .topo). Nommer `hello_world.topo`.

---

## 2. Prise en main d'eNSP

### 2.1. L'interface en 5 zones

1. **Palette d'équipements** (gauche) : catégories Routeur, Switch, Pare-feu, WLAN, Serveur, Terminal, etc. Glisser-déposer sur le canevas.
2. **Canevas** (centre) : la topologie. Molette = zoom, clic droit = menu contextuel d'un équipement.
3. **Barre d'outils** (haut) : Nouveau, Ouvrir, Enregistrer, Démarrer/Arrêter tous les équipements, sélection, ajout de texte/étiquettes, câblage (Copper/Serial/Auto), capture.
4. **Panneau de démarrage** : le bouton triangle vert démarre **tous** les équipements ; le carré rouge les arrête. On peut aussi démarrer/arrêter unitairement par clic droit.
5. **Barre de statut** (bas) : état des équipements.

### 2.2. Ajouter des équipements et câbler

- **Ajout** : glisser l'icône depuis la palette. Renommer immédiatement (clic droit > **Renommer** ou double-clic sur le nom) : `R1`, `SW1`, `FW1`... Un lab bien nommé se dépanne deux fois plus vite.
- **Câblage** : cliquer l'outil câble (ou touche rapide), puis cliquer le premier équipement (choisir l'interface dans la liste), puis le second. Types :
  - **Copper** : Ethernet (cas général).
  - **Serial** : liaisons série (WAN, TP Frame-Relay/PPP si besoin).
  - **Auto** : eNSP choisit (éviter en formation : préférer Copper explicite).
- **Règles de câblage à enseigner dès le début** :
  - Sur un routeur AR2220 d'eNSP : `GE0/0/0` = port de management/diag souvent, préférer `GE0/0/1` et `GE0/0/2` pour les labs (comme dans les TP ci-dessous).
  - Sur un S3700 : `Ethernet0/0/1` à `Ethernet0/0/22` + 2 ports `GE0/0/1-2`.
  - Sur un S5700 : `GE0/0/1` à `GE0/0/24`.
- **Supprimer un câble** : clic droit sur le câble > Supprimer. **Déplacer** : les câbles suivent les équipements.

### 2.3. La console (CLI)

- Clic droit sur un équipement démarré > **CLI** : ouvre la console série.
- Touches utiles : `?` (aide contextuelle), `Tab` (complétion), `Ctrl+Z` (retour en vue utilisateur depuis n'importe quelle vue), flèches haut/bas (historique).
- **Modes de vue** :
  - `<R1>` : user view — commandes `display`, `ping`, `save`.
  - `[R1]` : system view (via `system-view`) — toute la configuration.
  - `[R1-GigabitEthernet0/0/1]` : interface view.
- **Enregistrer la config** : `save` (répondre `y`). Sans `save`, la config est perdue à l'arrêt (eNSP peut aussi l'embarquer dans le .topo selon l'option, mais `save` reste le réflexe à enseigner).

### 2.4. Capture Wireshark sur un lien

1. L'équipement doit être démarré.
2. Clic droit sur le **câble** > **Démarrer la capture** (ou bouton capture dans la barre d'outils puis clic sur le câble).
3. Wireshark s'ouvre et capture en direct : idéal pour voir les hello OSPF, les DTP/CDP (absents chez Huawei), les BPDU STP, les échanges DHCP (Discover/Offer/Request/ACK), les paquets IKE/IPSec.
4. Arrêter : clic droit sur le câble > **Arrêter la capture**.

> **Astuce d'animateur** : la capture est l'outil pédagogique le plus puissant d'eNSP. Pour chaque TP, prévoir un moment "regardons les paquets" : BPDU (TP3), DHCP (TP4), Hello OSPF (TP6), IKE (TP9).

### 2.5. Sauvegarde : le fichier .topo

- **Fichier > Enregistrer** (`.topo`) : contient le placement, le câblage, les noms **et** les configurations des équipements.
- **Bonne pratique d'équipe** : un dossier par TP (`TP01_VLAN/`, `TP02_Trunk/`...), avec `sujet.md` (énoncé) + `topologie.topo` (topologie de départ, équipements vierges ou pré-câblés) + `correction.md`.
- **Partage** : un `.topo` s'ouvre sur n'importe quel poste eNSP de même version. Pour distribuer un TP "à compléter", enregistrer le .topo **après** avoir fait `reset saved-configuration` + `reboot` sur chaque équipement (ou simplement ne jamais faire `save` sur la topologie de départ).

### 2.6. L'objet "Cloud" : pont vers le réel

L'équipement **Cloud** permet de relier eNSP au réseau de la machine hôte (carte physique ou interface loopback) : utile pour, par exemple, faire sortir un ping du lab vers le LAN du formateur, ou brancher un vrai PC. Configuration : clic droit sur le Cloud > **Paramètres**, lier une interface UDP ou une carte réseau à un port du Cloud, puis câbler comme un équipement normal. (Non utilisé dans les TP ci-dessous, mais à connaître.)

### 2.7. Réflexes à faire acquérir dès cette séance

1. Toujours nommer les équipements (`sysname` + renommage graphique).
2. Toujours vérifier l'état des interfaces (`display interface brief`) avant de configurer.
3. `save` en fin de TP.
4. Capturer un lien pour "voir" un protocole au moins une fois par TP.
5. Documenter : copier la config finale (`display current-configuration`) dans un fichier texte.

**Durée estimée de la séance** : 1 h 30 (installation déjà faite en amont).
**Barème** : non noté — séance de découverte, validation par le "hello world" fonctionnel.

---

## 3. TP1 — VLAN de base sur S3700

### Objectif
Comprendre la notion de VLAN : segmentation logique d'un switch, ports access, isolation entre VLAN, communication intra-VLAN.

### Prérequis
Séance de prise en main (section 2). Savoir ouvrir une console et passer en `system-view`.

### Topologie

```
                    ┌─────────────────────────┐
  PC1 (192.168.10.11/24) ──E0/0/1 │                         │ E0/0/3── PC3 (192.168.20.13/24)
  PC2 (192.168.10.12/24) ──E0/0/2 │      SW1 (S3700)        │ E0/0/4── PC4 (192.168.20.14/24)
                    └─────────────────────────┘
```

- 1 switch **S3700** nommé `SW1`.
- 4 **PC** : PC1, PC2 (VLAN 10, réseau 192.168.10.0/24), PC3, PC4 (VLAN 20, réseau 192.168.20.0/24).
- Câblage : E0/0/1→PC1, E0/0/2→PC2, E0/0/3→PC3, E0/0/4→PC4.
- Configuration IP des PC : double-clic sur le PC > onglet de configuration IPv4 (passerelle : laisser vide pour ce TP, pas de routage).

### Énoncé (à distribuer aux stagiaires)

1. Créer les VLAN 10 (nom : `USERS`) et 20 (nom : `GUESTS`) sur SW1.
2. Affecter E0/0/1 et E0/0/2 en **access VLAN 10**, E0/0/3 et E0/0/4 en **access VLAN 20**.
3. Configurer les IP des 4 PC selon le plan d'adressage.
4. Tester : PC1 ping PC2 (doit réussir), PC1 ping PC3 (doit échouer — pourquoi ?).
5. Vérifications : afficher la table VLAN et l'état des ports.

### Correction pas à pas

**Sur SW1 :**

```
<Huawei>system-view
[Huawei]sysname SW1
[SW1]vlan 10
[SW1-vlan10]description USERS
[SW1-vlan10]quit
[SW1]vlan 20
[SW1-vlan20]description GUESTS
[SW1-vlan20]quit

# Affectation des ports access (méthode par batch, à enseigner)
[SW1]port-group group-member Ethernet 0/0/1 to Ethernet 0/0/2
[SW1-port-group]port link-type access
[SW1-port-group]port default vlan 10
[SW1-port-group]quit

[SW1]port-group group-member Ethernet 0/0/3 to Ethernet 0/0/4
[SW1-port-group]port link-type access
[SW1-port-group]port default vlan 20
[SW1-port-group]quit

[SW1]save
```

**Sur les PC** (double-clic > configuration) :

| PC | IP | Masque |
|---|---|---|
| PC1 | 192.168.10.11 | 255.255.255.0 |
| PC2 | 192.168.10.12 | 255.255.255.0 |
| PC3 | 192.168.20.13 | 255.255.255.0 |
| PC4 | 192.168.20.14 | 255.255.255.0 |

**Tests** (clic droit sur PC > invite de commande, ou double-clic) :

```
PC1> ping 192.168.10.12     # OK : même VLAN, même sous-réseau
PC1> ping 192.168.20.13     # ECHEC : VLAN différents = broadcast domains séparés
PC3> ping 192.168.20.14     # OK
```

### Vérifications

```
[SW1]display vlan
# Affiche les VLAN créés et les ports membres (untagged)

[SW1]display port vlan
# Vue synthétique : Port | Link Type | PVID | VLAN List

[SW1]display mac-address
# Table MAC apprise par VLAN : constater que PC1/PC2 sont en VLAN 10, PC3/PC4 en VLAN 20
```

### Pièges classiques

1. **Oublier `port default vlan`** : le port reste en VLAN 1, les PC ne se voient pas. Diagnostic : `display port vlan` montre PVID=1.
2. **Confondre VLAN et sous-réseau** : mettre PC1 en 192.168.20.x tout en le laissant en VLAN 10 ne fera pas communiquer avec le VLAN 20. Le VLAN isole au niveau 2, l'IP au niveau 3 : il faut les deux alignés.
3. **PC mal configuré** : masque oublié (eNSP met parfois 0.0.0.0) — toujours revérifier la fiche IP du PC.
4. **VLAN créé mais port en `hybrid` par défaut ?** Non : sur VRP les ports sont `hybrid`... en fait sur les S Huawei, le type par défaut est **hybrid** avec PVID 1. Le passer explicitement en `access` évite toute ambiguïté — c'est pourquoi la correction le fait.

### Barème indicatif (20 points)

| Critère | Points |
|---|---|
| VLAN 10/20 créés et nommés | 4 |
| Ports en access dans les bons VLAN | 6 |
| Adressage IP des PC correct | 4 |
| Tests ping conformes aux attendus + explication de l'échec inter-VLAN | 4 |
| Vérifications `display` produites | 2 |

### Durée estimée
**45 minutes** (dont 10 min de démo animateur).

### Fiche animateur — points à insister
- Un VLAN = un domaine de broadcast = un sous-réseau IP (en pratique d'entreprise).
- Faire le parallèle avec le terrain : sur vos S310, les VLAN séparent typiquement `DATA`, `VOIX` (téléphonie), `WIFI`, `GESTION`.
- Montrer `display mac-address` : la table MAC est **par VLAN**, c'est la preuve concrète de la segmentation.
- Question piège à poser : "PC1 en 192.168.10.11/24 et PC3 en 192.168.10.13/24 mais PC3 branché sur un port VLAN 20 : ping ?" → Échec, car le tag VLAN bloque au niveau 2 avant même l'IP.

---

## 4. TP2 — Trunk 802.1Q entre deux switches

### Objectif
Transporter plusieurs VLAN sur un lien inter-switch (trunk 802.1Q), comprendre le taggage, diagnostiquer un trunk mal configuré.

### Prérequis
TP1 validé (VLAN, ports access).

### Topologie

```
PC1 (VLAN 10, .11) ──E0/0/1
PC2 (VLAN 20, .13) ──E0/0/2    SW1 (S3700) ═══ trunk GE0/0/1 ═══ GE0/0/1 SW2 (S3700)    E0/0/1── PC3 (VLAN 10, .12)
                                                                                      E0/0/2── PC4 (VLAN 20, .14)
```

- 2 switches **S3700** : SW1, SW2.
- Lien trunk : **GE0/0/1 ↔ GE0/0/1** (câble Copper).
- Ports access : comme indiqué, VLAN 10 = 192.168.10.0/24, VLAN 20 = 192.168.20.0/24.

### Énoncé

1. Sur SW1 et SW2, créer VLAN 10 (`USERS`) et VLAN 20 (`GUESTS`).
2. Configurer les ports access (E0/0/1 → VLAN 10, E0/0/2 → VLAN 20 sur chaque switch).
3. Configurer le lien GE0/0/1 en **trunk** autorisant les VLAN 10 et 20, des deux côtés.
4. Tester tous les pings croisés (PC1↔PC3 OK, PC2↔PC4 OK, PC1↔PC2 KO).
5. **Dépannage** : l'animateur casse volontairement un côté du trunk (ex. `port trunk allow-pass vlan 10` seul sur SW2). Le stagiaire doit diagnostiquer avec les `display` et réparer.

### Correction pas à pas

**SW1 :**

```
<Huawei>system-view
[Huawei]sysname SW1
[SW1]vlan batch 10 20
[SW1-vlan10]description USERS
[SW1-vlan20]description GUESTS
[SW1]quit  # (quitter les vues vlan)

[SW1]interface Ethernet 0/0/1
[SW1-Ethernet0/0/1]port link-type access
[SW1-Ethernet0/0/1]port default vlan 10
[SW1-Ethernet0/0/1]quit
[SW1]interface Ethernet 0/0/2
[SW1-Ethernet0/0/2]port link-type access
[SW1-Ethernet0/0/2]port default vlan 20
[SW1-Ethernet0/0/2]quit

# Le trunk
[SW1]interface GigabitEthernet 0/0/1
[SW1-GigabitEthernet0/0/1]port link-type trunk
[SW1-GigabitEthernet0/0/1]port trunk allow-pass vlan 10 20
[SW1-GigabitEthernet0/0/1]quit
[SW1]save
```

**SW2 :** identique (mêmes VLAN, mêmes ports access, même trunk sur GE0/0/1).

**Adressage PC :**

| PC | IP | VLAN |
|---|---|---|
| PC1 | 192.168.10.11/24 | 10 |
| PC3 | 192.168.10.12/24 | 10 |
| PC2 | 192.168.20.13/24 | 20 |
| PC4 | 192.168.20.14/24 | 20 |

**Tests attendus :**

```
PC1> ping 192.168.10.12   # OK (VLAN 10 à travers le trunk)
PC2> ping 192.168.20.14   # OK (VLAN 20 à travers le trunk)
PC1> ping 192.168.20.13   # KO (VLAN différents)
```

### Vérifications

```
[SW1]display port vlan
# GE0/0/1 doit apparaître en trunk, PVID 1, VLAN List incluant 10 et 20 (tagged)

[SW1]display vlan 10
[SW1]display vlan 20
# Vérifier que GE0/0/1 est membre tagged des deux VLAN
```

**Capture Wireshark sur le lien trunk** (moment pédagogique fort) : lancer un ping PC1→PC3 et observer les trames Ethernet avec le champ **802.1Q VLAN ID = 10**. Puis ping PC2→PC4 : VLAN ID = 20. Le tag est la preuve visible du trunk.

### Pièges classiques

1. **Trunk configuré d'un seul côté** : l'autre côté reste en hybrid/PVID 1 → les trames taggées arrivent sur un port qui ne les attend pas. Symptôme : ping KO dans les deux VLAN. Diagnostic : `display port vlan` des deux côtés et comparer.
2. **`port trunk allow-pass vlan` oublié ou incomplet** : par défaut un trunk ne laisse passer que le VLAN 1. Si on autorise seulement le VLAN 10, le VLAN 20 ne passe pas → **le cas "VLAN qui ne passe pas"** de l'énoncé. Diagnostic :
   ```
   [SW2]display port vlan   # VLAN List du trunk : 10 seul → le 20 est filtré
   ```
   Réparation : `port trunk allow-pass vlan 10 20`.
3. **VLAN non créé d'un côté** : un trunk qui autorise le VLAN 20 alors que le VLAN 20 n'existe pas sur SW2 → le VLAN est "inactif". `display vlan` le montre.
4. **PVID (native VLAN) différent des deux côtés** : en 802.1Q pur avec que des trames taggées, pas d'impact ici, mais à signaler comme cause classique de fuite inter-VLAN en production.
5. **Câbler sur le mauvais port** : configurer GE0/0/1 alors que le câble est sur GE0/0/2. `display interface brief` montre le lien up sur le port réellement câblé.

### Barème indicatif (20 points)

| Critère | Points |
|---|---|
| VLAN créés des deux côtés | 3 |
| Ports access corrects | 4 |
| Trunk configuré des deux côtés (link-type + allow-pass 10 et 20) | 6 |
| Tests ping conformes | 3 |
| Diagnostic et réparation de la panne provoquée (méthode + commandes) | 4 |

### Durée estimée
**1 heure** (dont 15 min de dépannage guidé).

### Fiche animateur — points à insister
- Le trunk ne "crée" pas de connectivité : il **transporte** des VLAN existants des deux côtés. Les trois conditions : VLAN créés des deux côtés + trunk des deux côtés + allow-pass symétrique.
- La capture Wireshark du tag 802.1Q est le moment "aha" du TP : prévoir 10 minutes.
- Lien terrain : entre vos S310 d'étages, les liens montants sont toujours des trunks ; un VLAN oublié dans le allow-pass = le symptôme "ça marche sur un switch mais pas sur l'autre".
- Faire verbaliser la méthode de dépannage : 1) le lien est-il up ? 2) les VLAN existent-ils des deux côtés ? 3) le trunk est-il symétrique ? (`display port vlan` des deux côtés, côte à côte).

---

## 5. TP3 — STP/RSTP : boucle volontaire, élection root, coûts, portfast/edge

### Objectif
Comprendre pourquoi les boucles de niveau 2 sont dangereuses, comment STP/RSTP les neutralise, et comment contrôler l'élection du root bridge.

### Prérequis
TP1, TP2 (VLAN, trunk).

### Topologie

```
                    ┌──────────┐
   PC1 ──E0/0/1     │  SW1     │     E0/0/1── PC3
   (VLAN 10)        │ (S3700)  │
                    └────┬─────┘
              GE0/0/1    │    GE0/0/2        <- boucle volontaire à 3 switches
                         │         (triangle)
              ┌──────────┼──────────┐
              │          │          │
        GE0/0/1    SW2 (S3700)   GE0/0/1
              │     E0/0/1│          │
              │   PC2     │    SW3 (S3700)
              │  (VLAN10) │     E0/0/1── PC4 (VLAN 10)
              └──────────┴──────────┘
                   GE0/0/2 ↔ GE0/0/2  (SW2 ↔ SW3)
```

- 3 switches **S3700** en triangle : SW1↔SW2 (GE0/0/1↔GE0/0/1), SW1↔SW3 (GE0/0/2↔GE0/0/1), SW2↔SW3 (GE0/0/2↔GE0/0/2).
- Trunks 802.1Q sur les 3 liens inter-switch, VLAN 10 autorisé partout.
- 4 PC en VLAN 10 : 192.168.10.11 à 192.168.10.14.

### Énoncé

**Partie A — Observer le danger (5 min) :**
1. Câbler le triangle **sans activer STP** (par défaut, sur les S eNSP, STP est... à vérifier : l'animateur le désactive au préalable avec `stp disable` sur les 3 switches pour la démo).
2. Depuis PC1, pinger PC4 en continu. Observer : tempête de broadcast, ping qui part en timeout, CPU des switches qui monte. **Arrêter vite** (débrancher un câble du triangle) pour retrouver un réseau sain.
   > En lab eNSP l'effet est atténué par la virtualisation, mais les doublons de trames et l'instabilité MAC sont visibles dans `display mac-address` (adresses qui "flappent").

**Partie B — Activer RSTP et observer :**
3. Réactiver STP en mode **RSTP** sur les 3 switches.
4. Identifier le **root bridge** élu, les **ports designated / root / alternate (bloqué)**.
5. Forcer **SW1** comme root primaire et **SW2** comme secondaire (priorités).
6. Observer la convergence après coupure d'un lien du triangle (débrancher/rebrancher un câble).

**Partie C — Edge ports :**
7. Configurer les ports vers les PC en **edge** (portfast) : `stp edged-port enable`.
8. Constater la différence de temps de passage à l'état forwarding (immédiat vs ~30 s en STP classique / ~2 s en RSTP avec handshake).

### Correction pas à pas

**Préparation (animateur, avant la séance) : topologie câblée, STP désactivé pour la partie A :**

```
[SWx]stp disable
```

**Partie B — activation RSTP :**

```
# Sur les 3 switches :
[SW1]stp mode rstp
[SW1]stp enable

[SW2]stp mode rstp
[SW2]stp enable

[SW3]stp mode rstp
[SW3]stp enable
```

**Identifier le root :**

```
[SW1]display stp brief
# Affiche pour chaque port : rôle (DESI/ROOT/ALTE), état (FORWARDING/DISCARDING)
# Le switch dont tous les ports sont DESI est le root.

[SW1]display stp
# Détail : "CIST Bridge" avec priorité + MAC ; "Root" indique le root élu.
# Par défaut, priorité 32768 partout : le root = la plus petite MAC.
```

**Forcer l'élection :**

```
[SW1]stp root primary
# Équivaut à stp priority 0 (ou 4096 selon version) : SW1 devient root.

[SW2]stp root secondary
# Priorité 4096 : SW2 devient root si SW1 disparaît.

# Vérification :
[SW3]display stp
# Root ID doit correspondre au Bridge ID de SW1.
```

**Partie C — edge ports vers les PC :**

```
[SW1]interface Ethernet 0/0/1
[SW1-Ethernet0/0/1]stp edged-port enable
[SW1-Ethernet0/0/1]quit
# Répéter sur chaque port connecté à un PC (jamais sur un lien inter-switch !)
```

**Test de convergence :** débrancher le câble SW1↔SW2, observer dans `display stp brief` le port alternate de SW3 passer en forwarding (RSTP : quasi immédiat grâce au proposal/agreement). Rebrancher : retour à l'état initial.

### Vérifications

```
[SWx]display stp brief
# Carte d'identité du spanning tree : rôles + états de chaque port.

[SWx]display stpee inconsistent-port   # (si supporté) ports en incohérence

[SW1]display stp history
# Historique des changements de topologie (TCN).
```

**Capture Wireshark** sur un lien inter-switch : observer les **BPDU** (destination multicast `01:80:C2:00:00:00`), avec les champs Root Identifier, Bridge Identifier, Port Identifier. En RSTP, noter le flag "Proposal/Agreement".

### Pièges classiques

1. **Activer `stp edged-port` sur un lien inter-switch** : si une BPDU arrive sur un edge port, le port perd son statut edge (protection intégrée), mais en formation c'est l'erreur à ne jamais commettre en production : un edge port ne doit jamais recevoir de BPDU.
2. **Oublier `stp enable` après `stp mode rstp`** : le mode seul ne suffit pas.
3. **Root bridge subi** : sans priorité configurée, le switch avec la plus petite MAC devient root — parfois un switch d'accès en bout de chaîne. En production : **toujours** fixer root primary/secondaire sur les switches de cœur/distribution.
4. **Confondre coût de port et priorité** : la priorité élit le root ; le **coût** (`stp cost`) influence le choix du chemin vers le root quand plusieurs chemins existent. Pour préférer un lien : `stp cost 20000` sur le port à défavoriser (coût plus élevé = moins préféré).
5. **VLAN 1 vs instances** : eNSP/VRP de base = une seule instance (CIST). MSTP multi-instances existe sur les vrais S5700 mais sort du cadre de ce TP.

### Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Mise en évidence du danger de la boucle (observation + explication) | 4 |
| RSTP activé sur les 3 switches, root identifié avec `display stp` | 5 |
| SW1 root primary / SW2 secondary configurés et vérifiés | 5 |
| Edge ports configurés sur les ports PC uniquement | 3 |
| Test de convergence documenté (avant/après coupure) | 3 |

### Durée estimée
**1 h 15** (dont 15 min de capture BPDU).

### Fiche animateur — points à insister
- La boucle L2 est l'incident le plus violent d'un réseau de campus : tempête de broadcast = réseau entier à genoux en quelques secondes. D'où l'importance de RSTP **toujours actif** (ne jamais le désactiver "parce que ça ralentit").
- Le root bridge doit être **choisi**, pas subi : sur le terrain, le root = le cœur (vos S310 d'agrégation, pas un switch d'étage).
- Edge port = uniquement vers équipements terminaux (PC, imprimantes, AP). Jamais vers un autre switch.
- Montrer la différence de convergence : en RSTP la bascule est quasi instantanée ; faire le lien avec la haute disponibilité attendue par les utilisateurs.
- Question piège : "Que se passe-t-il si on branche deux câbles entre les mêmes deux switches sans STP ?" → Boucle immédiate. "Et avec RSTP ?" → Un des deux ports passe en discarding (alternate), le lien sert de backup.

---

## 6. TP4 — Serveur DHCP sur routeur AR

### Objectif
Fournir automatiquement adresses IP, masque, passerelle et DNS via DHCP depuis un routeur AR ; comprendre pools, exclusions, durées de bail ; mettre en place un relais DHCP vers un site distant.

### Prérequis
TP1 (VLAN), notions d'adressage IP.

### Topologie

```
Site A (VLAN 10)                          Site B (VLAN 20)
PC1 ──E0/0/1 SW1 (S3700) GE0/0/1 ── GE0/0/1 R1 (AR2220) GE0/0/2 ── GE0/0/1 SW2 (S3700) E0/0/1── PC3
PC2 ──E0/0/2         (trunk)                (VLAN 10: 192.168.10.254)   (trunk)              E0/0/2── PC4
                                               (VLAN 20: 192.168.20.254, via sous-interfaces)
```

- **R1 (AR2220)** : routeur central, serveur DHCP.
- **SW1 (S3700)** : ports E0/0/1-2 en access VLAN 10, trunk GE0/0/1 vers R1.
- **SW2 (S3700)** : ports E0/0/1-2 en access VLAN 20, trunk GE0/0/1 vers R1.
- R1 utilise des **sous-interfaces** (router-on-a-stick) : GE0/0/1.10 (VLAN 10, 192.168.10.254/24) et GE0/0/2.20 (VLAN 20, 192.168.20.254/24).
- Les PC sont en **DHCP** (double-clic > DHCP activé).

### Énoncé

1. Configurer les VLAN et trunks sur SW1/SW2 (rappel TP2).
2. Configurer les sous-interfaces 802.1Q sur R1 avec les IP passerelles.
3. Activer le **serveur DHCP** sur R1, créer deux pools :
   - `POOL-USERS` : réseau 192.168.10.0/24, passerelle 192.168.10.254, DNS 192.168.10.254 (ou 8.8.8.8 en exemple), **exclure** 192.168.10.1 à 192.168.10.10 (plage réservée aux équipements).
   - `POOL-GUESTS` : réseau 192.168.20.0/24, passerelle 192.168.20.254, bail de 1 heure.
4. Basculer les 4 PC en DHCP, vérifier qu'ils reçoivent une adresse du bon pool.
5. **Relais DHCP** : ajouter un second routeur R2 entre R1 et un "site C" (VLAN 30, 192.168.30.0/24) ; le serveur DHCP reste sur R1 ; configurer `dhcp relay` sur R2.
6. Vérifier les baux (`display ip pool`) et tester la connectivité inter-VLAN via R1.

### Correction pas à pas

**SW1 :**

```
[SW1]vlan batch 10 20
[SW1]interface Ethernet 0/0/1
[SW1-Ethernet0/0/1]port link-type access
[SW1-Ethernet0/0/1]port default vlan 10
[SW1]interface Ethernet 0/0/2
[SW1-Ethernet0/0/2]port link-type access
[SW1-Ethernet0/0/2]port default vlan 10
[SW1]interface GigabitEthernet 0/0/1
[SW1-GigabitEthernet0/0/1]port link-type trunk
[SW1-GigabitEthernet0/0/1]port trunk allow-pass vlan 10
```

**SW2 :** même logique avec VLAN 20 (E0/0/1-2 en access VLAN 20, trunk allow-pass vlan 20).

**R1 — sous-interfaces (router-on-a-stick) :**

```
[R1]interface GigabitEthernet 0/0/1.10
[R1-GigabitEthernet0/0/1.10]dot1q termination vid 10
[R1-GigabitEthernet0/0/1.10]ip address 192.168.10.254 24
[R1-GigabitEthernet0/0/1.10]arp broadcast enable
[R1-GigabitEthernet0/0/1.10]quit

[R1]interface GigabitEthernet 0/0/2.20
[R1-GigabitEthernet0/0/2.20]dot1q termination vid 20
[R1-GigabitEthernet0/0/2.20]ip address 192.168.20.254 24
[R1-GigabitEthernet0/0/2.20]arp broadcast enable
[R1-GigabitEthernet0/0/2.20]quit
```

> `arp broadcast enable` est **indispensable** sur les sous-interfaces VRP : sans lui, le routeur ne répond pas aux ARP des clients du VLAN et le ping échoue. C'est LE piège classique de ce TP.

**R1 — serveur DHCP :**

```
[R1]dhcp enable

[R1]ip pool POOL-USERS
[R1-ip-pool-POOL-USERS]network 192.168.10.0 mask 24
[R1-ip-pool-POOL-USERS]gateway-list 192.168.10.254
[R1-ip-pool-POOL-USERS]dns-list 192.168.10.254
[R1-ip-pool-POOL-USERS]excluded-ip-address 192.168.10.1 192.168.10.10
[R1-ip-pool-POOL-USERS]lease day 1 hour 0
[R1-ip-pool-POOL-USERS]quit

[R1]ip pool POOL-GUESTS
[R1-ip-pool-POOL-GUESTS]network 192.168.20.0 mask 24
[R1-ip-pool-POOL-GUESTS]gateway-list 192.168.20.254
[R1-ip-pool-POOL-GUESTS]dns-list 8.8.8.8
[R1-ip-pool-POOL-GUESTS]lease hour 1
[R1-ip-pool-POOL-GUESTS]quit

# Activer DHCP sur les interfaces (mode global) :
[R1]interface GigabitEthernet 0/0/1.10
[R1-GigabitEthernet0/0/1.10]dhcp select global
[R1]interface GigabitEthernet 0/0/2.20
[R1-GigabitEthernet0/0/2.20]dhcp select global
[R1]save
```

**Comment le bon pool est-il choisi ?** Le serveur sélectionne le pool dont le `network` correspond au sous-réseau de l'interface d'arrivée de la requête. D'où l'importance que chaque sous-interface ait son pool.

**PC en DHCP** : double-clic sur chaque PC > cocher **DHCP**. Puis en console du PC : `ipconfig` doit montrer une adresse 192.168.10.x (PC1/PC2) ou 192.168.20.x (PC3/PC4).

**Partie relais — topologie étendue :**

```
R1 GE0/0/3 (192.168.100.1/30) ── (192.168.100.2/30) R2 (AR2220) GE0/0/1.30 (VLAN 30, 192.168.30.254/24, dot1q vid 30)
                                                                                    │
                                                                              SW3 (S3700), PC5/PC6 en DHCP
```

```
# Sur R2 : route vers les pools + relais
[R2]dhcp enable
[R2]interface GigabitEthernet 0/0/1.30
[R2-GigabitEthernet0/0/1.30]dot1q termination vid 30
[R2-GigabitEthernet0/0/1.30]ip address 192.168.30.254 24
[R2-GigabitEthernet0/0/1.30]arp broadcast enable
[R2-GigabitEthernet0/0/1.30]dhcp relay server-ip 192.168.100.1
[R2-GigabitEthernet0/0/1.30]quit

# Routage statique pour joindre le serveur (ou OSPF du TP6 en version avancée)
[R2]ip route-static 0.0.0.0 0.0.0.0 192.168.100.1
[R1]ip route-static 192.168.30.0 24 192.168.100.2

# Sur R1 : pool pour le site C
[R1]ip pool POOL-SITEC
[R1-ip-pool-POOL-SITEC]network 192.168.30.0 mask 24
[R1-ip-pool-POOL-SITEC]gateway-list 192.168.30.254
[R1-ip-pool-POOL-SITEC]quit
```

> Le relais fonctionne parce que la requête DHCP (broadcast) est interceptée par R2, convertie en unicast vers 192.168.100.1 avec l'option `giaddr` = 192.168.30.254 : le serveur choisit alors le pool POOL-SITEC.

### Vérifications

```
[R1]display ip pool
# Pools, plages, baux attribués, adresses utilisées/libres.

[R1]display ip pool name POOL-USERS used
# Détail des baux actifs : IP, MAC, durée restante.

[R1]display dhcp server statistics
# Compteurs Discover/Offer/Request/ACK : idéal pour voir le dialogue.

# Sur PC : ipconfig /all → adresse, masque, passerelle, DNS, bail.
```

**Capture Wireshark** sur le lien SW1↔R1 : filtrer `bootp` → observer **Discover → Offer → Request → ACK** (DORA). Faire remarquer que le client envoie en broadcast (0.0.0.0 → 255.255.255.255) et que le serveur répond en unicast.

### Pièges classiques

1. **`arp broadcast enable` oublié** sur les sous-interfaces → DHCP et ping KO. Premier réflexe de dépannage de ce TP.
2. **`dhcp enable` oublié** au niveau global → le service ne tourne pas, `dhcp select global` ne suffit pas.
3. **Pool dont le network ne correspond à aucune interface** → aucune adresse attribuée, sans message d'erreur explicite.
4. **Relais : route de retour manquante** sur R1 vers 192.168.30.0/24 → l'Offer n'arrive jamais au client. Toujours vérifier le routage dans les deux sens.
5. **Bail trop long en lab** : pour voir les renouvellements, utiliser un bail court (ex. 10 minutes) sur un pool de test.
6. **Deux serveurs DHCP sur le même broadcast domain** (en production : box opérateur + serveur) → adresses incohérentes. En lab : ne jamais activer DHCP sur deux équipements du même VLAN.

### Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Sous-interfaces 802.1Q + `arp broadcast enable` | 4 |
| Serveur DHCP : 2 pools complets (network, gateway, DNS, excluded, lease) | 6 |
| PC en DHCP : adresses correctes par VLAN | 3 |
| Relais DHCP fonctionnel vers le site C | 4 |
| Vérifications (`display ip pool`, statistiques) + capture DORA commentée | 3 |

### Durée estimée
**1 h 30** (dont 20 min pour le relais).

### Fiche animateur — points à insister
- Le DHCP, c'est 4 paramètres critiques : **adresse, masque, passerelle, DNS**. Un poste qui "a une IP mais pas Internet" = souvent passerelle ou DNS manquants → `ipconfig /all` est le premier réflexe.
- `excluded-ip-address` : en production, toujours réserver le bas de plage pour les équipements fixes (imprimantes, AP, onduleurs avec carte réseau...). Faire le lien avec le métier de l'équipe.
- Le relais DHCP est indispensable dès qu'on a plusieurs sites/VLAN : un seul serveur centralisé, des relais sur chaque routeur d'accès.
- Montrer `display dhcp server statistics` : les compteurs qui n'augmentent pas indiquent où le dialogue se bloque (Discover sans Offer = problème serveur/pool ; Offer sans Request = problème réseau retour).

---

## 7. TP5 — Routage statique et floating static

### Objectif
Maîtriser les routes statiques : route vers un réseau distant, route par défaut, et route flottante (floating static) comme mécanisme de backup.

### Prérequis
Adressage IP, sous-interfaces (TP4).

### Topologie

```
LAN A (192.168.10.0/24)                    LAN B (192.168.20.0/24)
PC1 ──SW1── GE0/0/1 R1 (AR2220) GE0/0/2 ══════════ GE0/0/2 R2 (AR2220) GE0/0/1──SW2── PC2
              .254      10.0.12.1/30   lien principal    10.0.12.2/30      .254
                              GE0/0/3 ══════════ GE0/0/3
                              10.0.13.1/30   lien backup    10.0.13.2/30
```

- **R1** : GE0/0/1 = 192.168.10.254/24 (LAN A), GE0/0/2 = 10.0.12.1/30 (lien principal), GE0/0/3 = 10.0.13.1/30 (lien backup).
- **R2** : GE0/0/1 = 192.168.20.254/24 (LAN B), GE0/0/2 = 10.0.12.2/30, GE0/0/3 = 10.0.13.2/30.
- SW1 : E0/0/1 (PC1) en access VLAN 10 ; SW2 : idem VLAN 20. Trunks vers les routeurs, sous-interfaces comme au TP4 (ou interfaces physiques en access si on simplifie : ici on utilise des interfaces physiques routées directement pour le LAN, plus simple : GE0/0/1 en `undo portswitch`).

### Énoncé

1. Adresser toutes les interfaces selon le plan.
2. Sur R1 : route statique vers 192.168.20.0/24 via 10.0.12.2 (lien principal) ; route flottante via 10.0.13.2 avec **préférence 100** (la statique par défaut = préférence 60).
3. Sur R2 : miroir (vers 192.168.10.0/24 via 10.0.12.1, backup via 10.0.13.1 préférence 100).
4. Tester PC1 ↔ PC2. Vérifier dans la table de routage quelle route est active.
5. **Test de bascule** : couper le lien principal (shutdown GE0/0/2 sur R1) → vérifier que la route flottante prend le relais, ping continu pendant la coupure.
6. Ajouter une route par défaut sur R1 et R2 vers un "ISP" simulé (R3 avec 10.0.99.0/30, ou un Cloud) — comprendre `0.0.0.0/0`.

### Correction pas à pas

**Adressage R1 :**

```
[R1]interface GigabitEthernet 0/0/1
[R1-GigabitEthernet0/0/1]undo portswitch
[R1-GigabitEthernet0/0/1]ip address 192.168.10.254 24
[R1]interface GigabitEthernet 0/0/2
[R1-GigabitEthernet0/0/2]undo portswitch
[R1-GigabitEthernet0/0/2]ip address 10.0.12.1 30
[R1]interface GigabitEthernet 0/0/3
[R1-GigabitEthernet0/0/3]undo portswitch
[R1-GigabitEthernet0/0/3]ip address 10.0.13.1 30
```

> Sur les AR eNSP, les ports GE sont en mode switch par défaut : `undo portswitch` les passe en mode routé (L3). Alternative : utiliser des sous-interfaces comme au TP4.

**Routes statiques R1 :**

```
[R1]ip route-static 192.168.20.0 24 10.0.12.2
# Préférence par défaut 60 : route principale.

[R1]ip route-static 192.168.20.0 24 10.0.13.2 preference 100
# Floating static : inactive tant que la principale est valide.

[R1]ip route-static 0.0.0.0 0.0.0.0 10.0.12.2
# Route par défaut (exemple : vers l'ISP via R2, à adapter).
```

**R2 (miroir) :**

```
[R2]ip route-static 192.168.10.0 24 10.0.12.1
[R2]ip route-static 192.168.10.0 24 10.0.13.1 preference 100
```

**Test de bascule :**

```
# Ping continu depuis PC1 vers 192.168.20.11 (PC2)
[R1]interface GigabitEthernet 0/0/2
[R1-GigabitEthernet0/0/2]shutdown
# Observer 1 à 3 paquets perdus puis reprise via 10.0.13.0/30.

[R1]display ip routing-table 192.168.20.0
# La route active pointe maintenant vers 10.0.13.2 avec Proto=Static, Pre=100.

[R1]interface GigabitEthernet 0/0/2
[R1-GigabitEthernet0/0/2]undo shutdown
# Retour automatique sur la route préférence 60 (préemption immédiate en statique).
```

### Vérifications

```
[R1]display ip routing-table
# Colonnes : Destination/Mask, Proto (Direct/Static), Pre (préférence), Cost, NextHop, Interface.

[R1]display ip routing-table 192.168.20.0 verbose
# Détail : préférence, tag, état actif/inactif.

[R1]tracert 192.168.20.11
# Visualiser le chemin emprunté (doit passer par 10.0.12.2 en nominal, 10.0.13.2 en backup).

[R1]display ip route-static
# Toutes les statiques configurées, y compris les inactives.
```

### Pièges classiques

1. **Route configurée d'un seul côté** : le ping aller passe, le retour n'a pas de route → échec. Le routage statique est **bidirectionnel par construction** : toujours configurer les deux sens (ou une défaut).
2. **Next-hop injoignable directement** : une statique dont le next-hop n'est pas sur un réseau connecté est invalide (reste inactive). Vérifier avec `display ip routing-table` : la route n'apparaît pas.
3. **Préférence inversée** : mettre `preference 100` sur la principale et rien sur le backup → le backup devient la route active en permanence. Règle : **plus la préférence est petite, plus la route est préférée** (0 = connecté, 60 = statique, 10 = OSPF interne...).
4. **Oublier `undo portswitch`** : l'interface reste en L2, `ip address` est refusé.
5. **Masque /30 mal calculé** : 10.0.12.1/30 et 10.0.12.2/30 sont bien dans le même sous-réseau (10.0.12.0/30 : .0 réseau, .1-.2 hôtes, .3 broadcast). Erreur fréquente : utiliser .3 (broadcast) comme adresse.
6. **Floating static et détection de panne** : la statique ne tombe que si l'interface du next-hop tombe (lien direct). Si la panne est **au-delà** du next-hop, la statique reste active et le trafic part dans le vide → en production, coupler avec de la détection (BFD, NQA, IP SLA). À mentionner même si hors lab.

### Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Adressage complet et cohérent (interfaces + PC) | 5 |
| Routes statiques principales dans les deux sens | 5 |
| Floating static avec préférence correcte | 4 |
| Test de bascule documenté (ping continu + table de routage) | 4 |
| Route par défaut comprise et configurée | 2 |

### Durée estimée
**1 heure**.

### Fiche animateur — points à insister
- La table de routage se lit en 4 colonnes : **Destination, Préférence, Coût, Next-hop**. Faire réciter.
- Le routage est toujours une question d'**aller ET retour** : 80 % des pannes "ça ping dans un sens" viennent d'une route de retour manquante.
- La floating static est le mécanisme de backup le plus simple ; ses limites (pas de détection de panne distale) justifient les protocoles dynamiques → transition naturelle vers le TP6 (OSPF).
- Lien terrain : sur vos AR720, les routes statiques servent pour les petits sites et les backup 4G ; la préférence permet de basculer automatiquement entre fibre et 4G.

---

## 8. TP6 — OSPF mono-zone

### Objectif
Déployer OSPF en zone 0 sur 3-4 routeurs : adjacences, élection DR/BDR, coûts, et redistribution d'une route par défaut.

### Prérequis
TP5 (routage, tables de routage).

### Topologie

```
                    R1 (AR2220, Router-ID 1.1.1.1)
                   /        \
   10.0.12.0/30   /          \   10.0.13.0/30
                 /            \
   R2 (2.2.2.2) ────────────── R3 (3.3.3.3)
   10.0.23.0/30 (lien R2-R3)
       |                           |
  GE0/0/1                       GE0/0/1
  192.168.20.254/24             192.168.30.254/24
  LAN B (SW2, PC)               LAN C (SW3, PC)
       |
  (R1 a aussi le LAN A : 192.168.10.254/24 sur GE0/0/1)
```

- 3 routeurs **AR2220** en triangle (triangle = élection DR/BDR intéressante sur chaque segment).
- Chaque routeur a un LAN (switch S3700 + 1 PC).
- R1 simule la sortie Internet : route statique par défaut vers un Cloud/ISP, à **redistribuer** dans OSPF.

### Énoncé

1. Adresser toutes les interfaces (plan ci-dessus, /30 sur les liens inter-routeurs).
2. Configurer OSPF **process 1, area 0** sur les 3 routeurs, avec des **Router-ID manuels** (1.1.1.1, 2.2.2.2, 3.3.3.3).
3. Annoncer les réseaux : les /30 et les LAN.
4. Vérifier les voisins (`display ospf peer`), identifier **DR et BDR** sur chaque segment.
5. Forcer R1 comme DR du segment R1-R2 (priorité d'interface).
6. Modifier un **coût OSPF** pour influencer un chemin, observer le changement dans la table de routage.
7. Sur R1 : créer une route par défaut statique (vers l'ISP simulé) et la **redistribuer** dans OSPF (`default-route-advertise`).
8. Vérifier depuis R3 que la défaut est apprise en OSPF (O_ASE) et tester.

### Correction pas à pas

**Adressage (exemple R1) :**

```
[R1]interface GigabitEthernet 0/0/1
[R1-GigabitEthernet0/0/1]undo portswitch
[R1-GigabitEthernet0/0/1]ip address 192.168.10.254 24
[R1]interface GigabitEthernet 0/0/2
[R1-GigabitEthernet0/0/2]undo portswitch
[R1-GigabitEthernet0/0/2]ip address 10.0.12.1 30
[R1]interface GigabitEthernet 0/0/3
[R1-GigabitEthernet0/0/3]undo portswitch
[R1-GigabitEthernet0/0/3]ip address 10.0.13.1 30
```

(R2 : GE0/0/1 = 192.168.20.254/24, GE0/0/2 = 10.0.12.2/30, GE0/0/3 = 10.0.23.1/30 ; R3 : GE0/0/1 = 192.168.30.254/24, GE0/0/2 = 10.0.13.2/30, GE0/0/3 = 10.0.23.2/30.)

**OSPF sur R1 :**

```
[R1]ospf 1 router-id 1.1.1.1
[R1-ospf-1]area 0
[R1-ospf-1-area-0.0.0.0]network 192.168.10.0 0.0.0.255
[R1-ospf-1-area-0.0.0.0]network 10.0.12.0 0.0.0.3
[R1-ospf-1-area-0.0.0.0]network 10.0.13.0 0.0.0.3
[R1-ospf-1-area-0.0.0.0]quit
[R1-ospf-1]quit
```

> Le `network` OSPF utilise un **masque inversé** (wildcard) : /24 → 0.0.0.255, /30 → 0.0.0.3. C'est la source d'erreur n°1.

**R2 et R3 :** même structure avec leurs réseaux et leurs Router-ID.

**Vérifier les adjacences :**

```
[R1]display ospf peer
# État Full = adjacence établie. Sur un segment à 3 routeurs (pas ici, nos liens sont point-à-point
# à 2 routeurs : pas de DR/BDR sur les /30... voir note ci-dessous).

[R1]display ospf peer brief
```

> **Note DR/BDR** : sur des liens /30 à 2 routeurs, OSPF ne élit pas de DR/BDR utile (élection triviale). Pour observer une vraie élection, ajouter un **switch entre R1, R2 et R3** (un S3700 avec les 3 routeurs branchés dessus en 10.0.123.0/29) : le segment multi-accès élit alors un DR et un BDR. **Variante conseillée par l'animateur** : remplacer le triangle par une étoile via SW0.

**Variante étoile (recommandée pour DR/BDR) :**

```
            SW0 (S3700)
   ┌─────────┼─────────┐
  R1        R2        R3
  .1        .2        .3   (10.0.123.0/29)
```

```
# Sur chaque routeur, l'interface vers SW0 en mode L3 (undo portswitch), IP 10.0.123.x/29.
# OSPF : network 10.0.123.0 0.0.0.7 area 0

# Forcer R1 DR :
[R1]interface GigabitEthernet 0/0/4
[R1-GigabitEthernet0/0/4]ospf dr-priority 100
# (défaut = 1 ; 0 = inéligible)

[R1]display ospf peer
# Colonne "DR/BDR" : R1 doit apparaître comme DR.

# Pour que l'élection se refasse proprement après changement de priorité :
[R1]reset ospf 1 peer   # (ou reboot du process)
```

**Jouer sur les coûts :**

```
# Rendre le chemin via R2 moins attractif depuis R1 vers le LAN C :
[R1]interface GigabitEthernet 0/0/2
[R1-GigabitEthernet0/0/2]ospf cost 100
# (coût par défaut = 1 sur GE... en fait calculé sur la bande passante de référence ;
#  sur eNSP le coût affiché par défaut est souvent 1)

[R1]display ip routing-table 192.168.30.0
# Le chemin doit basculer sur l'autre lien. Comparer avant/après.
```

**Redistribution de la défaut (sur R1) :**

```
# Simuler l'ISP : route statique par défaut (next-hop = une IP du lab ou un Cloud)
[R1]ip route-static 0.0.0.0 0.0.0.0 10.0.99.2

# L'injecter dans OSPF :
[R1]ospf 1
[R1-ospf-1]default-route-advertise always
[R1-ospf-1]quit
# "always" : annonce même si R1 n'a pas lui-même de défaut active (pratique en lab).

# Vérification depuis R3 :
[R3]display ip routing-table 0.0.0.0
# Proto = O_ASE, Pre = 150, NextHop = vers R1.
```

### Vérifications

```
[R1]display ospf peer brief        # voisins et états
[R1]display ospf lsdb              # base de données des LSAs (Router-LSA, Network-LSA...)
[R1]display ospf routing           # table de routage calculée par OSPF
[R1]display ospf interface         # interfaces OSPF : area, cost, DR/BDR, timers
[R1]display ip routing-table protocol ospf   # routes OSPF dans la table globale
```

**Capture Wireshark** sur un lien inter-routeur : filtrer `ospf` → **Hello** (multicast 224.0.0.5, toutes les 10 s), puis provoquer une coupure et observer les **LSU/LSAck** (mises à jour). Le Hello contient : Router-ID, Area, timers, liste des voisins vus.

### Pièges classiques

1. **Wildcard inversé faux** : `network 10.0.12.0 0.0.0.255` au lieu de `0.0.0.3` → le réseau n'est pas forcément activé comme voulu (ici ça activerait OSPF sur trop d'interfaces). Toujours calculer : wildcard = 255.255.255.255 − masque.
2. **Router-ID dupliqué ou 0.0.0.0** : sans `router-id` manuel et sans IP de loopback, eNSP peut prendre une IP d'interface ; deux routeurs avec le même Router-ID = adjacence impossible. **Toujours fixer le Router-ID manuellement** (bonne pratique).
3. **Area différente des deux côtés** : les Hello sont rejetés (area mismatch) → état `Down`, sans message très explicite. `display ospf interface` des deux côtés pour comparer.
4. **Timers Hello/Dead différents** : même symptôme (mismatch). En lab, ne pas y toucher.
5. **Interface passive oubliée** : sur les interfaces LAN (vers les switchs), pas besoin de voisins OSPF : `silent-interface` (ou `ospf silent-interface`) évite d'envoyer des Hello inutiles vers les PC. Pas bloquant, mais bonne pratique à enseigner.
6. **MTU mismatch** : les AR eNSP ont une MTU homogène, mais en production un MTU différent bloque l'adjacence à l'état `ExStart`. À connaître.
7. **`default-route-advertise` sans `always`** : si R1 n'a pas de route par défaut active dans sa table, rien n'est annoncé. En lab, `always` simplifie.

### Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Adressage + Router-ID manuels | 3 |
| OSPF area 0 sur les 3 routeurs, networks corrects (wildcards) | 5 |
| Adjacences Full vérifiées (`display ospf peer`) | 3 |
| DR/BDR : élection observée + R1 forcé DR (variante étoile) | 4 |
| Coût modifié avec effet observé sur la table de routage | 2 |
| Défaut redistribuée et vue en O_ASE sur R3 | 3 |

### Durée estimée
**1 h 30** (dont 20 min de capture OSPF).

### Fiche animateur — points à insister
- OSPF ne se "configure" pas, il se **déclare** : on déclare les réseaux dans l'area, le protocole fait le reste (voisins, calcul SPF, convergence). C'est le changement de paradigme après le TP5.
- Les 3 conditions d'une adjacence : **même area, mêmes timers, Router-ID uniques** (+ même MTU, même subnet en production).
- DR/BDR : concept souvent mystifié — simplifier : "sur un segment à plus de 2 routeurs, on élit un représentant (DR) pour limiter les échanges ; la priorité la plus haute gagne, à égalité la plus haute Router-ID".
- Le coût = la métrique : **chemin de coût cumulé le plus faible gagne**. Faire manipuler `ospf cost` pour "sentir" la métrique.
- Lien terrain : OSPF est le protocole de vos réseaux d'agence multi-sites ; la redistribution de la défaut depuis le routeur de sortie est le standard.
- Transition vers TP7 : "maintenant que tout le monde se parle en interne, ouvrons vers l'extérieur : le NAT".

---

## 9. TP7 — NAT : NAPT et port mapping

### Objectif
Partager une adresse publique entre plusieurs postes (NAPT / Easy IP) et exposer un serveur interne vers l'extérieur (port mapping / NAT server).

### Prérequis
TP5 (routage), adressage.

### Topologie

```
LAN privé (192.168.10.0/24)                        "Internet" simulé (203.0.113.0/30)
PC1 (.11) ──SW1── GE0/0/1 R1 (AR2220) GE0/0/2 ══════════ GE0/0/2 R-ISP (AR2220) GE0/0/1── Serv-Public (203.0.113.10/24)
PC2 (.12)          .254          198.51.100.1/30 (IP publique)      198.51.100.2/30
Serveur (.100) ── (serveur web interne à exposer)
```

- **R1** : GE0/0/1 = 192.168.10.254/24 (inside), GE0/0/2 = 198.51.100.1/30 (outside, IP "publique").
- **R-ISP** : routeur simulant l'opérateur/Internet, GE0/0/2 = 198.51.100.2/30, GE0/0/1 = 203.0.113.1/24, avec un **Server** (203.0.113.10) jouant le rôle d'un site web public.
- **Serveur interne** : un objet **Server** eNSP en 192.168.10.100 (HTTP activé dans sa configuration).

### Énoncé

1. Adresser et router : R1 connaît 192.168.10.0/24 (direct) ; R-ISP a une route vers 198.51.100.0/30 (direct) ; ajouter sur R-ISP une route vers... attention : **R-ISP ne doit PAS connaître 192.168.10.0/24** (c'est le principe du NAT : le privé n'est pas routé sur Internet).
2. Configurer sur R1 le **NAPT (Easy IP)** : tout le LAN sort avec l'adresse de GE0/0/2.
3. Depuis PC1/PC2, pinger 203.0.113.10 et ouvrir la page web (le client HTTP du PC eNSP ou `curl` si dispo). Vérifier les translations.
4. Configurer le **port mapping** : le serveur web interne (192.168.10.100:80) doit être joignable depuis "Internet" via 198.51.100.1:8080.
5. Depuis le Server public (ou un PC côté ISP), accéder à `http://198.51.100.1:8080` → doit afficher le site du serveur interne.
6. Vérifier les sessions NAT et la table de translation.

### Correction pas à pas

**Routage de base :**

```
# R1 : défaut vers l'ISP
[R1]ip route-static 0.0.0.0 0.0.0.0 198.51.100.2

# R-ISP : RIEN vers 192.168.10.0/24 (volontaire). Route vers 198.51.100.0/30 = direct.
```

**NAPT Easy IP sur R1 :**

```
[R1]acl 2000
[R1-acl-basic-2000]rule permit source 192.168.10.0 0.0.0.255
[R1-acl-basic-2000]quit

[R1]interface GigabitEthernet 0/0/2
[R1-GigabitEthernet0/0/2]nat outbound 2000
[R1-GigabitEthernet0/0/2]quit
```

> `nat outbound 2000` **sans adresse** = Easy IP : utilise l'IP de l'interface de sortie. C'est le cas standard d'un site avec une seule IP publique (box, AR720 avec IP WAN).

**Port mapping (NAT server) sur R1 :**

```
[R1]interface GigabitEthernet 0/0/2
[R1-GigabitEthernet0/0/2]nat server protocol tcp global 198.51.100.1 8080 inside 192.168.10.100 80
[R1-GigabitEthernet0/0/2]quit
```

> Avec Easy IP, `global 198.51.100.1` = l'IP de l'interface elle-même. On peut aussi écrire `global current-interface 8080`.

**Tests :**

```
# Depuis PC1 (client) :
PC1> ping 203.0.113.10        # OK via NAT
# Accès web : utiliser le navigateur du PC eNSP vers http://203.0.113.10

# Depuis le Server public (côté ISP) :
# http://198.51.100.1:8080  →  doit afficher le site hébergé en 192.168.10.100
```

### Vérifications

```
[R1]display nat outbound
# Politiques NAT configurées par interface.

[R1]display nat server
# Mappings statiques (port mapping).

[R1]display nat session all
# Sessions actives : Proto, SrcIP:Port -> DstIP:Port, translation.
# Exemple : TCP 192.168.10.11:1234 -> 203.0.113.10:80  traduit en  198.51.100.1:2048 -> 203.0.113.10:80

[R1]display firewall session table   # (selon version, équivalent)
```

**Capture Wireshark** : capturer simultanément côté LAN (SW1↔R1) et côté WAN (R1↔R-ISP) pendant un ping PC1→203.0.113.10 : constater que l'IP source **change** en traversant R1 (192.168.10.11 → 198.51.100.1). C'est la démonstration la plus parlante du NAT.

### Pièges classiques

1. **ACL du NAT trop restrictive ou dans le mauvais sens** : `rule permit source 192.168.10.0 0.0.0.255` — le wildcard est 0.0.0.255 (pas 0.0.255.255 !). Une ACL qui ne matche pas = pas de translation = ping KO sans message d'erreur.
2. **`nat outbound` sur la mauvaise interface** : il se configure sur l'interface de **sortie** (outside, GE0/0/2), pas sur l'interface LAN.
3. **Oublier la route par défaut** sur R1 : le NAT ne crée pas le routage ; sans défaut, le paquet est droppé avant même la translation.
4. **Port mapping : tester depuis le LAN** : depuis PC1, `http://198.51.100.1:8080` peut échouer (hairpin NAT non supporté par défaut) alors que ça marche depuis l'extérieur. Toujours tester le port mapping **depuis l'extérieur**.
5. **Conflit de port** : exposer le port 80 alors que l'interface externe... en lab ça passe, mais en production le port 80 externe peut être pris par l'interface web du routeur lui-même → utiliser 8080 comme dans l'énoncé.
6. **Le serveur interne sans passerelle** : le Server eNSP en 192.168.10.100 doit avoir 192.168.10.254 comme gateway, sinon le retour du port mapping n'arrive jamais.

### Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Routage de base + absence volontaire de route privée côté ISP (comprise) | 3 |
| ACL 2000 correcte (source + wildcard) | 3 |
| NAPT Easy IP fonctionnel (ping + web depuis PC1/PC2) | 5 |
| Port mapping fonctionnel depuis l'extérieur | 5 |
| Vérifications (`display nat session`, `display nat server`) + double capture commentée | 4 |

### Durée estimée
**1 h 15**.

### Fiche animateur — points à insister
- NAT = **translation d'adresse**, pas du routage et pas de la sécurité (même si ça masque le réseau interne). Distinguer les trois rôles.
- Easy IP couvre 95 % des sites PME (une seule IP publique). Le port mapping couvre l'exposition de services (vidéosurveillance, site web, accès distant).
- La double capture (avant/après translation) est le moment clé : faire formuler par les stagiaires ce qui a changé dans l'en-tête IP.
- Lien terrain : sur vos AR720 en sortie d'agence, le NAT sortant est identique ; sur les USG6000, le NAT se configure dans les politiques (voir TP11) — même logique, autre syntaxe.
- Question piège : "Pourquoi R-ISP ne connaît-il pas 192.168.10.0/24 ?" → Parce que sur Internet, les adresses privées ne sont pas routées ; c'est précisément pour ça que le NAT existe.

---

## 10. TP8 — PPPoE client et serveur

### Objectif
Comprendre l'établissement d'une session PPPoE (cas classique des accès xDSL/fibre via boîtier opérateur en mode bridge) : discovery, session, authentification PAP/CHAP, IP négociée.

### Prérequis
TP5 (routage), TP7 (NAT — le PPPoE débouche souvent sur du NAT).

### Topologie

```
PC1 ──SW1── GE0/0/1 R-Client (AR2220) GE0/0/2 ══════════ GE0/0/2 R-Fournisseur (AR2220) GE0/0/1── "Internet" (Server 203.0.113.10)
              (client PPPoE, dialer 1)   lien Ethernet   (serveur PPPoE, virtual-template 1)
```

- **R-Fournisseur** : joue le rôle du BAS/BRAS de l'opérateur : serveur PPPoE sur GE0/0/2, pool d'adresses publiques simulées.
- **R-Client** : client PPPoE sur GE0/0/2 via une interface **Dialer1**, authentification CHAP (login `client-lab`, mot de passe `huawei123` — identifiants de lab uniquement).
- Après établissement, R-Client reçoit une IP "publique" sur le Dialer et fait du NAT (Easy IP) pour son LAN.

### Énoncé

1. Configurer **R-Fournisseur** comme serveur PPPoE : virtual-template, pool d'adresses, authentification CHAP, utilisateur local.
2. Configurer **R-Client** : interface Dialer1 (requête d'IP, CHAP, dialer-group), lier GE0/0/2 au PPPoE (`pppoe-client dial-bundle-number 1`).
3. Monter la session, vérifier l'IP obtenue sur le Dialer.
4. Ajouter le NAT Easy IP sur le Dialer + route par défaut via le Dialer.
5. Tester : PC1 ping 203.0.113.10 (à travers PPPoE + NAT).
6. **Dépannage** : l'animateur casse l'authentification (mauvais mot de passe côté client) → diagnostiquer avec les `display` et les debugs.

### Correction pas à pas

**R-Fournisseur (serveur PPPoE) :**

```
[R-F]ip pool POOL-PPPOE
[R-F-ip-pool-POOL-PPPOE]network 198.51.100.0 mask 30
[R-F-ip-pool-POOL-PPPOE]quit
# Le pool fournit ici 198.51.100.1 (le .2 sera pris par... attention : /30 = .1 et .2 utilisables)

[R-F]aaa
[R-F-aaa]local-user client-lab password cipher huawei123
[R-F-aaa]local-user client-lab service-type ppp
[R-F-aaa]quit

[R-F]interface Virtual-Template 1
[R-F-Virtual-Template1]ppp authentication-mode chap
[R-F-Virtual-Template1]ip address 198.51.100.2 30
[R-F-Virtual-Template1]remote address pool POOL-PPPOE
[R-F-Virtual-Template1]quit

[R-F]interface GigabitEthernet 0/0/2
[R-F-GigabitEthernet0/0/2]undo portswitch
[R-F-GigabitEthernet0/0/2]pppoe-server bind virtual-template 1
[R-F-GigabitEthernet0/0/2]quit
```

**R-Client (client PPPoE) :**

```
[R-C]dialer-rule
[R-C-dialer-rule]dialer-rule 1 ip permit
[R-C-dialer-rule]quit

[R-C]interface Dialer 1
[R-C-Dialer1]dialer user client-lab
[R-C-Dialer1]dialer-group 1
[R-C-Dialer1]dialer bundle 1
[R-C-Dialer1]ppp chap user client-lab
[R-C-Dialer1]ppp chap password cipher huawei123
[R-C-Dialer1]ip address ppp-negotiate
[R-C-Dialer1]quit

[R-C]interface GigabitEthernet 0/0/2
[R-C-GigabitEthernet0/0/2]undo portswitch
[R-C-GigabitEthernet0/0/2]pppoe-client dial-bundle-number 1
[R-C-GigabitEthernet0/0/2]quit
```

**Vérifier la session :**

```
[R-C]display pppoe-client session summary
# État UP, Session ID attribuée, interface physique liée.

[R-C]display ip interface brief
# Dialer1 doit avoir une IP (ex. 198.51.100.1) en "PPP-negotiate".
```

**NAT + défaut via le Dialer :**

```
[R-C]acl 2000
[R-C-acl-basic-2000]rule permit source 192.168.10.0 0.0.0.255
[R-C-acl-basic-2000]quit
[R-C]interface Dialer 1
[R-C-Dialer1]nat outbound 2000
[R-C-Dialer1]quit
[R-C]ip route-static 0.0.0.0 0.0.0.0 Dialer 1
```

**Test final :** `PC1> ping 203.0.113.10` → OK (LAN → NAT → PPPoE → fournisseur → Internet simulé).

### Vérifications

```
[R-C]display pppoe-client session summary   # session UP ?
[R-C]display pppoe-client session packet     # compteurs PADI/PADO/PADR/PADS
[R-F]display virtual-access                   # interfaces d'accès virtuelles créées par le serveur
[R-F]display pppoe-server session all         # sessions vues côté serveur
[R-F]display access-user                       # utilisateurs PPP connectés
```

**Capture Wireshark** sur le lien R-C↔R-F : filtrer `pppoe` → observer la phase **Discovery** (PADI → PADO → PADR → PADS) puis la phase **Session** (LCP → authentification CHAP Challenge/Response → IPCP avec l'IP négociée). C'est toute la séquence d'un accès ADSL/fibre résumée en quelques paquets.

### Pièges classiques

1. **Mot de passe CHAP différent des deux côtés** : la session reste Down, `display pppoe-client session summary` ne montre rien. Diagnostic : `debugging ppp chap` (avec modération) ou vérifier les deux configs côte à côte. C'est la panne provoquée de l'énoncé.
2. **PAP vs CHAP incohérents** : serveur en `chap`, client en `pap` → échec d'authentification. Toujours aligner.
3. **`dialer-group` sans `dialer-rule`** : le Dialer ne sait pas quel trafic déclenche l'appel → la session ne monte pas. La règle `dialer-rule 1 ip permit` est obligatoire.
4. **`pppoe-server bind` sur une interface restée en mode switch** : `undo portswitch` d'abord.
5. **Pool PPPoE vide ou mal dimensionné** : avec un /30, une seule adresse attribuable ; un second client échouerait (IPCP nak). En lab c'est voulu pour comprendre, en production dimensionner le pool.
6. **MTU PPPoE** : 1492 au lieu de 1500 (8 octets d'en-tête PPPoE+PPP). Symptôme en production : "les petits sites web passent, les gros chargements bloquent" → régler `mtu 1492` ou activer le TCP MSS adjust. À mentionner.

### Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Serveur PPPoE complet (virtual-template, pool, AAA, bind) | 6 |
| Client PPPoE complet (Dialer, CHAP, dialer-rule, bind) | 6 |
| Session UP + IP négociée vérifiées | 3 |
| NAT + défaut via Dialer, ping de bout en bout | 3 |
| Diagnostic de la panne d'authentification (méthode + correction) | 2 |

### Durée estimée
**1 h 30** (dont 20 min de capture PPPoE).

### Fiche animateur — points à insister
- PPPoE = 2 phases : **Discovery** (trouver le serveur, 4 paquets PADI/PADO/PADR/PADS) puis **Session** (LCP, authentification, IPCP). Faire retrouver chaque phase dans la capture.
- CHAP ne transmet jamais le mot de passe en clair (challenge/response) contrairement à PAP : d'où sa préférence.
- Lien terrain : quand un boîtier opérateur est en mode **bridge**, c'est l'AR720 qui monte la session PPPoE — exactement ce TP. En mode routeur, le boîtier fait le PPPoE et l'AR720 fait juste du DHCP/NAT derrière.
- Le MTU 1492 est un classique des tickets "ça rame sur certaines applis" sur les accès PPPoE : à garder en tête pour le support.
- Question piège : "Où est configurée l'IP publique du client ?" → Nulle part en statique : elle est **négociée** (IPCP). C'est la différence avec une liaison louée.

---

## 11. TP9 — IPSec site-à-site

### Objectif
Monter un tunnel IPSec entre deux sites : phase 1 (IKE), phase 2 (IPSec), interesting traffic via ACL, vérifications et pièges (ACL miroir, NAT-T).

### Prérequis
TP5 (routage), TP7 (NAT), adressage.

### Topologie

```
Site A (192.168.10.0/24)                  "Internet" (10.0.0.0/30)                  Site B (192.168.20.0/24)
PC1 ──SW1── GE0/0/1 R1 (AR2220) GE0/0/2 ════════════════ GE0/0/2 R2 (AR2220) GE0/0/1──SW2── PC2
              .254          100.64.1.1/30  tunnel IPSec   100.64.1.2/30         .254
```

- **R1** : GE0/0/1 = 192.168.10.254/24, GE0/0/2 = 100.64.1.1/30 (IP publique simulée).
- **R2** : GE0/0/1 = 192.168.20.254/24, GE0/0/2 = 100.64.1.2/30.
- Routage : R1 a une défaut vers 100.64.1.2 ; R2 une défaut vers 100.64.1.1 (ils se joignent directement en lab).
- Clé pré-partagée de lab : `Cle-IPSec-Lab-2026` (jamais en production telle quelle).

### Énoncé

1. Configurer la **phase 1 (IKE)** : proposal (AES-256, SHA-256, DH group 14), peer avec pre-shared-key, des deux côtés.
2. Configurer la **phase 2 (IPSec)** : proposal ESP (AES-256 + SHA-256), ACL d'**interesting traffic** (trafic Site A ↔ Site B), policy qui lie le tout.
3. Appliquer la policy IPSec sur les interfaces externes (GE0/0/2).
4. Vérifier : `display ike sa`, `display ipsec sa` → tunnel établi.
5. Tester : PC1 ping PC2 → passe **dans** le tunnel (vérifier les compteurs IPSec qui augmentent).
6. **Pièges à provoquer** (un par un, diagnostiquer à chaque fois) :
   - a) ACL non miroir (oubli du sens retour sur R2).
   - b) Clé pré-partagée différente d'un côté.
   - c) Policy IPSec appliquée sur la mauvaise interface.

### Correction pas à pas

**R1 — Phase 1 (IKE) :**

```
[R1]ike proposal 10
[R1-ike-proposal-10]encryption-algorithm aes-256
[R1-ike-proposal-10]authentication-algorithm sha2-256
[R1-ike-proposal-10]dh group14
[R1-ike-proposal-10]quit

[R1]ike peer R2
[R1-ike-peer-R2]pre-shared-key simple Cle-IPSec-Lab-2026
[R1-ike-peer-R2]ike-proposal 10
[R1-ike-peer-R2]remote-address 100.64.1.2
[R1-ike-peer-R2]quit
```

**R1 — Phase 2 (IPSec) :**

```
[R1]ipsec proposal PROP-A
[R1-ipsec-proposal-PROP-A]esp authentication-algorithm sha2-256
[R1-ipsec-proposal-PROP-A]esp encryption-algorithm aes-256
[R1-ipsec-proposal-PROP-A]quit

# Interesting traffic : le trafic à protéger (les deux sens du point de vue de R1)
[R1]acl 3000
[R1-acl-adv-3000]rule permit ip source 192.168.10.0 0.0.0.255 destination 192.168.20.0 0.0.0.255
[R1-acl-adv-3000]quit

[R1]ipsec policy POLICY-A 10 isakmp
[R1-ipsec-policy-POLICY-A-10]security acl 3000
[R1-ipsec-policy-POLICY-A-10]ike-peer R2
[R1-ipsec-policy-POLICY-A-10]proposal PROP-A
[R1-ipsec-policy-POLICY-A-10]quit

[R1]interface GigabitEthernet 0/0/2
[R1-GigabitEthernet0/0/2]ipsec policy POLICY-A
[R1-GigabitEthernet0/0/2]quit
```

**R2 — miroir exact :**

```
[R2]ike proposal 10
[R2-ike-proposal-10]encryption-algorithm aes-256
[R2-ike-proposal-10]authentication-algorithm sha2-256
[R2-ike-proposal-10]dh group14
[R2-ike-proposal-10]quit

[R2]ike peer R1
[R2-ike-peer-R1]pre-shared-key simple Cle-IPSec-Lab-2026
[R2-ike-peer-R1]ike-proposal 10
[R2-ike-peer-R1]remote-address 100.64.1.1
[R2-ike-peer-R1]quit

[R2]ipsec proposal PROP-A
[R2-ipsec-proposal-PROP-A]esp authentication-algorithm sha2-256
[R2-ipsec-proposal-PROP-A]esp encryption-algorithm aes-256
[R2-ipsec-proposal-PROP-A]quit

# ACL MIROIR : source/destination inversées par rapport à R1
[R2]acl 3000
[R2-acl-adv-3000]rule permit ip source 192.168.20.0 0.0.0.255 destination 192.168.10.0 0.0.0.255
[R2-acl-adv-3000]quit

[R2]ipsec policy POLICY-A 10 isakmp
[R2-ipsec-policy-POLICY-A-10]security acl 3000
[R2-ipsec-policy-POLICY-A-10]ike-peer R1
[R2-ipsec-policy-POLICY-A-10]proposal PROP-A
[R2-ipsec-policy-POLICY-A-10]quit

[R2]interface GigabitEthernet 0/0/2
[R2-GigabitEthernet0/0/2]ipsec policy POLICY-A
```

> **Important** : le tunnel ne monte que quand du trafic "intéressant" le déclenche (ou avec `sa trigger` selon version). Lancer `PC1> ping 192.168.20.11` pour déclencher la négociation.

### Vérifications

```
[R1]display ike sa
# Phase 1 : état RD|ST (Ready/StayAlive) = tunnel IKE établi.
# Si vide ou état négociant : problème phase 1 (clé, proposal, joignabilité du peer).

[R1]display ipsec sa
# Phase 2 : une SA dans chaque sens (inbound/outbound), avec les SPI, compteurs de paquets.

[R1]display ipsec statistics
# Compteurs chiffrés/déchiffrés : lancer des pings et voir les compteurs augmenter
# = preuve que le trafic passe DANS le tunnel.

[R1]display ike proposal
[R1]display ipsec policy
```

**Capture Wireshark** sur le lien inter-routeurs : sans IPSec on verrait des ICMP en clair ; avec le tunnel, on voit des paquets **ESP** (protocole 50) entre 100.64.1.1 et 100.64.1.2, illisibles. C'est la démonstration de la confidentialité. On distingue aussi au début les échanges **IKE sur UDP 500**.

### Pièges classiques

1. **ACL non miroir** (panne a) : R2 avec `source 192.168.10.0 destination 192.168.20.0` (copié-collé de R1 sans inverser) → la phase 2 ne négocie pas les bons proxy-ID → `display ipsec sa` vide d'un côté. **Règle d'or : l'ACL de R2 est le miroir de celle de R1.**
2. **Clé pré-partagée différente** (panne b) : la phase 1 échoue → `display ike sa` vide. Message peu explicite : toujours revérifier la clé en premier (erreur la plus fréquente en production aussi).
3. **Policy appliquée sur la mauvaise interface** (panne c) : sur GE0/0/1 (LAN) au lieu de GE0/0/2 (WAN) → le trafic n'est jamais intercepté. `display ipsec policy` montre où la policy est appliquée.
4. **Proposals incompatibles** : AES-256 d'un côté, 3DES de l'autre → échec de négociation. Aligner strictement.
5. **NAT devant IPSec** : si un NAT se trouve entre les deux routeurs (cas d'un site derrière une box), l'ESP (protocole 50, pas de ports) ne traverse pas le NAPT → activer **NAT-T** (encapsulation ESP dans UDP 4500). En lab, à évoquer ; sur eNSP : `ike nat-traversal` si supporté. En production derrière box : quasi systématique.
6. **Le trafic interessant inclut le trafic de gestion** : si l'ACL est trop large (ex. `permit ip`), même les pings entre les IP publiques passent dans le tunnel → à éviter, l'ACL doit être précise.
7. **Oublier que le tunnel est déclenché par le trafic** : après configuration, `display ike sa` peut rester vide jusqu'au premier ping. Toujours générer du trafic avant de conclure à une panne.

### Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Phase 1 IKE complète et symétrique (proposal + peer + PSK) | 5 |
| Phase 2 complète (proposal ESP + ACL interesting traffic) | 4 |
| ACL miroir correcte sur R2 | 3 |
| Policy créée et appliquée sur les bonnes interfaces | 3 |
| Vérifications (`ike sa`, `ipsec sa`, compteurs) + capture ESP commentée | 3 |
| Diagnostic d'au moins une panne provoquée | 2 |

### Durée estimée
**1 h 30** (dont 20 min de pannes provoquées).

### Fiche animateur — points à insister
- Deux phases, deux négociants : **IKE** (phase 1 : "peut-on se parler en secret ?") puis **IPSec** (phase 2 : "quel trafic protège-t-on et comment ?"). Faire réciter.
- L'**interesting traffic** est le cœur du policy-based IPSec : tout ce qui matche l'ACL est chiffré, le reste passe en clair. D'où l'importance d'ACL précises et miroirs.
- La capture ESP illisible = la meilleure preuve de la confidentialité. Comparer avec une capture sans IPSec (pings en clair).
- Lien terrain : IPSec site-à-site entre agences sur vos AR720 ; NAT-T dès qu'une box opérateur est devant.
- Question piège : "Le ping entre les deux IP publiques (100.64.1.1 ↔ 100.64.1.2) est-il chiffré ?" → Non, il ne matche pas l'ACL d'interesting traffic. Seul le trafic LAN-à-LAN l'est.

---

## 12. TP10 — WLAN : AC + AP en mode Fit

### Objectif
Déployer un réseau Wi-Fi d'entreprise en mode **Fit AP** : un contrôleur (AC) centralise la configuration, les AP diffusent le SSID, les clients s'associent avec authentification WPA2-PSK.

### Prérequis
TP1 (VLAN), adressage. Notions Wi-Fi (SSID, PSK).

### Topologie

```
STA1 (client Wi-Fi)               STA2
   \  ))  wifi  ((  /              (wifi)
    ╲                ╱
   AP1 (AP6010DN) ──câble── GE0/0/1 SW1 (S3700, PoE simulé) GE0/0/2 ── GE0/0/1 AC1 (AC6605)
   AP2 (AP6010DN) ──câble── GE0/0/3            VLAN 100 (gestion AP)
```

- **AC1 (AC6605)** : contrôleur WLAN, GE0/0/1 = 192.168.100.1/24 (VLAN 100).
- **SW1 (S3700)** : VLAN 100 (ports GE0/0/1-3 en access VLAN 100) + VLAN 10 (service, 192.168.10.0/24).
- **AP1, AP2 (AP6010DN-AGN)** : branchés sur SW1, obtiennent leur IP en DHCP (ou statique) dans le VLAN 100, découvrent l'AC.
- **STA1, STA2** : stations Wi-Fi simulées (objets STA d'eNSP), à associer au SSID.
- SSID : `LABO-ENTREPRISE`, WPA2-PSK, clé `WifiLabo2026!`, VLAN de service 10.

> **Note de cadrage** : eNSP émule des AP Wi-Fi 4/5 (AP6010DN), pas vos AP361/AP761 (Wi-Fi 6). La **logique Fit AP** (profils SSID/sécurité/VAP, découverte CAPWAP, radios) est identique : ce TP entraîne aux concepts que vous retrouverez sur le vrai matériel (voir vos guides AP361/AP761 pour les spécificités Wi-Fi 6).

### Énoncé

1. Adresser le VLAN 100 (gestion) : AC1 en 192.168.100.1/24, AP en DHCP ou statique dans 192.168.100.0/24.
2. Sur AC1 : activer le WLAN (`wlan`), déclarer les AP (par MAC), les nommer AP1/AP2.
3. Créer les profils : `security-profile` (WPA2-PSK), `ssid-profile` (nom du SSID), `vap-profile` (lie les deux + VLAN de service 10 + mode de transfert).
4. Créer le groupe AP / domaine de régulation, associer le VAP aux radios des AP.
5. Vérifier que les AP passent à l'état **normal** (tunnel CAPWAP établi).
6. Associer STA1/STA2 au SSID avec la clé, vérifier l'obtention d'IP (DHCP du VLAN 10 via R1 ou via AC).
7. Tester la connectivité Wi-Fi ↔ filaire.

### Correction pas à pas

**Réseau de gestion (VLAN 100) :**

```
# SW1
[SW1]vlan batch 10 100
[SW1]interface GigabitEthernet 0/0/1
[SW1-GigabitEthernet0/0/1]port link-type access
[SW1-GigabitEthernet0/0/1]port default vlan 100
# Idem GE0/0/2, GE0/0/3 (vers AC et AP)
```

**AC1 — base :**

```
[AC1]interface GigabitEthernet 0/0/1
[AC1-GigabitEthernet0/0/1]undo portswitch
[AC1-GigabitEthernet0/0/1]ip address 192.168.100.1 24
[AC1]wlan
[AC1-wlan-view]quit
```

**Déclaration des AP (récupérer leurs MAC via `display ap all` après qu'ils ont démarré, ou les lire sur l'étiquette eNSP) :**

```
[AC1]wlan
[AC1-wlan-view]ap-id 1 ap-mac 00e0-fc12-3456
[AC1-wlan-ap-1]ap-name AP1
[AC1-wlan-ap-1]quit
[AC1-wlan-view]ap-id 2 ap-mac 00e0-fc65-4321
[AC1-wlan-ap-2]ap-name AP2
[AC1-wlan-ap-2]quit
```

> En lab, les MAC des AP eNSP sont visibles dans les propriétés de l'équipement ou via `display ap all` (état idle avant configuration). Sur le vrai matériel, on utilise souvent l'authentification par **numéro de série (SN)**.

**Profils WLAN :**

```
# 1. Profil de sécurité : WPA2-PSK
[AC1-wlan-view]security-profile name SEC-LABO
[AC1-wlan-sec-prof-SEC-LABO]security wpa2 psk pass-phrase WifiLabo2026! aes
[AC1-wlan-sec-prof-SEC-LABO]quit

# 2. Profil SSID
[AC1-wlan-view]ssid-profile name SSID-LABO
[AC1-wlan-ssid-prof-SSID-LABO]ssid LABO-ENTREPRISE
[AC1-wlan-ssid-prof-SSID-LABO]quit

# 3. Profil VAP : lie SSID + sécurité + VLAN de service + transfert direct
[AC1-wlan-view]vap-profile name VAP-LABO
[AC1-wlan-vap-prof-VAP-LABO]service-vlan vlan-id 10
[AC1-wlan-vap-prof-VAP-LABO]ssid-profile SSID-LABO
[AC1-wlan-vap-prof-VAP-LABO]security-profile SEC-LABO
[AC1-wlan-vap-prof-VAP-LABO]forward-mode direct-forward
[AC1-wlan-vap-prof-VAP-LABO]quit
```

**Domaine de régulation + groupe AP :**

```
[AC1-wlan-view]regulatory-domain-profile name REG-FR
[AC1-wlan-regulatory-domain-prof-REG-FR]country-code FR
[AC1-wlan-regulatory-domain-prof-REG-FR]quit

[AC1-wlan-view]ap-group name GROUPE-LABO
[AC1-wlan-ap-group-GROUPE-LABO]regulatory-domain-profile REG-FR
[AC1-wlan-ap-group-GROUPE-LABO]vap-profile VAP-LABO wlan 1 radio all
[AC1-wlan-ap-group-GROUPE-LABO]quit

# Rattacher les AP au groupe :
[AC1-wlan-view]ap-id 1
[AC1-wlan-ap-1]ap-group GROUPE-LABO
[AC1-wlan-ap-1]quit
[AC1-wlan-view]ap-id 2
[AC1-wlan-ap-2]ap-group GROUPE-LABO
[AC1-wlan-ap-2]quit
[AC1]save
```

**Vérifier l'état des AP :**

```
[AC1]display ap all
# État "nor" (normal) = tunnel CAPWAP établi, profils poussés, radios actives.
# Les états intermédiaires (config, configFailed...) indiquent où ça bloque.
```

**Associer les STA :** double-clic sur STA1 > onglet WLAN > SSID `LABO-ENTREPRISE`, sécurité WPA2-PSK, clé `WifiLabo2026!`. La STA doit s'associer (état connecté visible dans eNSP) puis obtenir une IP du VLAN 10 (prévoir un serveur DHCP sur ce VLAN, ex. R1 du TP4 ou un pool sur l'AC).

**Vérifications côté AC :**

```
[AC1]display ap all                    # AP en "nor"
[AC1]display station all               # clients associés : MAC, SSID, AP, radio, VLAN
[AC1]display vap all                   # VAP diffusés par AP/radio
[AC1]display ssid-profile name SSID-LABO
```

### Pièges classiques

1. **AP qui reste en `idle`** : pas de connectivité IP avec l'AC (VLAN 100 mal configuré, trunk oublié) → le tunnel CAPWAP ne monte pas. Vérifier le ping AP→AC.
2. **AP en `configFailed`** : souvent un profil référencé qui n'existe pas (faute de frappe dans le nom du ssid-profile) → `display ap config-info ap-id 1` pour le détail.
3. **`vap-profile ... wlan 1 radio all` oublié** : les profils existent mais ne sont pas diffusés → le SSID n'apparaît pas. C'est l'oubli n°1.
4. **Mauvais `service-vlan`** : clients associés mais sans IP / sans connectivité → le VLAN de service ne correspond pas au VLAN du DHCP/routeur.
5. **`forward-mode`** : en `tunnel-forward` (centralisé, défaut sur certains modèles), tout le trafic client remonte à l'AC ; en `direct-forward`, l'AP bridge localement vers le switch. En lab les deux marchent, mais le mode doit être cohérent avec le câblage (en tunnel-forward, pas besoin du VLAN 10 sur le lien AP→switch).
6. **Clé PSK avec caractères spéciaux** : en lab, rester sur de l'alphanumérique pour éviter les problèmes de saisie côté STA.
7. **Régulation** : sans `country-code`, les radios peuvent rester éteintes (puissance/canaux non autorisés). Toujours configurer le domaine de régulation.

### Barème indicatif (20 points)

| Critère | Points |
|---|---|
| VLAN 100 + adressage AC/AP | 3 |
| AP déclarés (MAC) et nommés | 3 |
| Les 3 profils (sécurité, SSID, VAP) corrects et liés | 5 |
| Groupe AP + domaine de régulation + VAP sur les radios | 3 |
| AP en état normal, STA associées avec IP | 4 |
| Vérifications (`display ap all`, `display station all`) | 2 |

### Durée estimée
**1 h 30**.

### Fiche animateur — points à insister
- La logique **Fit AP** : l'AP est "bête", l'AC est "intelligent" (configuration centralisée, tunnel CAPWAP). C'est exactement l'architecture de vos AP361/AP761 gérés en central.
- Les **profils** sont des briques réutilisables : un même `security-profile` peut servir à plusieurs SSID, un même `vap-profile` à plusieurs groupes d'AP. C'est la clé d'un déploiement propre à l'échelle d'une entreprise.
- `display ap all` est LE réflexe : l'état de l'AP dit tout (idle = réseau, config = en cours, nor = OK, configFailed = profil).
- Lien avec vos guides AP361/AP761 : la syntaxe des profils est la même famille ; les différences portent sur le Wi-Fi 6 (OFDMA, etc.), pas sur la logique Fit.
- Question piège : "Un client voit le SSID mais n'obtient pas d'IP : où chercher ?" → service-vlan, DHCP du VLAN, forward-mode. "Un client ne voit pas le SSID ?" → VAP non lié aux radios, AP pas en normal.

---

## 13. TP11 — USG : zones, politiques, NAT sortant

### Objectif
Prendre en main un pare-feu Huawei USG : zones de sécurité, politiques interzones, NAT sortant — la base de l'administration de vos USG6000.

### Prérequis
TP1 (VLAN), TP5 (routage), TP7 (NAT).

### Topologie

```
                    ┌──────────── zone trust ────────────┐
                    │                                    │
PC1 (.11) ──SW1── GE0/0/1 FW1 (USG5500) GE0/0/2 ── R-ISP ── "Internet" (Server 203.0.113.10)
PC2 (.12)   VLAN10   192.168.10.254/24   198.51.100.1/30 │ 198.51.100.2/30
Serveur (.100, web)  └────── zone untrust ──────────────┘
                     zone DMZ (optionnelle) : GE0/0/3, 192.168.50.254/24, serveur DMZ
```

- **FW1 (USG5500)** : GE0/0/1 en zone **trust** (192.168.10.254/24), GE0/0/2 en zone **untrust** (198.51.100.1/30).
- **R-ISP** : 198.51.100.2/30 + LAN 203.0.113.0/24 avec un Server public.
- Politique par défaut d'un USG : **tout est refusé entre zones** sauf ce qui est explicitement autorisé.

> **Note de cadrage** : eNSP propose l'USG5500 (syntaxe `firewall zone` + `policy interzone`), vos USG6000 utilisent la même logique de zones avec la syntaxe `security-policy` plus récente. Les concepts (zones, interzone, NAT dans la politique) sont identiques : ce TP entraîne au raisonnement "zones", transférable tel quel.

### Énoncé

1. Créer les zones, y affecter les interfaces, adresser.
2. Vérifier que **par défaut rien ne passe** (PC1 ping 203.0.113.10 → échec) : comprendre le deny implicite.
3. Créer une politique **trust → untrust** autorisant tout (ou au moins ICMP/HTTP/DNS).
4. Configurer le **NAT sortant** (Easy IP) sur l'interface untrust.
5. Tester : PC1 ping et web vers 203.0.113.10 → OK.
6. Créer une politique **untrust → trust** autorisant **uniquement** HTTP vers le serveur interne 192.168.10.100 (port mapping via NAT server ou destination NAT).
7. Tester depuis le Server public : `http://198.51.100.1` → page du serveur interne ; mais `ping 198.51.100.1` → échec (ICMP non autorisé en inbound).
8. (Optionnel) Ajouter une zone **DMZ** avec un serveur, politique untrust→dmz (HTTP only) et trust→dmz (tout).

### Correction pas à pas

**Zones et interfaces :**

```
[FW1]firewall zone trust
[FW1-zone-trust]add interface GigabitEthernet 0/0/1
[FW1-zone-trust]quit
[FW1]firewall zone untrust
[FW1-zone-untrust]add interface GigabitEthernet 0/0/2
[FW1-zone-untrust]quit

[FW1]interface GigabitEthernet 0/0/1
[FW1-GigabitEthernet0/0/1]ip address 192.168.10.254 24
[FW1]interface GigabitEthernet 0/0/2
[FW1-GigabitEthernet0/0/2]ip address 198.51.100.1 30
[FW1]ip route-static 0.0.0.0 0.0.0.0 198.51.100.2
```

> Sur l'USG5500 d'eNSP, les interfaces sont déjà en mode L3 (pas de `undo portswitch` nécessaire en général — vérifier avec `display ip interface brief`).

**Test du deny par défaut :**

```
PC1> ping 203.0.113.10    # ÉCHEC : aucune politique interzone n'existe encore.
```

**Politique trust → untrust :**

```
[FW1]policy interzone trust untrust outbound
[FW1-policy-interzone-trust-untrust-outbound]policy 1
[FW1-policy-interzone-trust-untrust-outbound-1]action permit
[FW1-policy-interzone-trust-untrust-outbound-1]policy source 192.168.10.0 mask 24
[FW1-policy-interzone-trust-untrust-outbound-1]quit
[FW1-policy-interzone-trust-untrust-outbound]quit
```

> `outbound` vu depuis la zone source (trust) : c'est le sens trust→untrust. Sans `policy source`, la règle s'applique à tout le trafic de la zone.

**NAT sortant (Easy IP) :**

```
[FW1]acl 2000
[FW1-acl-basic-2000]rule permit source 192.168.10.0 0.0.0.255
[FW1-acl-basic-2000]quit
[FW1]interface GigabitEthernet 0/0/2
[FW1-GigabitEthernet0/0/2]nat outbound 2000
[FW1-GigabitEthernet0/0/2]quit
```

**Test :** `PC1> ping 203.0.113.10` → OK maintenant (politique + NAT).

**Exposition du serveur web interne (untrust → trust, HTTP only) :**

```
# 1. Port mapping (NAT server) comme au TP7 :
[FW1]interface GigabitEthernet 0/0/2
[FW1-GigabitEthernet0/0/2]nat server protocol tcp global 198.51.100.1 www inside 192.168.10.100 www
[FW1-GigabitEthernet0/0/2]quit

# 2. Politique untrust -> trust : UNIQUEMENT http vers le serveur
[FW1]policy interzone untrust trust inbound
[FW1-policy-interzone-untrust-trust-inbound]policy 1
[FW1-policy-interzone-untrust-trust-inbound-1]action permit
[FW1-policy-interzone-untrust-trust-inbound-1]policy destination 192.168.10.100 mask 32
[FW1-policy-interzone-untrust-trust-inbound-1]service http
[FW1-policy-interzone-untrust-trust-inbound-1]quit
[FW1-policy-interzone-untrust-trust-inbound]quit
```

**Tests depuis l'extérieur (Server public 203.0.113.10) :**

```
# http://198.51.100.1  →  OK (page du serveur 192.168.10.100)
# ping 198.51.100.1    →  KO (ICMP non autorisé en untrust->trust : deny implicite)
```

**Variante DMZ (optionnelle) :**

```
[FW1]firewall zone dmz
[FW1-zone-dmz]add interface GigabitEthernet 0/0/3
[FW1]interface GigabitEthernet 0/0/3
[FW1-GigabitEthernet0/0/3]ip address 192.168.50.254 24

# untrust -> dmz : HTTP only vers le serveur DMZ
[FW1]policy interzone untrust dmz inbound
[FW1-policy-interzone-untrust-dmz-inbound]policy 1
[FW1-policy-interzone-untrust-dmz-inbound-1]action permit
[FW1-policy-interzone-untrust-dmz-inbound-1]policy destination 192.168.50.100 mask 32
[FW1-policy-interzone-untrust-dmz-inbound-1]service http
```

### Vérifications

```
[FW1]display zone
# Zones, interfaces membres, priorités (trust=85, dmz=50, untrust=5, local=100).

[FW1]display policy interzone trust untrust outbound
# Règles configurées dans ce sens.

[FW1]display firewall session table
# Sessions suivies par le firewall (stateful) : constater que le retour est autorisé
# automatiquement (pas besoin de règle retour : le firewall est stateful).

[FW1]display nat outbound
[FW1]display nat server
```

> **Point clé à faire verbaliser** : le firewall est **stateful** : une seule règle trust→untrust suffit pour l'aller-retour d'une session initiée depuis le trust. C'est la différence avec une simple ACL de routeur (stateless).

### Pièges classiques

1. **Oublier que le deny est implicite** : "j'ai autorisé trust→untrust, pourquoi le retour ne passe pas ?" → En fait le retour passe (stateful). La vraie question inverse : "j'ai ouvert untrust→trust pour le web, pourquoi le ping entrant ne passe pas ?" → Parce que seul `http` est autorisé. Bien distinguer.
2. **`inbound`/`outbound` inversés** : `policy interzone trust untrust outbound` = dans le sens trust→untrust. Se tromper de sens = règle sans effet.
3. **NAT server sans politique** : le mapping existe mais le firewall bloque → toujours les **deux** (NAT + politique). Ordre de traitement : NAT destination d'abord, puis politique interzone sur l'adresse traduite.
4. **Oublier la route de retour** côté ISP vers 198.51.100.1 (ici directe, mais en production : route vers l'IP publique du firewall).
5. **Zone `local`** : le trafic **à destination du firewall lui-même** (ping de son IP, web d'admin) passe par les politiques vers/depuis la zone `local`. En lab, si le ping vers 198.51.100.1 échoue depuis l'extérieur alors qu'on l'attendait : c'est la politique untrust→local qui manque (et c'est voulu dans l'énoncé).
6. **Tester le port mapping depuis le LAN** : même piège qu'au TP7 (hairpin) → tester depuis l'extérieur.

### Barème indicatif (20 points)

| Critère | Points |
|---|---|
| Zones créées, interfaces affectées et adressées | 4 |
| Compréhension du deny par défaut (test initial documenté) | 2 |
| Politique trust→untrust + NAT sortant fonctionnels | 5 |
| Port mapping + politique untrust→trust restrictive (HTTP only) | 6 |
| Vérifications (`display zone`, sessions stateful) + tests inbound/outbound | 3 |

### Durée estimée
**1 h 30** (dont 20 min pour la variante DMZ).

### Fiche animateur — points à insister
- Le raisonnement "zones" : on ne filtre plus des IP, on autorise des **flux entre zones**. C'est le changement de paradigme par rapport aux ACL de routeur.
- **Deny implicite partout** : chaque flux doit être explicitement autorisé. En audit, partir du principe "tout ce qui n'est pas autorisé est interdit".
- Stateful : une règle aller suffit pour le retour d'une session établie. Démonstration : `display firewall session table` pendant un ping.
- Ordre : **NAT destination → politique interzone** : la politique voit l'adresse **après** translation (d'où `policy destination 192.168.10.100`, l'adresse interne).
- Lien terrain : vos USG6000 = même logique, syntaxe `security-policy` (règles nommées avec `rule name ... source-zone ... destination-zone ... action permit`). Le raisonnement appris ici s'applique tel quel.
- Question piège : "Faut-il une règle untrust→trust pour que le retour d'une navigation web passe ?" → Non (stateful). "Et pour exposer un serveur ?" → Oui, c'est un flux **initié** depuis untrust.

---

## 14. TP12 — Dépannage guidé : la topologie cassée

### Objectif
Mettre en pratique une **méthode de diagnostic** structurée sur une topologie contenant 5 pannes volontaires couvrant les couches 2, 3 et la sécurité.

### Prérequis
Tous les TP précédents (ou au minimum TP1, TP2, TP5, TP6, TP7).

### Topologie (saine de référence)

```
PC1 (VLAN 10, 192.168.10.11/24) ──E0/0/1 SW1 (S3700) GE0/0/1 ═══ GE0/0/1 SW2 (S3700) E0/0/1── PC3 (VLAN 10, 192.168.10.13/24)
PC2 (VLAN 20, 192.168.20.12/24) ──E0/0/2        (trunk)                    E0/0/2── PC4 (VLAN 20, 192.168.20.14/24)
                                            GE0/0/2 │ (trunk vers R1)
                                                    │ GE0/0/1.10 (192.168.10.254) / GE0/0/1.20 (192.168.20.254)
                                                    │ R1 (AR2220, OSPF area 0)
                                            GE0/0/2 │ (10.0.12.1/30)
                                                    │ GE0/0/2 (10.0.12.2/30)
                                                    │ R2 (AR2220, OSPF area 0, NAT Easy IP sur GE0/0/3)
                                            GE0/0/3 │ (198.51.100.1/30)
                                                    │ "Internet" : R-ISP (198.51.100.2/30) + Server (203.0.113.10)
```

- Routage inter-VLAN via R1 (sous-interfaces), OSPF entre R1 et R2, NAT sur R2 vers l'extérieur.
- Objectif final : PC1 peut joindre PC3 (L2), PC2 (routage), et 203.0.113.10 (NAT+Internet).

### Les 5 pannes (à introduire par l'animateur, une fiche par stagiaire/groupe)

| # | Panne | Couche | Symptôme | Commande de sabotage (animateur) |
|---|---|---|---|---|
| 1 | Port E0/0/1 de SW2 resté en **VLAN 1** au lieu du VLAN 10 | L2 / VLAN | PC1 ne ping pas PC3, mais PC1 ping SW1... (en fait PC1 ping PC2 ? non : PC3 injoignable) | `interface E0/0/1` → `undo port default vlan` (retour PVID 1) |
| 2 | Trunk SW1↔SW2 : `allow-pass` **sans le VLAN 20** côté SW1 | L2 / trunk | PC2 ne ping pas PC4 ; PC1 ping PC3 OK | `port trunk allow-pass vlan 10` seul sur GE0/0/1 de SW1 |
| 3 | OSPF : **Router-ID dupliqué** (R2 configuré avec 1.1.1.1 comme R1) | L3 / OSPF | Adjacence Down, pas de route OSPF, PC1 ne ping pas... (en fait ici R2 ne route plus vers Internet : PC1 ne ping pas 203.0.113.10) | `ospf 1 router-id 1.1.1.1` sur R2 |
| 4 | NAT : **ACL 2000 avec mauvais wildcard** (`0.0.255.255` au lieu de `0.0.0.255`) | L3 / NAT | Le ping vers Internet échoue (pas de translation) alors que le routage est OK | `rule permit source 192.168.0.0 0.0.255.255` |
| 5 | **ACL sur GE0/0/2 de R1** bloquant tout (oubliée d'un TP précédent) | Sécurité | Tout le trafic inter-sites bloqué, pings KO partout sauf en local | `traffic-filter inbound acl 3000` avec une acl 3000 `rule deny` |

### Énoncé (distribué aux stagiaires)

> "Le réseau ci-dessus **fonctionnait hier soir**. Ce matin, plusieurs utilisateurs se plaignent : certains PC ne se joignent plus, Internet est coupé par intermittence... Votre mission : **trouver et réparer les 5 pannes**, en suivant la méthode de diagnostic. Pour chaque panne : symptômes observés, hypothèse, commande de vérification, correction, test de validation. Temps imparti : 1 h 30."

### Correction : la méthode pas à pas (à faire appliquer)

**Méthode en 6 étapes (à afficher en salle) :**

```
1. DÉFINIR le symptôme précisément (qui ne joint pas qui ? depuis quand ?).
2. DÉLIMITER : le problème est-il local (un PC), par VLAN, par site, global ?
3. VÉRIFIER la couche 1/2 : liens up ? (display interface brief)
4. VÉRIFIER la couche 2 : VLAN, trunk, STP (display port vlan, display vlan)
5. VÉRIFIER la couche 3 : IP, routage, protocoles (display ip routing-table, display ospf peer)
6. VÉRIFIER les fonctions avancées : DHCP, NAT, ACL, firewall (display nat session, display acl all)
   → Corriger UNE panne à la fois, re-tester, documenter.
```

**Déroulé type attendu :**

**Étape 1 — Cartographier les symptômes :**

```
PC1> ping 192.168.10.13 (PC3)   → KO   # même VLAN, switches différents
PC1> ping 192.168.10.12 (PC2)   → ...  # autre VLAN, via R1
PC2> ping 192.168.20.14 (PC4)   → KO   # même VLAN, switches différents
PC1> ping 203.0.113.10          → KO   # Internet
```

> Premier tri : les pannes intra-VLAN inter-switch (1 et 2) vs le reste.

**Panne 1 — PC1 ne ping pas PC3 :**

```
[SW2]display port vlan
# E0/0/1 : PVID 1, pas membre du VLAN 10 → le port n'est pas dans le bon VLAN.
[SW2]interface Ethernet 0/0/1
[SW2-Ethernet0/0/1]port link-type access
[SW2-Ethernet0/0/1]port default vlan 10
# Test : PC1 ping PC3 → OK.
```

**Panne 2 — PC2 ne ping pas PC4 (mais PC1 ping PC3 OK après réparation 1) :**

```
[SW1]display port vlan
# GE0/0/1 (trunk) : allow-pass vlan 10 seul → le VLAN 20 ne traverse pas.
[SW2]display port vlan
# GE0/0/1 : allow-pass vlan 10 20 → asymétrie détectée !
[SW1]interface GigabitEthernet 0/0/1
[SW1-GigabitEthernet0/0/1]port trunk allow-pass vlan 10 20
# Test : PC2 ping PC4 → OK.
```

**Panne 3 — OSPF ne monte pas (pas de route vers Internet / inter-sites KO) :**

```
[R1]display ospf peer
# Aucun voisin (ou état anormal).
[R1]display ospf interface
# Area 0 des deux côtés, timers OK...
[R2]display current-configuration | include router-id
# R2 a router-id 1.1.1.1 → DOUBLON avec R1 !
[R2]ospf 1
[R2-ospf-1]router-id 2.2.2.2
[R2-ospf-1]quit
[R2]reset ospf 1   # (ou reboot du process pour prendre en compte)
[R1]display ospf peer
# État Full → adjacence rétablie.
```

**Panne 4 — Internet KO malgré OSPF OK :**

```
# Le ping vers 198.51.100.2 (IP publique de R2) passe depuis R1,
# mais PC1> ping 203.0.113.10 échoue → le routage est bon, c'est le NAT.
[R2]display acl 2000
# rule permit source 192.168.0.0 0.0.255.255 → wildcard faux !
# (matche 192.168.0.0-192.168.255.255... en fait le wildcard 0.0.255.255 matche bien
#  192.168.10.0 ! → subtilité : la règle matche, donc le NAT devrait marcher...)
```

> **Variante plus parlante** : l'animateur mettra plutôt `rule permit source 192.168.11.0 0.0.0.255` (mauvais réseau) → aucun paquet de 192.168.10.0 ne matche → pas de translation. `display nat session all` reste vide pendant les pings = preuve que le NAT ne s'enclenche pas.

```
[R2]acl 2000
[R2-acl-basic-2000]undo rule 5   # (numéro de la règle fautive)
[R2-acl-basic-2000]rule permit source 192.168.10.0 0.0.0.255
[R2-acl-basic-2000]rule permit source 192.168.20.0 0.0.0.255
# Test : PC1 ping 203.0.113.10 → OK (display nat session all montre les sessions).
```

**Panne 5 — l'ACL parasite sur R1 :**

```
# Si après les réparations 1-4 certains flux restent bloqués de façon "globale"...
[R1]display current-configuration | include traffic-filter
# traffic-filter inbound acl 3000 sur GE0/0/2 !
[R1]display acl 3000
# rule deny → tout est bloqué en entrée de l'interface.
[R1]interface GigabitEthernet 0/0/2
[R1-GigabitEthernet0/0/2]undo traffic-filter inbound
# Re-test complet.
```

**Validation finale :** matrice de pings complète au tableau, tout au vert.

### Vérifications (checklist de fin de TP)

```
[SW1/SW2]display port vlan          # VLAN et trunks symétriques
[R1/R2]display ospf peer brief      # Full des deux côtés
[R1/R2]display ip routing-table protocol ospf
[R2]display nat session all          # sessions pendant un ping Internet
[R1]display current-configuration | include traffic-filter   # aucune ACL parasite
```

### Pièges classiques (côté stagiaire)

1. **Corriger plusieurs choses à la fois** sans re-tester entre chaque → on ne sait plus ce qui a réparé quoi. Discipline : une correction = un test.
2. **Sauter les couches basses** : chercher du côté d'OSPF alors qu'un lien est down. Toujours commencer par `display interface brief`.
3. **Le "ça marchait hier"** : en dépannage réel, la première question est "qu'est-ce qui a changé ?" (`display current-configuration`, historique des `save`).
4. **Confondre symptôme et cause** : "Internet est coupé" peut être du NAT (panne 4), du routage (panne 3) ou une ACL (panne 5) → la méthode par couches départage.
5. **Négliger la documentation** : chaque panne doit donner lieu à une fiche (symptôme → diagnostic → correction → validation). C'est noté.

### Barème indicatif (30 points — TP noté)

| Critère | Points |
|---|---|
| Méthode suivie et documentée (6 étapes appliquées) | 6 |
| Panne 1 (VLAN) trouvée et corrigée | 4 |
| Panne 2 (trunk asymétrique) trouvée et corrigée | 5 |
| Panne 3 (Router-ID dupliqué) trouvée et corrigée | 5 |
| Panne 4 (NAT/ACL) trouvée et corrigée | 5 |
| Panne 5 (ACL parasite) trouvée et corrigée | 5 |

### Durée estimée
**1 h 30 à 2 h** (dont 15 min de débrief collectif).

### Fiche animateur — points à insister
- Ce TP est **le plus important** du parcours : en production, on dépanne plus qu'on ne configure. La méthode compte plus que la vitesse.
- Imposer l'écrit : fiche par panne. Un dépanneur qui ne documente pas fait perdre à l'équipe.
- Débrief collectif en fin de séance : chaque groupe présente UNE panne (symptôme → raisonnement → correction). C'est là que l'apprentissage se consolide.
- Adapter la difficulté : pour un groupe débutant, donner le nombre de pannes (5) et leurs couches ; pour un groupe avancé, ne rien dire ("le réseau est en panne, débrouillez-vous").
- Lien terrain : cette méthode (couches 1→2→3→fonctions) est exactement celle à appliquer sur un ticket réel, que ce soit un S310, un AR720 ou un USG6000.
- Insister sur `display` avant `config` : **on ne configure jamais pour diagnostiquer, on observe d'abord**.

---

## 15. Quiz final (10 questions corrigées)

> À faire passer en fin de parcours (30 minutes, sans notes). Barème : 2 points par question, total 20.

**Q1. Sur un switch Huawei, un port en `port link-type trunk` avec `port trunk allow-pass vlan 10 20` : que se passe-t-il pour une trame du VLAN 30 qui arrive sur ce port ?**

<details>
<summary>Réponse</summary>

Elle est **rejetée (filtrée)** : le trunk ne laisse passer que les VLAN explicitement autorisés par `allow-pass`. Par défaut, seul le VLAN 1 passe. C'est le mécanisme du "VLAN qui ne passe pas" (TP2).
</details>

**Q2. Vous configurez une sous-interface `GigabitEthernet0/0/1.10` avec `dot1q termination vid 10` et une adresse IP, mais les PC du VLAN 10 ne pingent pas la passerelle. Quelle commande manque très probablement ?**

<details>
<summary>Réponse</summary>

`arp broadcast enable` : sans elle, le routeur VRP ne traite pas les requêtes ARP entrantes sur la sous-interface (TP4). Vérification : `display interface GigabitEthernet0/0/1.10`.
</details>

**Q3. En OSPF, deux routeurs ont la même area (0), des timers identiques, mais l'adjacence reste Down. Citez deux autres causes possibles.**

<details>
<summary>Réponse</summary>

- **Router-ID dupliqué** (ou 0.0.0.0) : deux routeurs avec le même Router-ID ne peuvent pas devenir voisins (TP12, panne 3).
- **Masques/MTU incohérents** sur le lien, ou **sous-réseaux différents** des deux côtés du lien, ou une ACL filtrant les Hello (multicast 224.0.0.5).
</details>

**Q4. Quelle est la différence entre `preference 60` et `preference 100` sur deux routes statiques vers la même destination ?**

<details>
<summary>Réponse</summary>

La route avec la **préférence la plus basse** est préférée : celle à 60 est active, celle à 100 est en backup (floating static). Si la route à 60 disparaît (interface down), celle à 100 prend automatiquement le relais (TP5).
</details>

**Q5. Dans un tunnel IPSec policy-based, à quoi sert l'ACL (ex. `acl 3000`) et pourquoi doit-elle être "miroir" sur les deux routeurs ?**

<details>
<summary>Réponse</summary>

L'ACL définit l'**interesting traffic** : seul le trafic qui la matche est chiffré dans le tunnel. Elle doit être miroir (source/destination inversées) car chaque routeur la lit dans son propre sens : R1 protège `10.0→20.0`, R2 doit protéger `20.0→10.0`. Sinon la phase 2 (proxy-ID) ne négocie pas (TP9).
</details>

**Q6. Sur un USG, vous créez une règle `trust → untrust` en `action permit`. Faut-il aussi créer la règle retour `untrust → trust` pour que la navigation web fonctionne ? Pourquoi ?**

<details>
<summary>Réponse</summary>

**Non** : le firewall est **stateful** — il suit les sessions et autorise automatiquement le trafic retour d'une session initiée depuis trust. Une règle untrust→trust ne serait nécessaire que pour un flux **initié** depuis l'extérieur (ex. serveur exposé, TP11).
</details>

**Q7. Un AP en mode Fit reste à l'état `idle` dans `display ap all`. Citez deux vérifications à faire, dans l'ordre.**

<details>
<summary>Réponse</summary>

1. **Connectivité IP** entre l'AP et l'AC (ping dans le VLAN de gestion) : sans IP, pas de tunnel CAPWAP (TP10).
2. **Déclaration de l'AP** sur l'AC (bonne MAC / bon SN, AP rattaché à un groupe avec un VAP diffusé). Si l'AP est en `configFailed`, vérifier les noms de profils référencés.
</details>

**Q8. Expliquez la séquence DHCP DORA et dites à quelle étape le relais DHCP intervient.**

<details>
<summary>Réponse</summary>

**D**iscover (broadcast client) → **O**ffer (proposition du serveur) → **R**equest (le client choisit) → **A**CK (confirmation). Le **relais** intervient dès le Discover : le routeur du site distant intercepte le broadcast, le convertit en unicast vers le serveur central en ajoutant son adresse (`giaddr`), ce qui permet au serveur de choisir le bon pool (TP4).
</details>

**Q9. Vous activez RSTP sur trois switches en triangle sans fixer les priorités. Qui devient root bridge, et pourquoi est-ce un problème en production ?**

<details>
<summary>Réponse</summary>

Le switch avec le **Bridge ID le plus petit** (priorité 32768 partout par défaut, donc la **plus petite adresse MAC**). Problème : le root peut être un switch d'accès en bout de chaîne, mal placé et peu puissant → chemins sous-optimaux et convergence fragile. En production, on fixe toujours `stp root primary/secondary` sur les switches de cœur (TP3).
</details>

**Q10. Un utilisateur se plaint : "J'ai une adresse IP mais pas d'Internet". Citez, dans l'ordre de la méthode de diagnostic, les 4 vérifications à faire sur son poste puis sur le réseau.**

<details>
<summary>Réponse</summary>

1. `ipconfig /all` : adresse, **masque**, **passerelle**, **DNS** — une IP sans passerelle/DNS = pas d'Internet (TP4).
2. Ping de la **passerelle** (couche 3 locale OK ?), puis ping d'une IP publique (routage/NAT OK ?), puis ping d'un **nom de domaine** (DNS OK ?).
3. Côté réseau : `display ip routing-table` (défaut présente ?), `display nat session all` (translation active ?), `display firewall session table` (politique USG ?).
4. Remonter par couches (méthode du TP12) sans sauter d'étape.
</details>

---

## 16. Glossaire

| Terme | Définition |
|---|---|
| **AC** (Access Controller) | Contrôleur WLAN centralisant la gestion des AP en mode Fit. |
| **ACL** (Access Control List) | Liste de règles de filtrage (permit/deny) ; sert aussi à sélectionner du trafic (NAT, IPSec, QoS). |
| **AP Fit / Fat** | Fit : AP "léger" piloté par un AC (tunnel CAPWAP). Fat : AP autonome. |
| **BDR** (Backup Designated Router) | Routeur OSPF de secours du DR sur un segment multi-accès. |
| **BPDU** | Trames d'échange STP/RSTP entre switches (élection root, états de ports). |
| **Broadcast domain** | Ensemble d'équipements recevant les diffusions de niveau 2 ; un VLAN = un broadcast domain. |
| **CAPWAP** | Protocole de tunnel entre AP Fit et AC (contrôle + données selon le mode). |
| **CHAP** | Protocole d'authentification PPP par challenge/response (ne transmet pas le mot de passe en clair). |
| **CIST** | Instance STP unique couvrant tous les VLAN (mode de base sur eNSP). |
| **DHCP DORA** | Discover, Offer, Request, Ack : les 4 étapes d'attribution d'adresse. |
| **DR** (Designated Router) | Routeur OSPF élu sur un segment multi-accès pour centraliser les échanges. |
| **Easy IP** | NAPT utilisant l'adresse IP de l'interface de sortie (cas standard PME). |
| **Edge port** | Port STP/RSTP vers un équipement terminal, passant immédiatement en forwarding (équivalent du portfast). |
| **ESP** | Protocole 50 : charge utile chiffrée d'IPSec (phase 2). |
| **Floating static** | Route statique de backup avec une préférence élevée, inactive tant que la route principale existe. |
| **Forward-mode** | Mode de transfert du trafic Wi-Fi : `direct-forward` (bridgé localement par l'AP) ou `tunnel-forward` (remonté à l'AC). |
| **IKE** | Protocole de négociation de la phase 1 IPSec (UDP 500, UDP 4500 avec NAT-T). |
| **Interesting traffic** | Trafic sélectionné par ACL pour être protégé par le tunnel IPSec. |
| **LSA / LSDB** | Annonces d'état de lien (Link State Advertisement) et base de données OSPF construite à partir d'elles. |
| **NAPT** | NAT avec translation de ports : plusieurs IP privées partagent une seule IP publique. |
| **NAT server** | Port mapping statique : expose un service interne vers l'extérieur (IP publique:port → IP privée:port). |
| **PVID** | VLAN natif d'un port : VLAN attribué aux trames non taggées entrantes. |
| **Router-ID** | Identifiant 32 bits d'un routeur OSPF (format d'adresse IP) ; doit être unique. |
| **RSTP** | Rapid STP (802.1w) : convergence rapide (~quelques secondes) via proposal/agreement. |
| **Silent-interface** | Interface OSPF qui annonce son réseau sans y envoyer de Hello (vers les LAN). |
| **SSID** | Nom du réseau Wi-Fi diffusé par les AP. |
| **STP** | Spanning Tree Protocol (802.1D) : élimine les boucles de niveau 2 en bloquant des ports redondants. |
| **Trunk 802.1Q** | Lien transportant plusieurs VLAN via taggage des trames. |
| **VAP** | Profil virtuel liant SSID + sécurité + VLAN de service, diffusé par les radios des AP. |
| **VRP** | Versatile Routing Platform : le système d'exploitation des équipements Huawei (équivalent d'IOS chez Cisco). |
| **Wildcard (masque inversé)** | Masque OSPF/ACL inversé (ex. /24 → 0.0.0.255). |
| **WPA2-PSK** | Authentification Wi-Fi par clé pré-partagée (Pre-Shared Key). |
| **Zone (USG)** | Domaine de sécurité du firewall (trust, untrust, dmz, local) ; les politiques s'expriment entre zones. |

---

## 17. Pour aller plus loin

### 17.1. Prolongements directs des TP (niveau 2 du parcours)

1. **OSPF multi-zones** : ajouter une area 1 derrière R2, observer les LSA inter-zones et le rôle de l'ABR.
2. **VRRP** : deux routeurs en passerelle redondante pour un même LAN (IP virtuelle, master/backup, preempt).
3. **Eth-Trunk (agrégation de liens)** : agréger 2 liens entre SW1 et SW2 (`interface Eth-Trunk`), mesurer le débit et tester la résilience.
4. **MSTP** : deux instances (VLAN 10 → instance 1, VLAN 20 → instance 2) avec des roots différents → équilibrage de charge des VLAN sur les liens.
5. **DHCP snooping + DAI** : sécuriser le VLAN 10 contre les serveurs DHCP pirates.
6. **IPSec avec NAT-T** : placer un NAT entre R1 et R2 (topologie du TP7+TP9 combinés) et activer la traversée de NAT.
7. **GRE over IPSec** : encapsuler du multicast (ou de l'OSPF !) dans du GRE lui-même protégé par IPSec — le classique des interconnexions dynamiques.
8. **PPPoE + IPv6** : attribuer un préfixe IPv6 via IPCP6/DHCPv6-PD sur la session du TP8.
9. **WLAN avancé** : deuxième SSID invité (portail captif simulé), limitation de débit par SSID, band steering.
10. **USG avancé** : politiques avec plages horaires, NAT bidirectionnel, VPN IPSec terminé sur l'USG lui-même (au lieu du routeur).

### 17.2. Du lab au terrain : transposer sur vos équipements

| Concept du lab (eNSP) | Équivalent terrain | Différences à connaître |
|---|---|---|
| AR2220 (lab) | AR720 | Interfaces, débits, modules 4G/USB ; CLI VRP quasi identique |
| S3700 (L2) / S5700 (L3) | S310 | Le S310 fait du L3 léger ; vérifier la fiche exacte (routage inter-VLAN, DHCP) |
| USG5500 (`policy interzone`) | USG6000 (`security-policy`) | Même logique de zones ; syntaxe des règles nommées sur USG6000 |
| AC6605 + AP6010DN | AC + AP361/AP761 | Wi-Fi 6 sur le terrain (OFDMA, MU-MIMO) ; logique Fit AP et profils identiques |
| STA eNSP | Smartphones/PC portables | Comportements roaming réels, à tester sur site |

### 17.3. Ressources

- **Vos guides d'équipe** (déjà en votre possession) : `huawei_ap361_guide.md`, `huawei_ap761_guide.md`, `huawei_s310_guide.md`, `huawei_ar720_guide.md`, `huawei_usg6000_guide.md`, `huawei_ekit_guide.md`, `huawei_esight_guide.md` — chacun détaille le matériel réel correspondant aux concepts des TP.
- **Documentation officielle Huawei** (support.huawei.com) : *Configuration Guides* VRP des AR/S/USG pour la syntaxe exacte par version.
- **Fichiers du parcours** : conserver un dossier par TP avec `sujet.md`, `topologie.topo` (vierge) et `correction.md` — ce guide en est la matière première.

### 17.4. Organisation conseillée de la formation d'équipe

| Semaine | Séance | Durée |
|---|---|---|
| S1 | Installation + prise en main + TP1 | 2 h 30 |
| S2 | TP2 + TP3 | 2 h 15 |
| S3 | TP4 | 1 h 30 |
| S4 | TP5 + TP6 | 2 h 30 |
| S5 | TP7 + TP8 | 2 h 45 |
| S6 | TP9 + TP10 | 3 h |
| S7 | TP11 + TP12 (évaluation) | 3 h |
| S8 | Quiz final + débrief + plan de progression individuel | 1 h |

> **Total : ~18 h** de formation, découpables en sessions d'1 h 30 à 3 h. Le TP12 noté + le quiz final donnent une évaluation à 50 points (30 + 20) convertible en appréciation par équipier.

---

## Annexe A — Aide-mémoire des commandes VRP

### Vues et navigation

```
<Huawei>system-view              # passer en vue système
[Huawei]sysname R1               # nommer l'équipement
Ctrl+Z                           # retour en vue utilisateur depuis n'importe où
quit                             # remonter d'un niveau
?                                # aide contextuelle
display this                     # config de la vue courante
```

### Informations et vérifications

```
display version                  # version VRP, modèle, uptime
display current-configuration    # configuration complète
display saved-configuration      # configuration sauvegardée
display interface brief          # état de toutes les interfaces (up/down)
display ip interface brief       # adresses IP des interfaces
display ip routing-table         # table de routage
display mac-address              # table MAC (par VLAN sur switch)
display vlan [vlan-id]           # VLAN et ports membres
display port vlan                # synthèse ports : type, PVID, VLAN autorisés
display arp                      # table ARP
save                             # sauvegarder la configuration
reset saved-configuration        # effacer la config sauvegardée
reboot                           # redémarrer (après reset saved-configuration = reset usine)
```

### Switching

```
vlan batch 10 20 30              # créer plusieurs VLAN
port link-type access | trunk | hybrid
port default vlan 10             # VLAN d'un port access (PVID)
port trunk allow-pass vlan 10 20 # VLAN autorisés sur un trunk
port-group group-member Ethernet 0/0/1 to Ethernet 0/0/4  # config groupée
stp mode rstp | stp enable       # activer RSTP
stp root primary | secondary     # forcer l'élection
stp priority 4096                # priorité manuelle (multiple de 4096)
stp edged-port enable            # port vers terminal (jamais inter-switch)
display stp brief                # rôles et états des ports
```

### DHCP

```
dhcp enable                      # activer le service DHCP (global)
ip pool NOM                      # créer un pool
 network 192.168.10.0 mask 24   # réseau distribué
 gateway-list 192.168.10.254    # passerelle
 dns-list 8.8.8.8               # DNS
 excluded-ip-address 192.168.10.1 192.168.10.10  # exclusions
 lease day 1 hour 0              # durée du bail
dhcp select global               # sur l'interface : utiliser les pools globaux
dhcp relay server-ip 192.168.100.1  # relais vers un serveur distant
display ip pool [name NOM]       # pools et baux
```

### Routage

```
ip route-static 192.168.20.0 24 10.0.12.2 [preference 100]  # route statique
ip route-static 0.0.0.0 0.0.0.0 10.0.12.2                    # route par défaut
undo portswitch                  # passer un port GE en mode routé (L3)
ospf 1 router-id 1.1.1.1         # process OSPF + Router-ID
 area 0
  network 10.0.12.0 0.0.0.3      # déclaration avec wildcard
ospf dr-priority 100             # priorité DR (sur l'interface)
ospf cost 100                    # coût manuel (sur l'interface)
default-route-advertise always   # injecter la défaut dans OSPF
display ospf peer brief          # voisins OSPF
display ospf lsdb                # base des LSAs
```

### NAT

```
acl 2000
 rule permit source 192.168.10.0 0.0.0.255   # qui a droit au NAT
interface GigabitEthernet 0/0/2
 nat outbound 2000                            # Easy IP (NAPT) en sortie
 nat server protocol tcp global 198.51.100.1 8080 inside 192.168.10.100 80  # port mapping
display nat session all          # sessions actives
display nat server               # mappings statiques
```

### PPPoE

```
# Serveur :
interface Virtual-Template 1
 ppp authentication-mode chap
 ip address 198.51.100.2 30
 remote address pool POOL-PPPOE
interface GigabitEthernet 0/0/2
 pppoe-server bind virtual-template 1
# Client :
dialer-rule
 dialer-rule 1 ip permit
interface Dialer 1
 dialer user client-lab
 dialer-group 1
 dialer bundle 1
 ppp chap user client-lab
 ppp chap password cipher huawei123
 ip address ppp-negotiate
interface GigabitEthernet 0/0/2
 pppoe-client dial-bundle-number 1
display pppoe-client session summary
```

### IPSec

```
ike proposal 10
 encryption-algorithm aes-256
 authentication-algorithm sha2-256
 dh group14
ike peer R2
 pre-shared-key simple Cle-IPSec-Lab-2026
 ike-proposal 10
 remote-address 100.64.1.2
ipsec proposal PROP-A
 esp authentication-algorithm sha2-256
 esp encryption-algorithm aes-256
acl 3000
 rule permit ip source 192.168.10.0 0.0.0.255 destination 192.168.20.0 0.0.0.255
ipsec policy POLICY-A 10 isakmp
 security acl 3000
 ike-peer R2
 proposal PROP-A
interface GigabitEthernet 0/0/2
 ipsec policy POLICY-A
display ike sa
display ipsec sa
```

### WLAN (AC)

```
wlan                                     # entrer en vue WLAN
ap-id 1 ap-mac 00e0-fc12-3456            # déclarer un AP
 ap-name AP1
security-profile name SEC-LABO
 security wpa2 psk pass-phrase Cle123! aes
ssid-profile name SSID-LABO
 ssid LABO-ENTREPRISE
vap-profile name VAP-LABO
 service-vlan vlan-id 10
 ssid-profile SSID-LABO
 security-profile SEC-LABO
 forward-mode direct-forward
regulatory-domain-profile name REG-FR
 country-code FR
ap-group name GROUPE-LABO
 regulatory-domain-profile REG-FR
 vap-profile VAP-LABO wlan 1 radio all
ap-id 1
 ap-group GROUPE-LABO
display ap all
display station all
```

### USG (zones et politiques)

```
firewall zone trust
 add interface GigabitEthernet 0/0/1
policy interzone trust untrust outbound
 policy 1
  action permit
  policy source 192.168.10.0 mask 24
display zone
display policy interzone trust untrust outbound
display firewall session table
```

---

## Annexe B — Modèle de fiche de lab vierge

> À dupliquer pour chaque nouveau TP créé par l'équipe.

```markdown
# TPXX — Titre

## Objectif
(1-2 phrases : ce que le stagiaire saura faire à la fin)

## Prérequis
(TP précédents, notions)

## Topologie
(Schéma textuel : équipements, interconnexions, adressage)

## Énoncé
(Étapes numérotées, sans les commandes)

## Correction pas à pas
(Commandes VRP complètes par équipement)

## Vérifications
(display ... attendus)

## Pièges classiques
(3-6 pièges avec diagnostic)

## Barème indicatif (20 points)
(Tableau critère / points)

## Durée estimée
## Fiche animateur
(Points à insister, questions pièges, lien terrain)
```

---

*Fin du guide — Labs eNSP pour former son équipe. Bon courage à l'équipe, et que les pings soient avec vous.*

---

## Annexe C — Dépannage de l'installation eNSP (pense-bête formateur)

| Symptôme | Cause probable | Solution |
|---|---|---|
| Équipements affichés `???` après démarrage | Images VirtualBox non enregistrées / antivirus bloquant | Réinstaller eNSP **en administrateur**, désactiver l'antivirus pendant l'install |
| Message "VirtualBox version too low" | Version de VirtualBox incompatible | Installer **VirtualBox 5.2.x** (ex. 5.2.44), chemin sans accents |
| Les nœuds démarrent puis s'arrêtent aussitôt | Hyper-V / WSL2 actif | `bcdedit /set hypervisorlaunchtype off` + désactiver Hyper-V + reboot |
| Erreur VT-x / AMD-V | Virtualisation désactivée dans le BIOS | Activer Intel VT-x / AMD-V (SVM Mode) dans le BIOS/UEFI |
| La capture Wireshark ne démarre pas | WinPcap absent ou mal installé | Installer **WinPcap 4.1.3** (pas seulement Npcap) |
| eNSP ne détecte pas VirtualBox | Ordre d'installation incorrect | Toujours installer dans l'ordre : VirtualBox → WinPcap → eNSP → reboot |
| Console CLI noire / pas de prompt | Équipement pas encore booté | Attendre 1 à 2 min (premier boot long), vérifier l'icône verte |
| Lenteurs avec 6+ équipements | RAM insuffisante | 16 Go conseillés ; ne démarrer que les équipements du TP en cours |
| Le .topo s'ouvre mais les configs ont disparu | `save` jamais exécuté sur les équipements | Réflexe : `save` en fin de chaque TP avant d'enregistrer le .topo |
| Conflit avec Docker Desktop | Docker utilise WSL2/Hyper-V | Choisir : Docker **ou** eNSP sur le poste (ou 2 postes / dual-boot) |

> **Checklist de validation d'un poste stagiaire** (5 minutes) : eNSP lancé en admin → palette complète visible → lab "hello world" (2 routeurs) démarré → ping OK → capture Wireshark OK sur le lien → `hello_world.topo` enregistré et rouvert. Si ces 5 points passent, le poste est prêt pour tout le parcours.
