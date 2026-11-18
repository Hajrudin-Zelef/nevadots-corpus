# Huawei eSight — Guide ultra-complet d'exploitation terrain

> **NMS (Network Management System) Huawei pour l'entreprise**
> Rédigé le 27 septembre 2026 — orienté terrain, pour chef de service systèmes & énergies.
>
> ⚠️ **Avertissement de méthode** : ce guide s'appuie sur la documentation constructeur
> publique (datasheet eSight, brochure eSight 23.1 de mars 2024, retours d'exploitants).
> Les comportements précis (numéros de ports, chemins de menus, comptes par défaut)
> **dépendent de la version installée**. Chaque fois qu'une information est
> version-dépendante ou non vérifiable publiquement, la mention
> **« à vérifier sur la documentation officielle »** est indiquée explicitement.
> Ne jamais appliquer une procédure d'administration sans l'avoir confrontée
> à la documentation de VOTRE version.

---

## Table des matières

- [1. Présentation d'eSight](#1-présentation-desight)
- [2. Architecture technique](#2-architecture-technique)
- [3. Dimensionnement](#3-dimensionnement)
- [4. Installation](#4-installation)
- [5. Découverte des équipements](#5-découverte-des-équipements)
- [6. Topologie](#6-topologie)
- [7. Gestion des alarmes](#7-gestion-des-alarmes)
- [8. Supervision des performances](#8-supervision-des-performances)
- [9. Gestion des configurations](#9-gestion-des-configurations)
- [10. Gestion WLAN](#10-gestion-wlan)
- [11. Comptes, rôles et audit](#11-comptes-rôles-et-audit)
- [12. Sauvegarde d'eSight et PRA](#12-sauvegarde-desight-et-pra)
- [13. Intégration northbound et écosystème](#13-intégration-northbound-et-écosystème)
- [14. Dépannage : 17 cas terrain](#14-dépannage--17-cas-terrain)
- [15. Bonnes pratiques d'exploitation](#15-bonnes-pratiques-dexploitation)
- [16. Maintenance](#16-maintenance)
- [17. Pour aller plus loin](#17-pour-aller-plus-loin)
- [18. Pense-bête de poche](#18-pense-bête-de-poche)
- [19. Les 20 pièges classiques](#19-les-20-pièges-classiques)
- [20. Glossaire](#20-glossaire)
- [21. Quiz](#21-quiz)
- [Annexe A — Recette de mise en service](#annexe-a--recette-de-mise-en-service)
- [Annexe B — Fiches réflexes par type d'équipement](#annexe-b--fiches-réflexes-par-type-déquipement)
- [Annexe C — Automatisation autour d'eSight](#annexe-c--automatisation-autour-desight)
- [Annexe D — Cas pratiques commentés](#annexe-d--cas-pratiques-commentés)

---

# 1. PRÉSENTATION D'ESIGHT

## 1. Ce qu'est eSight, en une phrase

**Huawei eSight est la plateforme unifiée d'exploitation et de maintenance (O&M)
du système d'information d'entreprise de Huawei** : un NMS qui supervise de façon
centralisée le réseau (routeurs, commutateurs, firewalls, WLAN), les serveurs,
le stockage, la vidéosurveillance, les applications et les liaisons micro-ondes —
qu'ils soient Huawei ou, dans une certaine mesure, d'autres constructeurs.

Le positionnement officiel du constructeur : *« eSight ICT O&M system is a Huawei
new-generation solution for enterprise users to centrally manage the basic network,
unified communications (UC), telepresence meetings, video surveillance devices,
and data centers »* (brochure eSight 23.1, mars 2024).

Concrètement, sur le terrain, eSight fait quatre métiers :

1. **Fault management** — collecte et traite les alarmes (SNMP traps, syslog).
2. **Performance management** — collecte les indicateurs (CPU, mémoire, interfaces)
   et génère tableaux de bord et rapports.
3. **Topology management** — cartographie automatique du réseau.
4. **Configuration management** — sauvegarde, comparaison et restauration
   des configurations des équipements.

## 2. À qui s'adresse eSight

- **PME/ETI et grands comptes** disposant d'un parc majoritairement Huawei
  (ou mixte avec une base Huawei significative).
- **Équipes d'exploitation** (NOC, helpdesk niveau 2/3) qui ont besoin d'un
  point d'entrée unique pour voir l'état du SI.
- **Chefs de service systèmes & réseaux** qui veulent des rapports
  d'activité, des KPI et de la traçabilité pour piloter l'équipe.
- **Intégrateurs** qui déploient du Huawei chez leurs clients et ont besoin
  d'un outil de supervision « constructeur » qui parle parfaitement aux
  équipements Huawei (MIB propriétaires comprises).

Ce n'est PAS un outil pour : superviser uniquement du multi-constructeur
générique (des outils comme Zabbix ou PRTG sont plus agnostiques),
faire du SDN/automatisation avancée (c'est le terrain d'iMaster NCE),
ou gérer du datacenter cloud-native.

## 3. Périmètre fonctionnel : les modules (composants)

eSight est **modulaire** : on achète la plateforme de base puis on ajoute
des composants selon le besoin. D'après la brochure officielle eSight 23.1,
les composants sont :

| Composant | Ce qu'il gère |
|---|---|
| eSight basic management | Plateforme O&M : ressources, topologies, alarmes, performances, rapports, portails |
| eSight network device management | Découverte et maintenance des équipements réseau, interfaces, liens, configurations |
| eSight WLAN management | Ressources du réseau Wi-Fi de campus (contrôleurs AC et points d'accès AP), diagnostic des pannes sans fil, topologie unifiée filaire/sans fil |
| eSight server device management | Ressources serveurs : CPU, mémoire, disques, ports réseau, vues simulées, systèmes d'exploitation |
| eSight storage device management | Ressources, performances et capacité des baies OceanStor (y compris fonctions de protection anti-ransomware) |
| eSight video surveillance management | Ressources de vidéosurveillance (IVS, caméras), topologies de service, performances |
| eSight microwave device management | O&M et montées de version des équipements micro-ondes (RTN) |
| eSight Application Management | Supervision centralisée des OS, bases de données, serveurs web et applicatifs, URLs |
| eSight PON device management | Réseaux GPON : OLT et ONU — état, alarmes, performances, configurations, localisation rapide des pannes |
| eSight network SLA management | Diagnostic périodique automatique des chemins réseau |

💡 **Lecture terrain** : dans la plupart des déploiements PME/ETI, on ne retient
que 2 à 3 modules : *basic management* + *network device management*,
éventuellement + *WLAN management* si le Wi-Fi est critique. Chaque module
ajouté = licence + charge sur le serveur + surface d'administration.
Ne pas acheter « au cas où ».

## 4. Les éditions : Express, Compact, Standard, Professional

Le datasheet constructeur distingue quatre éditions (les trois principales
documentées en détail) :

| Fonction | Compact | Standard | Professional |
|---|---|---|---|
| Alarmes, performance, topologie, fichiers de config | ✅ | ✅ | ✅ |
| Équipements (NE), liens, logs, ressources physiques | ✅ | ✅ | ✅ |
| Étiquettes électroniques, topologie IP | ✅ | ✅ | ✅ |
| Smart Configuration Tool | ✅ | ✅ | ✅ |
| Gestion de la sécurité, accès terminaux | ✅ | ✅ | ✅ |
| System Monitor Tool, Database Backup & Restore Tool, Fault Information Collection Tool | ✅ | ✅ | ✅ |
| Gestion WLAN | ❌ | ✅ | ✅ |
| NTA (analyse de trafic réseau / NetStream) | ❌ | ✅ | ✅ |
| Policy Center, SLA, QoS | ❌ | ✅ | ✅ |
| MPLS VPN, tunnels MPLS, IPSec VPN | ❌ | ✅ | ✅ |
| Gestion des rapports | ❌ | ✅ | ✅ |
| Interface northbound SNMP | ❌ | ✅ | ✅ |
| NMS hiérarchique (supervision de superviseurs) | ❌ | ❌ | ✅ |
| DC nCenter | ❌ | ❌ | ✅ |
| Double système / cluster (Linux uniquement) | ❌ | ❌ | ✅ |

*(Source : Huawei eSight Full Product Datasheet — à vérifier sur la
documentation officielle pour la version exacte que vous déployez,
les périmètres ont pu évoluer.)*

**Règle de choix terrain :**
- Moins de ~200 équipements, besoin simple (alarmes + topo + perfs + backup
  de configs) → **Standard** suffit dans 90 % des cas.
- Besoin de WLAN unifié ou d'analyse de trafic (NTA) → **Standard** minimum.
- Multi-sites avec NMS locaux remontant vers un NMS central, ou besoin de
  haute disponibilité en cluster → **Professional**.
- Maquette, labo, tout petit site → **Compact**.

## 5. eSight face à iMaster NCE : la question de la fin de vie, honnêtement

C'est LA question à se poser avant d'investir. Voici les faits vérifiés :

- **eSight est toujours commercialisé** : la brochure « Huawei eSight 23.1 »
  est datée du **27 mars 2024** (document « Huawei confidential » diffusé
  par les partenaires). Le produit reçoit donc encore des versions.
- **iMaster NCE est la plateforme stratégique de Huawei** pour l'avenir :
  système autonome de gestion et de contrôle réseau pour l'entreprise,
  qui intègre gestion traditionnelle + fonctions de contrôle SDN + analyse
  de données basée sur les intentions de service, avec moteur IA
  (recommandation de topologies, provisioning automatique, telemetry).
  Les analystes (Appledore Research, 2023) décrivent NCE comme la gamme
  « destinée à remplacer les EMS historiques » de Huawei.
- **Aucune date officielle de fin de vie (EoL/EoS) d'eSight n'a été trouvée
  dans les sources publiques consultées** → **à vérifier sur la
  documentation officielle** auprès de votre partenaire Huawei.

**Positionnement honnête pour un chef de service :**

| Critère | eSight | iMaster NCE |
|---|---|---|
| Nature | NMS classique (FCAPS) | Plateforme autonome (management + contrôle + analyse) |
| Idéal pour | Superviser un parc existant, multi-constructeur partiel | Nouveaux déploiements campus/SD-WAN, automatisation |
| SDN / provisioning auto | Non (ou marginal) | Oui (cœur du produit) |
| Courbe d'apprentissage | Modérée | Plus forte |
| Investissement | Licence perpétuelle + SnS historiquement | Modèle plus orienté souscription |

**Trajectoire de migration à anticiper :** si vous déployez eSight aujourd'hui
sur un parc existant, prévoyez dans votre feuille de route à 3-5 ans une
évaluation de NCE (notamment NCE-Campus pour le Wi-Fi/filaire). Concrètement :
gardez vos processus d'exploitation (runbooks, nommage, gestion des alarmes)
**indépendants de l'outil** pour que la migration ne soit qu'un changement
de console, pas une réinvention. Les exports réguliers d'inventaire et de
configurations depuis eSight sont votre assurance-vie pour toute migration.

## 6. Forces et limites (retours d'exploitants)

Synthèse de retours d'utilisateurs (plateformes d'avis, 2022-2026) :

**Forces :**
- Stabilité saluée (notes 9/10) : « pas de crashs ni de glitches majeurs ».
- Parfaitement intégré aux équipements Huawei (MIB propriétaires, NTA).
- Installation « one-click » simple sur appliance Huawei fournie.
- Scalabilité correcte dans l'écosystème Huawei.

**Limites :**
- Installation sur VM jugée « très complexe » quand ça coince ;
  prévoir un admin qui connaît le produit.
- Support multi-constructeur perfectible (très bon sur Huawei, moyen
  sur le reste — un avis le note 2-3/10 hors Huawei).
- Cluster/redondance : procédure jugée complexe.
- Documentation parfois ardue à naviguer pour les cas d'erreur.

**Traduction pour Zelef** : eSight est un excellent choix si votre parc est
majoritairement Huawei ; prévoyez de la compétence interne ou un partenaire
pour l'installation et la montée en charge, et ne lui demandez pas d'être
un Zabbix générique.

## 7. Modèle de licences

D'après les grilles partenaires publiques (ex. S-Part Numbers) :

- Licence **par équipement géré** (1 device), par instance applicative,
  par caméra (vidéosurveillance), par AP/contrôleur (WLAN).
- Chaque licence est couplée à **1 an de souscription et support (SnS)**
  minimum obligatoire (ex. : *« eSight Server Management License,
  1 Year Subscription and Support, 1 Device »*).
- Les composants optionnels (stockage, applicatif, vidéosurveillance,
  micro-ondes, PON) se licencient séparément.

**Points de vigilance terrain :**
- Comptez vos équipements **avant** d'acheter : faites un inventaire
  (script SNMP walk ou export existant) et ajoutez 20 % de marge pour
  la croissance.
- Vérifiez ce qui compte comme « 1 device » : un châssis avec cartes ?
  Un stack ? Un contrôleur + ses AP ? → **à vérifier sur la documentation
  officielle** de votre version, les règles de comptage changent selon
  les composants.
- Renouvelez le SnS : sans support, pas de patchs de sécurité — et un NMS
  non patché qui a les accès SNMP en écriture sur tout le réseau, c'est
  un risque majeur.
- Gardez une **copie des fichiers/actes de licence** dans votre PRA
  (voir section 12).

---

# 2. ARCHITECTURE TECHNIQUE

## 8. Vue d'ensemble de l'architecture

eSight repose sur une architecture **centralisée** avec éventuellement
une **hiérarchie** de NMS (édition Professional) :

```
                    ┌─────────────────────────────────┐
                    │        eSight Server            │
                    │  ┌──────────┐  ┌──────────────┐ │
                    │  │ Services │  │  Base de     │ │
                    │  │ eSight   │◄─┤  données     │ │
                    │  │ (Java)   │  │ (MySQL/SQL   │ │
                    │  └────┬─────┘  │  Server/Oracle)│
                    │       │        └──────────────┘ │
                    └───────┼─────────────────────────┘
                            │ SNMP / syslog / NetStream
                            │ SSH/Telnet / ICMP / HTTP(S)
                ┌───────────┼───────────────┐
                ▼           ▼               ▼
           Routeurs    Commutateurs      Firewalls
           Huawei      (tiers: HP,       WLAN (AC/AP)
           (+ tiers)   Cisco...)         Serveurs, stockage...
```

Le serveur eSight **interroge** (polling SNMP/ICMP) et **écoute**
(traps SNMP, syslog) les équipements. Il n'y a pas d'agent à installer
sur les équipements réseau : tout passe par les protocoles standard.
C'est une architecture **sans agent** côté réseau — un point fort pour
le déploiement.

## 9. Architecture Browser/Server : aucun client à installer

Point structurant : eSight utilise une architecture **B/S (Browser/Server)**.

- L'administration se fait **à 100 % via navigateur web** (interface
  Web 2.0 d'après le constructeur). Aucun client lourd à déployer
  sur les postes des exploitants.
- Conséquence directe : pour donner l'accès à un technicien d'astreinte,
  il suffit d'une URL + un compte (voir section 11). En astreinte de nuit,
  on consulte les alarmes depuis n'importe quel PC — voire une tablette.
- En contrepartie : le serveur eSight doit être **joignable en HTTPS
  depuis tous les postes d'exploitation**, et sa disponibilité conditionne
  tout l'outillage (d'où l'importance du PRA, section 12).
- Le constructeur annonce qu'eSight peut même tourner sur un **PC portable**
  (édition compacte) : utile pour une maquette ou un audit ponctuel sur site.

## 10. La base de données : le cœur (et le point faible) du système

eSight s'appuie sur une base de données relationnelle qui stocke :
l'inventaire des équipements, la topologie, les alarmes (courantes et
historiques), les données de performance, les configurations sauvegardées,
les comptes utilisateurs et les journaux d'audit.

Selon le datasheet (versions historiques — **à vérifier sur la
documentation officielle** pour les versions récentes) :

| OS du serveur eSight | Base supportée |
|---|---|
| Windows Server | MySQL 5.5 ou Microsoft SQL Server 2008 R2 |
| SUSE Linux Enterprise Server | Oracle Database 11g R2 |

**Enjeux terrain :**
- La base est **le premier goulot d'étranglement** : quand eSight rame,
  c'est elle dans 80 % des cas (voir cas n°13 en section 14).
- Prévoyez le **dimensionnement disque** en fonction de la **durée de
  rétention** des données de performance et d'alarmes (voir section 3).
- La base doit être **sauvegardée séparément** du système (outil
  *Database Backup & Restore* fourni — voir section 12).
- Ne jamais bidouiller la base à la main (UPDATE/DELETE en SQL direct) :
  vous casseriez la cohérence applicative et le support Huawei.

## 11. Protocoles de supervision : le langage entre eSight et les équipements

| Protocole | Sens | Usage dans eSight | Port usuel |
|---|---|---|---|
| SNMP v1/v2c/v3 | eSight ↔ équipement | Découverte, polling d'état et de compteurs, traps | UDP 161 (poll), UDP 162 (traps) |
| Syslog | Équipement → eSight | Remontée d'événements/logs, source d'alarmes | UDP 514 |
| NetStream (NTA) | Équipement → eSight | Analyse de trafic (qui parle à qui, top talkers) | UDP configuré (souvent 5000+) |
| ICMP | eSight → équipement | Test de joignabilité (ping) pendant la découverte | — |
| SSH / Telnet | eSight → équipement | Sauvegarde/restauration de configurations, Smart Config Tool | TCP 22 / 23 |
| HTTP/HTTPS | Navigateur → eSight | Console d'administration web | TCP 80/443 (ports exacts **à vérifier sur la documentation officielle**) |
| SMTP | eSight → relais mail | Notifications d'alarmes par e-mail | TCP 25/587 |
| SMPP/API SMS | eSight → passerelle | Notifications d'alarmes par SMS | Selon opérateur |

**Choix structurant : SNMP v2c vs v3.**
- **v2c** : simple (community string), mais **non chiffré et authentification
  faible**. Acceptable uniquement sur un VLAN de management isolé.
- **v3** : authentification + chiffrement (authPriv). **C'est le standard
  à viser** pour tout nouveau déploiement. eSight gère les deux.
- Règle d'or : la community SNMP ou les credentials v3 utilisés par eSight
  doivent être **dédiés à la supervision** (un compte par outil), jamais
  partagés avec d'autres usages.

## 12. Haute disponibilité : le cluster à deux nœuds

Le constructeur annonce : *« eSight supports two-node clusters in hot
standby mode »* (cluster à 2 nœuds en **secours à chaud**).

- Principe : deux serveurs eSight, un actif et un en standby qui prend
  le relais en cas de panne (bascule).
- D'après le datasheet, la fonction **Double Système n'est supportée que
  sous Linux** et relève de l'édition **Professional**.
- Protection DR (Disaster Recovery) également mentionnée pour la
  continuité de service.

**Lecture terrain :**
- La HA ne se bricole pas après coup : si vous visez 200+ équipements
  ou une supervision critique (site industriel, H24), **prévoyez le
  cluster dès l'installation**.
- Les retours d'exploitants qualifient la mise en cluster de « complexe » :
  faites-vous accompagner par le partenaire/intégrateur pour cette étape.
- Même avec un cluster, gardez des **sauvegardes externalisées**
  (section 12) : la HA protège de la panne serveur, pas de la corruption
  de base ni de l'erreur humaine.
- Détails de mise en œuvre (VIP, synchro de base, procédure de bascule)
  → **à vérifier sur la documentation officielle** de votre version.

## 13. NMS hiérarchique : superviser des superviseurs

En édition Professional, eSight supporte le **network management hiérarchique** :
des eSight « subordonnés » (par site, par filiale, par région) remontent
leurs alarmes, topologies et performances vers un eSight « supérieur »
au siège.

Cas d'usage typiques :
- Groupe multi-sites : chaque site a son eSight local (autonomie en cas
  de coupure WAN), le siège a la vision consolidée.
- Séparation des responsabilités : l'équipe locale gère son périmètre,
  la direction voit les indicateurs globaux.

**Attention** : chaque niveau ajoute de la latence de remontée d'alarmes
et de la complexité de licences. Ne hiérarchisez que si l'organisation
l'exige vraiment.

## 14. Interfaces externes : eSight n'est pas une île

La brochure 23.1 liste les interfaces externes fournies :

1. **Southbound** — accès aux équipements (SNMP, syslog, NetStream…).
2. **Northbound** — intégration vers un NMS de niveau supérieur
   (interface SNMP northbound en édition Standard/Professional,
   voir section 13).
3. **Administrator login** — console web.
4. **SMS server** — envoi de SMS d'alarmes.
5. **Authentication server** — intégration à un serveur d'authentification
   (annuaire d'entreprise, voir section 11).

C'est ce qui permet d'inscrire eSight dans un écosystème : un eSight qui
remonte ses alarmes critiques vers un manager-of-managers, qui envoie
des SMS via la passerelle de l'entreprise, qui authentifie les exploitants
sur l'AD — c'est un eSight bien intégré (section 13).

## 15. Sécurité de l'architecture : le NMS est une cible

Un NMS concentre des pouvoirs dangereux : il connaît la topologie complète,
possède des accès SNMP (parfois en écriture) et SSH sur tous les équipements,
et stocke les configurations (avec leurs secrets). **Compromettre eSight =
compromettre le réseau.**

Règles non négociables :
- eSight vit sur un **VLAN de management dédié**, filtré par firewall ;
  seuls les équipements supervisés et les postes d'exploitation y accèdent.
- **HTTPS uniquement** pour la console (pas de HTTP en clair).
- Comptes nominatifs (section 11), jamais de compte générique partagé
  pour l'exploitation courante.
- SNMP v3 authPriv partout où c'est possible ; à défaut, v2c avec des
  ACL strictes côté équipements (n'autoriser que l'IP d'eSight).
- Patchs eSight et OS suivis (section 16).

---

# 3. DIMENSIONNEMENT

## 16. Tableau de dimensionnement constructeur

Le datasheet officiel donne les configurations minimales par palier
d'équipements gérés (**données issues du datasheet historique** ;
les OS cités — Windows Server 2008, MySQL 5.5 — datent de cette édition :
**à vérifier sur la documentation officielle** pour les prérequis
actuels, qui ont évolué).

**Édition Compact (jusqu'à 20 nœuds) :**

| Palier | CPU | Mémoire | Disque |
|---|---|---|---|
| 0 – 20 nœuds | Dual-core 2 GHz ou + | 4 Go | 40 Go |

**Édition Standard :**

| Équipements gérés | CPU | Mémoire | Disque |
|---|---|---|---|
| 0 – 200 | 1 × dual-core 2 GHz ou + | 4 Go | 60 Go |
| 200 – 500 | 2 × dual-core 2 GHz ou + | 4 Go | 60 Go |
| 500 – 2 000 | 2 × quad-core 2 GHz ou + | 8 Go | 120 Go |
| 2 000 – 5 000 | 2 × quad-core 2 GHz ou + | 16 Go | 250 Go |

Le constructeur recommande des **serveurs PC** (serveurs x86 standards).

## 17. Lire ce tableau en chef de service : les règles de marge

Le tableau donne des **minimums**. En pratique :

1. **Prenez le palier au-dessus** de votre parc réel. Un parc de 180
   équipements avec une croissance de 10 %/an dépasse 200 en un an :
   dimensionnez pour 200-500 dès le départ.
2. **La RAM est le premier levier** : si le budget est serré, privilégiez
   la RAM au CPU. eSight est gourmand en mémoire (services Java + base).
3. **Le disque dépend de la rétention** : le tableau suppose une rétention
   standard. Si vous gardez 1 an de données de performance en pas de
   5 minutes sur 500 équipements, prévoyez large (plusieurs centaines
   de Go) — et un contrôle de croissance (voir cas n°13).
4. **Virtualisez** : eSight tourne très bien en VM (malgré une installation
   jugée plus délicate que sur appliance). Avantages : snapshots avant
   mise à jour, redimensionnement à chaud, PRA simplifié.
5. **Séparez si gros parc** : au-delà de ~1 000 équipements, discutez avec
   le partenaire d'une architecture dédiée (base sur serveur séparé,
   cluster) — **à valider sur la documentation officielle**.

## 18. Check-list de dimensionnement avant achat

- [ ] Inventaire des équipements à superviser (par type : routeurs,
      switches, firewalls, AP, serveurs…) avec **+20 % de marge**.
- [ ] Modules eSight retenus (WLAN ? serveurs ? NTA ?).
- [ ] Durée de rétention souhaitée (alarmes : 6-12 mois ; perfs : 3-12 mois
      selon le pas de collecte).
- [ ] Édition choisie (Standard vs Professional : faut-il la HA ? le NMS
      hiérarchique ?).
- [ ] OS du serveur (Windows vs Linux — la HA/cluster exige Linux).
- [ Bloc de texte : ressources VM réservées (pas de surallocation CPU/RAM),
- [ ] Réseau : le serveur eSight joignable en SNMP/syslog depuis tous les
      équipements (routage + firewall).
- [ ] Sauvegarde : espace de stockage externe pour les backups eSight.

---

# 4. INSTALLATION

## 19. Prérequis d'installation

**Serveur :**
- Ressources selon le tableau de la section 16 (palier supérieur).
- OS supporté par votre version d'eSight (**à vérifier sur la
  documentation officielle** — les versions récentes supportent des
  Windows Server et Linux bien plus récents que le datasheet historique).
- Nom d'hôte fixe, **IP fixe**, résolution DNS correcte (le changement
  d'IP après installation est une source classique de pannes — voir
  cas n°15).
- Synchronisation horaire **NTP** (écarts d'horloge = alarmes incohérentes
  et corrélation faussée).

**Base de données :**
- Installée et accessible (MySQL / SQL Server / Oracle selon l'OS et
  la version — **à vérifier sur la documentation officielle**).
- Compte dédié eSight avec les droits requis, mot de passe robuste
  conservé au coffre.

**Réseau :**
- Connectivité IP entre eSight et tous les équipements à superviser.
- Firewall : ouvrir les flux SNMP (UDP 161/162), syslog (UDP 514),
  NetStream, SSH (TCP 22) **dans le sens équipement ↔ eSight** selon
  le besoin, et HTTPS vers les postes d'exploitation.
- Relais SMTP joignable pour les notifications mail ; passerelle SMS
  si notification SMS prévue.

**Licences :** fichiers/actes de licence disponibles avant l'installation
(voir section 7).

## 20. Check-list pré-installation (à cocher le jour J)

- [ ] Snapshot/clone de la VM (si virtualisé) **avant** de commencer.
- [ ] OS à jour, antivirus configuré avec exclusions sur les répertoires
      eSight et de la base (un antivirus qui verrouille les fichiers
      de base = eSight qui rame ou plante).
- [ ] NTP synchronisé et vérifié (`w32tm /query /status` ou `chronyc tracking`).
- [ ] IP fixe, hostname, DNS : notés dans le dossier d'exploitation.
- [ ] Base de données installée, test de connexion OK.
- [ ] Ports firewall ouverts et testés (telnet/nc vers les équipements
      pilotes).
- [ ] Fichiers d'installation eSight + documentation de la version
      téléchargés et vérifiés (checksum si fourni).
- [ ] Fenêtre de maintenance planifiée et communiquée (même si eSight
      est nouveau, les tests de découverte génèrent du trafic SNMP).
- [ ] Binôme : ne jamais installer seul un outil qui deviendra critique —
      à deux, on documente en même temps.

## 21. Installation pas à pas

Déroulé type (les libellés exacts d'écrans **dépendent de la version** —
**à vérifier sur la documentation officielle**) :

**Étape 1 — Lancer l'installeur.**
Sur appliance Huawei : option **one-click install** (retours d'exploitants :
« très simple »). Sur VM/serveur standard : lancer le setup, qui installe
les services eSight et prépare la base.

**Étape 2 — Paramétrer la connexion à la base de données.**
Saisir hôte, port, nom de base, compte et mot de passe dédiés.
*Point de vigilance :* tester la connexion avant de poursuivre ;
80 % des échecs d'installation viennent d'ici (mauvais port, compte
sans droits, firewall local).

**Étape 3 — Choisir les composants à installer.**
Ne cocher que les modules achetés/licenciés (section 3). Installer un
module non licencié = service en erreur au démarrage.

**Étape 4 — Définir les paramètres système.**
Répertoires d'installation et de données (mettre les données sur un
volume dédié si possible — ça simplifie les sauvegardes et le
redimensionnement), ports d'écoute.

**Étape 5 — Créer le compte administrateur initial.**
Compte `admin` initial (nom exact **à vérifier sur la documentation
officielle**). Lui attribuer immédiatement un mot de passe robuste
et le consigner au coffre. **Ne jamais laisser le mot de passe
d'installation par défaut.**

**Étape 6 — Finaliser et démarrer les services.**
L'installeur démarre les services eSight. Sur Linux, vérifier avec
les outils système (`systemctl status` / scripts fournis) ;
sur Windows, via la console des services.

**Étape 7 — Vérifications post-installation.**
- [ ] Les services eSight sont démarrés et stables (pas de restart en boucle).
- [ ] La console web répond en HTTPS.
- [ ] Login avec le compte admin initial : OK.
- [ ] L'outil **Environmental Health Check** (icône eSight Console →
      Tools) ne remonte aucune anomalie bloquante (il contrôle les
      paramètres de base de l'OS, de la base et du serveur).
- [ ] Date/heure correctes dans l'interface.

## 22. Premier login et tour du propriétaire

1. Ouvrez `https://<IP-eSight>:<port>/` (**port exact à vérifier sur la
   documentation officielle**).
2. Connectez-vous avec le compte administrateur initial.
3. **Changez immédiatement le mot de passe** du compte admin
   (politique : 12+ caractères, coffre-fort d'équipe).
4. Faites le tour des menus : Resource (inventaire), Topology, Fault
   (alarmes), Performance, Configuration, Report, System (administration).
5. Vérifiez la version exacte installée (menu Système / À propos) et
   **notez-la dans le dossier d'exploitation** : toutes les procédures
   futures s'y référeront.

## 23. Activation des licences

1. Récupérez vos fichiers/actes de licence (fournis par le partenaire).
2. Dans la console : menu Système/Licence (**libellé à vérifier sur la
   documentation officielle**) → import/activation.
3. Vérifiez que chaque module apparaît comme **licencié et actif**,
   et que le **nombre d'équipements autorisés** couvre votre parc + marge.
4. Testez immédiatement avec 2-3 équipements pilotes : si la licence
   bloque, mieux vaut le savoir le jour J que pendant la généralisation.
5. Archivez les licences dans le PRA (section 12) : sans elles, une
   réinstallation après sinistre est bloquée.

**Piège classique** : licence importée mais module correspondant non
installé (ou l'inverse) → fonctionnalités grisées ou erreurs obscures.
Alignez toujours *modules installés = modules licenciés*.

## 24. Sécurisation post-installation (à faire dans la semaine)

- [ ] Mot de passe admin initial changé + comptes nominatifs créés
      (section 11) ; le compte admin générique n'est plus utilisé
      au quotidien.
- [ ] HTTPS forcé, certificat remplacé par un certificat d'entreprise
      (le certificat auto-signé d'installation fait hurler les navigateurs
      et habitue les exploitants à cliquer « accepter le risque » —
      une mauvaise habitude de sécurité).
- [ ] Comptes de service SNMP v3 créés (jamais de v2c avec community
      « public »/« private » — voir section 5).
- [ ] Politique de verrouillage : tentatives de login, complexité des
      mots de passe, délai d'expiration de session.
- [ ] Journalisation d'audit activée et vérifiée (qui s'est connecté,
      qui a acquitté quoi — voir section 11).
- [ ] Sauvegarde initiale réalisée et testée (section 12).

## 25. L'outil Environmental Health Check : votre premier réflexe

eSight fournit un **outil de contrôle de santé de l'environnement**
(accessible depuis l'icône eSight Console sur le bureau du serveur →
Tools → Environmental Health Check). D'après la documentation, il vérifie
les paramètres de base de l'OS, de la base de données et du serveur.

**En faire un rituel :**
- Après chaque installation / mise à jour : contrôle systématique.
- En cas de comportement anormal d'eSight (lenteurs, services qui
  tombent) : le lancer **avant** de toucher à quoi que ce soit —
  il pointe souvent la cause (disque plein, mémoire, base injoignable).
- Intégrez-le à la check-list mensuelle (section 16).

---

# 5. DÉCOUVERTE DES ÉQUIPEMENTS

## 26. Principe de la découverte

La découverte = faire connaître à eSight les équipements à superviser.
Deux modes complémentaires :

- **Découverte automatique** : eSight balaie des plages IP (ou un segment),
  interroge chaque adresse en SNMP/ICMP, identifie le type d'équipement
  et l'ajoute à l'inventaire.
- **Ajout manuel** : saisie d'un équipement (IP, profil SNMP) — utile pour
  les équipements isolés, les DMZ, ou quand l'auto-découverte est interdite.

La découverte alimente ensuite **l'inventaire (Resource)**, la **topologie**
et la **collecte d'alarmes/performance**. Tant qu'un équipement n'est pas
découvert *et* correctement paramétré en SNMP, il est invisible : c'est
l'étape fondatrice, à ne pas bâcler.

## 27. Préparer les équipements Huawei en SNMP v2c

Côté équipement (exemple de logique sur un switch Huawei — **syntaxe à
adapter à votre modèle/version**) :

- Activer l'agent SNMP.
- Définir une **community de lecture** dédiée à eSight (exemple fictif :
  `SupervisionLecture2026`).
- Si eSight doit modifier la config via SNMP (rare ; préférer SSH),
  définir aussi une community d'écriture — sinon, lecture seule suffit.
- **Restreindre par ACL** : n'autoriser que l'adresse IP du serveur eSight
  à interroger l'agent SNMP.
- Déclarer eSight comme **destination des traps** (trap-target vers
  l'IP d'eSight, port UDP 162).
- Vérifier que le firewall local de l'équipement (s'il existe) autorise
  SNMP depuis eSight.

💡 Côté eSight, on crée ensuite un **profil SNMP v2c** contenant cette
community, réutilisé pour toute la découverte.

## 28. Préparer les équipements en SNMP v3 (recommandé)

SNMP v3 apporte authentification et chiffrement. Côté équipement :

- Créer un **utilisateur SNMP v3** dédié à eSight (exemple fictif :
  `esight-ro`).
- Niveau de sécurité **`authPriv`** : protocole d'authentification
  (SHA de préférence à MD5) + protocole de chiffrement (AES de
  préférence à DES).
- Mots de passe robustes distincts pour auth et priv, consignés au coffre.
- Vue MIB : restreindre si besoin aux branches utiles (surtout en lecture).
- Déclarer eSight comme destinataire des **inform/traps v3**.

Côté eSight : créer le **profil SNMP v3** miroir (même utilisateur,
mêmes protocoles, mêmes mots de passe). **Le moindre écart (SHA vs MD5,
mauvais password) = échec silencieux de la découverte** : c'est la
première chose à vérifier quand « SNMP ne répond pas » (cas n°2,
section 14).

## 29. Créer les profils de découverte dans eSight

Un **profil** regroupe les paramètres d'accès (version SNMP, community ou
utilisateur v3, timeout, retries, paramètres SSH/Telnet pour la gestion
de configuration). Marche à suivre type :

1. Menu Resource/Discovery (**libellé à vérifier sur la documentation
   officielle**) → gestion des profils.
2. Créer un profil par **couple (version SNMP × population d'équipements)** :
   ex. `SNMPv3-Huawei-authPriv`, `SNMPv2c-legacy`, `SNMPv2c-imprimantes`.
3. Régler **timeout** (2-3 s en LAN, plus en WAN) et **retries** (2-3) :
   trop courts = équipements déclarés injoignables à tort ; trop longs =
   découverte interminable.
4. Associer les credentials SSH/Telnet si la sauvegarde de configuration
   est prévue (section 9).

**Bonne pratique** : nommer les profils de façon explicite
(`<proto>-<usage>-<périmètre>`) et documenter dans le dossier
d'exploitation quel profil s'applique à quel parc.

## 30. Lancer une découverte par plages IP

1. Définir les **plages IP** à balayer (ex. : `10.20.0.0/16` pour le LAN,
   en excluant les plages DHCP des postes utilisateurs pour aller vite).
2. Associer le(s) profil(s) SNMP à utiliser, dans l'ordre d'essai.
3. Choisir la **profondeur** : découverte simple (les équipements de la
   plage) vs avec propagation vers les voisins (via LLDP/topologie).
4. Lancer en **fenêtre de maintenance** la première fois (un balayage
   SNMP sur un /16, ça se remarque sur les sondes IDS et ça charge
   les équipements).
5. Suivre l'avancement, puis **contrôler le résultat** : nombre
   d'équipements découverts vs inventaire attendu (section 31).

💡 Découpez les plages par **site/fonction** (une tâche de découverte par
bâtiment ou par VLAN de management) : en cas d'échec, on sait où chercher,
et on peut planifier des redécouvertes ciblées.

## 31. Vérifier que la découverte est complète et saine

Check-list post-découverte :

- [ ] **Quantitatif** : nb d'équipements dans eSight ≈ nb attendu
      (écart > 5 % = enquête).
- [ ] **Qualitatif** : chaque équipement a son **sysName, son type/modèle
      et sa version** correctement identifiés (pas de « unknown device »
      en masse).
- [ ] **Test SNMP** : depuis eSight, un walk de contrôle sur quelques
      équipements (réponse < 1 s en LAN).
- [ ] **Traps** : générer un trap de test (ex. : `linkDown` simulé ou
      reboot d'un équipement de labo) et vérifier sa réception dans
      eSight en moins d'une minute.
- [ ] **Syslog** : vérifier la réception des logs si configurée.
- [ ] **Doublons** : traquer les équipements découverts deux fois
      (multi-homing, plusieurs IP) et fusionner/nettoyer.

## 32. Découverte des équipements tiers (non-Huawei)

eSight gère des équipements de grands constructeurs tiers (HP, Cisco
cités par le constructeur) et des ressources IT (serveurs, imprimantes)
via les **MIB standard** (MIB-II, ENTITY-MIB, IF-MIB…).

- Attendez-vous à une supervision **« générique »** : état up/down,
  interfaces, CPU/mémoire si les MIB standard les exposent — mais pas
  les subtilités propriétaires (d'où la note moyenne hors Huawei
  dans les retours d'exploitants).
- Pour un équipement tiers critique, testez **avant** de généraliser :
  découverte, traps, et 2-3 indicateurs de performance.
- Si un équipement n'est pas reconnu, eSight permet de **personnaliser
  la gestion des équipements tiers** (adaptateurs MIB) — chantier
  avancé, à confier à quelqu'un qui connaît les MIB.

---

# 6. TOPOLOGIE

## 33. Les vues de topologie : la carte du réseau

La topologie est la **vue d'accueil** d'eSight : une carte graphique des
équipements et de leurs liens, avec l'état (couleur selon les alarmes).
C'est l'écran que le NOC garde affiché en permanence.

Vues disponibles (selon édition et version) :
- **Topologie physique** : équipements et liens réels (découverts via
  LLDP/CDP, tables ARP/forwarding).
- **Topologie IP** : vision par sous-réseaux/adresses.
- **Vues personnalisées** : par site, par service, par client — construites
  à la main.

**Réflexe terrain** : la topologie ne vaut que si elle est **à jour**.
Une carte avec des équipements décommissionnés depuis 6 mois = une carte
qu'on ne regarde plus. Nettoyage trimestriel obligatoire.

## 34. Personnaliser les vues

- Créer une vue **par site** (bâtiment A, bâtiment B, datacenter) et une
  vue **« backbone »** (cœur de réseau uniquement).
- Positionner les équipements manuellement pour refléter la géographie
  réelle (pas en vrac automatique).
- Colorer / étiqueter par criticité : le lien du datacenter n'a pas la
  même importance visuelle que celui de l'imprimante du 3ᵉ étage.
- Définir la **vue par défaut** affichée à la connexion selon le profil
  (le NOC voit la vue globale, le technicien site voit son site).

## 35. Fonds de plan : rendre la carte parlante

eSight permet d'utiliser des **fonds de plan** (images) derrière la
topologie : plan de bâtiment, carte de la ville/région pour du multi-sites.

- Utilisez des plans **simplifiés et à jour** (pas le plan DWG de
  l'architecte de 2012).
- Placez les équipements à leur position approximative réelle :
  en panne, on localise en 10 secondes (« le switch du local technique
  R+2, bâtiment B »).
- Pour le multi-sites : une vue « France » avec un point par site,
  chaque point ouvrant la vue détaillée du site (navigation en 2 clics).

## 36. Les liens : ce qu'ils montrent (et ce qu'ils cachent)

- Les liens affichent l'état (up/down/dégradé) et peuvent afficher
  l'utilisation (épaisseur ou couleur selon la charge — **selon version**).
- Un lien « down » dans eSight doit correspondre à une alarme :
  si ce n'est pas le cas, la découverte de topologie ou les traps
  sont mal configurés (voir cas n°5).
- **Limite connue** : les liens agrégés (LACP), les tunnels et les
  chemins MPLS ne se représentent pas toujours fidèlement — complétez
  par des vues logiques si vous en avez beaucoup (édition Standard
  requise pour MPLS VPN/tunnels).

## 37. Bonnes pratiques topologie (le minimum vital)

1. Une vue « NOC » globale + une vue par site : pas 47 vues.
2. Nettoyage trimestriel : équipements sortis, liens morts.
3. Fonds de plan à jour après chaque déménagement/travaux.
4. Former les nouveaux exploitants à **lire** la topologie (codes couleur,
   navigation) dès leur arrivée.
5. Captures d'écran de la topologie de référence archivées : en cas
   d'incident majeur, comparer « avant/après » fait gagner un temps fou.

---

# 7. GESTION DES ALARMES

## 38. Le cycle de vie d'une alarme

Comprendre le cycle de vie, c'est comprendre tout le fault management :

```
Événement équipement (panne, seuil dépassé)
        │ trap SNMP / syslog / polling
        ▼
┌──────────────────┐
│ Alarme COURANTE  │ ← visible, à traiter
│ (active)         │
└────────┬─────────┘
         │ acquittement (prise en charge)
         ▼
┌──────────────────┐
│ Alarme ACQUITTÉE │ ← quelqu'un s'en occupe
└────────┬─────────┘
         │ fin du défaut (clear) ou suppression manuelle
         ▼
┌──────────────────┐
│ Alarme HISTORIQUE│ ← archivée, consultable
└──────────────────┘
```

- **Courante** : le défaut est présent (ou pas encore soldé).
- **Acquittée** : un exploitant a pris en charge (ça n'éteint PAS le
  défaut, ça dit « je m'en occupe »).
- **Historique** : soldée, conservée pour analyse et reporting.

**Règle d'or du NOC** : aucune alarme critique ne reste « courante non
acquittée » plus de X minutes (X défini dans vos SLA internes, ex. 15 min).
C'est LE KPI du fault management (section 15).

## 39. Les sévérités : parler le même langage

Échelle usuelle (libellés exacts **à vérifier sur la documentation
officielle** de votre version) :

| Sévérité | Signification | Exemple |
|---|---|---|
| Critical | Service interrompu ou risque imminent | Lien backbone down, équipement injoignable |
| Major | Dégradation forte | Perte d'un membre d'un stack, alim redondante HS |
| Minor | Dégradation faible / anomalie | Seuil CPU dépassé brièvement, ventilateur en défaut (redondé) |
| Warning | Signal préventif | Certificat expirant, espace disque > 80 % |
| Info / Cleared | Informatif / fin d'alarme | Fin de défaut, acquittement |

**Travail de fond indispensable** : la sévérité « sortie d'usine » des
traps n'est pas toujours pertinente pour VOTRE contexte. Une alarme
« ventilateur » sur un switch d'accès redondé n'est pas critique ;
la même sur le seul switch d'un site isolé, si. D'où la **redéfinition
des sévérités** (section 44) : prévoyez un atelier de 2 heures avec
l'équipe pour calibrer les 30 alarmes les plus fréquentes.

## 40. Acquitter : la discipline de base

- **Acquitter = « je prends en charge »**. Toute alarme critique/majeure
  acquittée doit avoir un **commentaire** (ticket, action en cours).
- Interdire l'acquittement « pour faire du propre » sans action :
  c'est la porte ouverte aux pannes ignorées.
- L'**acquittement automatique** (règles) existe : eSight peut
  automatiquement acquitter et basculer en historique les alarmes
  traitées qui correspondent à des règles (d'après la documentation :
  *« after automatic acknowledgement rules are set, eSight automatically
  confirms processed and cleared alarms and moves them to the historical
  alarm list »*). À utiliser avec parcimonie, sur des alarmes
  bien connues et sans impact.
- **Journal d'expérience** : la documentation mentionne l'enregistrement
  de l'expérience de traitement des alarmes (*Recording Alarm Handling
  Experience*) : capitalisez les résolutions (« alarme X → cause Y →
  action Z ») pour les nouveaux arrivants.

## 41. Notifications : mail et SMS

eSight permet de définir des **règles de notification** : pour une source
d'alarme + une sévérité données, notifier un **groupe d'utilisateurs**
par e-mail et/ou SMS.

Mise en place type :
1. Configurer le **serveur SMTP** (relais de l'entreprise) et tester.
2. Configurer la **passerelle SMS** (interface SMS server — selon
   l'opérateur/équipement, **à vérifier sur la documentation officielle**).
3. Créer les **groupes** : `astreinte-reseau`, `astreinte-systeme`,
   `managers` (qui ne reçoit que le critical).
4. Créer les règles : ex. *Critical sur backbone → SMS + mail à
   astreinte-reseau ; Major → mail ; Minor/Warning → pas de notification
   (consultation en console)*.
5. **Tester** chaque règle (alarme de test) et vérifier la réception
   réelle sur les téléphones.

**Règles d'hygiène :**
- Le SMS est réservé au **critical** (sinon on ne lit plus les SMS).
- Inclure dans le message : sévérité, équipement, libellé, heure —
  pas un « alarme eSight » générique qui oblige à se connecter pour
  comprendre.
- Tester les notifications **tous les mois** (un relais SMTP qui change
  d'IP et plus personne n'est prévenu — grand classique, voir cas n°9).

## 42. Agrégation : lutter contre le bruit

L'**agrégation** (aggregation rules) regroupe les alarmes répétitives
identiques en une seule alarme « agrégée » avec compteur, au lieu
d'inonder la console.

Cas d'école : un lien qui flappe (up/down toutes les 2 minutes) génère
des dizaines d'alarmes. Avec agrégation : **une** alarme
« Interface flapping — 47 occurrences ».

- Définir les règles d'agrégation sur les alarmes connues pour être
  bavardes (flapping, seuils limites).
- L'alarme agrégée doit rester **visible et acquittable** comme les autres.

## 43. Masquage : taire ce qu'on sait déjà

Les **règles de masquage** (masking rules) empêchent la remontée
d'alarmes pendant des situations connues :

- Fenêtre de **maintenance** sur un équipement : on masque ses alarmes
  (sinon le NOC est noyé et rate les vraies pannes ailleurs).
- Équipement en **dérangement connu** en attente de pièce : masqué avec
  date de fin, pas « oublié ».

**Discipline** : tout masquage a une **date de fin** et un **motif**.
Un masquage permanent = une panne invisible = à interdire.
Revue hebdomadaire des masquages actifs (section 16).

## 44. Redéfinition des sévérités et des noms

eSight permet de **redéfinir la sévérité et le type** des alarmes
(severity and type redefinition rules) et même leur **nom**
(name redefinition rules).

Usages :
- Descendre en Minor une alarme remontée Critical par défaut mais sans
  impact chez vous.
- Renommer une alarme au jargon de l'entreprise (« Alarme carte
  d'alimentation » → « Défaut alim — switch cœur ») pour que le NOC
  comprenne sans dictionnaire MIB.
- Aligner les types pour le reporting (toutes les alarmes « énergie »
  dans la même catégorie, par exemple — utile pour un chef de service
  systèmes & énergies qui veut suivre les alimentations/onduleurs).

## 45. Alarmes intermittentes (toggling) : les traiter à part

Les **alarmes intermittentes** (qui apparaissent/disparaissent en boucle)
polluent et masquent les vrais problèmes. La documentation prévoit des
**règles de traitement des alarmes intermittentes/toggling**.

- Identifier les équipements « toggleurs » chroniques (rapport
  d'occurrences).
- Traiter la **cause racine** (souvent : lien défectueux, SFP fatigué,
  seuil trop sensible) plutôt que de masquer.
- En attendant la réparation : règle de toggling pour contenir le bruit,
  avec date de fin (comme tout masquage).

## 46. Tempête d'alarmes (alarm storm) : le scénario catastrophe

**Symptômes** : des centaines/milliers d'alarmes en quelques minutes
(panne du cœur de réseau, boucle, flap massif).

**Conduite à tenir :**
1. **Ne pas tout acquitter en panique** : on perd la chronologie.
2. Utiliser la **corrélation** (section 47) : eSight peut rattacher les
   alarmes « conséquences » à l'alarme « cause racine » (ex. : tous les
   « équipement injoignable » rattachés au « lien backbone down »).
3. Traiter la cause racine ; les alarmes conséquences se solderont
   (clear) en cascade.
4. Après l'incident : analyser la tempête (quelles règles de corrélation
   manquaient ?) et les ajouter — chaque tempête doit rendre le système
   plus intelligent.

**Prévention** : seuils de tempête (si X alarmes en Y minutes → alarme
méta « storm suspected » + notification renforcée) — **selon les
possibilités de votre version, à vérifier sur la documentation officielle**.

## 47. Corrélation : trouver la cause racine

Les **règles de corrélation** (correlation rules) lient des alarmes entre
elles selon des critères (temps, topologie, type) pour désigner une
**alarme racine** et des alarmes **corrélées/conséquences**.

Exemple : à 14:02, « Lien inter-sites down » (racine) ; à 14:02:05,
« 12 équipements injoignables » (conséquences). Sans corrélation : 13
alarmes critiques à traiter. Avec : 1 alarme racine à traiter + 12
rattachées automatiquement.

- Construire les règles de corrélation sur la **topologie réelle**
  (dépendances : tel site dépend de tel lien).
- Les tester sur incidents passés (rejouer l'historique si possible).
- C'est un chantier **continu** : chaque incident majeur enseigne une
  nouvelle règle.

## 48. Synchronisation des alarmes

La **synchronisation** (synchronizing alarms) consiste à réaligner eSight
avec l'état réel des équipements (re-polling) : utile après une coupure
de communication entre eSight et les équipements (les traps perdus en
route laissent des alarmes « fantômes »).

- Après toute coupure réseau entre eSight et un site : **synchroniser**
  avant de traiter (sinon on traite des alarmes obsolètes).
- À intégrer au runbook « retour de coupure » (section 15).

## 49. Filtrage northbound : ne remonter que l'utile

Quand eSight remonte ses alarmes vers un NMS supérieur (manager-of-managers),
le **filtrage northbound** (northbound filtering rules) choisit **quelles**
alarmes sont transmises : par sévérité, par alarmes ciblées, par source
(équipements), avec des conditions avancées.

Objectif : le NMS central ne reçoit que le significatif (pas les warnings
d'un site distant). Sans filtrage : congestion et perte des vraies alertes
— la documentation le dit explicitement.

## 50. Mettre en place une politique d'alarmes : le plan en 5 étapes

1. **Recenser** : extraire les 50 alarmes les plus fréquentes sur 3 mois.
2. **Calibrer** : atelier équipe — sévérité juste, nom parlant, action
   attendue pour chacune (runbook, section 15).
3. **Régler** : redéfinitions, agrégations, masquages types, corrélations
   de base, notifications.
4. **Tester** : générer des alarmes de test, vérifier notifications et
   corrélation.
5. **Revoir** : comité mensuel « alarmes » — bruit résiduel, nouvelles
   règles, masquages à lever. Une politique d'alarmes est un organisme
   vivant.

---

# 8. SUPERVISION DES PERFORMANCES

## 51. Les indicateurs suivis

eSight collecte en polling SNMP les indicateurs standards :

**Équipement (santé) :**
- CPU (utilisation %, par processeur/carte sur les châssis).
- Mémoire (utilisation %, disponible).
- Température, état des ventilateurs et alimentations (selon MIB).
- Temps de fonctionnement (uptime) — un uptime qui retombe à zéro =
  reboot à investiguer.

**Interfaces (trafic) :**
- Débit entrant/sortant (bps), utilisation en % de la capacité.
- Erreurs, discards, paquets unicast/multicast/broadcast.
- État opérationnel et administratif.

**Selon modules** : performances serveurs (CPU/RAM/disque), stockage
(IOPS, latence, capacité), WLAN (utilisateurs par AP, bruit radio),
applications (temps de réponse d'URL, état des services).

## 52. Seuils : quand un chiffre devient une alarme

Un **seuil** (threshold) = une valeur limite sur un indicateur, dont le
dépassement génère une alarme.

Seuils de départ raisonnables (à affiner par équipement) :

| Indicateur | Warning | Critical | Remarque |
|---|---|---|---|
| CPU équipement | > 70 % pendant 15 min | > 90 % pendant 5 min | Toujours avec durée pour éviter les pics |
| Mémoire | > 80 % | > 95 % | Une fuite mémoire se voit en tendance |
| Utilisation lien | > 70 % (tendance) | > 90 % | Le critical sur un lien = risque de saturation |
| Erreurs interfaces | > 0,1 % des paquets | > 1 % | Zéro erreur = normal sur cuivre/fibre sain |
| Température | Selon spec constructeur - 10 °C | Selon spec | À adapter par modèle |
| Espace disque (serveurs) | > 80 % | > 95 % | Classique mais vital |

**Principes :**
- Toujours un **délai** (le dépassement doit durer) : un pic CPU de
  30 secondes n'est pas une alarme.
- Seuils **différenciés** : le seuil du lien datacenter n'est pas celui
  du lien d'une agence.
- Revoir les seuils **tous les 6 mois** : un seuil qui ne déclenche
  jamais est inutile ; un qui déclenche tout le temps est du bruit.

## 53. Tableaux de bord : l'écran du matin

Construire des **dashboards** par public :

- **Dashboard NOC** : topologie + alarmes courantes critiques/majeures +
  top 10 des liens les plus chargés + équipements injoignables.
- **Dashboard chef de service** : nb d'alarmes par sévérité (semaine),
  disponibilité par site (%), top 5 des équipements les plus alarmés,
  tickets ouverts liés réseau.
- **Dashboard énergie** (votre casquette !) : état des alimentations,
  onduleurs supervisés, température des locaux techniques.

Rituel : le dashboard chef de service s'ouvre **tous les matins** en
5 minutes. Si un chiffre déraille, on creuse avant la réunion d'équipe.

## 54. NTA / NetStream : qui consomme la bande passante

Le composant **NTA (Network Traffic Analyzer)**, en édition Standard,
exploite les flux **NetStream** exportés par les équipements Huawei
(équivalent NetFlow/IPFIX) pour répondre à : *qui parle à qui, avec quel
protocole, quel volume ?*

Usages terrain :
- Identifier le **top talker** qui sature un lien (sauvegarde qui part
  en pleine journée, poste infecté, réplication mal planifiée).
- Vérifier qu'une **QoS** est respectée (le trafic prioritaire passe-t-il
  vraiment en priorité ?).
- Enquêtes « le réseau est lent » : prouver par les chiffres où part
  la bande passante.

Prérequis : activer l'export NetStream côté équipements vers eSight
(charge CPU non négligeable sur les petits modèles — à tester) et
prévoir l'espace disque (les données de flux sont volumineuses).

## 55. Rapports : la preuve du travail accompli

eSight fournit des **rapports prédéfinis** et un **concepteur de rapports**
personnalisés (d'après le constructeur : *« predefined reports and an
easy-to-use report design function »*).

Rapports à mettre en place d'office :
- **Disponibilité mensuelle par site/équipement** (le % qui va au COPIL).
- **Top des alarmes** (qu'est-ce qui nous pourrit la vie ? → plan d'action).
- **Capacité** : liens > 70 %, équipements CPU/RAM tendus (anticiper les
  investissements).
- **Inventaire** : parc supervisé, versions logicielles (préparer les
  montées de version).
- **Activité d'exploitation** : alarmes traitées, temps d'acquittement
  (le reporting de l'équipe).

**Automatiser** : génération et envoi par e-mail en début de mois.
Un rapport que personne ne lit ne sert à rien : 1 page de synthèse pour
la direction, le détail pour l'équipe.

## 56. SLA réseau : le diagnostic périodique automatique

Le composant **network SLA management** diagnostique automatiquement et
périodiquement les chemins réseau (type ping/traceroute supervisés,
mesures de latence/perte).

- Définir les **chemins critiques** : siège ↔ datacenter, siège ↔ sites
  distants, vers les applications SaaS critiques.
- Seuils : latence et perte de paquets (ex. : alerte si perte > 1 % ou
  latence > 2× la normale sur 15 min).
- En cas de plainte « l'application X est lente », le graphe SLA dit
  immédiatement si le réseau est en cause — **fini les guerres de
  tranchées entre équipes réseau et applicatives**.

---

# 9. GESTION DES CONFIGURATIONS

## 57. Sauvegarde automatique des configurations

C'est l'une des fonctions les plus rentables d'eSight : la **sauvegarde
périodique et automatique des fichiers de configuration** des équipements
(d'après le constructeur : sauvegarde immédiate, périodique, et déclenchée
par changement de configuration).

Mise en place :
1. Vérifier l'accès **SSH/Telnet** (ou SNMP selon version) depuis eSight
   vers chaque équipement avec un compte dédié.
2. Planifier la sauvegarde : **quotidienne** (nuit) + **à chaque changement
   détecté** si la version le permet.
3. Conserver N générations (ex. : 30 jours glissants) — vérifier l'espace
   disque alloué.
4. **Tester la restauration** sur un équipement de labo : une sauvegarde
   non testée n'est pas une sauvegarde.

**Le jour où un switch meurt à 3h du matin**, pouvoir pousser la dernière
config connue sur le matériel de remplacement en 10 minutes au lieu de
la reconstruire de mémoire en 3 heures : c'est ça, le ROI de cette fonction.

## 58. Comparaison de configurations : voir ce qui a changé

eSight permet de **comparer** deux fichiers de configuration (ex. : la
version d'hier vs celle d'aujourd'hui, ou la config d'un site vs le
modèle de référence).

Usages :
- « Ça marchait hier, ça ne marche plus » → diff des configs, la cause
  est dedans 9 fois sur 10.
- **Audit de conformité** : comparer les configs du parc à un
  template de référence (mot de passe, SNMP, syslog, NTP, banners).
- Avant/après une intervention : figer la config avant, comparer après.

## 59. Restauration : remettre une configuration

- Restauration **manuelle** : choisir une génération sauvegardée et la
  pousser vers l'équipement (via SSH/Telnet).
- **Procédure encadrée** : toute restauration se fait en fenêtre de
  maintenance, avec config actuelle sauvegardée juste avant (filet de
  sécurité), et test de non-régression après.
- **Ne jamais restaurer « à l'aveugle »** une config de plus de quelques
  jours : entre-temps, des changements légitimes ont pu intervenir
  (nouvelles VLAN, routes). Comparer d'abord (section 58).

## 60. Smart Configuration Tool : déployer en masse

Le **Smart Configuration Tool** (inclus dès l'édition Compact) permet
d'appliquer des configurations ou des scripts à **plusieurs équipements**
en une fois.

Cas d'usage :
- Déployer un changement standard sur 50 switch d'accès (ex. : ajouter
  le serveur syslog, changer la community SNMP).
- Montées de version logicielles planifiées (firmware) par lots.

**Garde-fous :**
- Toujours tester sur **2-3 équipements pilotes** avant généralisation.
- Prévoir le **plan de rollback** (config sauvegardée avant, section 57).
- Tracer : qui a lancé quoi, quand, sur quels équipements (audit,
  section 11).

## 61. Inventaire, ressources physiques et étiquettes électroniques

Au-delà des configs, eSight gère l'**inventaire** :
- **Ressources physiques** : châssis, cartes, alimentations, SFP —
  savoir ce qui est *dans* chaque équipement sans se déplacer.
- **Étiquettes électroniques** (electronic labels) : les infos d'identification
  lues dans l'équipement (numéros de série, modèles) — précieuses pour
  le suivi de garantie et les RMA.
- **IP topology / gestion des liens** : inventaire des interconnexions.

**Exploitation** : export mensuel de l'inventaire (CSV) vers votre CMDB
ou votre tableau de suivi de parc. C'est la base du plan de renouvellement
et des déclarations de garantie.

---

# 10. GESTION WLAN

## 62. Supervision des AC et des AP

Le module **WLAN management** (édition Standard minimum) supervise les
**contrôleurs (AC)** et les **points d'accès (AP)** :

- État des AP (en ligne/hors ligne), nombre d'utilisateurs associés
  par AP et par SSID.
- Santé des AC (CPU, mémoire, licences AP).
- **Topologie unifiée filaire + sans fil** : voir d'un coup d'œil le
  chemin complet d'un utilisateur Wi-Fi jusqu'au cœur de réseau.

**Seuil d'alerte à configurer d'office** : pourcentage d'AP hors ligne
par site (ex. : > 10 % des AP d'un site down = alerte major — un AP
isolé qui tombe, c'est du minor).

## 63. Diagnostic des pannes sans fil

Le module aide au **diagnostic des pannes du réseau sans fil** :

- Un utilisateur se plaint : retrouver son **historique de connexion**
  (quel AP, quel signal, quelles déconnexions) au lieu de lui demander
  « vous êtes où exactement ? ».
- Interférences / bruit radio : repérer les AP qui souffrent et
  envisager un ajustement de canaux/puissance.
- Saturation : repérer les AP avec trop d'utilisateurs simultanés →
  densifier ou équilibrer.

**Limite honnête** : eSight reste un NMS généraliste ; pour du
troubleshooting radio très fin (spectrum analysis poussé, planification
prédictive), des outils spécialisés complètent. Pour l'exploitation
quotidienne d'un Wi-Fi de campus, le module suffit.

## 64. WLAN et énergie : le clin d'œil au chef de service

Les AP sont souvent alimentés en **PoE** par les switches d'accès :
- Superviser la **consommation PoE** par switch (budget PoE utilisé vs
  disponible) : un switch dont le budget PoE est à 95 % ne peut plus
  accueillir d'AP supplémentaires.
- En cas de coupure secteur sur onduleur, savoir quels switches portent
  le Wi-Fi critique permet de **prioriser** (éteindre le Wi-Fi invité
  pour préserver l'autonomie du Wi-Fi production, par exemple).
- Corréler « AP hors ligne » avec « switch en défaut d'alimentation » :
  la cause n'est pas radio, elle est électrique — et c'est votre domaine.

---

# 11. COMPTES, RÔLES ET AUDIT

## 65. Rôles : le moindre privilège

eSight gère des **rôles** avec des droits différenciés. Découpage type
à mettre en place :

| Rôle | Périmètre | Exemple |
|---|---|---|
| Administrators | Tout, y compris système et comptes | 1-2 personnes max |
| Network operators | Alarmes, topo, perfs, acquittement | Équipe NOC |
| Config operators | + gestion des configurations | Réseau niveau 2/3 |
| Read-only / Guests | Consultation seule | Managers, auditeurs |
| Site-restricted | Périmètre géographique limité | Technicien d'un site |

**Règles :**
- Comptes **nominatifs** : jamais de `reseau1` partagé. En cas d'incident,
  on doit savoir *qui* a fait *quoi*.
- Le compte admin générique ne sert qu'aux opérations d'administration
  système, jamais à l'exploitation courante.
- Revue des comptes **tous les 6 mois** : départs, changements de poste.

## 66. Domaines : cloisonner par organisation

La gestion par **domaines** permet de restreindre la visibilité et les
actions à un périmètre (site, filiale, client) :

- Le technicien du site B ne voit que le site B (moins de bruit, moins
  de risque d'action sur le mauvais équipement).
- En environnement multi-entités, chaque entité a son domaine avec ses
  exploitants, et la DSI centrale garde la vision globale.

À combiner avec les vues de topologie par site (section 34) pour une
expérience cohérente.

## 67. Audit : qui a fait quoi

eSight journalise les **logs** d'administration et d'exploitation :
connexions, acquittements d'alarmes, modifications de configuration,
changements de seuils, imports de licences…

- **Vérifier** que l'audit est actif et que les logs sont conservés
  assez longtemps (ex. : 1 an — contrainte réglementaire possible).
- En cas d'incident, les logs d'audit sont la **première pièce** de
  l'enquête (« qui a modifié la config du firewall vendredi à 18h ? »).
- Protéger les logs : un admin ne doit pas pouvoir effacer ses propres
  traces (droits différenciés, export vers un syslog externe si possible).

## 68. Intégration à l'annuaire d'entreprise

La brochure 23.1 mentionne l'**intégration à un serveur
d'authentification** (authentication server integration).

- Raccorder eSight à l'**AD/LDAP** de l'entreprise : un seul mot de
  passe par exploitant, désactivation automatique au départ (plus de
  compte orphelin).
- Mapper les **groupes AD** vers les rôles eSight (ex. : groupe
  `G-reseau-NOC` → rôle Network operators).
- Garder **un compte admin local de secours** (coffre-fort) pour accéder
  à eSight si l'AD est indisponible — sinon, panne AD = plus de supervision.
- Détails de configuration (LDAPS, attributs, mapping) → **à vérifier
  sur la documentation officielle** de votre version.

---

# 12. SAUVEGARDE D'ESIGHT ET PRA

## 69. Ce qu'il faut sauvegarder (et pourquoi)

eSight concentre des années de paramétrage : profils de découverte,
règles d'alarmes, seuils, rapports, comptes. Perdre eSight sans sauvegarde
= des semaines de reconfiguration. À sauvegarder :

1. **La base de données** (inventaire, alarmes, perfs, configs
   d'équipements sauvegardées, paramétrage) — via le **Database Backup &
   Restore Tool** fourni.
2. **Le système eSight** (binaires, fichiers de configuration applicative).
3. **Les licences** (fichiers/actes — sans eux, pas de réinstallation
   fonctionnelle).
4. **Le dossier d'exploitation** (schémas, mots de passe au coffre,
   procédures) — hors eSight, évidemment.

## 70. Stratégie de sauvegarde : le plan type

| Quoi | Fréquence | Rétention | Destination |
|---|---|---|---|
| Base eSight (complète) | Quotidienne (nuit) | 30 jours glissants | Stockage externe (NAS, pas le serveur eSight lui-même !) |
| Base eSight | Avant chaque changement majeur | Jusqu'à validation du changement + 30 j | Stockage externe |
| Système eSight | Hebdomadaire | 4 semaines | Stockage externe |
| Licences + dossier d'exploitation | À chaque changement | Permanent (versionné) | Coffre + PRA |

**Automatiser** : sauvegarde planifiée (le Backup Tool le permet en
périodique d'après le constructeur), avec **contrôle du succès**
(e-mail de confirmation ou supervision du fichier généré — une sauvegarde
qui échoue en silence ne sert à rien).

## 71. Restauration : la procédure à tester

1. **Environnement de test** : restaurer au moins une fois par an sur
   une VM isolée pour valider que la sauvegarde est exploitable.
2. **Procédure écrite** : pas à pas, avec les commandes exactes, les
   comptes à utiliser, les durées constatées lors du test.
3. **Ordre de restauration** : OS → base → applicatif eSight → licences →
   vérifications (login, découverte d'un équipement test, alarme de test).
4. **Ne restaurez jamais « par-dessus » un eSight en production qui
   fonctionne** sans avoir figé une sauvegarde juste avant.

**Test annuel obligatoire** : une sauvegarde non testée = pas de
sauvegarde. Inscrivez le test au plan de maintenance (section 16) et
faites-le signer.

## 72. PRA : aller au-delà de la sauvegarde

Le PRA (Plan de Reprise d'Activité) d'eSight comprend :

- **RPO/RTO cibles** : ex. RPO 24 h (perte max d'une journée de données),
  RTO 4 h (console à nouveau opérationnelle). À valider avec la direction.
- **Scénarios** : panne serveur simple (restauration VM/snapshot),
  corruption de base (restauration Backup Tool), perte du site
  (réinstallation sur site de secours + restauration).
- **Cluster hot standby** (édition Professional, Linux) : c'est de la
  **haute disponibilité**, pas un PRA — ça ne remplace pas les
  sauvegardes externalisées (section 12 du chapitre architecture).
- **Dépendance circulaire** : eSight supervise le réseau… dont il a
  besoin pour être supervisé et sauvegardé. Prévoyez un chemin
  **hors bande** (console, réseau de management indépendant) pour
  administrer eSight quand le réseau de production est en panne.
- **Annuaire des contacts** : support Huawei/partenaire, contrats,
  numéros — imprimé et dans le PRA, pas seulement dans eSight.

---

# 13. INTÉGRATION NORTHBOUND ET ÉCOSYSTÈME

## 73. Interface SNMP northbound : eSight vu d'en haut

En éditions Standard et Professional, eSight expose une **interface
northbound SNMP** : il peut **transférer ses alarmes** (traps) vers un
NMS de niveau supérieur (manager-of-managers du groupe, hyperviseur
national…).

- Configurer le **filtrage northbound** (section 49) pour ne remonter
  que l'utile.
- Déclarer le NMS supérieur comme destinataire des traps northbound
  d'eSight.
- Tester la chaîne complète : défaut sur équipement → alarme eSight →
  trap northbound → alarme visible dans le NMS supérieur.

## 74. API et développements spécifiques

La brochure 23.1 décrit eSight comme une **plateforme ouverte** pour une
exploitation personnalisée (*« flexible open platform for enterprises to
customize their own network management systems »*).

- Des **interfaces northbound** existent pour l'intégration et le
  développement secondaire (détails : REST ? SOAP ? modèles de données ? →
  **à vérifier sur la documentation officielle** de votre version, les
  API évoluent).
- Cas d'usage : alimenter un portail interne, créer des tickets
  automatiquement dans l'ITSM, exporter des indicateurs vers une
  plateforme BI.
- **Conseil** : avant de développer, vérifiez ce que l'export standard
  (rapports programmés, traps northbound) couvre déjà — 80 % des besoins
  ne nécessitent aucun code.

## 75. Intégration avec l'ITSM (tickets)

Le couple gagnant : **alarme eSight → ticket automatique**.

- Via l'API northbound ou via e-mail parser côté ITSM (GLPI, etc.) :
  à la création d'une alarme critical/major, création d'un ticket avec
  le contexte (équipement, alarme, heure).
- À la résolution (clear), clôture ou commentaire automatique du ticket.
- **Ne pas tout ticketiser** : seules les sévérités critical/major
  (sinon le centre de tickets devient une poubelle).
- Bénéfice : traçabilité complète incident → alarme → action, et des
  KPI d'exploitation fiables (section 15).

## 76. Notifications SMS et passerelles

L'interface **SMS server** permet à eSight d'envoyer des SMS d'alarmes
via une passerelle (modem GSM, passerelle IP de l'opérateur…).

- Réserver le SMS aux **critical** et à l'astreinte (section 41).
- Tester mensuellement (changement de carte SIM, d'APN, de passerelle :
  le SMS est le canal qui tombe en panne en silence).
- Prévoir un **canal de secours** (appel vocal automatisé ou messagerie
  d'astreinte) : si eSight est down, il ne peut pas prévenir qu'il est down
  — d'où la supervision d'eSight lui-même (section 16).

## 77. eSight et eKit : que faut-il savoir ?

Huawei **eKit** est la marque/offre Huawei orientée distribution et PME
(équipements et outils simplifiés pour les petits réseaux). **Aucune
documentation publique consultée ne décrit une intégration native
eSight ↔ eKit** → **à vérifier sur la documentation officielle** et
auprès de votre partenaire.

En pratique :
- Si votre parc eKit est petit (< 20 équipements), l'édition **Compact**
  ou les outils cloud/Simplifiés associés à eKit peuvent suffire —
  un eSight complet serait surdimensionné.
- Si le parc grandit ou se mélange avec du Huawei entreprise classique,
  eSight redevient pertinent comme superviseur unifié.
- Ne présumez d'aucune compatibilité : **testez la supervision d'un
  équipement eKit depuis eSight en maquette avant d'acheter**.

## 78. Coexister avec d'autres outils (Zabbix, PRTG…)

eSight n'a pas besoin d'être seul :
- **Zabbix/Prometheus** pour la supervision fine des serveurs et
  applicatifs, **eSight** pour le réseau Huawei : complémentarité saine.
- Éviter les **doubles pollings agressifs** sur les mêmes équipements
  (deux NMS qui interrogent en SNMP toutes les 30 s = charge CPU
  inutile sur les petits switches).
- Répartir les rôles par écrit : quel outil est **référent** pour
  quelles alarmes (sinon deux équipes se renvoient la balle).

---

# 14. DÉPANNAGE : 17 CAS TERRAIN

> Format : **Symptômes → Diagnostic → Solution**. Les chemins de menus
> exacts dépendent de la version (**à vérifier sur la documentation
> officielle**).

## 79. Cas n°1 — Équipement non découvert

**Symptômes** : l'équipement n'apparaît pas dans l'inventaire après
la tâche de découverte, alors qu'il est bien dans la plage IP.

**Diagnostic :**
1. L'équipement répond-il au **ping** depuis le serveur eSight ?
   Non → problème réseau/routage/firewall avant même SNMP.
2. L'agent **SNMP est-il activé** sur l'équipement ? (`display snmp-agent`
   sur Huawei — la commande exacte dépend du modèle.)
3. La **community / l'utilisateur v3** correspond-elle au profil eSight ?
   Tester avec un outil externe (snmpwalk) depuis le serveur eSight :
   si snmpwalk échoue, le problème est côté équipement ou réseau,
   pas côté eSight.
4. Une **ACL** côté équipement bloque-t-elle l'IP d'eSight ?
5. Le **timeout/retry** du profil est-il adapté (WAN lent) ?

**Solution :** corriger le point bloquant (activer SNMP, aligner la
community, ouvrir l'ACL, augmenter le timeout), puis **relancer une
découverte ciblée** sur l'IP unique — pas tout le /16.

## 80. Cas n°2 — SNMP ne répond pas / timeouts en masse

**Symptômes** : des dizaines d'équipements passent « SNMP timeout »
simultanément.

**Diagnostic :**
1. **Tout le monde ou un segment ?** Tout le monde = problème côté
   eSight ou réseau central (lien, firewall). Un segment = problème
   local (lien montant, ACL).
2. Vérifier que l'**IP source** d'eSight n'a pas changé (nouvelle VM,
   nouvelle carte réseau — le cas classique après migration).
3. Vérifier les **ACL SNMP** des équipements : autorisent-elles toujours
   l'IP d'eSight ?
4. Charge du serveur eSight : un polling trop agressif + serveur
   sous-dimensionné = timeouts (voir cas n°13).
5. Un changement de **mot de passe SNMP v3** ou de community déployé
   en masse sans mettre à jour le profil eSight ?

**Solution :** selon la cause — corriger le routage/firewall, mettre à
jour l'IP dans les ACL (ou utiliser une IP virtuelle stable), aligner
les credentials, espacer le polling.

## 81. Cas n°3 — Les traps n'arrivent pas (alarmes en retard ou absentes)

**Symptômes** : eSight voit l'équipement en polling (up), mais les
pannes ne génèrent pas d'alarmes en temps réel — elles n'apparaissent
qu'au prochain cycle de polling, voire jamais.

**Diagnostic :**
1. Côté équipement : le **trap-target** pointe-t-il vers la bonne IP
   d'eSight (port UDP 162) ?
2. Un **firewall** filtre-t-il l'UDP 162 entre l'équipement et eSight ?
   (UDP = pas de connexion à tester au telnet ; utilisez un compteur
   ou une capture.)
3. Côté eSight : le service de réception des traps est-il démarré ?
4. L'équipement **génère-t-il** vraiment le trap ? Provoquer un événement
   de test (down/up d'un port de test) et observer.

**Solution :** corriger le trap-target / le firewall, redémarrer le
service de traps si nécessaire, puis valider avec un événement de test.
**Règle** : après chaque ajout d'équipement, le test trap fait partie
de la recette (section 31).

## 82. Cas n°4 — Tempête d'alarmes (storm)

**Symptômes** : des centaines d'alarmes en quelques minutes, console
inutilisable.

**Diagnostic :** identifier l'événement déclencheur (panne du cœur,
boucle réseau, flap d'un lien d'agrégation, redémarrage en cascade
après coupure électrique).

**Solution immédiate :**
1. Ne pas tout acquitter : **trier par heure** et identifier la
   première alarme (souvent la cause racine).
2. Appliquer la **corrélation** si configurée ; sinon, traiter manuellement
   la cause racine.
3. Si la tempête vient d'un équipement qui flap : **masquage temporaire**
   avec date de fin, le temps de réparer.

**Solution durable :** ajouter les règles de corrélation et d'agrégation
qui manquaient (sections 42, 47). Chaque tempête = une leçon documentée.

## 83. Cas n°5 — La topologie ne se met pas à jour

**Symptômes** : liens affichés up alors qu'ils sont down, équipements
fantômes, nouveaux liens absents.

**Diagnostic :**
1. La **découverte de topologie** (LLDPvoisinage) est-elle activée et
   planifiée ?
2. Le **LLDP est-il activé** sur les équipements (des deux côtés du lien) ?
3. Les traps linkUp/linkDown arrivent-ils (voir cas n°3) ?
4. Rafraîchissement manuel : forcer une resynchronisation de la vue.

**Solution :** activer LLDP partout (c'est aussi utile pour le
dépannage manuel), planifier la resynchronisation topologique
(quotidienne la nuit, par exemple), nettoyer les équipements
décommissionnés.

## 84. Cas n°6 — Fausse alerte « équipement down » (flapping de supervision)

**Symptômes** : eSight déclare un équipement injoignable alors qu'il
fonctionne (les utilisateurs ne voient rien).

**Diagnostic :**
1. **Perte de paquets** entre eSight et l'équipement ? (ping prolongé,
   MTR.) Un lien WAN chargé fait perdre les requêtes SNMP.
2. **Timeout trop court** dans le profil de découverte/polling.
3. L'équipement est-il **surchargé** (CPU à 100 % → il ne répond plus
   au SNMP) ? Dans ce cas l'alarme est un symptôme, pas un faux positif.
4. **IP dupliquée** sur le réseau ?

**Solution :** augmenter timeout/retries pour les sites distants,
traiter la cause réseau (QoS du trafic de management en priorité),
corriger le duplex/IP dupliquée. Ne jamais « régler » en allongeant
les timeouts à l'infini : ça masque les vrais problèmes.

## 85. Cas n°7 — Sauvegarde des configurations qui échoue

**Symptômes** : les tâches de backup de config passent en erreur sur
tout ou partie du parc.

**Diagnostic :**
1. Le compte **SSH/Telnet** utilisé par eSight est-il toujours valide
   (mot de passe expiré ? compte verrouillé après trop de tentatives ?).
2. L'accès SSH depuis l'IP d'eSight est-il autorisé (ACL vty) ?
3. L'équipement a-t-il changé d'IP ou de hostname ?
4. Espace disque côté eSight pour stocker les configs ?

**Solution :** compte de service **dédié** avec mot de passe
n'expirant pas (ou géré par rotation documentée), ACL vty autorisant
eSight, relance des tâches en échec après correction. **Alerte sur
l'échec des backups** : un backup qui échoue en silence pendant 6 mois,
c'est la catastrophe du jour où on en a besoin.

## 86. Cas n°8 — Les rapports sont vides ou incohérents

**Symptômes** : rapport de disponibilité à 0 % ou vide, chiffres
aberrants.

**Diagnostic :**
1. La **période** du rapport couvre-t-elle des données existantes ?
   (Rétention dépassée → données purgées → rapport vide.)
2. Les **équipements sources** étaient-ils bien supervisés sur la période ?
   (Un équipement découvert le 15 ne peut pas avoir de stats du 1er au 14.)
3. Les **seuils de disponibilité** sont-ils bien définis (qu'est-ce qui
   compte comme « down ») ?
4. Fuseau horaire du serveur vs équipements : un décalage fausse les
   fenêtres.

**Solution :** aligner périodes et rétention, vérifier la supervision
effective sur la période, synchroniser les horloges (NTP partout).

## 87. Cas n°9 — Les notifications mail/SMS ne partent plus

**Symptômes** : plus aucun e-mail/SMS d'alarme, alors que les alarmes
sont bien dans la console.

**Diagnostic :**
1. Le **relais SMTP** répond-il ? (IP changée ? authentification
   modifiée ? TLS exigé désormais ?)
2. La **passerelle SMS** est-elle joignable (crédit épuisé ? APN modifié ?).
3. Les **règles de notification** sont-elles toujours actives (pas
   désactivées lors d'une maintenance puis oubliées) ?
4. Les **groupes destinataires** sont-ils à jour (départs, changements
   de numéros) ?

**Solution :** corriger le connecteur, **tester mensuellement**
l'envoi réel (alarme de test → vérifier la réception sur le téléphone),
revoir les destinataires à chaque mouvement d'équipe.

## 88. Cas n°10 — eSight lent (console qui rame)

**Symptômes** : pages qui mettent 30 s à s'afficher, timeouts navigateur.

**Diagnostic :**
1. **Ressources serveur** : CPU, RAM (swap ?), disque (plein ? I/O
   saturés ?). Voir le System Monitor Tool.
2. **Base de données** : taille, fragmentation, requêtes lentes —
   le coupable n°1 (voir cas n°13).
3. **Trop d'alarmes courantes** non soldées (des dizaines de milliers
   d'alarmes actives ralentissent l'affichage : solder/nettoyer).
4. **Sessions** : trop de consoles ouvertes simultanément ?
5. Antivirus qui scanne les répertoires eSight/base en temps réel.

**Solution :** selon la cause — ajouter RAM, purger les alarmes soldées,
exclure les répertoires de l'antivirus temps réel, redimensionner la VM.
Si ça persiste : **Fault Information Collection Tool** → package pour
le support.

## 89. Cas n°11 — Un service eSight ne démarre plus

**Symptômes** : après reboot ou mise à jour, un service reste en erreur,
la console est partiellement ou totalement inaccessible.

**Diagnostic :**
1. Consulter les **logs** du service (répertoire de logs eSight).
2. La **base de données** est-elle démarrée et accessible ? (La plupart
   des services eSight en dépendent : base down = tout down.)
3. **Port déjà utilisé** par un autre processus ?
4. **Espace disque** plein (les logs eux-mêmes peuvent remplir le disque).
5. Changement récent (patch OS, mise à jour) : quoi, quand ?

**Solution :** libérer le disque, démarrer la base d'abord puis les
services dans l'ordre, restaurer la sauvegarde pré-changement si
nécessaire. **Toujours snapshoter avant une mise à jour** (section 16).

## 90. Cas n°12 — Base de données pleine / croissance anormale

**Symptômes** : alertes d'espace disque, eSight qui se fige, sauvegardes
qui échouent.

**Diagnostic :**
1. Qu'est-ce qui grossit ? Données de **performance** (pas de collecte
   trop fin sur trop d'indicateurs), **alarmes historiques** jamais
   purgées, **logs** applicatifs, **fichiers de collecte** oubliés.
2. Vérifier les **politiques de rétention/purge** : sont-elles actives ?
3. Un équipement qui **flap** génère des milliers d'alarmes/jour :
   traiter la cause (section 45), pas seulement purger.

**Solution :** purger selon la politique (jamais de DELETE sauvage en
SQL direct), ajuster la rétention et le pas de collecte, archiver les
données anciennes avant purge si besoin réglementaire, étendre le disque.
**Prévention** : supervision de l'espace disque d'eSight lui-même
(voir section 16 — eSight doit se superviser).

## 91. Cas n°13 — Performances dégradées : la base rame

**Symptômes** : lenteurs générales malgré des ressources serveur correctes.

**Diagnostic :**
1. Taille de la base vs RAM : si la base active ne tient plus en mémoire,
   chaque requête va au disque.
2. **Index/fragmentation** : une base qui n'a jamais été maintenue
   (rebuild d'index, statistiques) se dégrade avec le temps.
3. Rétention trop longue pour le dimensionnement (voir cas n°12).
4. Requêtes concurrentes : rapports lourds lancés en pleine journée +
   polling + consoles utilisateurs.

**Solution :** maintenance de la base (selon le SGBD : rebuild index,
mise à jour des stats — **via les outils fournis ou avec l'aval du
support**, jamais d'opération exotique), déplacer les gros rapports la
nuit, réduire la rétention ou augmenter les ressources. Si récurrent :
revoir le dimensionnement (section 3) avec le partenaire.

## 92. Cas n°14 — Après une coupure électrique : tout est rouge

**Symptômes** : au retour du courant, des centaines d'alarmes, des
équipements qui ne reviennent pas dans eSight.

**Diagnostic :** distinguer le **vrai** du **fantôme** :
1. Quels équipements sont réellement joignables (ping) ?
2. Lesquels ne sont pas repartis (alim, boot bloqué) ?
3. Les alarmes « équipement down » d'équipements qui sont en fait UP =
   traps perdus pendant la coupure → **synchroniser** (section 48).

**Solution :** traiter les vrais down (intervention terrain), synchroniser
les alarmes pour solder les fantômes, vérifier que les sauvegardes de
config planifiées n'ont pas tourné dans le vide pendant la coupure.
**Leçon** : ce scénario se prépare — runbook « retour de coupure »
(section 15) et onduleurs supervisés (votre domaine !).

## 93. Cas n°15 — Changement d'IP du serveur eSight : tout casse

**Symptômes** : après migration/changement d'IP du serveur, plus de
traps, SNMP en échec, console injoignable à l'ancienne adresse.

**Diagnostic :** recenser tout ce qui référençait l'ancienne IP :
trap-targets des équipements, ACL SNMP, règles firewall, favoris des
exploitants, relais, supervision d'eSight lui-même, DNS.

**Solution :** mettre à jour **partout** (c'est un chantier : script
de déploiement de la nouvelle IP en trap-target via le Smart Config
Tool, section 60), mettre à jour le DNS, communiquer la nouvelle URL.
**Prévention** : utiliser un **nom DNS** partout où c'est possible
plutôt que l'IP en dur, et documenter l'IP comme paramètre critique
du dossier d'exploitation. Idéalement : ne jamais changer l'IP d'un
NMS en production.

## 94. Cas n°16 — Compte verrouillé / plus d'accès admin

**Symptômes** : personne ne peut se connecter en admin (mot de passe
perdu, compte verrouillé après tentatives, AD indisponible).

**Diagnostic :** s'agit-il du compte local ou de l'authentification
annuaire ? L'AD répond-il ?

**Solution :**
- Si AD en panne : utiliser le **compte admin local de secours**
  (celui du coffre-fort, section 68).
- Si mot de passe local perdu : procédure de réinitialisation
  constructeur (**à vérifier sur la documentation officielle** —
  généralement via accès console au serveur eSight).
- Après récupération : changer les mots de passe, vérifier les
  verrouillages, consigner au coffre.
- **Prévention** : deux détenteurs du coffre-fort, jamais un seul.

## 95. Cas n°17 — Montée de version qui se passe mal (rollback)

**Symptômes** : après upgrade, fonctionnalités cassées, services
instables, données manquantes.

**Diagnostic :** identifier ce qui a changé (notes de version lues ?
prérequis vérifiés ? backup pré-upgrade existant ?).

**Solution :**
1. Ne pas « réparer » à chaud pendant des heures : si le rollback est
   possible (snapshot VM, sauvegarde pré-upgrade), **revenir en arrière**
   proprement — c'est pour ça qu'on snapshotte.
2. Restaurer la sauvegarde pré-upgrade (section 71).
3. Analyser à froid : cause de l'échec, prérequis manqué, puis
   replanifier avec le partenaire/support.
4. **Règle** : jamais d'upgrade en production sans : snapshot + sauvegarde
   testée + procédure de rollback écrite + fenêtre de maintenance +
   binôme.

---

# 15. BONNES PRATIQUES D'EXPLOITATION

## 96. Organiser l'exploitation autour d'eSight

- **Le NOC vit dans eSight** : topologie + alarmes courantes = écran
  principal. Tout le reste (tickets, docs) gravite autour.
- **Rôles clairs** : qui surveille (NOC), qui traite (niveau 2/3),
  qui décide (chef de service). eSight reflète cette organisation
  dans ses rôles et domaines (section 11).
- **La console n'est pas un mur d'alarmes** : si le NOC voit 200 alarmes
  non acquittées en permanence, le système est mal réglé (revoir la
  politique d'alarmes, section 50) — pas l'équipe qui est négligente.
- **eSight ne remplace pas le terrain** : une alarme « température haute »
  dans un local technique se termine par quelqu'un qui va vérifier la
  clim. Le NMS détecte, l'humain constate et agit.

## 97. Astreinte : que l'outil serve l'astreinte, pas l'inverse

- L'astreinte ne reçoit que le **critical** (SMS) + le **major** (mail).
  Tout le reste attend le lendemain 8h.
- **Procédure d'astreinte écrite** : réception du SMS → connexion VPN →
  console eSight → acquitter avec commentaire → appliquer le runbook →
  escalader si besoin. Un astreintier qui improvise à 3h du matin,
  c'est un incident qui dure 3 fois plus longtemps.
- **Test mensuel** du SMS d'astreinte (section 41).
- **Rotation** : personne ne doit être le seul à savoir lire eSight.
  La formation des nouveaux inclut « lire une alarme et l'acquitter »
  dès la première semaine.

## 98. Runbooks : un par alarme critique

Un **runbook** = la fiche réflexe pour une alarme donnée :

```markdown
# RUNBOOK — Alarme : Lien backbone SITE-A ↔ SITE-B down (Critical)

## Symptômes dans eSight
- Alarme Critical « linkDown » sur l'interface concernée + alarmes
  corrélées « équipements injoignables » sur SITE-B.

## Vérifications (dans l'ordre)
1. Topologie : le lien est-il vraiment down ? (pas un flap : voir
   l'historique de l'interface sur 1h)
2. Équipements d'extrémité : joignables en SSH ? (si oui : problème
   de lien ; si non : problème d'équipement ou d'énergie)
3. Fournisseur : incident connu sur la liaison ? (numéro du support
   dans le PRA)

## Actions
- Si flap : masquage temporaire + ticket + intervention planifiée.
- Si down franc : bascule sur le lien de secours (procédure PRA),
  ticket fournisseur, information des utilisateurs.
- Acquitter l'alarme avec le n° de ticket.

## Escalade
- Si non résolu en 1h → chef de service. Si impact utilisateurs
  majeur → cellule de crise.

## Retour d'expérience
- Compléter le journal d'expérience eSight après clôture.
```

**Objectif** : runbook pour les **10 alarmes les plus fréquentes**
d'abord, puis élargir. Un runbook se teste (exercice à blanc) et se
met à jour après chaque incident réel.

## 99. KPI d'exploitation : piloter avec eSight

Tableau de bord mensuel du chef de service (alimenté par les rapports
eSight, section 55) :

| KPI | Cible indicative | Source |
|---|---|---|
| Disponibilité par site | ≥ 99,5 % (à définir par contrat) | Rapport SLA |
| Alarmes critical/major par mois | Tendance à la baisse | Rapport alarmes |
| Délai moyen d'acquittement (critical) | ≤ 15 min | Logs d'alarmes |
| % d'alarmes avec runbook associé | 100 % pour critical | Revue manuelle |
| Équipements non supervisés (hors parc) | 0 | Inventaire vs parc réel |
| Succès des sauvegardes de config | 100 % | Rapport tâches |
| Succès des sauvegardes eSight | 100 % | Contrôle backup |

**Utilisation** : ces KPI vont au COPIL, justifient les investissements
(« les liens à 85 % : il faut upgrader ») et mesurent le progrès de
l'équipe. Un KPI qui ne déclenche aucune décision est un KPI à supprimer.

## 100. Rituels d'équipe autour de l'outil

- **Quotidien (5 min)** : revue des alarmes de la nuit par le NOC,
  point à la prise de poste.
- **Hebdomadaire (30 min)** : revue des masquages actifs, des alarmes
  récurrentes, des backups en échec.
- **Mensuel (1 h)** : comité alarmes (politique d'alarmes, section 50),
  revue des KPI, plan d'action (seuils à ajuster, équipements à traiter).
- **Trimestriel** : nettoyage topologie + inventaire, revue des comptes,
  test de restauration (annuel pour la restauration complète).
- **Annuel** : test PRA, revue du dimensionnement, renouvellement SnS,
  plan de formation.

---

# 16. MAINTENANCE

## 101. Plan de maintenance annuel type

| Période | Actions |
|---|---|
| Quotidien | Revue alarmes nuit ; contrôle succès sauvegardes (eSight + configs) ; espace disque eSight |
| Hebdomadaire | Revue masquages ; alarmes récurrentes ; patchs OS critiques en attente |
| Mensuel | Comité alarmes ; test notifications SMS/mail ; Environmental Health Check ; revue logs d'audit (échantillon) ; exports inventaire |
| Trimestriel | Nettoyage topologie/inventaire ; revue comptes et rôles ; vérification rétention/purge ; test d'un runbook à blanc |
| Semestriel | Revue des seuils ; revue du dimensionnement ; exercice PRA partiel |
| Annuel | **Test de restauration complet** ; test PRA grandeur nature ; renouvellement licences/SnS ; plan de formation ; revue d'architecture |

## 102. Check-list quotidienne (NOC — 10 minutes)

- [ ] Alarmes critical/major non acquittées : 0 (ou chacune a un ticket).
- [ ] Équipements injoignables : liste connue et justifiée ?
- [ ] Sauvegarde eSight de la nuit : succès ?
- [ ] Sauvegardes de configs : 0 échec (ou échecs traités) ?
- [ ] Espace disque du serveur eSight : < 80 % ?
- [ ] Services eSight : tous démarrés ?

## 103. Check-list mensuelle (admin eSight — 1 heure)

- [ ] Environmental Health Check : tout au vert ?
- [ ] Test réel des notifications (SMS + mail) : reçus ?
- [ ] Revue des règles de masquage : encore justifiés ? dates de fin ?
- [ ] Alarmes les plus fréquentes du mois : top 10 extrait, actions ?
- [ ] Comptes : mouvements (arrivées/départs) pris en compte ?
- [ ] Espace disque et croissance de la base : tendance OK ?
- [ ] Rapports programmés : bien générés et envoyés ?
- [ ] Documentation : changements du mois consignés ?

## 104. Mises à jour et patchs : la méthode sans stress

1. **Lire les notes de version** : prérequis, incompatibilités,
   changements de comportement (surtout sur les API northbound et les
   MIB).
2. **Vérifier la compatibilité** avec vos modules et votre version de base.
3. **Maquette d'abord** : rejouer la mise à jour sur un clone/lab
   avant la production.
4. **Fenêtre de maintenance** : informer (la supervision sera
   indisponible — prévoir une surveillance manuelle minimale pendant
   l'opération).
5. **Snapshot + sauvegarde** juste avant (section 70-71).
6. **Appliquer**, vérifier les services, rejouer les tests (login,
   découverte test, alarme test, notification test).
7. **Garder le snapshot** quelques jours avant de le consolider.
8. Documenter : version avant/après, date, incidents rencontrés.

**Fréquence** : patchs de sécurité sans tarder (un NMS exposé avec des
accès SNMP/SSH sur tout le parc est une cible de choix) ; montées de
version fonctionnelles 1-2 fois par an, jamais « parce qu'une nouvelle
version existe ».

## 105. Superviser eSight lui-même

Paradoxe classique : l'outil qui supervise tout n'est supervisé par
personne. Corrigez ça :

- **Supervision externe** : un outil tiers (ou un second eSight léger,
  ou même un simple script) qui ping eSight, teste le port HTTPS et
  vérifie la fraîcheur des données (dernière alarme reçue il y a
  moins de X minutes).
- **Alerte sur les sauvegardes** : échec de backup = alerte immédiate.
- **Espace disque, CPU, RAM** du serveur eSight dans votre supervision
  système habituelle.
- **Canal d'alerte indépendant** : si eSight tombe, c'est un autre
  système qui prévient l'astreinte (sinon : silence radio pendant
  la panne — le pire scénario).

## 106. Le Fault Information Collection Tool : préparer le support

Quand un problème dépasse vos compétences, le support Huawei aura besoin
d'un **package de diagnostic**. eSight fournit le **Fault Information
Collection Tool** (icône eSight Console → Tools) :

1. Le lancer, renseigner les paramètres demandés, cliquer Collect.
2. Récupérer le package généré (noter le **mot de passe de décompression**
   affiché — sans lui, le support ne peut pas l'ouvrir).
3. Transmettre au support avec : version eSight exacte, description du
   problème, heure de survenue, ce qui a déjà été tenté.

**Réflexe** : générer ce package **avant** de bidouiller quand un
problème sérieux survient — les logs d'origine valent de l'or pour
le diagnostic.

---

# 17. POUR ALLER PLUS LOIN

## 107. Documentation officielle : où chercher

- **Portail support Huawei** (support.huawei.com) : guides d'installation,
  d'exploitation et notes de version **de votre version exacte** —
  c'est LA référence, ce guide ne la remplace pas.
- **Brochure eSight 23.1** (mars 2024) : vision produit et périmètre
  des composants.
- **Datasheet eSight** : dimensionnement et éditions.
- **Aide en ligne** intégrée à la console eSight : souvent plus à jour
  que les PDF pour les chemins de menus.
- Mention obligatoire : toute procédure sensible (cluster, restauration,
  upgrade) se fait **avec la doc de votre version sous les yeux**.

## 108. Formation et certification

- Parcours officiels Huawei (HCIA/HCIP) : utiles pour les fondamentaux
  réseau, moins pour eSight spécifiquement — la formation eSight se fait
  surtout via les **partenaires** (formations produit).
- **Formation interne** : le meilleur investissement. Programme type
  pour un nouvel exploitant : 1/2 journée « lire eSight » (topo, alarmes,
  acquittement), 1 journée « administrer » (découverte, seuils, rapports),
  compagnonnage d'un mois avec un senior.
- **Exercices** : alarmes simulées en labo, tempête simulée, restauration
  à blanc — on apprend eSight en le cassant (en labo).

## 109. Communauté et retours d'expérience

- Échanges entre exploitants (forums, groupes d'utilisateurs) : les
  problèmes eSight sont souvent les mêmes partout (découverte SNMP,
  base qui grossit, storms) — les solutions aussi.
- Capitaliser en interne : **journal d'expérience** des alarmes
  (section 40), runbooks (section 98), et ce guide annoté de vos
  spécificités locales.
- Rester attentif à la trajectoire **iMaster NCE** : suivre les
  annonces Huawei via votre partenaire pour anticiper la migration
  (section 5).

---

# 18. PENSE-BÊTE DE POCHE

## 110. Mémo protocoles et ports

| Protocole | Port | Sens | À retenir |
|---|---|---|---|
| SNMP poll | UDP 161 | eSight → équipement | Timeouts ? Vérifier ACL + routage |
| SNMP traps | UDP 162 | Équipement → eSight | Pas de traps = pas d'alarmes temps réel |
| Syslog | UDP 514 | Équipement → eSight | Complète les traps |
| NetStream | UDP (configuré) | Équipement → eSight | NTA, volumineux |
| SSH | TCP 22 | eSight → équipement | Backup de configs |
| Telnet | TCP 23 | eSight → équipement | À éviter si possible (clair) |
| HTTPS console | TCP (selon version) | Navigateur → eSight | **À vérifier sur la documentation officielle** |
| SMTP | TCP 25/587 | eSight → relais | Notif mail |
| ICMP | — | eSight → équipement | Test de joignabilité |

## 111. Mémo cycle de vie des alarmes

**Courante → (acquittement) → Acquittée → (clear) → Historique.**
Règles : on acquitte avec un commentaire/ticket ; on ne masque qu'avec
une date de fin ; le storm se traite par la cause racine, pas en
acquittant tout.

## 112. Mémo sauvegardes

- **eSight lui-même** : base quotidienne + système hebdo → stockage externe.
- **Configs équipements** : quotidienne + à chaque changement → via eSight.
- **Tester la restauration** 1×/an. Une sauvegarde non testée n'existe pas.
- **Licences** archivées dans le PRA.

---

# 19. LES 20 PIÈGES CLASSIQUES

## 113. Les 20 erreurs à ne pas commettre

1. **Community SNMP « public » en lecture sur tout le parc** — avec des
   ACL inexistantes. N'importe qui cartographie votre réseau.
2. **SNMP v2c là où v3 est possible** — par paresse. Le trafic de
   supervision en clair sur le réseau de management.
3. **Trap-target oublié** sur les nouveaux équipements — eSight ne voit
   les pannes qu'au polling suivant (ou jamais).
4. **Timeouts trop courts** vers les sites distants — fausses alertes
   « down » en série, puis on ne croit plus les alertes.
5. **Mot de passe SNMP v3 changé** en masse sans mettre à jour le profil
   eSight — découverte morte du jour au lendemain.
6. **Changement d'IP du serveur eSight** sans recenser les références
   (trap-targets, ACL, firewall, DNS) — tout casse (cas n°15).
7. **Aucune sauvegarde testée** — on découvre le jour du sinistre que
   les backups échouaient depuis 4 mois.
8. **Base jamais purgée** — eSight rame, puis se fige (cas n°12-13).
9. **Notifications jamais testées** — le relais SMTP a changé il y a
   6 mois, personne n'est prévenu depuis.
10. **Tout le monde admin** — 12 comptes administrators, zéro traçabilité.
11. **Acquittement sans commentaire** — « quelqu'un s'en occupe »…
    personne ne s'en occupe.
12. **Masquages permanents** — des pannes invisibles pendant des mois.
13. **200 alarmes non acquittées en permanence** — le NOC ne regarde plus
    la console. Recalibrer (section 50).
14. **Topologie jamais nettoyée** — équipements fantômes, carte mensongère.
15. **Pas de runbook** — chaque incident se réinvente à 3h du matin.
16. **eSight non supervisé** — quand il tombe, silence radio général.
17. **Upgrade sans snapshot ni rollback** — la mise à jour qui transforme
    un vendredi en week-end de crise (cas n°17).
18. **Modules installés ≠ modules licenciés** — fonctionnalités en erreur
    au démarrage, diagnostic obscur.
19. **Rétention infinie des données de performance** — disque plein,
    rapports vides, eSight à genoux.
20. **Investir dans eSight sans regarder iMaster NCE** — déployer un NMS
    classique aujourd'hui sans feuille de route 3-5 ans, c'est s'exposer
    à une migration subie (section 5).

---

# 20. GLOSSAIRE

## 114. Glossaire eSight / supervision

- **AC (Access Controller)** : contrôleur Wi-Fi qui pilote les points d'accès.
- **Acquittement** : prise en charge d'une alarme par un exploitant
  (« je m'en occupe ») — n'éteint pas le défaut.
- **Agrégation** : regroupement d'alarmes répétitives identiques en une
  seule alarme avec compteur.
- **AP (Access Point)** : point d'accès Wi-Fi.
- **B/S (Browser/Server)** : architecture où l'administration se fait
  via navigateur, sans client lourd.
- **Clear** : fin d'une alarme (le défaut a disparu).
- **Community SNMP** : « mot de passe » partagé de SNMP v1/v2c (faible
  sécurité — préférer SNMP v3).
- **Corrélation** : rattachement automatique d'alarmes conséquences à
  une alarme cause racine.
- **FCAPS** : les 5 domaines de la gestion réseau — Fault, Configuration,
  Accounting, Performance, Security.
- **Flapping** : bascule répétée up/down d'un lien ou d'un équipement.
- **Hot standby** : secours à chaud — un nœud passif prêt à prendre le
  relais immédiatement.
- **IPFIX** : standard d'export de flux IP (NetStream en est proche).
- **Masquage** : inhibition temporaire de la remontée d'alarmes
  (maintenance, dérangement connu) — toujours avec date de fin.
- **MIB** : base d'information de management — dictionnaire des objets
  supervisés via SNMP.
- **MPLS VPN** : VPN d'opérateur basés sur MPLS (supervision en édition
  Standard/Professional).
- **NE (Network Element)** : équipement réseau supervisé.
- **NetStream** : technologie Huawei d'export de flux de trafic
  (équivalent NetFlow), exploitée par le NTA d'eSight.
- **NMS** : Network Management System — système de supervision réseau.
- **Northbound** : interface d'eSight vers un système de niveau supérieur.
- **NTA (Network Traffic Analyzer)** : analyse du trafic dans eSight
  (édition Standard+).
- **Polling** : interrogation périodique des équipements par eSight.
- **PRA** : Plan de Reprise d'Activité.
- **RPO / RTO** : perte de données max acceptable / durée max de
  reprise — objectifs du PRA.
- **Sévérité** : criticité d'une alarme (Critical, Major, Minor,
  Warning, Info).
- **SLA** : accord de niveau de service — objectifs mesurés (disponibilité…).
- **SNMP** : Simple Network Management Protocol — protocole de supervision.
- **Southbound** : interface d'eSight vers les équipements supervisés.
- **Storm (tempête d'alarmes)** : avalanche d'alarmes suite à un
  incident majeur.
- **Syslog** : protocole de remontée de journaux d'événements.
- **Toggling** : alarme intermittente qui apparaît/disparaît en boucle.
- **Trap** : notification spontanée envoyée par l'équipement vers eSight.
- **v3 authPriv** : mode SNMP v3 avec authentification + chiffrement
  (le niveau à viser).

---

# 21. QUIZ

## 115. Quiz : 10 questions pour valider

**Q1.** Quelles sont les quatre fonctions historiques (FCAPS partielles)
assurées par eSight ?
**Q2.** Quelle édition minimale faut-il pour la gestion WLAN et le NTA ?
**Q3.** En SNMP, quelle est la différence fondamentale entre v2c et v3,
et lequel faut-il privilégier ?
**Q4.** Un équipement n'est pas découvert alors qu'il répond au ping.
Citez trois vérifications dans l'ordre.
**Q5.** Que signifie « acquitter » une alarme, et que ne signifie-t-il pas ?
**Q6.** À quoi servent respectivement l'agrégation, le masquage et la
corrélation des alarmes ?
**Q7.** Pourquoi faut-il une date de fin à tout masquage d'alarme ?
**Q8.** Que sauvegarder pour pouvoir reconstruire eSight après sinistre ?
**Q9.** Que faire en premier face à une tempête d'alarmes ?
**Q10.** eSight est-il en fin de vie face à iMaster NCE ? Que répondre
honnêtement à votre direction ?

### Réponses

**R1.** Fault management (alarmes), Performance management (indicateurs,
tableaux de bord, rapports), Topology management (cartographie),
Configuration management (sauvegarde/comparaison/restauration des configs).
**R2.** L'édition **Standard** (WLAN management et NTA/NetStream y sont
inclus ; la Compact ne les a pas).
**R3.** v2c utilise une community string en clair (authentification faible,
pas de chiffrement) ; v3 apporte authentification et chiffrement
(authPriv). **Privilégier v3 partout où c'est possible.**
**R4.** (1) L'agent SNMP est-il activé sur l'équipement ? (2) La
community / l'utilisateur v3 correspond-il au profil eSight (tester au
snmpwalk) ? (3) Une ACL côté équipement bloque-t-elle l'IP d'eSight ?
**R5.** Acquitter = prendre en charge (« je m'en occupe », avec commentaire
et ticket). Ça n'éteint PAS le défaut et ne le résout pas.
**R6.** Agrégation : regrouper les alarmes répétitives identiques pour
réduire le bruit. Masquage : taire temporairement des alarmes connues
(maintenance). Corrélation : rattacher les alarmes conséquences à la
cause racine.
**R7.** Parce qu'un masquage permanent rend la panne invisible : sans date
de fin, on oublie le masquage et on ne voit plus jamais les alarmes de
cet équipement.
**R8.** La base de données (outil Database Backup & Restore), le système
eSight, les fichiers/actes de licence, et le dossier d'exploitation —
le tout externalisé, avec un test de restauration annuel.
**R9.** Ne pas tout acquitter en panique : trier par heure, identifier la
première alarme (cause racine probable), s'appuyer sur la corrélation,
traiter la cause — les conséquences se solderont en cascade.
**R10.** Réponse honnête : eSight est **toujours commercialisé** (version
23.1 en mars 2024) et reste pertinent pour superviser un parc existant ;
iMaster NCE est la **plateforme stratégique d'avenir** de Huawei
(SDN, automatisation, IA). Aucune date officielle de fin de vie d'eSight
n'est publiée (**à vérifier sur la documentation officielle** / auprès
du partenaire) : on investit dans eSight en gardant des processus
indépendants de l'outil et une feuille de route 3-5 ans vers NCE.

---

# ANNEXE A — RECETTE DE MISE EN SERVICE

## 116. Recette : périmètre et méthode

La **recette** (acceptation) est la phase qui valide que l'installation
d'eSight est conforme au besoin avant de la déclarer « en production ».
Sans recette écrite et signée, les oublis se découvrent en incident.

**Méthode :**
- Recette **par scénarios** (pas par cases à cocher abstraites) : chaque
  scénario décrit une action, le résultat attendu et le résultat constaté.
- Recette **contradictoire** : l'intégrateur déroule, l'équipe interne
  valide et signe.
- **Critère GO/NO-GO** : tout scénario « bloquant » en échec = pas de
  mise en production.

## 117. Recette : infrastructure eSight

| # | Scénario | Résultat attendu |
|---|---|---|
| R-INF-01 | Redémarrer le serveur eSight | Tous les services remontent seuls en < 15 min, console accessible |
| R-INF-02 | Environmental Health Check | 100 % des contrôles au vert |
| R-INF-03 | Couper le réseau 2 min puis rétablir | eSight resynchronise seul, pas d'alarmes fantômes persistantes après synchro manuelle |
| R-INF-04 | Tester l'URL depuis un poste NOC, un poste VPN, une tablette | HTTPS OK partout, certificat reconnu (pas d'avertissement) |
| R-INF-05 | Simuler un disque à 85 % (ou vérifier les seuils) | Alerte espace disque remontée |
| R-INF-06 | Vérifier NTP | Écart < 1 s avec la référence |

## 118. Recette : découverte et supervision

| # | Scénario | Résultat attendu |
|---|---|---|
| R-DISC-01 | Découverte d'un switch pilote en SNMP v3 | Inventaire correct : modèle, version, interfaces |
| R-DISC-02 | Provoquer un down/up de port sur le pilote | Trap reçu en < 60 s, alarme visible avec la bonne sévérité |
| R-DISC-03 | Couper le polling (ACL test) 10 min | Alarme « SNMP timeout » puis retour à la normale au rétablissement |
| R-DISC-04 | Vérifier un équipement tiers (ex. imprimante) | A minima up/down + état générique |
| R-DISC-05 | Contrôler l'absence de doublons | Chaque équipement physique = 1 objet dans l'inventaire |
| R-DISC-06 | Sauvegarde de config du pilote | Fichier récupéré, contenu conforme à la config réelle |

## 119. Recette : alarmes et notifications

| # | Scénario | Résultat attendu |
|---|---|---|
| R-ALM-01 | Générer une alarme Critical de test | SMS reçu par l'astreinte en < 5 min avec équipement + libellé |
| R-ALM-02 | Générer une alarme Major de test | E-mail reçu par le groupe, pas de SMS |
| R-ALM-03 | Acquitter avec commentaire + ticket fictif | Alarme acquittée, commentaire visible, traçabilité en audit |
| R-ALM-04 | Simuler un flap (down/up × 5) | Agrégation : 1 alarme avec compteur (pas 10 alarmes) |
| R-ALM-05 | Activer un masquage de test avec date de fin | Pas de remontée pendant le masquage, retour auto après échéance |
| R-ALM-06 | Vérifier le filtrage northbound (si applicable) | Seules les sévérités configurées remontent au NMS supérieur |

## 120. Recette : sauvegardes, comptes et documentation

| # | Scénario | Résultat attendu |
|---|---|---|
| R-BKP-01 | Sauvegarde complète eSight | Fichier généré sur le stockage externe, taille cohérente |
| R-BKP-02 | Restauration sur VM isolée | Console fonctionnelle, inventaire et paramétrage retrouvés |
| R-ACC-01 | Connexion avec chaque rôle | Droits conformes à la matrice (un read-only ne peut pas acquitter) |
| R-ACC-02 | Couper l'AD (si intégré) | Compte local de secours fonctionnel |
| R-DOC-01 | Dossier d'exploitation livré | Schéma, IP, comptes (coffre), procédures, licences — complet et à jour |

**Signature** : recette signée par le chef de service et l'intégrateur,
archivée dans le dossier d'exploitation. C'est le « top départ » officiel.

---

# ANNEXE B — FICHES RÉFLEXES PAR TYPE D'ÉQUIPEMENT

> Que superviser en priorité sur chaque famille d'équipements, et avec
> quels seuils de départ. À affiner par modèle.

## 121. Fiche réflexe : switch d'accès

**À superviser :** état up/down, CPU, mémoire, température, alimentations,
utilisation des ports montants (uplinks), budget **PoE** utilisé/disponible,
erreurs sur les ports.

**Seuils de départ :** uplink > 70 % = warning (tendance) ; CPU > 80 %
persistant = major ; 1 alim sur 2 HS = major ; budget PoE > 90 % = warning
(plus de marge pour ajouter des AP/téléphones).

**Alarmes à ne jamais masquer :** uplink down, équipement injoignable.

## 122. Fiche réflexe : switch cœur / agrégation

**À superviser :** tout ce du switch d'accès, plus : état des membres
de stack/cluster, température par carte, protocoles de routage (voisins
OSPF/BGP), files QoS (drops), spanning-tree (changements de topologie).

**Seuils de départ :** CPU > 70 % = major (un cœur ne doit jamais être
tendu) ; perte d'un membre de stack = major ; changement de root STP =
warning à investiguer.

**Particularité :** chaque alarme sur le cœur est potentiellement un
début de tempête (section 46) — corrélation obligatoire vers les
équipements d'accès qui en dépendent.

## 123. Fiche réflexe : routeur / firewall

**Routeur — à superviser :** état des interfaces WAN, voisins de routage,
CPU (le routage + NetStream coûtent cher), mémoire, tunnels (VPN/IPSec :
état up/down en édition Standard).

**Firewall — à superviser :** CPU, mémoire, sessions (table de sessions
pleine = nouveaux flux rejetés), débit, état du cluster HA (bascule =
major), journaux d'attaques (à corréler, pas à alerter un par un).

**Seuils de départ :** sessions > 80 % de la capacité = warning ;
bascule HA = major + SMS ; tunnel VPN down = major (critical si c'est
le seul lien d'un site).

## 124. Fiche réflexe : AP et contrôleur Wi-Fi (AC)

**À superviser :** AP en ligne/hors ligne par site, charge (utilisateurs
simultanés par AP), santé de l'AC (CPU, licences), interférences signalées.

**Seuils de départ :** > 10 % des AP d'un site hors ligne = major ;
AP avec > 30 utilisateurs = warning (densifier) ; AC CPU > 70 % = major.

**Réflexe dépannage :** « le Wi-Fi ne marche pas » → d'abord vérifier
dans eSight si les AP sont en ligne et si le switch PoE qui les alimente
va bien (souvent un problème d'énergie, pas radio — section 64).

## 125. Fiche réflexe : serveur et stockage

**Serveur (module server device management) — à superviser :** CPU,
mémoire, espace disque, état du matériel (disques RAID, alimentations,
ventilateurs), OS (services critiques).

**Stockage (OceanStor…) — à superviser :** capacité utilisée/disponible,
IOPS et latence, état des disques et contrôleurs, fonctions de protection
(anti-ransomware selon version).

**Seuils de départ :** disque > 80 % = warning, > 90 % = critical ;
disque RAID en défaut = major ; contrôleur down = critical.

---

# ANNEXE C — AUTOMATISATION AUTOUR D'ESIGHT

> eSight s'administre à la console, mais tout ce qui est **répétitif et
> vérifiable** gagne à être scripté à côté. Exemples en bash (à adapter).

## 126. Script : audit SNMP du parc avant découverte

Avant de lancer eSight sur un parc inconnu, auditer qui répond en SNMP.
Exemple fictif (nécessite `snmpwalk` ; community d'exemple à remplacer) :

```bash
#!/bin/bash
# audit_snmp.sh — teste la réponse SNMP d'une liste d'IP
# Usage : ./audit_snmp.sh liste_ip.txt
COMMUNAUTE="COMMUNAUTE_DE_TEST"   # exemple fictif — mettre la vraie
while read -r ip; do
  if snmpwalk -v2c -c "$COMMUNAUTE" -t 2 -r 1 "$ip" sysName.0 >/dev/null 2>&1; then
    echo "OK   $ip"
  else
    echo "KO   $ip"
  fi
done < "$1"
```

**Usage :** les KO alimentent la check-list de préparation (section 27-28)
avant la découverte eSight. On ne découvre que du OK.

## 127. Script : vérification des trap-targets

Vérifier que chaque équipement Huawei pointe bien vers eSight pour les
traps. Principe (via SSH en boucle, ou via le Smart Configuration Tool
d'eSight) : lire la configuration des `snmp-agent target-host` et comparer
à l'IP d'eSight. Tout écart = trap-target à corriger (voir cas n°3).

**Automatiser** : rejouer ce contrôle **tous les mois** — les
trap-targets « s'évaporent » lors des remplacements d'équipements ou des
restaurations de vieilles configs.

## 128. Script : contrôle des sauvegardes eSight

```bash
#!/bin/bash
# check_backup_esight.sh — alerte si pas de backup récent
# À brancher sur votre supervision système (appel quotidien)
REPERTOIRE_BACKUP="/mnt/sauvegardes/esight"
DELAI_MAX_HEURES=30
dernier=$(find "$REPERTOIRE_BACKUP" -type f -name '*.bak' -o -name '*.zip' \
  | xargs ls -t 2>/dev/null | head -1)
if [ -z "$dernier" ]; then
  echo "CRITICAL - aucun fichier de sauvegarde eSight trouve"
  exit 2
fi
age_heures=$(( ( $(date +%s) - $(stat -c %Y "$dernier") ) / 3600 ))
if [ "$age_heures" -gt "$DELAI_MAX_HEURES" ]; then
  echo "CRITICAL - derniere sauvegarde eSight il y a ${age_heures}h ($dernier)"
  exit 2
fi
echo "OK - derniere sauvegarde eSight il y a ${age_heures}h"
exit 0
```

**Règle** : ce script (ou équivalent) tourne **tous les jours** et remonte
vers votre supervision système — jamais de confiance aveugle dans la
planification eSight (voir cas n°7).

## 129. Inventaire : rapprocher eSight du parc réel

**Le rituel trimestriel :**
1. Exporter l'inventaire eSight (CSV).
2. Le rapprocher de la **source de vérité du parc** (CMDB, tableau
   d'immobilisations, inventaire physique).
3. Traiter les écarts : équipements dans eSight mais réformés (à
   supprimer), équipements réels absents d'eSight (à découvrir),
   modèles/versions divergents (à resynchroniser).
4. Consigner le rapprochement (date, écarts, actions).

Un inventaire eSight qui dérive du réel, c'est des licences payées pour
rien et des équipements non supervisés — les deux en même temps.

## 130. Superviser eSight depuis l'extérieur (sonde minimale)

Avec n'importe quel outil (même un cron + curl sur une autre machine) :

- **Ping** du serveur eSight toutes les minutes.
- **Test HTTPS** : la page de login répond-elle en < 5 s ?
- **Fraîcheur des données** : via l'API northbound ou un export,
  vérifier que des alarmes/perfs récentes existent (une supervision qui
  ne reçoit plus rien depuis 2 h est peut-être aveugle, pas calme).
- En cas d'échec : **alerte par un canal indépendant** d'eSight
  (SMS via une autre passerelle, appel d'astreinte).

---

# ANNEXE D — CAS PRATIQUES COMMENTÉS

## 131. Cas pratique n°1 : coupure du lien inter-sites, vendredi 18h05

**Contexte** : ETI, 3 sites. eSight Standard, 140 équipements. Lien
fibre opérateur entre le siège et le site B.

**Déroulé :**
- 18h05 : tempête — 1 alarme Critical « linkDown » + 34 alarmes
  « équipement injoignable » (tout le site B).
- 18h06 : l'astreinte reçoit le **SMS** (règle : Critical backbone → SMS).
  Connexion VPN, console eSight : la corrélation (configurée 2 mois plus
  tôt après un incident similaire) rattache les 34 alarmes à l'alarme racine.
- 18h10 : runbook « lien inter-sites down » appliqué : vérification
  topologie (lien vraiment down, pas un flap — historique stable),
  équipements d'extrémité injoignables des deux côtés → pas un problème
  d'équipement local.
- 18h15 : appel opérateur — incident connu sur la fibre, délai annoncé 22h.
  Alarme acquittée avec n° de ticket opérateur. **Masquage temporaire**
  des alarmes du site B (date de fin : lendemain 8h) pour garder la
  console lisible.
- 21h40 : retour opérateur, lien up, traps linkUp reçus, alarmes soldées
  en cascade. Levée du masquage. Vérification : tout le site B est revenu
  (synchronisation manuelle pour solder 2 alarmes fantômes).

**Ce qui a bien marché :** corrélation configurée, SMS reçu, runbook suivi,
pas de panique. **À améliorer :** le lien de secours 4G du site B n'a pas
basculé automatiquement — action planifiée au comité mensuel.

## 132. Cas pratique n°2 : migration SNMP v2c → v3 sur 120 équipements

**Contexte** : audit interne — community v2c identique partout depuis
5 ans. Décision : passage en SNMP v3 authPriv.

**Déroulé :**
1. **Préparation** : création du profil eSight `SNMPv3-authPriv`
   (utilisateur dédié, SHA/AES — exemples fictifs, mots de passe au coffre).
2. **Pilote** : 5 équipements (1 par famille). Déploiement de l'utilisateur
   v3 côté équipement via Smart Configuration Tool, bascule du profil
   eSight sur ces 5, vérification polling + traps pendant 1 semaine.
3. **Généralisation par vagues** : 20-30 équipements par soirée de
   maintenance, en commençant par les accès et en finissant par le cœur.
   Après chaque vague : contrôle des timeouts et des traps.
4. **Bascule** : une fois tout le parc en v3 et stable depuis 2 semaines,
   **suppression des community v2c** côté équipements + suppression du
   profil v2c dans eSight.
5. **Vérification finale** : audit snmpwalk (section 126) — 0 équipement
   répondant encore en v2c.

**Pièges évités :** ne pas supprimer v2c avant d'avoir validé v3 partout
(sinon supervision aveugle) ; traiter les équipements anciens ne
supportant pas v3 (maintien en v2c avec ACL strictes, plan de remplacement).

## 133. Cas pratique n°3 : déploiement eSight sur 3 sites

**Contexte** : groupe avec siège (80 équipements) + 2 sites distants
(30 chacun). Choix : **1 eSight au siège** (pas de hiérarchie — le WAN
est fiable et l'équipe est centralisée).

**Déroulé :**
1. Dimensionnement : 140 équipements + 20 % = palier 200-500.
2. Installation au siège sur VM, profil SNMP v3 unique (même politique
   partout — simplifie tout).
3. Découverte **site par site** : siège semaine 1 (rodage des profils et
   des seuils), site B semaine 2, site C semaine 3. Chaque site = sa tâche
   de découverte + sa vue topologique + son fond de plan.
4. Politique d'alarmes calibrée sur les données du siège avant
   généralisation (les 30 alarmes les plus fréquentes traitées en atelier).
5. Notifications : SMS uniquement pour le critical du backbone et des
   cœurs de site ; le reste en mail.
6. Recette selon l'annexe A, signée avant la bascule officielle
   (« à partir du 1er, eSight est l'outil de supervision officiel »).

**Leçon** : déployer **progressivement** (un site à la fois) plutôt que
« big bang ». Chaque site rend le suivant plus facile.

## 134. Cas pratique n°4 : « le Wi-Fi est lent » dans un bâtiment

**Contexte** : plaintes récurrentes d'un open space, 60 utilisateurs.
Module WLAN actif.

**Déroulé :**
1. Dans eSight : vue WLAN du bâtiment — 2 AP portent 45 utilisateurs
   à eux seuls, les 4 autres sont quasi vides (mauvaise répartition,
   les utilisateurs s'accrochent aux AP les plus « visibles »).
2. Historique : pics de déconnexions corrélés aux réunions de 10h
   (saturation).
3. Vérification énergie : les 2 AP surchargés sont sur le même switch,
   budget PoE à 88 % — pas de marge pour ajouter un AP sur ce switch.
4. **Actions** : ajustement des seuils de roaming côté contrôleur pour
   mieux répartir, ajout d'un AP sur un **autre** switch (avec marge PoE),
   information aux utilisateurs.
5. Suivi : 2 semaines après, répartition homogène, plaintes tombées à zéro.
   Rapport eSight avant/après archivé.

**Leçon** : eSight a transformé une plainte floue (« c'est lent ») en
diagnostic chiffré en 20 minutes. Et la contrainte était énergétique
(PoE), pas radio — d'où l'intérêt de croiser les données (section 64).

## 135. Synthèse : ce que ces quatre cas enseignent

1. **La corrélation et les runbooks transforment les incidents**
   (cas n°1) : sans eux, la même panne prend 3 fois plus de temps.
2. **Les migrations de supervision se font par vagues**, jamais en
   big bang (cas n°2 et n°3).
3. **Le diagnostic chiffré bat l'intuition** (cas n°4) : eSight sert
   d'abord à prouver où est le problème.
4. **Chaque incident améliore le système** : corrélation ajoutée,
   lien de secours planifié, seuils ajustés — la boucle de retour
   d'expérience est le vrai moteur (sections 40, 50).

---

## Conclusion : eSight, un bon ouvrier à bien manager

eSight n'est ni magique ni obsolète : c'est un **NMS classique, stable et
éprouvé**, excellent sur parc Huawei, qui fait très bien les quatre
métiers de base (alarmes, perfs, topo, configs) **à condition d'être
exploité avec discipline** : découverte propre, politique d'alarmes
calibrée, sauvegardes testées, runbooks, rituels d'équipe.

Pour un chef de service systèmes & énergies, les trois messages à retenir :

1. **La valeur d'eSight = la discipline d'exploitation qu'on met autour.**
   L'outil seul ne supervise rien ; c'est la politique d'alarmes, les
   runbooks et les rituels qui font le NOC.
2. **Pensez énergie** : alimentations, PoE, onduleurs, température des
   locaux — eSight peut les superviser, et c'est votre domaine d'expertise
   qui fera la différence en incident.
3. **Gardez la trajectoire NCE en tête** : exploitez eSight à fond
   aujourd'hui, mais avec des processus portables pour ne pas subir
   la migration de demain.

*Bon courage sur le terrain. — Document généré le 27/09/2026.*
