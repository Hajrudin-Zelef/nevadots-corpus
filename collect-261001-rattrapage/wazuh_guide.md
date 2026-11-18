# Guide Wazuh — SIEM & XDR Open Source en production

> **Public :** Zelef, chef de service systèmes & énergies, sysadmin avec des responsabilités d'équipe.
> **Angle :** pratique, production, petites équipes SOC. Chaque section contient des commandes et des fichiers prêts à copier.
> **Version :** Wazuh **4.x** (4.7 → 4.12). Les chemins, ports et paramètres ci-dessous sont ceux de la 4.x. En cas de doute, vérifiez le changelog de votre version mineure : certaines options changent entre versions (ex. `/etc/filebeat`, Indexer basé sur OpenSearch).
> **Avertissement :** ce guide contient des exemples avec des valeurs factices (`motdepasse`, IP `203.0.113.x`). Ne copiez jamais ces valeurs en production ; générez vos propres secrets et adaptez les réseaux.

---

## Table des matières

1. [Ce qu'est Wazuh et à quoi ça sert](#1-ce-quest-wazuh-et-à-quoi-ça-sert)
2. [Architecture : manager, indexer, dashboard](#2-architecture--manager-indexer-dashboard)
3. [Concepts clés à maîtriser avant d'installer](#3-concepts-clés-à-maîtriser-avant-dinstaller)
4. [Licence, coût et positionnement vs concurrents](#4-licence-coût-et-positionnement-vs-concurrents)
5. [Dimensionnement : CPU, RAM, disque](#5-dimensionnement--cpu-ram-disque)
6. [Prérequis système et réseau](#6-prérequis-système-et-réseau)
7. [Méthodes de déploiement : choisir la bonne](#7-méthodes-de-déploiement--choisir-la-bonne)
8. [All-in-one : le déploiement le plus simple](#8-all-in-one--le-déploiement-le-plus-simple)
9. [Installation pas à pas sur Debian/Ubuntu (all-in-one)](#9-installation-pas-à-pas-sur-debianubuntu-all-in-one)
10. [Vérification post-installation](#10-vérification-post-installation)
11. [Accéder au dashboard : première connexion](#11-accéder-au-dashboard--première-connexion)
12. [Déploiement distribué : quand et comment](#12-déploiement-distribué--quand-et-comment)
13. [Déploiement conteneurs (Docker)](#13-déploiement-conteneurs-docker)
14. [Cluster de managers : multi-nœuds](#14-cluster-de-managers--multi-nœuds)
15. [Durcissement du serveur Wazuh (base)](#15-durcissement-du-serveur-wazuh-base)
16. [Certificats TLS : comprendre et régénérer](#16-certificats-tls--comprendre-et-régénérer)
17. [L'agent Wazuh : rôle et fonctionnement](#17-lagent-wazuh--rôle-et-fonctionnement)
18. [Installer un agent sur Debian/Ubuntu](#18-installer-un-agent-sur-debianubuntu)
19. [Installer un agent sur Windows](#19-installer-un-agent-sur-windows)
20. [Inscription d'un agent : les 4 méthodes](#20-inscription-dun-agent--les-4-méthodes)
21. [Clés d'agent : gestion et rotation](#21-clés-dagent--gestion-et-rotation)
22. [Groupes d'agents : l'arme de la configuration centralisée](#22-groupes-dagents--larme-de-la-configuration-centralisée)
23. [Configuration centralisée : pousser la conf aux agents](#23-configuration-centralisée--pousser-la-conf-aux-agents)
24. [Vérifier la santé des agents](#24-vérifier-la-santé-des-agents)
25. [Désinstaller / réinscrire un agent proprement](#25-désinstaller--réinscrire-un-agent-proprement)
26. [Le moteur de règles : comment Wazuh analyse les logs](#26-le-moteur-de-règles--comment-wazuh-analyse-les-logs)
27. [Decoders : extraire du sens des logs bruts](#27-decoders--extraire-du-sens-des-logs-bruts)
28. [Écrire son premier decoder personnalisé](#28-écrire-son-premier-decoder-personnalisé)
29. [Écrire sa première règle personnalisée](#29-écrire-sa-première-règle-personnalisée)
30. [Niveaux de règles (0 à 16) : la grammaire de la sévérité](#30-niveaux-de-règles-0-à-16--la-grammaire-de-la-sévérité)
31. [Tester ses règles sans casser la production](#31-tester-ses-règles-sans-casser-la-production)
32. [Organiser ses règles : local_rules, CDB lists](#32-organiser-ses-règles--local_rules-cdb-lists)
33. [FIM — surveillance d'intégrité des fichiers : principes](#33-fim--surveillance-dintégrité-des-fichiers--principes)
34. [Configurer syscheck (FIM) sur Linux](#34-configurer-syscheck-fim-sur-linux)
35. [Configurer syscheck (FIM) sur Windows](#35-configurer-syscheck-fim-sur-windows)
36. [FIM temps réel vs who-data : bien choisir](#36-fim-temps-réel-vs-who-data--bien-choisir)
37. [Exploiter les alertes FIM au quotidien](#37-exploiter-les-alertes-fim-au-quotidien)
38. [Détection de vulnérabilités : inventaire logiciel](#38-détection-de-vulnérabilités--inventaire-logiciel)
39. [Configurer le module vulnerability detector](#39-configurer-le-module-vulnerability-detector)
40. [Lire et prioriser les alertes de vulnérabilités](#40-lire-et-prioriser-les-alertes-de-vulnérabilités)
41. [SCA — audit de conformité CIS : principes](#41-sca--audit-de-conformité-cis--principes)
42. [Activer et lire les audits SCA](#42-activer-et-lire-les-audits-sca)
43. [Écrire une politique SCA personnalisée](#43-écrire-une-politique-sca-personnalisée)
44. [Active Response : le blocage automatique](#44-active-response--le-blocage-automatique)
45. [Configurer le blocage automatique d'IP (firewall-drop)](#45-configurer-le-blocage-automatique-dattaque-ip-firewall-drop)
46. [Active Response avancé : scripts personnalisés](#46-active-response-avancé--scripts-personnalisés)
47. [Correspondance MITRE ATT&CK](#47-correspondance-mitre-att&ck)
48. [Gestion des alertes : cycle de vie](#48-gestion-des-alertes--cycle-de-vie)
49. [Niveaux de sévérité : que faire pour chaque niveau](#49-niveaux-de-sévérité--que-faire-pour-chaque-niveau)
50. [Tuning : réduire les faux positifs](#50-tuning--réduire-les-faux-positifs)
51. [Supervision réseau via agents (état des interfaces, ports)](#51-supervision-réseau-via-agents-état-des-interfaces-ports)
52. [Collecter les logs d'équipements réseau (syslog)](#52-collecter-les-logs-déquipements-réseau-syslog)
53. [Surveiller un pare-feu / un switch avec Wazuh](#53-surveiller-un-pare-feu--un-switch-avec-wazuh)
54. [Intégration TheHive : créer des cases depuis les alertes](#54-intégration-thehive--créer-des-cases-depuis-les-alertes)
55. [Intégration Shuffle (SOAR)](#55-intégration-shuffle-soar)
56. [Webhooks génériques : Slack, Teams, Mattermost](#56-webhooks-génériques--slack-teams-mattermost)
57. [Envoyer des alertes par e-mail](#57-envoyer-des-alertes-par-e-mail)
58. [API Wazuh : automatiser sans le dashboard](#58-api-wazuh--automatiser-sans-le-dashboard)
59. [Superviser Wazuh lui-même : le manager en bonne santé](#59-superviser-wazuh-lui-même--le-manager-en-bonne-santé)
60. [Superviser l'indexer (OpenSearch)](#60-superviser-lindexer-opensearch)
61. [Superviser le dashboard](#61-superviser-le-dashboard)
62. [Sauvegarde : stratégie complète](#62-sauvegarde--stratégie-complète)
63. [Snapshots de l'indexer : mise en place pas à pas](#63-snapshots-de-lindexer--mise-en-place-pas-à-pas)
64. [Restaurer après sinistre : procédure](#64-restaurer-après-sinistre--procédure)
65. [Mise à jour du manager et des composants centraux](#65-mise-à-jour-du-manager-et-des-composants-centraux)
66. [Mise à jour des agents (WPK et manuelle)](#66-mise-à-jour-des-agents-wpk-et-manuelle)
67. [Dépannage : méthodologie générale](#67-dépannage--méthodologie-générale)
68. [Dépannage : l'agent ne remonte pas](#68-dépannage--lagent-ne-remonte-pas)
69. [Dépannage : indexer rouge / cluster unhealthy](#69-dépannage--indexer-rouge--cluster-unhealthy)
70. [Dépannage : pas d'alertes / alertes en retard](#70-dépannage--pas-dalertes--alertes-en-retard)
71. [Dépannage : dashboard inaccessible](#71-dépannage--dashboard-inaccessible)
72. [Dépannage : disque plein](#72-dépannage--disque-plein)
73. [10 erreurs classiques (et comment les éviter)](#73-10-erreurs-classiques-et-comment-les-éviter)
74. [Bonnes pratiques SOC pour une petite équipe](#74-bonnes-pratiques-soc-pour-une-petite-équipe)
75. [Cas pratique 1 : intrusion SSH par force brute](#75-cas-pratique-1--intrusion-ssh-par-force-brute)
76. [Cas pratique 2 : ransomware simulé (FIM + active response)](#76-cas-pratique-2--ransomware-simulé-fim--active-response)
77. [Cas pratique 3 : serveur web compromis (PHP webshell)](#77-cas-pratique-3--serveur-web-compromis-php-webshell)
78. [Cas pratique 4 : conformité d'un parc Windows (CIS)](#78-cas-pratique-4--conformité-dun-parc-windows-cis)
79. [Cas pratique 5 : vulnérabilité critique sur le parc (CVE)](#79-cas-pratique-5--vulnérabilité-critique-sur-le-parc-cve)
80. [Cas pratique 6 : exfiltration de données via DNS](#80-cas-pratique-6--exfiltration-de-données-via-dns)
81. [Checklist quotidienne / hebdomadaire / mensuelle](#81-checklist-quotidienne--hebdomadaire--mensuelle)
82. [Pense-bête de poche : commandes essentielles](#82-pense-bête-de-poche--commandes-essentielles)
83. [Ports et flux réseau : tableau de référence](#83-ports-et-flux-réseau--tableau-de-référence)
84. [Fichiers importants : où est quoi](#84-fichiers-importants--où-est-quoi)
85. [Glossaire](#85-glossaire)
86. [Quiz : 10 questions + réponses](#86-quiz--10-questions--réponses)
87. [Pour aller plus loin](#87-pour-aller-plus-loin)

---

## 1. Ce qu'est Wazuh et à quoi ça sert

Wazuh est une plateforme **open source** de sécurité qui combine :

- **SIEM** (*Security Information and Event Management*) : collecte et corrélation centralisée des logs et événements de sécurité.
- **XDR** (*Extended Detection and Response*) : détection et réponse sur les postes/serveurs via des agents.
- **FIM** (*File Integrity Monitoring*) : surveillance de l'intégrité des fichiers.
- **Détection de vulnérabilités** : inventaire des logiciels installés croisé avec les bases CVE.
- **Audit de conformité** : contrôles CIS, PCI-DSS, etc.
- **Réponse active** : blocage automatique (IP, compte, etc.).

Concrètement, vous installez un **agent** sur chaque machine (Linux, Windows, macOS), les agents envoient leurs événements à un **manager** central, le manager analyse (règles, decoders), stocke dans un **indexer**, et vous visualisez tout dans un **dashboard** web.

Cas d'usage typiques pour un service systèmes & énergies :

| Besoin | Ce que Wazuh apporte |
|---|---|
| Savoir qui s'est connecté où (SSH, RDP) | Alertes de connexion, géolocalisation, brute-force |
| Détecter un fichier modifié sur un serveur critique | FIM sur `/etc`, `C:\Windows\System32` |
| Suivre les CVE du parc | Inventaire logiciel + alertes CVE quotidiennes |
| Prouver la conformité (audit) | Rapports CIS, PCI-DSS exportables |
| Réagir vite la nuit | Active Response : bannir l'IP attaquante automatiquement |
| Centraliser les logs des équipements réseau | Collecte syslog (switchs, onduleurs, pare-feu) |

Wazuh ne remplace pas un antivirus ni un pare-feu : il les **complète** en donnant la vision centrale.

---

## 2. Architecture : manager, indexer, dashboard

Trois composants centraux, trois rôles distincts. Retenez ce schéma :

```
┌─────────────┐      ┌─────────────┐      ┌──────────────┐
│   AGENTS    │─────▶│   MANAGER   │─────▶│   INDEXER    │
│ (postes,    │ 1514 │ (analyse :  │ 9200 │ (stockage :  │
│  serveurs)  │/udp  │  règles,    │/tcp  │  OpenSearch) │
└─────────────┘      │  decoders,  │      └──────┬───────┘
                     │  FIM, SCA)  │             │
┌─────────────┐      └──────┬──────┘             │
│ SYSLOG      │─────▶│      │ Filebeat           │
│ (équipements│ 514  │      ▼                    ▼
│  réseau)    │/udp  │  ┌──────────────┐  ┌──────────────┐
└─────────────┘      │  │  DASHBOARD   │◀─│  (requêtes)  │
                     │  │ (IHM web :   │  │              │
                     │  │  443)        │  │              │
                     │  └──────────────┘  └──────────────┘
                     └─────────────────────────────┘
```

### 2.1 Le manager (`wazuh-manager`)

- Reçoit les événements des agents (port **1514**, UDP par défaut en 4.x ; TCP possible).
- Applique les **decoders** (parse les logs) puis les **règles** (détecte).
- Exécute les modules : FIM (syscheck), détection de vulnérabilités, SCA, rootcheck, active response.
- Expose l'**API** (port **55000**) pour le dashboard et l'automatisation.
- Envoie les alertes vers l'indexer via **Filebeat**.

### 2.2 L'indexer (`wazuh-indexer`)

- Basé sur **OpenSearch** : stocke et indexe les alertes + les événements.
- Port **9200** (API REST). En cluster : plusieurs nœuds.
- C'est lui qui grossit en disque : dimensionnez large (voir section 5).

### 2.3 Le dashboard (`wazuh-dashboard`)

- Basé sur **OpenSearch Dashboards** : interface web (port **443**).
- Affiche alertes, agents, vulnérabilités, conformité, MITRE ATT&CK.
- S'authentifie auprès de l'indexer ; ne stocke rien lui-même.

### 2.4 Filebeat, le tuyau discret

Filebeat lit `/var/ossec/logs/alerts/alerts.json` sur le manager et l'envoie à l'indexer. Si Filebeat est arrêté, **le manager continue d'analyser** mais le dashboard ne voit plus rien. Symptôme classique : « le dashboard est vide alors que les agents sont verts » → vérifiez Filebeat (voir section 70).

---

## 3. Concepts clés à maîtriser avant d'installer

| Concept | Définition courte | Où ça vit |
|---|---|---|
| **Agent** | Programme sur la machine surveillée | `/var/ossec` (Linux), `C:\Program Files (x86)\ossec-agent` (Windows) |
| **Decoder** | Parse un log brut → champs structurés | `/var/ossec/etc/decoders/local_decoder.xml` |
| **Règle** | Condition sur les champs → alerte + niveau | `/var/ossec/etc/rules/local_rules.xml` |
| **Niveau** | Sévérité 0–16 (voir section 30) | champ `rule.level` |
| **Groupe** | Ensemble d'agents partageant une conf | `/var/ossec/etc/shared/<groupe>/` |
| **FIM / syscheck** | Sommes de contrôle des fichiers surveillés | module `syscheck` |
| **SCA** | Contrôles de conformité (CIS...) | module `sca`, politiques `.yml` |
| **Active Response** | Script exécuté sur alerte | `/var/ossec/active-response/bin/` |
| **CDB list** | Liste de valeurs pour enrichir les règles | `/var/ossec/etc/lists/` |

Retenez surtout : **decoder d'abord, règle ensuite**. Un log non décodé ne déclenchera jamais de règle fine.

---

## 4. Licence, coût et positionnement vs concurrents

- **Licence :** GPLv2 pour le cœur (fork d'OSSEC), composants OpenSearch sous Apache 2.0 / SSPL selon version. Usage commercial autorisé, sans limite d'agents ni d'EPS dans la version communautaire.
- **Coût :** 0 € de licence. Le coût réel = **serveurs + disque + votre temps**.
- **Support :** communautaire (Slack, GitHub, forum) gratuit ; support commercial via Wazuh Inc. si besoin.

Comparatif rapide (ordre de grandeur, à valider selon votre contexte) :

| Solution | Licence | Agents illimités | Prise en main |
|---|---|---|---|
| Wazuh | Open source | Oui | Moyenne (ce guide aide) |
| Wazuh Cloud | Abonnement | Oui (facturé/agent) | Facile |
| Splunk | Propriétaire, au volume | Non (coût/volume) | Moyenne |
| Elastic Security | Freemium (limites) | Selon offre | Moyenne |
| Graylog | Open core | Selon offre | Facile |

Pour une petite équipe avec un budget serré et des compétences Linux, **Wazuh auto-hébergé** est imbattable en rapport fonctionnalités/coût.

---

## 5. Dimensionnement : CPU, RAM, disque

Le dimensionnement dépend de deux métriques : **nombre d'agents** et **EPS** (*events per second*, événements/seconde).

### 5.1 Repères (all-in-one, valeurs indicatives Wazuh 4.x)

| Agents | EPS approx. | CPU | RAM | Disque |
|---|---|---|---|---|
| < 25 | < 50 | 4 vCPU | 8 Go | 100 Go |
| 25 – 100 | 50 – 200 | 8 vCPU | 16 Go | 300 Go |
| 100 – 500 | 200 – 1000 | 16 vCPU | 32 Go | 1 To |
| 500+ | > 1000 | Distribué (voir section 12) | — | — |

> Ces chiffres sont des **ordres de grandeur**. Un parc Windows verbeux (audit avancé) génère 3 à 5× plus d'EPS qu'un parc Linux sobre. Mesurez vos EPS réels après 1 semaine (voir section 59) et ajustez.

### 5.2 Le disque : le poste critique

L'indexer stocke **chaque alerte** (et, selon réglage, chaque événement archivé). Règle pratique :

```
Espace disque ≈ EPS × 86400 s × taille_moyenne_doc × jours_rétention × 1,3 (marge)
```

Exemple : 100 EPS × 86400 × 1,5 Ko × 90 jours × 1,3 ≈ **1,5 To**.

- Séparez les données sur un volume dédié (`/var/lib/wazuh-indexer`).
- Prévoyez la **rotation des index** (voir section 60) pour ne pas saturer.
- En production : **ne mettez jamais l'indexer sur la partition racine**.

### 5.3 RAM de l'indexer

OpenSearch utilise la moitié de la RAM pour le *heap* Java (max ~32 Go). Le reste sert au cache du système de fichiers. **Sous-dimensionner la RAM = indexer lent puis rouge.**

---

## 6. Prérequis système et réseau

### 6.1 Système (serveur central)

- Debian 11/12 ou Ubuntu 20.04/22.04/24.04, 64 bits.
- `curl`, `gnupg`, `lsb-release` installés.
- Nom d'hôte résolvable (FQDN) — les certificats s'appuient dessus.
- **Désactivez tout autre service sur les ports 1514, 1515, 443, 9200, 55000** avant install.

```bash
# Vérification rapide des ports avant installation
sudo ss -tulpn | grep -E '1514|1515|443|9200|55000' || echo "Ports libres : OK"
```

### 6.2 Réseau et pare-feu

Flux à autoriser (détail complet section 83) :

| Source → Destination | Port | Protocole | Usage |
|---|---|---|---|
| Agents → Manager | 1514 | UDP (ou TCP) | Remontée événements |
| Agents → Manager | 1515 | TCP | Inscription |
| Admin → Manager | 55000 | TCP | API |
| Manager → Indexer | 9200 | TCP | Envoi alertes |
| Admin → Dashboard | 443 | TCP | IHM web |
| Dashboard → Indexer | 9200 | TCP | Requêtes |

### 6.3 Temps et DNS

- **NTP synchronisé partout** (manager, agents, indexer) : des horloges décalées cassent la corrélation et les certificats.
- DNS direct + reverse corrects sur le manager.

```bash
timedatectl status | grep "synchronized"
# doit afficher : System clock synchronized: yes
```

---

## 7. Méthodes de déploiement : choisir la bonne

| Méthode | Quand l'utiliser | Limites |
|---|---|---|
| **All-in-one** (assistant d'install) | < 100 agents, POC, PME | Ne scale pas au-delà de ~500 agents |
| **Distribué** (paquets pas à pas) | > 100 agents, HA, séparation des rôles | Plus complexe à maintenir |
| **Docker** (conteneurs) | Lab, tests, CI | **Déconseillé en production** par Wazuh (persistance, perf) |
| **Cluster multi-managers** | Continuité si un manager tombe | Nécessite un répartiteur de charge |

**Recommandation pour démarrer :** all-in-one sur une VM dédiée. C'est ce que décrit la section 9. Vous migrerez vers du distribué le jour où les métriques (section 59) le justifient.

---

## 8. All-in-one : le déploiement le plus simple

Wazuh fournit un **assistant d'installation** (`wazuh-install.sh`) qui installe et configure manager + indexer + dashboard + Filebeat en une commande, avec génération des certificats.

Principe :

```bash
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh
sudo bash wazuh-install.sh -a
```

L'option `-a` = all-in-one. L'assistant :
1. Ajoute le dépôt Wazuh.
2. Installe `wazuh-manager`, `wazuh-indexer`, `wazuh-dashboard`, `filebeat`.
3. Génère les certificats TLS (nœuds + admin).
4. Démarre les services dans le bon ordre.
5. Affiche les identifiants du dashboard.

> ⚠️ **Ne lancez jamais l'assistant deux fois sans désinstaller** (`-u`) entre les deux : vous corrompriez les certificats. En cas d'échec, désinstallez proprement puis recommencez.

---

## 9. Installation pas à pas sur Debian/Ubuntu (all-in-one)

### Étape 1 — Préparer le système

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl gnupg lsb-release ca-certificates
sudo hostnamectl set-hostname wazuh-soc       # adaptez
echo "127.0.0.1 wazuh-soc" | sudo tee -a /etc/hosts
```

Vérifiez que le FQDN se résout :

```bash
hostname -f
getent hosts "$(hostname -f)"
```

### Étape 2 — Télécharger l'assistant (version 4.x)

```bash
cd /tmp
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh
# Remplacez 4.12 par la dernière 4.x disponible sur https://documentation.wazuh.com
```

> Vérifiez toujours la dernière version mineure 4.x dans la documentation officielle avant de lancer : les correctifs de sécurité sortent régulièrement.

### Étape 3 — Lancer l'installation

```bash
sudo bash wazuh-install.sh -a
```

Durée indicative : 5 à 15 minutes selon la machine et le réseau. **Ne l'interrompez pas** (Ctrl+C en plein milieu = état incohérent → désinstallation requise).

### Étape 4 — Noter les identifiants affichés

En fin d'installation, l'assistant affiche quelque chose comme :

```
INFO: --- Summary ---
INFO: You can now start using your Wazuh cluster with:
INFO:   Wazuh dashboard: https://wazuh-soc
INFO:     User: admin
INFO:     Password: <mot-de-passe-généré>
```

**Copiez immédiatement ce mot de passe** dans votre coffre (Bitwarden, KeePass...). Il n'est affiché qu'une fois.

### Étape 5 — Sécuriser l'accès initial

```bash
# Changez le mot de passe admin dès la première connexion (dashboard > menu utilisateur)
# Puis régénérez proprement si besoin via le script fourni :
sudo tar -xzf wazuh-install-files.tar -C /tmp   # archive créée par l'assistant
```

L'archive `wazuh-install-files.tar` contient les certificats et mots de passe : **protégez-la** (droits 600, sauvegarde chiffrée) puis supprimez-la du serveur une fois archivée ailleurs.

### Étape 6 — Ouvrir le pare-feu (exemple UFW)

```bash
sudo ufw allow 1514/udp comment 'Wazuh agents'
sudo ufw allow 1515/tcp comment 'Wazuh enrollment'
sudo ufw allow 443/tcp  comment 'Wazuh dashboard'
sudo ufw allow 55000/tcp comment 'Wazuh API (restreindre aux admins)'
sudo ufw enable
```

> L'API (55000) et l'indexer (9200) ne doivent être exposés qu'aux IP d'administration, jamais à tout le parc.

### Étape 7 — Figer les versions (éviter les mises à jour surprises)

```bash
# Empêcher apt de mettre à jour Wazuh hors maintenance planifiée
sudo apt-mark hold wazuh-manager wazuh-indexer wazuh-dashboard filebeat
```

Les mises à jour se font **planifiées** (voir section 65), jamais au fil de l'eau.

---

## 10. Vérification post-installation

```bash
# 1. Les 4 services tournent ?
sudo systemctl is-active wazuh-manager wazuh-indexer wazuh-dashboard filebeat

# 2. Le manager écoute les agents ?
sudo ss -tulpn | grep -E '1514|1515'

# 3. L'indexer répond ? (admin / mot de passe dashboard, -k car autosigné)
curl -k -u admin:'<MOT_DE_PASSE>' https://localhost:9200

# 4. Santé du cluster indexer
curl -k -u admin:'<MOT_DE_PASSE>' 'https://localhost:9200/_cluster/health?pretty'

# 5. L'API du manager répond ?
curl -k -u wazuh-wui:'<MOT_DE_PASSE_API>' https://localhost:55000/security/user/authenticate -X GET
```

Résultat attendu pour la santé : `"status" : "green"` (ou `yellow` en mono-nœud, c'est normal : voir section 60).

Checklist de validation :

- [ ] Les 4 services sont `active`
- [ ] `curl` sur `:9200` répond 200
- [ ] Le dashboard s'affiche sur `https://<ip-ou-fqdn>`
- [ ] La page *Agents* du dashboard est accessible (0 agent pour l'instant, c'est normal)
- [ ] `wazuh-install-files.tar` sauvegardé hors serveur

---

## 11. Accéder au dashboard : première connexion

1. Ouvrez `https://<ip-ou-fqdn-du-serveur>` (acceptez l'avertissement de certificat autosigné, ou installez votre CA — voir section 16).
2. Connectez-vous avec `admin` / mot de passe généré.
3. **Changez immédiatement le mot de passe** : menu haut-droit → *Security* → utilisateurs → `admin`.

### 11.1 Créer des utilisateurs (principe du moindre privilège)

Ne travaillez jamais au quotidien avec `admin`. Créez :

| Utilisateur | Rôle | Usage |
|---|---|---|
| `admin` | Administrateur | Urgences, mises à jour |
| `soc-analyste` | `wazuh-ui-user` + lecture alertes | Analyse quotidienne |
| `soc-responsable` | Rôle avec active response | Validation des réponses |

Via le dashboard : *Security → Internal users → Create user*, puis assignez les rôles. Via API (voir section 58).

### 11.2 Régler le fuseau horaire

Dashboard → *Stack Management → Advanced Settings* → `dateFormat:tz` → `Europe/Paris` (ou votre TZ). Des alertes affichées en UTC alors que vos équipes pensent en heure locale = erreurs d'interprétation garanties.

---

## 12. Déploiement distribué : quand et comment

Passez en distribué quand : > 100–150 agents, besoin de HA, ou séparation réseau (DMZ).

### 12.1 Topologie type

```
                ┌──────────────────┐
                │  Load Balancer   │  (optionnel, ex. HAProxy)
                │  1514/udp 1515   │
                └───────┬──────────┘
                        │
           ┌────────────┼────────────┐
           ▼            ▼            ▼
     ┌──────────┐ ┌──────────┐ ┌──────────┐
     │ Manager  │ │ Manager  │ │ Manager  │  cluster Wazuh
     │    1     │ │    2     │ │    3     │
     └────┬─────┘ └────┬─────┘ └────┬─────┘
          │            │            │
          └────────────┼────────────┘
                       ▼ 9200
              ┌─────────────────┐
              │ Indexer cluster │  3 nœuds OpenSearch
              │ (dédiés)        │
              └────────┬────────┘
                       ▼
              ┌─────────────────┐
              │ Dashboard (1-2) │
              └─────────────────┘
```

### 12.2 Installation pas à pas (méthode paquets)

L'assistant gère aussi le distribué via un fichier de configuration. Procédure (4.x) :

```bash
# 1. Sur la première machine, générer la config
curl -sO https://packages.wazuh.com/4.12/wazuh-install.sh
sudo bash wazuh-install.sh --generate-config-files
# → crée wazuh-install-files.tar + config.yml à éditer
```

```yaml
# config.yml : déclarez chaque nœud
nodes:
  indexer:
    - name: indexer-1
      ip: 10.0.0.11
    - name: indexer-2
      ip: 10.0.0.12
    - name: indexer-3
      ip: 10.0.0.13
  server:
    - name: wazuh-1
      ip: 10.0.0.21
      node_type: master
    - name: wazuh-2
      ip: 10.0.0.22
      node_type: worker
  dashboard:
    - name: dashboard
      ip: 10.0.0.31
```

```bash
# 2. Générer certificats + mots de passe
sudo bash wazuh-install.sh --generate-config-files
# 3. Copier wazuh-install-files.tar sur chaque nœud, puis :
sudo bash wazuh-install.sh --indexer --node-name indexer-1      # sur chaque indexer
sudo bash wazuh-install.sh --server --node-name wazuh-1         # sur chaque manager
sudo bash wazuh-install.sh --dashboard --node-name dashboard    # sur le dashboard
# 4. Initialiser le cluster (sur un nœud indexer) :
sudo bash wazuh-install.sh --start-cluster
```

> Suivez **l'ordre** : indexers → démarrage cluster → managers → dashboard. Inverser l'ordre = erreurs de connexion TLS.

---

## 13. Déploiement conteneurs (Docker)

### 13.1 Quand l'utiliser

Lab, formation, tests de règles, CI. **Pas la production** : la documentation Wazuh elle-même recommande les paquets pour la prod (persistance des données, performances, support).

### 13.2 Lancement rapide (lab)

```bash
# Prérequis : Docker + Docker Compose
git clone https://github.com/wazuh/wazuh-docker.git
cd wazuh-docker/single-node
docker compose up -d
```

Accès : `https://localhost` (admin / `SecretPassword` par défaut — **changez-le**).

### 13.3 Persistance des données

Montez des volumes pour ne pas tout perdre au `docker compose down` :

```yaml
# extrait docker-compose.yml (single-node)
services:
  wazuh.manager:
    volumes:
      - wazuh_api_configuration:/var/ossec/api/configuration
      - wazuh_etc:/var/ossec/etc
      - wazuh_logs:/var/ossec/logs
      - wazuh_queue:/var/ossec/queue
      - wazuh_var_multigroups:/var/ossec/var/multigroups
```

> En lab c'est confortable ; en prod, préférez la méthode paquets (section 9 ou 12).

---

## 14. Cluster de managers : multi-nœuds

Le cluster Wazuh synchronise entre managers : règles, decoders, groupes d'agents, CDB lists.

### 14.1 Fichier `/var/ossec/etc/ossec.conf` (extrait cluster)

```xml
<cluster>
  <name>wazuh-production</name>
  <node_name>wazuh-1</node_name>
  <node_type>master</node_type>
  <key>MOT_DE_PASSE_PARTAGE_32_CARACTERES_MINIMUM</key>
  <port>1516</port>
  <bind_addr>0.0.0.0</bind_addr>
  <nodes>
      <node>10.0.0.21</node>
      <node>10.0.0.22</node>
  </nodes>
  <hidden>no</hidden>
  <disabled>no</disabled>
</cluster>
```

- **master** : 1 seul. C'est lui qui centralise la conf et la pousse aux workers.
- **workers** : reçoivent les agents (via LB) et synchronisent depuis le master.
- La clé `<key>` doit être **identique** sur tous les nœuds (32 caractères alphanumériques minimum).

### 14.2 Vérifier la synchronisation

```bash
sudo /var/ossec/bin/cluster_control -l          # liste les nœuds
sudo /var/ossec/bin/cluster_control -s           # statut de synchro
```

Si un worker affiche `synced: no` durablement : vérifiez la clé, le port 1516 entre managers, et les logs `/var/ossec/logs/cluster.log`.

---

## 15. Durcissement du serveur Wazuh (base)

Le serveur Wazuh est **la couronne** : qui le compromet voit tout le parc. Durcissez-le comme un bastion.

```bash
# 1. Mises à jour système (hors paquets Wazuh figés, cf. section 9)
sudo apt update && sudo apt upgrade -y

# 2. SSH : clés uniquement, pas de root
sudo sed -i 's/^#\?PermitRootLogin.*/PermitRootLogin no/' /etc/ssh/sshd_config
sudo sed -i 's/^#\?PasswordAuthentication.*/PasswordAuthentication no/' /etc/ssh/sshd_config
sudo systemctl reload ssh

# 3. Pare-feu restrictif : API et indexer réservés au réseau admin
sudo ufw allow from 10.0.10.0/24 to any port 55000 comment 'API Wazuh admins'
sudo ufw allow from 10.0.10.0/24 to any port 9200  comment 'Indexer admins'

# 4. Fail2ban sur SSH (et dashboard si exposé)
sudo apt install -y fail2ban
```

### 15.1 Durcir l'API Wazuh

`/var/ossec/api/configuration/api.yaml` :

```yaml
host: 127.0.0.1          # n'écouter qu'en local + reverse proxy, si possible
port: 55000
https:
  enabled: yes
  key: api/configuration/ssl/server.key
  cert: api/configuration/ssl/server.crt
access:
  max_login_attempts: 5
  block_time: 300
  max_request_per_minute: 300
```

Redémarrez le manager après modification : `sudo systemctl restart wazuh-manager`.

### 15.2 Comptes et mots de passe

- Changez `admin` dashboard (section 11), `wazuh-wui` (API dashboard) et `kibanaserver`.
- Mot de passe API dashboard : `/usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml` côté dashboard + utilisateur `wazuh-wui` côté manager — **les deux doivent correspondre**.

---

## 16. Certificats TLS : comprendre et régénérer

Wazuh 4.x chiffre : agents↔manager (clé symétrique par agent, voir section 21), manager↔indexer (TLS), dashboard↔indexer (TLS), navigateur↔dashboard (TLS).

### 16.1 Où sont les certificats (all-in-one)

| Usage | Emplacement |
|---|---|
| Indexer (nœud) | `/etc/wazuh-indexer/certs/` |
| Dashboard → indexer | `/etc/wazuh-dashboard/certs/` |
| Filebeat → indexer | `/etc/filebeat/certs/` |

### 16.2 Régénérer après changement de FQDN/IP

Si vous changez le nom ou l'IP du serveur, les certificats (liés au CN/SAN) deviennent invalides :

```bash
# Méthode propre : relancer la génération via l'assistant
cd /tmp
sudo bash wazuh-install.sh --generate-config-files   # régénère CA + certs
# Puis réinstallez les composants concernés, ou copiez les certs aux bons endroits
# et redémarrez : wazuh-indexer, wazuh-dashboard, filebeat
```

> ⚠️ En production, utilisez des certificats signés par **votre CA interne** plutôt que l'autosigné : déploiement plus propre sur les agents et les navigateurs du SOC.

### 16.3 Vérifier un certificat

```bash
echo | openssl s_client -connect localhost:9200 2>/dev/null \
  | openssl x509 -noout -subject -issuer -dates
```

---

## 17. L'agent Wazuh : rôle et fonctionnement

L'agent est un programme léger installé sur chaque machine surveillée. Il :

1. **Collecte** : logs système (`/var/log/*`, Event Viewer Windows), FIM, inventaire logiciel, état réseau, exécution de commandes (ex. `netstat`), audit SCA.
2. **Envoie** au manager via le port 1514 (UDP par défaut ; chiffrement symétrique avec la clé d'agent).
3. **Exécute** les réponses actives demandées par le manager (ex. bannir une IP via le pare-feu local).

L'agent **n'analyse pas** : il collecte et transmet. Toute l'intelligence est côté manager. Conséquence : un agent compromis ne peut pas falsifier les règles, mais peut cesser d'envoyer → d'où la supervision des agents (section 24).

Systèmes supportés (4.x) : Debian/Ubuntu, RHEL/CentOS/Rocky, SUSE, Windows 10/11 et Server 2016+, macOS. Architectures x86_64 et ARM64.

---

## 18. Installer un agent sur Debian/Ubuntu

### 18.1 Ajouter le dépôt Wazuh

```bash
# Sur la machine AGENT (pas le manager)
sudo apt update && sudo apt install -y curl gnupg lsb-release
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --dearmor \
  -o /usr/share/keyrings/wazuh.gpg
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" \
  | sudo tee /etc/apt/sources.list.d/wazuh.list
sudo apt update
```

### 18.2 Installer en déclarant le manager à l'avance

```bash
# WAZUH_MANAGER : IP ou FQDN du manager (ou du load balancer)
sudo WAZUH_MANAGER='10.0.0.21' WAZUH_AGENT_GROUP='linux-serveurs' \
  apt install -y wazuh-agent
```

Les variables `WAZUH_MANAGER` et `WAZUH_AGENT_GROUP` pré-remplissent `/var/ossec/etc/ossec.conf`. C'est **la méthode la plus fiable** pour du déploiement automatisé (Ansible, cloud-init).

### 18.3 Démarrer et figer

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now wazuh-agent
sudo apt-mark hold wazuh-agent     # mises à jour planifiées uniquement
```

### 18.4 Vérifier côté agent

```bash
# L'agent a-t-il une clé et voit-il le manager ?
sudo grep -E 'MANAGER_IP|GROUP' /var/ossec/etc/ossec.conf | head -5
sudo tail -20 /var/ossec/logs/ossec.log
# Attendu : "Successfully connected to server" / pas d'erreur "Unable to connect"
```

### 18.5 Déploiement automatisé avec Ansible (exemple)

```yaml
# playbook extrait : déploiement agent sur un parc Debian/Ubuntu
- hosts: serveurs_linux
  become: yes
  vars:
    wazuh_manager: "10.0.0.21"
    wazuh_group: "linux-serveurs"
  tasks:
    - name: Ajouter la clé GPG Wazuh
      ansible.builtin.get_url:
        url: https://packages.wazuh.com/key/GPG-KEY-WAZUH
        dest: /usr/share/keyrings/wazuh.gpg
        mode: '0644'
    - name: Ajouter le dépôt
      ansible.builtin.apt_repository:
        repo: "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main"
        filename: wazuh
    - name: Installer l'agent
      ansible.builtin.apt:
        name: wazuh-agent
        state: present
      environment:
        WAZUH_MANAGER: "{{ wazuh_manager }}"
        WAZUH_AGENT_GROUP: "{{ wazuh_group }}"
    - name: Activer et démarrer
      ansible.builtin.systemd:
        name: wazuh-agent
        enabled: yes
        state: started
```

---

## 19. Installer un agent sur Windows

### 19.1 Installation graphique (un poste isolé)

1. Téléchargez le MSI : `https://packages.wazuh.com/4.x/windows/wazuh-agent-4.12-1.msi` (adaptez la version).
2. Lancez l'installeur → renseignez l'**IP du manager**.
3. Terminez l'assistant, puis ouvrez **Wazuh Agent** → *Manage → Start*.

### 19.2 Installation silencieuse (parc, GPO, script)

```powershell
# PowerShell administrateur
$msi = "$env:TEMP\wazuh-agent.msi"
Invoke-WebRequest -Uri "https://packages.wazuh.com/4.x/windows/wazuh-agent-4.12-1.msi" -OutFile $msi
msiexec /i $msi /q WAZUH_MANAGER="10.0.0.21" WAZUH_AGENT_GROUP="windows-postes" WAZUH_REGISTRATION_SERVER="10.0.0.21"
Start-Service wazuh-agent
```

Paramètres MSI utiles :

| Paramètre | Rôle |
|---|---|
| `WAZUH_MANAGER` | IP/FQDN du manager |
| `WAZUH_AGENT_GROUP` | Groupe initial |
| `WAZUH_REGISTRATION_SERVER` | Serveur d'inscription (1515) |
| `WAZUH_REGISTRATION_PASSWORD` | Mot de passe d'inscription (voir section 20) |

### 19.3 Vérifier côté Windows

```powershell
Get-Service wazuh-agent
Get-Content "C:\Program Files (x86)\ossec-agent\ossec.log" -Tail 20
```

Attendu : `STATUS: Connected`.

---

## 20. Inscription d'un agent : les 4 méthodes

Un agent doit être **inscrit** (clé partagée avec le manager) avant d'envoyer quoi que ce soit. Quatre méthodes :

### Méthode A — Manuelle (`manage_agents`, la classique)

```bash
# Sur le MANAGER
sudo /var/ossec/bin/manage_agents
# A → Add an agent → nom, IP (ou 'any'), ID auto
# E → Extract key → copiez la clé
# Sur l'AGENT
sudo /var/ossec/bin/manage_agents   # I → Import key → collez
sudo systemctl restart wazuh-agent
```

Simple, mais **ne passe pas à l'échelle**.

### Méthode B — Mot de passe d'inscription (recommandée pour les parcs)

```bash
# Sur le MANAGER : définir un mot de passe d'inscription
echo 'MonMotDePasseInscription' | sudo tee /var/ossec/etc/authd.pass
sudo chmod 640 /var/ossec/etc/authd.pass
sudo chown root:wazuh /var/ossec/etc/authd.pass
```

`/var/ossec/etc/ossec.conf` du manager (service `authd`) :

```xml
<auth>
  <disabled>no</disabled>
  <port>1515</port>
  <use_source_ip>yes</use_source_ip>
  <force_insert>yes</force_insert>
  <force_time>0</force_time>
  <purge>yes</purge>
  <use_password>yes</use_password>
  <ciphers>HIGH:!ADH:!EXP:!MD5:!RC4:!3DES:!CAMELLIA:@STRENGTH</ciphers>
  <!-- <ssl_agent_ca> pour du TLS mutuel en environnement exposé -->
</auth>
```

Côté agent :

```bash
# Sur l'AGENT Linux
sudo /var/ossec/bin/agent-auth -m 10.0.0.21 -P 'MonMotDePasseInscription' -G linux-serveurs
sudo systemctl restart wazuh-agent
```

> Le mot de passe d'inscription transite en clair si vous n'activez pas le TLS sur `authd` : **réservez cette méthode au réseau interne** ou activez `ssl_auto_negotiate`.

### Méthode C — Inscription via API (automatisation)

```bash
# Obtenir un token API (manager)
TOKEN=$(curl -sk -u wazuh-wui:'<MDP>' https://localhost:55000/security/user/authenticate | grep -o '"token":"[^"]*"' | cut -d'"' -f4)
# Créer l'agent
curl -sk -X POST https://localhost:55000/agents \
  -H "Authorization: Bearer $TOKEN" -H 'Content-Type: application/json' \
  -d '{"name":"srv-web-01","ip":"10.0.1.15"}'
```

Pratique pour coupler à votre CMDB ou à la création de VM.

### Méthode D — Provisioning cloud-init / image dorée

Incluez l'agent + un script `agent-auth` au premier boot dans vos templates. **Attention** : ne clonez jamais une VM avec une clé déjà inscrite → doublons d'ID. La séquence sûre dans l'image : agent installé mais **non inscrit** ; inscription au premier démarrage via cloud-init.

### Comparatif

| Méthode | Échelle | Sécurité | Cas d'usage |
|---|---|---|---|
| Manuelle | < 10 agents | Bonne | Lab, exceptions |
| Mot de passe | 10 – 1000 | Moyenne (TLS conseillé) | Parc standard |
| API | 100+ | Bonne | Automatisation, CMDB |
| Cloud-init | 100+ | Bonne | Infra as Code |

---

## 21. Clés d'agent : gestion et rotation

La clé d'agent chiffre et authentifie le canal agent↔manager. Elle vit dans `/var/ossec/etc/client.keys` (agent) et dans la base du manager.

```bash
# Lister les clés (manager)
sudo /var/ossec/bin/manage_agents -l

# Extraire la clé d'un agent (pour réimport)
sudo /var/ossec/bin/manage_agents -e 007

# Supprimer un agent proprement (puis purger)
sudo /var/ossec/bin/manage_agents -r 007
```

### 21.1 Rotation des clés

Il n'y a pas de rotation automatique native : procédez par vague (groupe par groupe) :

```bash
# 1. Côté manager : supprimer l'ancienne inscription
sudo /var/ossec/bin/manage_agents -r 007
# 2. Côté agent : forcer une réinscription
sudo rm -f /var/ossec/etc/client.keys
sudo /var/ossec/bin/agent-auth -m 10.0.0.21 -P 'NouveauMotDePasse' -G linux-serveurs
sudo systemctl restart wazuh-agent
```

Planifiez une rotation annuelle et **après tout départ d'administrateur** ayant eu accès aux clés.

---

## 22. Groupes d'agents : l'arme de la configuration centralisée

Un groupe = un ensemble d'agents qui partagent des fichiers de configuration poussés par le manager.

```bash
# Créer un groupe (manager)
sudo /var/ossec/bin/agent_groups -a -g linux-web -q

# Assigner un agent à un groupe
sudo /var/ossec/bin/agent_groups -a -g linux-web -i 007 -q

# Lister
sudo /var/ossec/bin/agent_groups -l
```

Groupes conseillés pour démarrer :

| Groupe | Contenu |
|---|---|
| `linux-serveurs` | FIM `/etc`, audit SSH, inventaire |
| `linux-web` | + logs nginx/apache, FIM webroot |
| `windows-postes` | Event logs, FIM dossiers sensibles |
| `windows-serveurs` | + audit avancé, RDP |
| `dmz` | Politique restrictive, active response agressive |

Un agent peut appartenir à **plusieurs groupes** : la configuration est fusionnée (le groupe `default` s'applique toujours en base).

---

## 23. Configuration centralisée : pousser la conf aux agents

Les fichiers partagés vivent dans `/var/ossec/etc/shared/<groupe>/`. Le fichier magique : `agent.conf`.

```bash
sudo mkdir -p /var/ossec/etc/shared/linux-web
sudo nano /var/ossec/etc/shared/linux-web/agent.conf
```

```xml
<agent_config>
  <!-- Surveillance FIM du webroot -->
  <syscheck>
    <frequency>7200</frequency>
    <directories check_all="yes" realtime="yes">/var/www/html</directories>
  </syscheck>

  <!-- Collecte des logs nginx -->
  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/nginx/access.log</location>
  </localfile>
  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/nginx/error.log</location>
  </localfile>

  <!-- Inventaire logiciel hebdo -->
  <wodle name="syscollector">
    <disabled>no</disabled>
    <interval>1w</interval>
    <packages>yes</packages>
    <os>yes</os>
  </wodle>
</agent_config>
```

Le manager pousse automatiquement `agent.conf` aux agents du groupe (vérifiez avec `cluster_control` en multi-nœuds). L'agent applique au prochain redémarrage — ou **sans redémarrage** pour certains modules.

> ⚠️ Une erreur XML dans `agent.conf` = **aucun agent du groupe ne l'applique**. Validez toujours avec `/var/ossec/bin/wazuh-logtest` ou `xmllint` avant de pousser.

```bash
xmllint --noout /var/ossec/etc/shared/linux-web/agent.conf && echo "XML OK"
```

---

## 24. Vérifier la santé des agents

### 24.1 En ligne de commande (manager)

```bash
# Vue d'ensemble
sudo /var/ossec/bin/agent_control -l

# Détail d'un agent
sudo /var/ossec/bin/agent_control -i 007

# Agents déconnectés depuis plus de X
sudo /var/ossec/bin/agent_control -l | grep -i disconnected
```

### 24.2 Via l'API

```bash
curl -sk -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/agents?status=disconnected&pretty=true'
```

### 24.3 Les 4 états d'un agent

| État | Signification | Action |
|---|---|---|
| `active` | Connecté, envoie des données | Rien |
| `disconnected` | Ne répond plus (seuil : `notify_time` + `time`) | Voir section 68 |
| `pending` | Inscrit, jamais connecté | Vérifier l'inscription / réseau |
| `never_connected` | Inscrit mais aucune connexion historique | Agent jamais démarré ? |

### 24.4 Alerte sur agent silencieux

Wazuh peut alerter quand un agent se déconnecte (règle 501 / niveau 3). Pour une petite équipe, créez une règle surélevée (voir section 29) : un serveur qui cesse d'émettre un vendredi soir mérite un SMS, pas un log.

---

## 25. Désinstaller / réinscrire un agent proprement

```bash
# Linux : désinstallation propre
sudo systemctl stop wazuh-agent
sudo apt remove --purge wazuh-agent -y
sudo rm -rf /var/ossec

# Côté manager : purger l'inscription
sudo /var/ossec/bin/manage_agents -r <ID>

# Réinscription d'une machine réinstallée (évite les doublons) :
# 1. purger l'ancien ID côté manager
# 2. réinstaller l'agent
# 3. agent-auth avec le MÊME nom → nouvel ID, historique propre
```

> Ne réutilisez jamais un ancien `client.keys` sur une machine réinstallée : supprimez et réinscrivez. Les doublons d'ID sont une source classique d'alertes fantômes.

---

## 26. Le moteur de règles : comment Wazuh analyse les logs

Chaîne de traitement d'un événement :

```
Log brut (agent)
   │  1. DECODER : reconnaît le programme/source, extrait les champs
   ▼     (ex. sshd → srcip, dstuser, action)
Champs structurés
   │  2. RÈGLES : les règles sont évaluées par ID croissant ;
   ▼     la première qui matche peut en déclencher d'autres (if_sid)
Alerte (si level > seuil de log)
   │  3. Stockage alerts.json → Filebeat → Indexer → Dashboard
   ▼
Réponse active éventuelle
```

Points clés :

- Les règles sont dans `/var/ossec/ruleset/rules/` (ne **jamais** modifier : écrasé à chaque mise à jour).
- Vos règles vont dans `/var/ossec/etc/rules/local_rules.xml`.
- Une règle ne se déclenche que si le log a été **décodé** (decoder parent OK).
- `wazuh-logtest` (section 31) permet de simuler toute la chaîne.

---

## 27. Decoders : extraire du sens des logs bruts

Un decoder = une expression régulière nommée qui transforme `Failed password for root from 203.0.113.7 port 51234 ssh2` en champs `srcip=203.0.113.7`, `dstuser=root`.

```bash
# Où vivent les decoders
ls /var/ossec/ruleset/decoders/ | head -20
# Vos decoders :
sudo nano /var/ossec/etc/decoders/local_decoder.xml
```

Anatomie d'un decoder :

```xml
<decoder name="mon-app-web">
  <prematch>^monapp: </prematch>          <!-- filtre rapide -->
  <regex>^monapp: user=(\\S+) action=(\\S+) ip=(\\S+)</regex>
  <order>dstuser, action, srcip</order>   <!-- mappe les groupes -->
</decoder>
```

- `<prematch>` : testé en premier, peu coûteux. Mettez-y une chaîne distinctive.
- `<regex>` : groupes capturants dans l'ordre de `<order>`.
- Les champs extraits deviennent utilisables dans les règles (`<field name="srcip">`).

Decoders parents/enfants : un decoder peut affiner un decoder générique via `<parent>`. Exemple : le decoder `sshd` est parent de dizaines de sous-decoders (succès, échec, clé invalide...).

---

## 28. Écrire son premier decoder personnalisé

Cas concret : votre application métier logue dans `/var/log/monapp/app.log` :

```
2026-09-26 22:10:01 [WARN] user=jdupont action=export ip=10.0.5.23 file=clients.csv
```

### Étape 1 — Collecter le log côté agent

```xml
<!-- /var/ossec/etc/shared/mon-groupe/agent.conf -->
<agent_config>
  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/monapp/app.log</location>
  </localfile>
</agent_config>
```

### Étape 2 — Écrire le decoder (manager)

```xml
<!-- /var/ossec/etc/decoders/local_decoder.xml -->
<decoder name="monapp-base">
  <prematch>user=</prematch>
  <regex>^(\S+ \S+) \[(\w+)\] user=(\S+) action=(\S+) ip=(\S+) file=(\S+)</regex>
  <order>timestamp, loglevel, dstuser, action, srcip, filename</order>
</decoder>
```

### Étape 3 — Tester avec wazuh-logtest

```bash
sudo /var/ossec/bin/wazuh-logtest
# Collez la ligne de log, observez :
# **Phase 1: Completed decoding.
#        decoder: 'monapp-base'
#        srcip: '10.0.5.23'
#        dstuser: 'jdupont'
#        action: 'export'
```

Si le decoder ne matche pas : vérifiez les échappements (`\\S` dans XML), l'ordre des groupes, et que `<prematch>` est bien présent dans la ligne.

---

## 29. Écrire sa première règle personnalisée

On reprend le decoder `monapp-base` : alerter quand quelqu'un exporte `clients.csv` (donnée sensible).

```xml
<!-- /var/ossec/etc/rules/local_rules.xml -->
<group name="monapp,">
  <rule id="100100" level="10">
    <decoded_as>monapp-base</decoded_as>
    <field name="action">export</field>
    <field name="filename">clients\.csv</field>
    <description>Export sensible clients.csv par $(dstuser) depuis $(srcip)</description>
    <mitre>
      <id>T1048</id>   <!-- Exfiltration Over Alternative Protocol (exemple) -->
    </mitre>
  </rule>
</group>
```

Points d'attention :

- **IDs 100000–199999** réservés aux règles locales (jamais de collision avec le ruleset officiel).
- `$(dstuser)` : référence un champ décodé dans la description.
- Testez avec `wazuh-logtest` : la phase 2 doit afficher `**Rule 100100 matched.`
- Rechargez les règles : `sudo systemctl reload wazuh-manager` (ou redémarrage complet si doute).

---

## 30. Niveaux de règles (0 à 16) : la grammaire de la sévérité

| Niveau | Signification | Exemples |
|---|---|---|
| 0 | Ignoré (pas d'alerte) | Bruit filtré |
| 1 | Événement basique | Démarrage service |
| 2 | Notification système | — |
| 3 | Événement notable | **Agent déconnecté**, connexion réussie inhabituelle |
| 4 | Erreur système | Échec de service |
| 5 | Erreur utilisateur | Mauvais mot de passe isolé |
| 6 | Attaque de faible pertinence | Scan de ports léger |
| 7 | Mauvaise configuration / « bad word » | Mot-clé suspect |
| 8 | Première occurrence / sévérité notable | Premier succès après échecs |
| 9 | Erreur d'application critique | — |
| 10 | **Attaque confirmée** | Force brute SSH réussie à bloquer |
| 11 | Erreur d'intégrité | Fichier modifié (FIM) |
| 12 | Attaque très pertinente | Exploitation probable |
| 13 | Corrélation / anomalie | Comportement anormal |
| 14 | Corrélation temporelle forte | Plusieurs attaques liées |
| 15 | Compromission probable | **Alerte critique, agir immédiatement** |
| 16 | Attaque active en cours | Rare, réponse immédiate |

Règle d'or pour une petite équipe : **tout ce qui est ≥ 10 doit arriver sur un canal qui réveille** (e-mail prioritaire, webhook, SMS via passerelle). Le reste se traite en revue quotidienne (section 81).

---

## 31. Tester ses règles sans casser la production

`wazuh-logtest` simule decoders + règles sans toucher à la production :

```bash
sudo /var/ossec/bin/wazuh-logtest
```

Session type :

```
Type one log per line.

Sep 26 22:15:01 srv-web sshd[1234]: Failed password for root from 203.0.113.7 port 51234 ssh2

**Phase 1: Completed decoding.
       decoder: 'sshd'
       srcip: '203.0.113.7'
       dstuser: 'root'

**Phase 2: Completed filtering (rules).
       Rule id: '5716'
       Level: '5'
       Description: 'SSHD: authentication failed.'
```

Bonnes pratiques :

1. Testez **chaque** règle avant déploiement.
2. Testez aussi les **non-déclenchements** (une ligne normale ne doit pas matcher).
3. Gardez un fichier de lignes de test : `/root/wazuh-tests/echantillons.log` avec cas positifs et négatifs.

```bash
# Rejouer un fichier de test complet
while IFS= read -r ligne; do
  echo "$ligne" | sudo /var/ossec/bin/wazuh-logtest 2>/dev/null | grep -E "Rule id|Level"
done < /root/wazuh-tests/echantillons.log
```

---

## 32. Organiser ses règles : local_rules, CDB lists

### 32.1 Structure conseillée

```
/var/ossec/etc/rules/
├── local_rules.xml          # vos règles métier (inclus par défaut)
├── soc_tuning.xml           # surcharges : désactivation / baisse de niveau
└── listes-noires.xml        # règles basées sur CDB lists
/var/ossec/etc/lists/
├── bad-ips.txt              # une IP par ligne : 203.0.113.7:
├── comptes-sensibles.txt    # root: / admin: / backup:
└── ...
```

Incluez vos fichiers dans `ossec.conf` du manager :

```xml
<ruleset>
  <rule_dir>ruleset/rules</rule_dir>
  <decoder_dir>ruleset/decoders</decoder_dir>
  <rule_exclude>0215-policy_rules.xml</rule_exclude>  <!-- exemple : exclure un lot -->
</ruleset>
```

Et déclarez les listes :

```xml
<ruleset>
  <list>etc/lists/bad-ips.txt</list>
</ruleset>
```

### 32.2 Règle basée sur CDB list

```
# /var/ossec/etc/lists/bad-ips.txt  (format clé: — le ":" final est obligatoire)
203.0.113.7:
198.51.100.23:
```

```xml
<rule id="100200" level="12">
  <if_sid>5700</if_sid>
  <list field="srcip" lookup="address_match_key">etc/lists/bad-ips.txt</list>
  <description>Connexion depuis une IP inscrite sur liste noire SOC : $(srcip)</description>
</rule>
```

### 32.3 Surcharger une règle officielle (sans la modifier)

```xml
<!-- /var/ossec/etc/rules/soc_tuning.xml -->
<group name="tuning,">
  <!-- Baisser le niveau d'une règle trop bruyante -->
  <rule id="5710" level="3" overwrite="yes">
    <description>SSHD: Attempt to login using a non-existent user (bruit réduit)</description>
  </rule>
  <!-- Désactiver proprement -->
  <!-- <rule id="510" level="0" overwrite="yes" /> = ne JAMAIS faire : préférez l'exclusion ciblée -->
</group>
```

> ⚠️ `overwrite="yes"` sur une règle officielle : documentez **pourquoi** dans un commentaire XML. Dans 6 mois, personne ne s'en souviendra sinon.

---

## 33. FIM — surveillance d'intégrité des fichiers : principes

Le FIM (*File Integrity Monitoring*, module `syscheck`) calcule des empreintes (MD5, SHA1, SHA256) des fichiers surveillés et alerte à **toute modification** : contenu, permissions, propriétaire, taille.

C'est votre détecteur de :
- webshell déposé sur un serveur web,
- binaire système remplacé (rootkit),
- modification de `/etc/passwd`, `/etc/shadow`, des tâches cron,
- changement de configuration non déclaré.

Cycle : **scan initial** (baseline) → scans périodiques → comparaison → alerte si différence. La baseline initiale génère un pic d'alertes : normal, ne paniquez pas (voir section 50 pour le tuning).

---

## 34. Configurer syscheck (FIM) sur Linux

Via `agent.conf` du groupe (recommandé) ou `/var/ossec/etc/ossec.conf` de l'agent :

```xml
<agent_config>
  <syscheck>
    <frequency>7200</frequency>          <!-- scan complet toutes les 2h -->
    <scan_on_start>yes</scan_on_start>

    <!-- Dossiers critiques système -->
    <directories check_all="yes" realtime="yes" report_changes="yes">/etc,/usr/bin,/usr/sbin</directories>
    <directories check_all="yes" realtime="yes" report_changes="yes">/bin,/sbin,/boot</directories>

    <!-- Webroot applicatif -->
    <directories check_all="yes" realtime="yes" report_changes="yes" restrict="\.php$">/var/www/html</directories>

    <!-- Exclusions : bruit inutile -->
    <ignore>/etc/mtab</ignore>
    <ignore>/etc/hosts.deny</ignore>
    <ignore type="sregex">^/etc/.*~$</ignore>
    <ignore>/var/log</ignore>
    <ignore>/tmp</ignore>

    <!-- Ne pas suivre ces montages -->
    <skip_nfs>yes</skip_nfs>
    <skip_dev>yes</skip_dev>
    <skip_proc>yes</skip_proc>
    <skip_sys>yes</skip_sys>
  </syscheck>
</agent_config>
```

Options de `check_*` (au lieu de `check_all`) :

| Attribut | Surveille |
|---|---|
| `check_md5sum="yes"` | Empreinte MD5 |
| `check_sha1sum="yes"` | Empreinte SHA1 |
| `check_sha256sum="yes"` | Empreinte SHA256 |
| `check_size="yes"` | Taille |
| `check_owner="yes"` | Propriétaire |
| `check_group="yes"` | Groupe |
| `check_perm="yes"` | Permissions |
| `check_mtime="yes"` | Date de modification |
| `check_inode="yes"` | Inode |
| `report_changes="yes"` | **Inclut le diff du contenu dans l'alerte** (précieux !) |

> `report_changes="yes"` sur des fichiers texte de config = vous voyez **exactement** quelle ligne a changé. Sur des binaires, inutile (et volumineux) : limitez-le aux fichiers texte.

---

## 35. Configurer syscheck (FIM) sur Windows

```xml
<agent_config os="Windows">
  <syscheck>
    <frequency>7200</frequency>
    <scan_on_start>yes</scan_on_start>

    <directories check_all="yes" realtime="yes">C:\Windows\System32</directories>
    <directories check_all="yes" realtime="yes">C:\Windows\SysWOW64</directories>
    <directories check_all="yes">C:\Program Files</directories>
    <directories check_all="yes">C:\Program Files (x86)</directories>

    <!-- Registre : clés sensibles -->
    <windows_registry arch="both">HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run</windows_registry>
    <windows_registry arch="both">HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services</windows_registry>
    <windows_registry>HKEY_LOCAL_MACHINE\Software\Microsoft\Windows NT\CurrentVersion\Winlogon</windows_registry>

    <!-- Exclusions : bruit Windows -->
    <ignore>C:\Windows\System32\winevt\Logs</ignore>
    <ignore>C:\Windows\Temp</ignore>
    <ignore type="sregex">\.log$</ignore>
    <ignore type="sregex">\.tmp$</ignore>
  </syscheck>
</agent_config>
```

Le FIM registre est une pépite : 90 % des persistances malware Windows passent par `Run` ou les services. Une alerte « clé de registre ajoutée dans Run » un samedi à 3h = investigation immédiate.

---

## 36. FIM temps réel vs who-data : bien choisir

| Mode | Fonctionnement | Avantages | Inconvénients |
|---|---|---|---|
| **Planifié** (`frequency`) | Scan périodique complet | Simple, peu gourmand | Détection différée (jusqu'à `frequency`) |
| **Temps réel** (`realtime="yes"`) | Notification immédiate (inotify/Win32 API) | Détection instantanée | Plus gourmand sur dossiers très actifs |
| **Who-data** (`whodata="yes"`) | Auditd (Linux) : **qui** a modifié | Identifie l'utilisateur/processus | Linux seul, auditd requis, plus lourd |

Recommandation :

```xml
<!-- Binaires système : temps réel, on veut savoir TOUT DE SUITE -->
<directories check_all="yes" realtime="yes">/usr/bin,/usr/sbin,/bin,/sbin</directories>

<!-- Fichiers de conf : who-data pour savoir QUI a changé quoi -->
<directories check_all="yes" whodata="yes" report_changes="yes">/etc/ssh,/etc/sudoers.d</directories>

<!-- Gros dossiers applicatifs : planifié pour ne pas saturer -->
<directories check_all="yes">/opt/monapp</directories>
```

Prérequis who-data sur Linux : `auditd` installé et démarré sur l'agent.

```bash
sudo apt install -y auditd
sudo systemctl enable --now auditd
```

> ⚠️ **Ne mettez jamais `realtime` sur `/var/log`** : chaque écriture de log déclencherait un scan → boucle infernale et explosion d'EPS.

---

## 37. Exploiter les alertes FIM au quotidien

Une alerte FIM typique (niveau 7 par défaut, règles 550-557) :

```json
{
  "rule": { "id": "550", "level": 7, "description": "Integrity checksum changed." },
  "syscheck": {
    "path": "/etc/passwd",
    "mode": "whodata",
    "user_name": "root",
    "process_name": "/usr/sbin/useradd",
    "changed_attributes": ["mtime", "sha256_after", "size_after"],
    "diff": "+jdupont:x:1001:1001::/home/jdupont:/bin/bash\n"
  }
}
```

Lecture SOC :

1. **Quel fichier ?** `/etc/passwd` = sensible.
2. **Qui ?** `user_name: root`, `process_name: /usr/sbin/useradd` = ajout d'utilisateur légitime probable.
3. **Quoi ?** le `diff` montre la ligne ajoutée.
4. **Contexte** : y a-t-il un ticket de création de compte ? Si non → investigation.

Règles de triage FIM :

| Signal | Interprétation | Action |
|---|---|---|
| Fichier binaire modifié + `realtime`, heure ouvrée, package manager actif | Mise à jour logicielle | Vérifier les logs apt/yum, classer |
| Fichier web `.php` modifié la nuit | Compromission probable | Isoler, analyser (cas pratique 3) |
| `/etc/shadow` modifié + who-data `unknown` | Suspect | Investigation immédiate |
| Dizaines de fichiers modifiés d'un coup | Mise à jour système OU ransomware | Corréler avec l'activité (cas pratique 2) |

**Montez le niveau** des FIM critiques via `local_rules.xml` :

```xml
<rule id="100300" level="12">
  <if_sid>550</if_sid>
  <field name="file">/etc/shadow</field>
  <description>FIM CRITIQUE : /etc/shadow modifié !</description>
</rule>
```

---

## 38. Détection de vulnérabilités : inventaire logiciel

Le module **Vulnerability Detector** (manager) croise l'inventaire logiciel remonté par les agents (module `syscollector`) avec les flux CVE (NVD, Canonical, Microsoft, Red Hat...).

Ce que ça donne : pour chaque agent, la liste des CVE le concernant, avec score CVSS, sévérité et références. C'est votre **tableau de bord patch management**.

Prérequis côté agent : `syscollector` activé (inventaire paquets/OS). Il l'est par défaut sur les agents 4.x récents ; vérifiez :

```bash
# Sur l'agent : forcer un inventaire immédiat (au lieu d'attendre l'intervalle)
sudo /var/ossec/bin/wazuh-control restart   # syscollector tourne au démarrage
sudo grep -i syscollector /var/ossec/logs/ossec.log | tail -5
```

---

## 39. Configurer le module vulnerability detector

Sur le **manager**, `/var/ossec/etc/ossec.conf` :

```xml
<vulnerability-detector>
  <enabled>yes</enabled>
  <interval>1d</interval>              <!-- rescan quotidien -->
  <min_full_scan_interval>1d</min_full_scan_interval>
  <run_on_start>yes</run_on_start>

  <!-- Fournisseurs : laissez ceux de vos OS -->
  <provider name="canonical">
    <enabled>yes</enabled>
    <os>jammy</os>      <!-- Ubuntu 22.04 -->
    <os>focal</os>      <!-- Ubuntu 20.04 -->
    <update_interval>1h</update_interval>
  </provider>
  <provider name="debian">
    <enabled>yes</enabled>
    <os>bookworm</os>   <!-- Debian 12 -->
    <os>bullseye</os>   <!-- Debian 11 -->
    <update_interval>1h</update_interval>
  </provider>
  <provider name="redhat">
    <enabled>yes</enabled>
    <os>9</os>
    <update_interval>1h</update_interval>
  </provider>
  <provider name="nvd">
    <enabled>yes</enabled>
    <update_interval>1h</update_interval>
  </provider>
  <provider name="msu">
    <enabled>yes</enabled>
    <update_interval>1h</update_interval>
  </provider>
</vulnerability-detector>

<!-- Indexer : stocker l'inventaire -->
<indexer>
  <enabled>yes</enabled>
  <hosts>
    <host>https://127.0.0.1:9200</host>
  </hosts>
  <username>admin</username>
  <password><MOT_DE_PASSE></password>
  <ssl>
    <certificate_authorities>/etc/filebeat/certs/root-ca.pem</certificate_authorities>
    <certificate>/etc/filebeat/certs/filebeat.pem</certificate>
    <key>/etc/filebeat/certs/filebeat-key.pem</key>
  </ssl>
</indexer>
```

> ⚠️ **Ne mettez jamais de mot de passe en clair dans un exemple partagé.** En production, le bloc `<indexer>` est généré par l'assistant avec les bons secrets ; ne l'éditez qu'en connaissance de cause.

Vérification :

```bash
sudo grep -i "vulnerability" /var/ossec/logs/ossec.log | tail -10
# Attendu : téléchargement des bases CVE, puis "Vulnerability scan finished"
```

Le premier scan télécharge plusieurs centaines de Mo de bases CVE : **prévoyez le temps et l'espace disque** (`/var/ossec/queue/vulnerabilities/`).

---

## 40. Lire et prioriser les alertes de vulnérabilités

Dashboard → *Vulnerability Detection*. Chaque CVE affiche : score CVSS, sévérité, paquet concerné, version installée, version corrigée, agent(s).

Matrice de priorisation conseillée (petite équipe) :

| CVSS | Exposition | Action | Délai |
|---|---|---|---|
| ≥ 9.0 (Critique) | Internet (DMZ, frontal web) | Patch immédiat ou mitigation | 24–48 h |
| ≥ 9.0 | Interne | Planifier | 7 jours |
| 7.0–8.9 (Haute) | Internet | Planifier prioritaire | 14 jours |
| 7.0–8.9 | Interne | Cycle de patch normal | 30 jours |
| < 7.0 | — | Regrouper par lot | 90 jours |

Rituel hebdomadaire : exportez la liste des CVE critiques/hautes, croisez avec vos fenêtres de maintenance, suivez dans un tableau (ou ticket). Wazuh détecte, **vous** pilotez le patch management.

Faux positifs fréquents : CVE sur des paquets dont la distribution a **backporté** le correctif sans changer le numéro de version (classique Debian/Ubuntu). Vérifiez le changelog du paquet avant de paniquer :

```bash
# Exemple : vérifier si le CVE est corrigé sur Ubuntu
apt changelog <paquet> | grep -i CVE-2026-XXXX
```

---

## 41. SCA — audit de conformité CIS : principes

Le module **SCA** (*Security Configuration Assessment*) exécute des **politiques** (fichiers YAML) : une série de contrôles (ex. « le pare-feu est actif », « le compte root ne se connecte pas en SSH »). Chaque contrôle = *pass*, *fail* ou *not applicable*.

Politiques fournies (4.x) : **CIS** pour Debian, Ubuntu, RHEL, Windows 10/Server 2019+, macOS ; aussi PCI-DSS, NIST 800-53, TSC.

C'est votre outil pour :
- préparer un audit (interne ou client),
- mesurer la dérive de configuration dans le temps,
- prouver la conformité (« 92 % des contrôles CIS passent sur le parc »).

---

## 42. Activer et lire les audits SCA

SCA est actif par défaut sur les agents 4.x. Vérifiez côté agent :

```bash
sudo grep -A5 "<sca>" /var/ossec/etc/ossec.conf
```

```xml
<!-- Extrait ossec.conf agent Linux -->
<sca>
  <enabled>yes</enabled>
  <scan_on_start>yes</scan_on_start>
  <interval>12h</interval>
  <skip_nfs>yes</skip_nfs>
  <policies>
    <policy>/var/ossec/ruleset/sca/cis_debian12.yml</policy>
  </policies>
</sca>
```

Lecture : Dashboard → *Security Configuration Assessment* → choisissez l'agent → score global + détail par contrôle. Chaque contrôle *fail* donne la **remédiation** (la politique inclut `remediation`).

Automatisez le suivi :

```bash
# Via API : récupérer le dernier scan SCA d'un agent
curl -sk -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/sca/007?pretty=true' | grep -E '"pass"|"fail"|"score"'
```

> Les scans SCA consomment du CPU agent pendant quelques minutes : planifiez `interval` hors heures de pointe pour les serveurs chargés.

---

## 43. Écrire une politique SCA personnalisée

Vos exigences internes (ex. « l'antivirus d'entreprise doit tourner », « tel montage doit être chiffré ») n'existent pas dans CIS → écrivez votre politique.

```yaml
# /var/ossec/etc/shared/linux-serveurs/sca_metier.yml
policy:
  id: "soc_metier_linux"
  file: "sca_metier.yml"
  name: "Politique interne - serveurs Linux"
  description: "Contrôles métier du SOC."
  references:
    - "Interne: POL-SEC-001"

requirements:
  title: "Assurer le socle de sécurité interne"
  description: "Contrôles non couverts par CIS."
  condition: all

checks:
  - id: 10001
    title: "L'agent Wazuh est actif"
    description: "L'agent doit tourner en permanence."
    rationale: "Sans agent, aucune supervision."
    remediation: "systemctl enable --now wazuh-agent"
    compliance:
      - cis: ["1.1.1"]
    rules:
      - 'c:systemctl is-active wazuh-agent -> r:active'

  - id: 10002
    title: "Aucun compte avec UID 0 autre que root"
    description: "Un seul UID 0 autorisé."
    remediation: "Supprimer ou corriger le compte en trop."
    rules:
      - 'c:awk -F: ''$3 == 0 {print $1}'' /etc/passwd -> r:root && c:awk -F: ''$3==0'' /etc/passwd | wc -l -> r:^1$'

  - id: 10003
    title: "Le pare-feu est actif"
    description: "UFW ou nftables doit filtrer."
    remediation: "ufw enable"
    condition: any
    rules:
      - 'c:ufw status -> r:active'
      - 'c:nft list ruleset -> r:table'
```

Déclarez-la dans l'`agent.conf` du groupe :

```xml
<agent_config>
  <sca>
    <policies>
      <policy>/var/ossec/etc/shared/linux-serveurs/sca_metier.yml</policy>
    </policies>
  </sca>
</agent_config>
```

> Testez chaque check **manuellement** sur une machine avant de déployer : un check mal écrit = 100 % de *fail* et un SOC qui n'y croit plus.

---

## 44. Active Response : le blocage automatique

L'**Active Response** exécute un script (sur l'agent, ou sur le manager) quand une règle se déclenche. Cas d'usage roi : **bannir automatiquement l'IP** qui fait du force brute SSH.

Chaîne :

```
Règle 5763 (force brute SSH, niveau 10)
   │  <active-response> associé dans ossec.conf
   ▼
Manager → ordre à l'agent : exécute firewall-drop.sh add <ip>
   │  L'agent ajoute une règle iptables/nftables DROP
   ▼  Après <timeout> (ex. 10 min), l'agent retire la règle (delete)
```

Deux modes :

| Mode | Où s'exécute | Usage |
|---|---|---|
| Agent (défaut) | Sur l'agent qui a levé l'alerte | Bloquer l'attaquant au plus près |
| Manager (`location: server`) | Sur le manager | Centraliser (ex. alimenter un pare-feu central via script custom) |

> ⚠️ L'active response est une **arme** : un faux positif = vous vous bannissez vous-même (ou un client). Toujours : allowlist (section 45), timeout court au début, tests en lab.

---

## 45. Configurer le blocage automatique d'attaque IP (firewall-drop)

### Étape 1 — Activer côté manager

`/var/ossec/etc/ossec.conf` du **manager** :

```xml
<command>
  <name>firewall-drop</name>
  <executable>firewall-drop.sh</executable>
  <timeout_allowed>yes</timeout_allowed>
</command>

<active-response>
  <command>firewall-drop</command>
  <location>local</location>              <!-- sur l'agent source -->
  <rules_id>5763</rules_id>               <!-- force brute SSH (exemple) -->
  <timeout>600</timeout>                  <!-- 10 minutes -->
</active-response>
```

Règles candidates au blocage auto (à adapter) :

| Règle | Description | Timeout conseillé |
|---|---|---|
| 5763 | Force brute SSH (niveau 10) | 600 s |
| 31151 | Force brute web (selon ruleset) | 600 s |
| 100100+ | Vos règles métier critiques | 1800 s |

### Étape 2 — Allowlist : ne jamais se bannir soi-même

`/var/ossec/etc/ossec.conf` du manager :

```xml
<global>
  <white_list>127.0.0.1</white_list>
  <white_list>10.0.0.0/8</white_list>       <!-- votre LAN admin -->
  <white_list>198.51.100.0/24</white_list>  <!-- IP publiques du SOC / VPN -->
</global>
```

> Mettez-y **toutes** vos IP de management, votre VPN, votre prestataire infogéré. Le jour où vous testez un scan depuis votre poste, vous serez content de l'avoir fait.

### Étape 3 — Vérifier que ça marche (lab !)

```bash
# Depuis une machine de test NON allowlistée, bourrinez SSH :
for i in $(seq 1 12); do ssh -o ConnectTimeout=2 test@<cible> <<< "mauvais"; done
# Sur la cible :
sudo iptables -L -n | grep DROP     # ou : sudo nft list set inet filter ...
sudo tail -5 /var/ossec/logs/active-responses.log
```

Vous devez voir l'IP bannie puis **débannie** après le timeout. Si le débannissement n'a pas lieu : vérifiez `timeout_allowed` et les droits du script.

---

## 46. Active Response avancé : scripts personnalisés

Les scripts vivent dans `/var/ossec/active-response/bin/` (manager **et** agents : le script doit exister **où il s'exécute**).

Exemple : script qui **désactive un compte local** après 5 échecs (Linux) :

```bash
#!/bin/bash
# /var/ossec/active-response/bin/disable-account.sh
# Usage : appelé par Wazuh avec les variables d'environnement (alert JSON sur stdin)
read -r ALERT
USER=$(echo "$ALERT" | grep -o '"dstuser":"[^"]*"' | head -1 | cut -d'"' -f4)
ACTION=$1   # add | delete

if [ "$ACTION" = "add" ] && [ -n "$USER" ] && [ "$USER" != "root" ]; then
  /usr/sbin/usermod -L "$USER"
  logger -t wazuh-ar "Compte $USER verrouillé par active-response"
fi
exit 0
```

```bash
sudo chmod 750 /var/ossec/active-response/bin/disable-account.sh
sudo chown root:wazuh /var/ossec/active-response/bin/disable-account.sh
```

Déclaration côté manager :

```xml
<command>
  <name>disable-account</name>
  <executable>disable-account.sh</executable>
  <timeout_allowed>no</timeout_allowed>
  <expect>dstuser</expect>
</command>

<active-response>
  <command>disable-account</command>
  <location>local</location>
  <rules_id>100400</rules_id>   <!-- votre règle "5 échecs sur compte sensible" -->
</active-response>
```

> Déployez le script via les **fichiers partagés du groupe** (`/var/ossec/etc/shared/<groupe>/` ne pousse que la conf...). Pour les scripts, utilisez votre outil de conf (Ansible) ou copiez-les manuellement : **un active-response qui pointe vers un script absent échoue silencieusement** côté agent.

Autres idées de scripts : couper l'interface réseau d'un poste suspect (`quarantine.sh`), redémarrer un service, prendre un snapshot mémoire (forensique), appeler l'API de votre EDR.

---

## 47. Correspondance MITRE ATT&CK

Wazuh mappe nativement ses règles sur la matrice **MITRE ATT&CK** (tactiques, techniques, IDs comme `T1110` *Brute Force*).

Où le voir : Dashboard → *Threat Intelligence → MITRE ATT&CK*. Chaque alerte enrichie affiche tactique (ex. *Credential Access*) et technique.

Ajouter MITRE à vos règles (vu section 29) :

```xml
<rule id="100100" level="10">
  ...
  <mitre>
    <id>T1048</id>
    <id>T1020</id>
  </mitre>
  <group>exfiltration,</group>
</rule>
```

Usage SOC concret :

1. **Revue hebdo par tactique** : « cette semaine, 80 % de nos alertes niveau ≥ 10 sont du *Credential Access* » → priorisez le durcissement des mots de passe / MFA.
2. **Couverture** : identifiez les tactiques **sans aucune règle** chez vous (ex. *Lateral Movement*) → écrivez les règles manquantes.
3. **Reporting direction** : la heatmap MITRE parle mieux aux non-techniques qu'une liste de règles.

---

## 48. Gestion des alertes : cycle de vie

```
Détection → Qualification → Investigation → Réponse → Clôture → Retour d'expérience
```

### 48.1 Qualification (triage)

Pour chaque alerte niveau ≥ 7, répondez à 4 questions :

1. **C'est quoi ?** (règle, machine, utilisateur, IP)
2. **C'est attendu ?** (maintenance, batch, admin légitime)
3. **C'est grave ?** (niveau, MITRE, criticité de l'actif)
4. **J'agis quand ?** (immédiat / aujourd'hui / backlog tuning)

### 48.2 Statuts de traitement (convention d'équipe)

| Statut | Signification |
|---|---|
| `Nouveau` | Non qualifié |
| `En cours` | Un analyste investigue |
| `Faux positif` | Bruit → **crée un ticket de tuning** (section 50) |
| `Bénin expliqué` | Légitime mais à surveiller |
| `Incident` | Attaque réelle → procédure incident |
| `Clos` | Traité, documenté |

Wazuh ne gère pas nativement ce workflow : utilisez **TheHive** (section 54) ou, à défaut, un tableau partagé. L'important : **aucune alerte ≥ 10 ne reste « Nouveau » plus de 24 h**.

### 48.3 Investigation : les 3 pivots

Depuis une alerte, pivotez toujours sur :
- **l'agent** : que s'est-il passé sur cette machine ± 1 h ?
- **l'IP source** : a-t-elle touché d'autres machines ? (recherche `srcip` sur 7 jours)
- **l'utilisateur** : ses autres connexions sont-elles normales ? (géographie, horaires)

Requête dashboard type (KQL) :

```
rule.level >= 10 and agent.name : "srv-web-01"
```

---

## 49. Niveaux de sévérité : que faire pour chaque niveau

| Niveau | Canal | Délai de traitement | Exemple d'action |
|---|---|---|---|
| 0–2 | Aucun (log seul) | — | Rien |
| 3–6 | Dashboard (revue quotidienne) | 24 h | Qualifier, tuner si bruit |
| 7–9 | Dashboard + e-mail groupé | 4 h ouvrées | Investiguer |
| 10–12 | **Alerte push** (e-mail prioritaire, webhook) | 1 h | Investiguer + active response auto |
| 13–16 | **Réveil d'astreinte** | Immédiat | Procédure incident |

> Formalisez ce tableau dans votre **procédure SOC** (même une page) : en pleine nuit, on ne réfléchit pas, on exécute.

---

## 50. Tuning : réduire les faux positifs

Le tuning est **le** travail continu du SOC. Sans lui, Wazuh devient une machine à bruit que plus personne ne lit.

### 50.1 Mesurer avant de tuner

Dashboard → *Discover* : comptez les alertes par `rule.id` sur 7 jours. Les 10 règles les plus bruyantes = vos cibles.

```bash
# Top 10 des règles bruyantes via l'API (7 derniers jours, exemple)
curl -sk -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/alerts?pretty=true&limit=1' | head -5
# Pour l'agrégation fine, utilisez le dashboard (visualisation par rule.id)
```

### 50.2 Les 5 techniques de tuning

**1. Exclure des chemins/IP bruit (global)**

```xml
<global>
  <white_list>10.0.0.5</white_list>  <!-- sonde de supervision qui scanne -->
</global>
```

**2. Baisser le niveau d'une règle bruyante** (voir `soc_tuning.xml`, section 32.3).

**3. Restreindre une règle à un contexte** avec `<if_sid>` + conditions :

```xml
<!-- N'alerter sur les échecs sudo QUE pour les comptes sensibles -->
<rule id="100500" level="10">
  <if_sid>5402</if_sid>
  <list field="dstuser" lookup="address_match_key">etc/lists/comptes-sensibles.txt</list>
  <description>Échec sudo répété sur compte sensible : $(dstuser)</description>
</rule>
```

**4. Ignorer via `rule_exclude`** un lot entier non pertinent chez vous :

```xml
<ruleset>
  <rule_exclude>0400-imapd_rules.xml</rule_exclude>  <!-- pas de serveur IMAP -->
</ruleset>
```

**5. Ajuster les seuils de fréquence** (ex. brute force) :

```xml
<!-- Par défaut la règle 5763 se base sur 8 échecs ; resserrez ou élargissez -->
<rule id="5763" level="10" frequency="12" timeframe="120" overwrite="yes">
  <if_matched_sid>5716</if_matched_sid>
  <description>sshd: brute force attack (seuil SOC : 12/120s).</description>
</rule>
```

### 50.3 Rituel de tuning

- **Quotidien (15 min)** : qualifier les nouvelles alertes, taguer les faux positifs.
- **Hebdomadaire (1 h)** : top 10 bruit → 1–2 actions de tuning.
- **Mensuel** : revoir les règles `overwrite`, vérifier qu'elles sont toujours pertinentes.

> Un bon objectif : **moins de 50 alertes/jour/analyste à qualifier** après 3 mois de tuning. Au-delà, le SOC sature et rate les vraies attaques.

---

## 51. Supervision réseau via agents (état des interfaces, ports)

Le module `syscollector` remonte aussi l'état réseau ; le module `command` (wodle) exécute des commandes périodiques dont la sortie est analysée comme un log.

Exemple : surveiller les **ports en écoute** d'un serveur (détection d'un backdoor qui ouvre un port) :

```xml
<!-- agent.conf du groupe linux-serveurs -->
<agent_config>
  <wodle name="command">
    <disabled>no</disabled>
    <command>ss -tlnp</command>
    <interval>1h</interval>
    <ignore_output>no</ignore_output>
    <run_on_start>yes</run_on_start>
    <timeout>0</timeout>
  </wodle>
</agent_config>
```

```xml
<!-- Règle : alerter si un port inhabituel écoute -->
<rule id="100600" level="10">
  <if_sid>530</if_sid>   <!-- règle générique wodle command -->
  <regex>:(4444|31337|1337|6667)\s</regex>
  <description>Port suspect en écoute : possible backdoor ($(full_log))</description>
  <mitre><id>T1205</id></mitre>
</rule>
```

Autres commandes utiles en wodle : `ip addr` (changement d'IP), `arp -a`, `iptables -L -n` (règle pare-feu modifiée), `df -h` (disque plein, voir section 72).

---

## 52. Collecter les logs d'équipements réseau (syslog)

Switchs, pare-feu, **onduleurs**, bornes Wi-Fi : la plupart parlent syslog. Deux options :

### Option A — Syslog direct vers le manager

Sur le manager, activez la réception syslog (port 514) :

```xml
<!-- /var/ossec/etc/ossec.conf du MANAGER -->
<remote>
  <connection>syslog</connection>
  <port>514</port>
  <protocol>udp</protocol>
  <allowed-ips>10.0.0.0/8</allowed-ips>
  <local_ip>10.0.0.21</local_ip>
</remote>
```

```bash
# Autoriser le port
sudo ufw allow 514/udp comment 'Syslog equipements'
sudo systemctl restart wazuh-manager
```

Côté équipement : pointez son syslog vers `10.0.0.21:514`. Chaque équipement apparaît comme un « agent » syslog dans le dashboard.

### Option B — Via un agent relais (recommandé)

Un agent Linux collecte en local (rsyslog) puis envoie au manager via le canal chiffré 1514 :

```bash
# Sur l'agent relais : rsyslog écoute les équipements
# /etc/rsyslog.d/10-equipements.conf
module(load="imudp")
input(type="imudp" port="514" ruleset="tofile")
template(name="EqFmt" type="string" string="%timegenerated% %hostname% %msg%\n")
ruleset(name="tofile") {
  action(type="omfile" file="/var/log/equipements.log" template="EqFmt")
}
```

```xml
<!-- agent.conf : Wazuh lit ce fichier -->
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/equipements.log</location>
</localfile>
```

> L'option B est **chiffrée et authentifiée** (clé d'agent), contrairement au syslog UDP brut. En environnement sensible, préférez-la. Pensez aussi à vos **onduleurs** : leurs logs (coupures, batteries) méritent d'être centralisés — croisez-les avec NUT (voir guide onduleurs) pour une vision énergie + sécurité.

---

## 53. Surveiller un pare-feu / un switch avec Wazuh

Exemple concret : un pare-feu qui logue en syslog les connexions refusées.

### 53.1 Decoder pour logs de pare-feu générique

```
2026-09-26 22:30:01 fw-dmz kernel: DROP IN=eth0 OUT= SRC=203.0.113.7 DST=10.0.1.10 PROTO=TCP SPT=52341 DPT=22
```

```xml
<!-- /var/ossec/etc/decoders/local_decoder.xml -->
<decoder name="fw-drop">
  <prematch>kernel: DROP</prematch>
  <regex>kernel: DROP .* SRC=(\S+) DST=(\S+) .* PROTO=(\S+) .* SPT=(\d+) DPT=(\d+)</regex>
  <order>srcip, dstip, protocol, srcport, dstport</order>
</decoder>
```

```xml
<!-- Règle : scan de ports détecté par le pare-feu -->
<rule id="100700" level="10" frequency="20" timeframe="60">
  <decoded_as>fw-drop</decoded_as>
  <description>Scan probable : 20 paquets droppés en 60s depuis $(srcip)</description>
  <mitre><id>T1595</id></mitre>
</rule>
```

### 53.2 Ce que Wazuh sait déjà décoder

Le ruleset officiel inclut des decoders pour : Cisco IOS/ASA, pfSense/OPNsense, iptables, Windows Firewall, Juniper, Fortinet (partiel). Avant d'écrire un decoder, cherchez :

```bash
ls /var/ossec/ruleset/decoders/ | grep -iE 'cisco|forti|pfsense|iptables'
sudo /var/ossec/bin/wazuh-logtest   # collez une ligne réelle, voyez si ça matche déjà
```

> Pour vos équipements MikroTik / Huawei / Cisco du lab RAG : la plupart émettent du syslog standard. Un decoder `prematch` sur le hostname + 2–3 règles suffisent souvent pour les événements critiques (login, changement de config, lien down).

---

## 54. Intégration TheHive : créer des cases depuis les alertes

**TheHive** = plateforme de gestion d'incidents (open source). L'idée : toute alerte Wazuh niveau ≥ 12 crée automatiquement une *case* dans TheHive pour suivi.

### 54.1 Principe

```
Wazuh (alerte) → intégration custom (script) → API TheHive → Case créée
```

Wazuh appelle un script d'intégration via `<integration>` dans `ossec.conf` :

```xml
<!-- /var/ossec/etc/ossec.conf du MANAGER -->
<integration>
  <name>custom-thehive.py</name>
  <rule_id>100100,100300,100700</rule_id>
  <alert_format>json</alert_format>
</integration>
<!-- ou : <level>12</level> pour tout ce qui est ≥ 12 -->
```

### 54.2 Script d'intégration (squelette)

```python
#!/usr/bin/env python3
# /var/ossec/integrations/custom-thehive.py
import sys, json, requests

THEHIVE_URL = "https://thehive.exemple.lan:9000"
API_KEY = "VOTRE_CLE_API_THEHIVE"   # via variable d'environnement en prod !

def main():
    alert = json.load(sys.stdin)
    rule = alert.get("rule", {})
    case = {
        "title": f"[Wazuh] {rule.get('description', 'Alerte')}",
        "description": json.dumps(alert, indent=2)[:5000],
        "severity": 3 if int(rule.get("level", 0)) >= 12 else 2,
        "tags": ["wazuh", f"rule:{rule.get('id')}", f"agent:{alert.get('agent', {}).get('name')}"],
        "tlp": 2,
        "pap": 2,
    }
    r = requests.post(f"{THEHIVE_URL}/api/v1/case", json=case,
                      headers={"Authorization": f"Bearer {API_KEY}"},
                      timeout=15, verify=False)  # verify=False seulement avec autosigné interne
    r.raise_for_status()
    print(f"Case créée : {r.json().get('id')}")

if __name__ == "__main__":
    # Wazuh appelle : custom-thehive.py <fichier_alerte_json>
    sys.stdin = open(sys.argv[1])
    main()
```

```bash
sudo chmod 750 /var/ossec/integrations/custom-thehive.py
sudo chown root:wazuh /var/ossec/integrations/custom-thehive.py
```

> ⚠️ Ne laissez jamais une clé API en dur dans un script versionné : utilisez une variable d'environnement ou un fichier de secrets (droits 600).

### 54.3 Bonnes pratiques

- Ne créez des cases que pour les niveaux **≥ 12** (sinon TheHive devient une poubelle).
- Dédupliquez : une case par (règle, agent, jour), pas une par alerte.
- Enrichissez avec le lien direct vers l'alerte dans le dashboard Wazuh.

---

## 55. Intégration Shuffle (SOAR)

**Shuffle** = plateforme SOAR open source (automatisation de réponse). Cas d'usage : à la réception d'une alerte, Shuffle peut automatiquement : interroger un threat intel, isoler la machine (via API Wazuh active-response), créer un ticket, notifier.

### 55.1 Webhook Wazuh → Shuffle

Côté Wazuh, utilisez l'intégrateur `slack`-like générique ou un script custom (même mécanisme que section 54) qui POSTe vers le webhook d'entrée Shuffle :

```python
# extrait : envoi vers Shuffle
SHUFFLE_WEBHOOK = "https://shuffle.exemple.lan:3001/api/v1/hooks/<ID_DU_WORKFLOW>"
payload = {
    "rule_id": rule.get("id"),
    "level": rule.get("level"),
    "agent": alert.get("agent", {}).get("name"),
    "srcip": alert.get("data", {}).get("srcip"),
    "description": rule.get("description"),
}
requests.post(SHUFFLE_WEBHOOK, json=payload, timeout=10)
```

### 55.2 Workflow Shuffle type « force brute SSH »

1. **Trigger** : webhook reçoit l'alerte (règle 5763).
2. **Enrichissement** : appel API AbuseIPDB / VirusTotal sur la `srcip`.
3. **Décision** : si score malveillant > seuil → étape 4, sinon log seul.
4. **Action** : appel API Wazuh pour lancer `firewall-drop` (voir section 58), + création d'un ticket.
5. **Notification** : résumé sur le canal SOC.

> Commencez **en mode « dry-run »** (Shuffle notifie sans bloquer) pendant 2 semaines. Passez en blocage auto quand le taux de faux positifs est < 5 %.

---

## 56. Webhooks génériques : Slack, Teams, Mattermost

### 56.1 Intégrateur Slack natif

```xml
<!-- /var/ossec/etc/ossec.conf du MANAGER -->
<integration>
  <name>slack</name>
  <hook_url>https://hooks.slack.com/services/T000/B000/XXXXXXXX</hook_url>
  <rule_id>5763</rule_id>
  <alert_format>json</alert_format>
</integration>
```

### 56.2 Webhook générique (Teams, Mattermost, n8n...)

Pas d'intégrateur natif → script custom (mécanisme `<integration>` + script dans `/var/ossec/integrations/`) :

```python
#!/usr/bin/env python3
# /var/ossec/integrations/custom-webhook.py
import sys, json, requests

WEBHOOK_URL = "https://mattermost.exemple.lan/hooks/xxx"  # ou Teams, n8n...

alert = json.load(open(sys.argv[1]))
rule = alert.get("rule", {})
agent = alert.get("agent", {}).get("name", "?")
text = (
    f"**[Wazuh] Niveau {rule.get('level')}** — {rule.get('description')}\n"
    f"Agent : `{agent}` | Règle : `{rule.get('id')}` | Heure : `{alert.get('timestamp')}`"
)
# Format Mattermost/Slack :
requests.post(WEBHOOK_URL, json={"text": text}, timeout=10)
# Pour Teams : adaptez au format MessageCard / Adaptive Card.
```

> Filtrez par `<level>` ou `<rule_id>` : envoyer **toutes** les alertes sur un canal = le meilleur moyen que personne ne le lise. Niveau ≥ 10 uniquement.

---

## 57. Envoyer des alertes par e-mail

```xml
<!-- /var/ossec/etc/ossec.conf du MANAGER -->
<global>
  <email_notification>yes</email_notification>
  <email_to>soc@exemple.lan</email_to>
  <smtp_server>10.0.0.25</smtp_server>
  <email_from>wazuh@exemple.lan</email_from>
  <email_maxperhour>30</email_maxperhour>   <!-- anti-spam -->
</global>

<alerts>
  <log_alert_level>3</log_alert_level>
  <email_alert_level>10</email_alert_level>  <!-- e-mail seulement si ≥ 10 -->
</alerts>
```

Avec relais SMTP authentifié (ex. Postfix interne) :

```xml
<global>
  <smtp_server>10.0.0.25</smtp_server>
  <!-- Pour de l'auth SMTP, passez par un relais local postfix plutôt que Wazuh -->
</global>
```

> Astuce : faites relayer par votre **Postfix local** (déjà configuré, voir guide Debian/Ubuntu) plutôt que de configurer SMTP+TLS dans Wazuh. `email_maxperhour` évite de noyer la boîte SOC lors d'une tempête d'alertes.

Modèle d'e-mail utile : objet avec niveau + règle + agent. Wazuh le fait nativement ; vérifiez que l'objet est filtrable (règle de tri côté boîte SOC : `[Wazuh]` + niveau).

---

## 58. API Wazuh : automatiser sans le dashboard

L'API (port 55000) permet tout ce que fait le dashboard, en script.

### 58.1 Authentification

```bash
# Token (valide 15 min par défaut)
TOKEN=$(curl -sk -u wazuh-wui:'<MOT_DE_PASSE>' \
  https://localhost:55000/security/user/authenticate \
  | python3 -c "import sys,json; print(json.load(sys.stdin)['data']['token'])")
echo "Token OK : ${TOKEN:0:20}..."
```

### 58.2 Opérations courantes

```bash
# Lister les agents actifs
curl -sk -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/agents?status=active&select=id,name,ip&limit=500'

# Redémarrer un groupe d'agents
curl -sk -X PUT -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/agents/group/linux-web/restart'

# Lancer une active-response manuelle (bannir une IP)
curl -sk -X PUT -H "Authorization: Bearer $TOKEN" \
  -H 'Content-Type: application/json' \
  'https://localhost:55000/active-response?agents_list=007' \
  -d '{"command": "firewall-drop.sh", "arguments": ["-add", "203.0.113.7"]}'

# Récupérer les règles (pour audit)
curl -sk -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/rules?select=id,description,level&limit=100'

# Statistiques du manager (remontées, files)
curl -sk -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/manager/stats/analysisd?pretty=true'
```

### 58.3 Créer un utilisateur API dédié (au lieu de wazuh-wui)

```bash
# Créer l'utilisateur + rôle lecture seule (exemple)
curl -sk -X POST -H "Authorization: Bearer $TOKEN" \
  -H 'Content-Type: application/json' \
  https://localhost:55000/security/users \
  -d '{"username": "api-lecture", "password": "<MOT_DE_PASSE_FORT>"}'
```

> Un script de supervision (Zabbix, Prometheus exporter) doit utiliser un compte **dédié, en lecture seule**, jamais `wazuh-wui`.

---

## 59. Superviser Wazuh lui-même : le manager en bonne santé

Qui surveille le surveillant ? **Vous**, avec ces indicateurs.

### 59.1 Files d'attente (le pouls du manager)

```bash
# Taille des files analysisd / remoted
sudo /var/ossec/bin/wazuh-analysisd -t   # test de conf (ne pas confondre)
ls -la /var/ossec/queue/
sudo cat /var/ossec/var/run/wazuh-analysisd.state 2>/dev/null | head -30
```

Via API :

```bash
curl -sk -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/manager/stats/analysisd?pretty=true'
# Surveillez : events_processed, events_dropped, queue_size
```

Seuils d'alerte (à mettre dans Zabbix/Prometheus) :

| Métrique | Warning | Critical |
|---|---|---|
| `queue_size` analysisd | > 1000 | > 10000 |
| `events_dropped` (croissant) | > 0 sur 1 h | > 100 sur 1 h |
| Agents `disconnected` | > 5 % du parc | > 15 % |
| Espace `/var/ossec` | > 75 % | > 90 % |

### 59.2 EPS réel : mesurez votre charge

```bash
# Compter les alertes des dernières 24h (ordre de grandeur)
sudo wc -l /var/ossec/logs/alerts/alerts.json
# Mieux : via l'API stats, events_processed sur 1h / 3600 = EPS moyen
```

### 59.3 Logs à surveiller

| Log | Ce qu'il dit |
|---|---|
| `/var/ossec/logs/ossec.log` | Manager : erreurs de règles, authd, cluster |
| `/var/ossec/logs/active-responses.log` | Chaque réponse active exécutée |
| `/var/ossec/logs/alerts/alerts.log` | Alertes texte (debug) |
| `/var/log/filebeat/filebeat` | Envoi vers l'indexer |

> Mettez **Wazuh lui-même sous supervision** : un check « le manager tourne + Filebeat envoie + l'indexer est vert » dans votre Zabbix, avec alerte SMS. Un SIEM en panne silencieuse = pire que pas de SIEM.

---

## 60. Superviser l'indexer (OpenSearch)

### 60.1 Santé du cluster

```bash
curl -k -u admin:'<MDP>' 'https://localhost:9200/_cluster/health?pretty'
```

| Statut | Signification | Action |
|---|---|---|
| `green` | Tout va bien | Rien |
| `yellow` | Répliques non assignées (**normal en mono-nœud**) | Rien (ou passer en multi-nœuds) |
| `red` | Des *primaries* manquent = **perte de données possible** | Section 69, immédiat |

### 60.2 Espace disque et watermark

OpenSearch bloque l'écriture à 95 % d'usage disque (flood stage) :

```bash
curl -k -u admin:'<MDP>' 'https://localhost:9200/_cat/allocation?v'
# Surveillez disk.used_percent par nœud
```

Seuils : alerte à **75 %**, action à **85 %** (suppression/rotation d'index, voir section 72).

### 60.3 Rotation des index (ISM)

Wazuh crée un index par jour (`wazuh-alerts-4.x-YYYY.MM.DD`). Sans rotation, le disque se remplit. Via le dashboard (*Index Management → ISM policies*) ou API :

```json
// Politique : supprimer les index d'alertes après 90 jours
{
  "policy": {
    "description": "Rétention alertes 90 jours",
    "default_state": "hot",
    "states": [
      { "name": "hot", "transitions": [{ "state_name": "delete", "conditions": { "min_index_age": "90d" } }] },
      { "name": "delete", "actions": [{ "delete": {} }] }
    ]
  }
}
```

> ⚠️ Avant de supprimer : vérifiez vos **obligations légales** de conservation (souvent 1 an pour les logs de sécurité en entreprise). 90 jours en ligne + archives froides (snapshots, section 63) = bon compromis.

---

## 61. Superviser le dashboard

Le dashboard est stateless : s'il tombe, les données sont sauves, mais le SOC est aveugle.

```bash
# Le service tourne ?
sudo systemctl is-active wazuh-dashboard
# Il répond ?
curl -k -s -o /dev/null -w '%{http_code}\n' https://localhost:443
# Attendu : 302 (redirection vers /app/login) ou 200
```

Points de vigilance :

- **Mot de passe `kibanaserver`** : si changé côté indexer sans mise à jour dans `/etc/wazuh-dashboard/opensearch_dashboards.yml`, le dashboard affiche des erreurs 500.
- **Espace disque** : le dashboard écrit peu, mais vérifiez quand même.
- **Sessions** : en cas de comportement étrange après mise à jour, videz le cache navigateur + redémarrez le service.

```yaml
# /etc/wazuh-dashboard/opensearch_dashboards.yml (extrait)
opensearch.hosts: ["https://127.0.0.1:9200"]
opensearch.username: kibanaserver
opensearch.password: "<MOT_DE_PASSE>"
server.ssl.enabled: true
```

---

## 62. Sauvegarde : stratégie complète

### 62.1 Quoi sauvegarder

| Élément | Chemin | Fréquence | Outil |
|---|---|---|---|
| Conf manager (règles, decoders, ossec.conf) | `/var/ossec/etc/` | Quotidienne | Borg / rsync / git |
| Clés agents, groupes | `/var/ossec/etc/client.keys`, `/var/ossec/queue/` | Quotidienne | Idem |
| Certificats | `/etc/wazuh-indexer/certs/`, etc. | À chaque changement | Coffre chiffré |
| Indexer (données) | Snapshots OpenSearch | Quotidienne | Dépôt S3/NFS (section 63) |
| Base API (utilisateurs) | `/var/ossec/api/configuration/` | Quotidienne | Borg |
| `wazuh-install-files.tar` | Archive initiale | Une fois, hors serveur | Coffre |

### 62.2 Sauvegarde de la conf (exemple Borg)

```bash
# Initialisation (une fois)
sudo borg init --encryption=repokey /backup/wazuh-conf

# Quotidien (cron)
sudo borg create --compression lz4 \
  /backup/wazuh-conf::'{now:%Y-%m-%d}' \
  /var/ossec/etc /var/ossec/api/configuration \
  /var/ossec/integrations /var/ossec/active-response/bin

# Rétention
sudo borg prune --keep-daily=14 --keep-weekly=8 --keep-monthly=12 /backup/wazuh-conf
```

### 62.3 Versionnez vos règles dans git

```bash
# Vos règles/decoders/scripts custom = du code : versionnez !
cd /var/ossec/etc
sudo git init && sudo git add rules/local_rules.xml decoders/local_decoder.xml
sudo git commit -m "soc: règles initiales"
# Puis poussez vers votre Git interne après chaque modification
```

> Une règle qui a « toujours marché » et qui casse un matin : `git log` vous dira qui a changé quoi et quand. Inestimable.

---

## 63. Snapshots de l'indexer : mise en place pas à pas

Les snapshots OpenSearch = sauvegarde cohérente des index, restaurable.

### Étape 1 — Déclarer un dépôt de snapshots

```bash
# Sur chaque nœud indexer, montez le stockage (ex. NFS /mnt/snapshots)
# Puis déclarez le dépôt :
curl -k -u admin:'<MDP>' -X PUT 'https://localhost:9200/_snapshot/depot_quotidien?pretty' \
  -H 'Content-Type: application/json' -d '{
    "type": "fs",
    "settings": { "location": "/mnt/snapshots/wazuh" }
  }'
```

Le chemin doit être dans `path.repo` de `/etc/wazuh-indexer/opensearch.yml` sur **chaque** nœud :

```yaml
path.repo: ["/mnt/snapshots/wazuh"]
```

(Redémarrez l'indexer après modification.)

### Étape 2 — Politique de snapshot (SLM)

```bash
# Snapshot quotidien à 2h, rétention 30 jours
curl -k -u admin:'<MDP>' -X PUT 'https://localhost:9200/_slm/policy/wazuh-quotidien?pretty' \
  -H 'Content-Type: application/json' -d '{
    "schedule": "0 0 2 * * ?",
    "name": "<wazuh-snap-{now/d}>",
    "repository": "depot_quotidien",
    "config": { "indices": "wazuh-alerts-*" },
    "retention": { "expire_after": "30d", "min_count": 7, "max_count": 30 }
  }'
```

### Étape 3 — Vérifier

```bash
curl -k -u admin:'<MDP>' 'https://localhost:9200/_snapshot/depot_quotidien/_all?pretty' | grep -E 'snapshot|state'
curl -k -u admin:'<MDP>' 'https://localhost:9200/_slm/policy/wazuh-quotidien?pretty'
```

> Testez la **restauration** au moins une fois par semestre sur un environnement isolé (section 64). Une sauvegarde jamais testée = pas de sauvegarde.

---

## 64. Restaurer après sinistre : procédure

### Scénario A — Perte du manager (données indexer intactes)

1. Réinstallez un manager (même version !).
2. Restaurez `/var/ossec/etc/` depuis Borg (section 62).
3. Restaurez les certificats si le FQDN est identique.
4. Redémarrez, vérifiez les agents (ils se reconnectent seuls).

### Scénario B — Perte de l'indexer (manager intact)

```bash
# 1. Réinstallez l'indexer (même version)
# 2. Re-déclarez le dépôt de snapshots (section 63, étape 1)
# 3. Listez les snapshots disponibles
curl -k -u admin:'<MDP>' 'https://localhost:9200/_snapshot/depot_quotidien/_all?pretty' | grep '"snapshot"'
# 4. Restaurez (fermez d'abord les index existants si besoin)
curl -k -u admin:'<MDP>' -X POST \
  'https://localhost:9200/_snapshot/depot_quotidien/wazuh-snap-2026.09.25/_restore?pretty' \
  -H 'Content-Type: application/json' -d '{
    "indices": "wazuh-alerts-*",
    "ignore_unavailable": true
  }'
# 5. Vérifiez la santé : _cluster/health doit redevenir green/yellow
```

### Scénario C — Tout est perdu (rebuild complet)

1. Rebuild all-in-one (section 9).
2. Restaurez la conf manager (Borg + git).
3. Restaurez les snapshots indexer.
4. Les agents se réinscrivent ? **Non** : leurs clés sont dans `client.keys` restauré → ils se reconnectent automatiquement. (Si `client.keys` perdu : réinscription massive, prévoyez le mot de passe d'inscription.)

> Tenez cette procédure **imprimée** dans votre classeur d'astreinte. Le jour du sinistre, le dashboard sera peut-être inaccessible : le papier ne plante jamais.

---

## 65. Mise à jour du manager et des composants centraux

### 65.1 Règle d'or

**Jamais de mise à jour sans : sauvegarde (section 62) + lecture du changelog + fenêtre de maintenance.** Les montées de version mineure 4.x sont généralement fluides, mais les majeures (ex. 4.x → 5.x un jour) peuvent changer les formats.

### 65.2 Procédure (paquets Debian/Ubuntu)

```bash
# 1. Sauvegarde complète (conf + snapshot indexer)
# 2. Lire le changelog : https://documentation.wazuh.com/.../release-notes/
# 3. Défiger, mettre à jour dans l'ordre : indexer → manager → dashboard
sudo apt-mark unhold wazuh-indexer wazuh-manager wazuh-dashboard filebeat

sudo apt update
sudo apt install -y wazuh-indexer          # 1. indexer d'abord
sudo systemctl restart wazuh-indexer
# vérifier : curl .../_cluster/health → green/yellow

sudo apt install -y wazuh-manager          # 2. manager
sudo systemctl restart wazuh-manager

sudo apt install -y wazuh-dashboard filebeat   # 3. dashboard + filebeat
sudo systemctl restart wazuh-dashboard filebeat

sudo apt-mark hold wazuh-manager wazuh-indexer wazuh-dashboard filebeat

# 4. Vérifications post-update (section 10) + 1 agent test + 1 alerte test
```

### 65.3 Pièges fréquents

- **Fichiers de conf écrasés** : les paquets peuvent proposer de remplacer `ossec.conf`. Gardez toujours vos versions (Borg/git, section 62) et fusionnez à la main.
- **Ruleset officiel mis à jour** : vos `overwrite` (section 32) peuvent ne plus s'appliquer proprement si la règle officielle a changé → relancez `wazuh-logtest` sur vos échantillons.
- **Incompatibilité de version manager/agents** : un manager 4.12 avec des agents 4.7 fonctionne, mais l'inverse **non**. Mettez toujours le manager à jour **avant** les agents.

---

## 66. Mise à jour des agents (WPK et manuelle)

### 66.1 Via WPK (paquets Wazuh, depuis le manager)

Le manager peut pousser les mises à jour via l'API :

```bash
# 1. Vérifier les versions des agents
curl -sk -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/agents?select=id,name,version&limit=500' | grep -o '"version":"[^"]*"'

# 2. Lancer la mise à jour d'un groupe (par vagues !)
curl -sk -X PUT -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/agents/upgrade?agents_list=007,008,009'

# 3. Suivre la progression
curl -sk -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/agents/upgrade_result?agents_list=007'
```

**Par vagues** : d'abord 2–3 agents pilotes, puis un groupe, puis le reste. Jamais tout le parc d'un coup un vendredi soir.

### 66.2 Manuelle (Ansible / script)

```bash
# Linux
sudo apt-mark unhold wazuh-agent
sudo apt update && sudo apt install -y wazuh-agent
sudo apt-mark hold wazuh-agent
sudo systemctl restart wazuh-agent
```

```powershell
# Windows : poussez le nouveau MSI en silencieux (GPO / Intune / script)
msiexec /i wazuh-agent-4.12-1.msi /q REINSTALL=ALL REINSTALLMODE=vomus
```

---

## 67. Dépannage : méthodologie générale

```
1. DÉFINIR : quel est le symptôme exact ? (pas d'alerte ? agent rouge ? dashboard vide ?)
2. LOCALISER : à quelle étape de la chaîne ça casse ?
   Agent → (1514) → Manager (analysisd) → alerts.json → Filebeat → (9200) → Indexer → Dashboard
3. VÉRIFIER : chaque maillon avec UNE commande (tableau ci-dessous)
4. CORRIGER : une seule chose à la fois, puis re-tester
5. DOCUMENTER : notez la cause dans votre wiki (section 74)
```

| Maillon | Commande de test |
|---|---|
| Agent → Manager | `agent_control -i <ID>` : `active` ? |
| Manager analyse | `tail /var/ossec/logs/ossec.log` : erreurs ? |
| alerts.json s'écrit | `tail -5 /var/ossec/logs/alerts/alerts.json` |
| Filebeat envoie | `systemctl status filebeat`, `/var/log/filebeat/` |
| Indexer reçoit | `curl .../_cat/indices/wazuh-alerts-*?v` : docs.count augmente ? |
| Dashboard affiche | Requête *Discover* sur les 15 dernières minutes |

> 80 % des pannes se résolvent à l'étape 2 : **la chaîne est linéaire, suivez-la dans l'ordre** au lieu de tout redémarrer en vrac.

---

## 68. Dépannage : l'agent ne remonte pas

### Arbre de décision

```
Agent "disconnected" ou "never_connected" ?
│
├─► 1. L'agent tourne-t-il ?
│     Linux   : systemctl is-active wazuh-agent
│     Windows : Get-Service wazuh-agent
│     Non → démarrez-le, lisez ossec.log
│
├─► 2. L'agent est-il inscrit ? (clé présente ?)
│     Linux   : ls -la /var/ossec/etc/client.keys
│     Absent → (ré)inscription : agent-auth (section 20)
│
├─► 3. Le réseau passe-t-il ? (depuis l'AGENT)
│     Test UDP 1514 : nc -u -v -z <manager> 1514
│     Test TCP 1515 : nc -v -z <manager> 1515
│     Échec → pare-feu local, pare-feu réseau, routage, NAT
│
├─► 4. Le manager voit-il l'agent ?
│     /var/ossec/bin/agent_control -i <ID>
│     "Active" côté manager mais rien ne remonte → vérifiez l'heure (NTP !)
│
└─► 5. L'horloge est-elle synchro ? (± 5 min max)
      timedatectl status   (agent ET manager)
      Décalage → les messages sont rejetés silencieusement
```

### Cas vécu : l'agent derrière un NAT

Si plusieurs agents partagent la même IP publique, utilisez `use_source_ip=no` côté `authd` et des noms d'agents uniques. Sinon le manager confond les agents.

### Cas vécu : l'agent Windows « actif » mais sans logs

Souvent le service tourne mais l'inscription a échoué silencieusement : supprimez `C:\Program Files (x86)\ossec-agent\client.keys`, réinscrivez via l'interface (*Manage → Authenticate*), redémarrez le service.

---

## 69. Dépannage : indexer rouge / cluster unhealthy

```bash
# 1. Diagnostic express
curl -k -u admin:'<MDP>' 'https://localhost:9200/_cluster/health?pretty'
curl -k -u admin:'<MDP>' 'https://localhost:9200/_cat/shards?v' | grep -v STARTED
curl -k -u admin:'<MDP>' 'https://localhost:9200/_cat/allocation?v'
```

| Symptôme | Cause probable | Action |
|---|---|---|
| `status: red` | Shard primaire non assigné | Identifier le shard (`_cat/shards`), vérifier le nœud qui le portait |
| `unassigned_shards > 0` durable | Nœud manquant / disque plein | Libérer du disque (section 72), redémarrer le nœud |
| `number_of_nodes` < attendu | Nœud indexer down | `systemctl status wazuh-indexer`, logs `/var/log/wazuh-indexer/` |
| Erreurs `flood stage` | Disque > 95 % | **Urgence** : supprimer des vieux index (section 72) |

### Sortir du flood stage (disque plein côté indexer)

```bash
# 1. Libérer de la place EN DEHORS d'OpenSearch d'abord (logs, tmp)
# 2. Lever le bloc en lecture seule :
curl -k -u admin:'<MDP>' -X PUT 'https://localhost:9200/_all/_settings' \
  -H 'Content-Type: application/json' \
  -d '{"index.blocks.read_only_allow_delete": null}'
# 3. Supprimer les vieux index :
curl -k -u admin:'<MDP>' -X DELETE 'https://localhost:9200/wazuh-alerts-4.x-2026.06.*'
# 4. Vérifier : _cluster/health → yellow/green
```

> Un indexer `red` = **priorité absolue**, devant toute analyse d'alerte : sans stockage, le SIEM est aveugle ET perd des données.

---

## 70. Dépannage : pas d'alertes / alertes en retard

```
Le dashboard est vide mais les agents sont verts ?
│
├─► 1. alerts.json se remplit-il ? (MANAGER)
│     tail -5 /var/ossec/logs/alerts/alerts.json
│     Non → analysisd ne produit pas : regardez /var/ossec/logs/ossec.log
│           (erreur de règle ? testez avec wazuh-logtest, section 31)
│
├─► 2. Filebeat tourne-t-il et envoie-t-il ?
│     systemctl is-active filebeat
│     tail -20 /var/log/filebeat/filebeat
│     Erreurs TLS → certificats (section 16) ; erreurs 401 → mot de passe
│
├─► 3. L'indexer reçoit-il ? (doc count augmente ?)
│     watch -n 5 'curl -sk -u admin:... "https://localhost:9200/_cat/indices/wazuh-alerts-*?v&s=index" | tail -3'
│
├─► 4. Le dashboard interroge-t-il le bon index ?
│     Discover → vérifiez le pattern d'index et la plage de temps !
│     (90 % des "dashboard vide" = plage de temps sur "aujourd'hui" alors
│      que les données datent d'hier, ou pattern d'index cassé après update)
│
└─► 5. Retard seulement ? → file d'attente analysisd saturée (section 59)
      = manager sous-dimensionné : augmentez CPU/RAM ou passez en distribué
```

---

## 71. Dépannage : dashboard inaccessible

| Symptôme | Vérification | Solution |
|---|---|---|
| `Connection refused` sur 443 | `systemctl status wazuh-dashboard` | Redémarrer ; voir logs `/var/log/wazuh-dashboard/` |
| Erreur 500 / « Cannot connect to OpenSearch » | `curl -k https://localhost:9200` | Indexer down ? (section 69) ; mot de passe `kibanaserver` désynchronisé ? |
| Page blanche après mise à jour | Cache navigateur | Ctrl+Shift+R ; vider le cache ; tester en navigation privée |
| Certificat invalide | `openssl s_client` (section 16) | Certificat expiré → régénérer |
| Login rejeté en boucle | Utilisateur verrouillé ? | Vérifier via API ou réinitialiser avec `securityadmin.sh` |

Réinitialiser le mot de passe admin (indexer) en dernier recours :

```bash
# Générer le hash du nouveau mot de passe puis l'appliquer via securityadmin.sh
# (procédure détaillée dans la doc officielle "password management")
# Pensez ensuite à mettre à jour wazuh.yml côté dashboard (utilisateur wazuh-wui).
```

---

## 72. Dépannage : disque plein

### Où chercher (par ordre de probabilité)

```bash
df -h
du -sh /var/lib/wazuh-indexer /var/ossec/logs /var/log 2>/dev/null
# Index les plus gros :
curl -k -u admin:'<MDP>' 'https://localhost:9200/_cat/indices?v&s=store.size:desc' | head -15
```

### Actions immédiates

```bash
# 1. Purger les vieux logs manager (gardez 30 jours en local, le reste en archives)
sudo find /var/ossec/logs/alerts -name 'alerts.log-*' -mtime +30 -delete
sudo journalctl --vacuum-time=14d

# 2. Supprimer les vieux index (en respectant la rétention légale !)
curl -k -u admin:'<MDP>' -X DELETE 'https://localhost:9200/wazuh-alerts-4.x-2026.05.*'

# 3. Vérifier la rotation ISM (section 60) : est-elle vraiment active ?
```

### Prévention durable

- Alertes à 75 % / 85 % (section 59).
- Rétention ISM alignée sur vos obligations (souvent 1 an → snapshots froids, pas tout en ligne).
- **Séparez les volumes** : données indexer ≠ système ≠ logs.

---

## 73. 10 erreurs classiques (et comment les éviter)

### Erreur n°1 — Modifier le ruleset officiel au lieu de local_rules.xml
**Symptôme :** vos règles disparaissent à la mise à jour.
**Solution :** tout le custom va dans `/var/ossec/etc/rules/`, `/var/ossec/etc/decoders/`. Le dossier `/var/ossec/ruleset/` est en lecture seule mentale.

### Erreur n°2 — Oublier la allowlist avant d'activer l'active response
**Symptôme :** vous vous bannissez vous-même (ou votre supervision) en testant.
**Solution :** `<white_list>` complète (section 45) **avant** le premier `<active-response>`. Testez depuis une IP de lab non allowlistée.

### Erreur n°3 — FIM en temps réel sur /var/log ou /tmp
**Symptôme :** explosion d'EPS, manager saturé, alertes en retard.
**Solution :** `realtime` uniquement sur binaires et confs stables ; **jamais** sur les dossiers à forte écriture (section 36).

### Erreur n°4 — Ne pas figer les versions des paquets
**Symptôme :** un `apt upgrade` nocturne casse Wazuh un mardi à 4h.
**Solution :** `apt-mark hold` (section 9) + mises à jour planifiées (section 65).

### Erreur n°5 — Un seul mot de passe admin partagé par toute l'équipe
**Symptôme :** impossible de savoir qui a fait quoi ; mot de passe jamais changé.
**Solution :** un compte par analyste, rôles least-privilege (section 11), rotation après chaque départ.

### Erreur n°6 — Ignorer le tuning (« on regardera les alertes plus tard »)
**Symptôme :** 5 000 alertes/jour, plus personne ne lit, la vraie attaque passe.
**Solution :** rituel de tuning hebdomadaire (section 50). Un SIEM non tuné est un presse-papier coûteux.

### Erreur n°7 — Pas de supervision du SIEM lui-même
**Symptôme :** Filebeat arrêté depuis 3 jours, personne ne l'a vu.
**Solution :** checks Zabbix/Prometheus sur les 4 services + files + espace disque (section 59).

### Erreur n°8 — Sauvegarde jamais testée
**Symptôme :** le jour du sinistre, le snapshot est corrompu ou le dépôt inaccessible.
**Solution :** restauration testée 2×/an sur environnement isolé (sections 63–64).

### Erreur n°9 — Cloner des VM avec un agent déjà inscrit
**Symptôme :** doublons d'ID, agents qui se « volent » la connexion, alertes fantômes.
**Solution :** image avec agent **non inscrit**, inscription au premier boot (section 20, méthode D).

### Erreur n°10 — Exposer le dashboard ou l'API sur Internet sans protection
**Symptôme :** interface d'admin accessible au monde entier.
**Solution :** dashboard/API uniquement via VPN ou IP allowlistées ; TLS partout ; fail2ban ; 2FA si possible via reverse proxy.

---

## 74. Bonnes pratiques SOC pour une petite équipe

Vous êtes 1 à 3 personnes, pas un SOC de 30 analystes en 3×8. Adaptez :

### 74.1 Organisation

- **Un responsable SOC** (vous) : tuning, règles, astreinte niveau 2.
- **Un analyste** (même à temps partiel) : triage quotidien, investigations niveau 1.
- **Un suppléant** : connaît la procédure d'astreinte, peut qualifier une alerte critique.

### 74.2 Les rituels (non négociables)

| Rituel | Fréquence | Durée | Contenu |
|---|---|---|---|
| Triage alertes | Quotidien | 30 min | Qualifier les ≥ 7 de la veille (section 48) |
| Revue vulnérabilités | Hebdomadaire | 1 h | CVE critiques/hautes → plan de patch (section 40) |
| Tuning | Hebdomadaire | 1 h | Top bruit → actions (section 50) |
| Revue MITRE | Mensuelle | 1 h | Couverture par tactique, règles manquantes |
| Test restauration | Semestrielle | ½ journée | Procédure section 64 |
| Exercice incident | Semestrielle | ½ journée | Simulez un cas pratique (sections 75–80) |

### 74.3 Documentation minimale

1. **Procédure d'astreinte** (1 page) : qui appeler, seuils, actions immédiates (tableau section 49).
2. **Cartographie** : quels agents, quels groupes, quelles règles custom (pointez vers le git, section 62).
3. **Journal des incidents** : date, alerte, qualification, action. Même un tableur suffit.
4. **Journal du tuning** : quelle règle modifiée, pourquoi, quand (git log + commentaires XML).

### 74.4 Hygiène des alertes

- **Zéro alerte ≥ 10 « Nouveau » de plus de 24 h** (section 48).
- Chaque faux positif récurrent = **ticket de tuning**, pas « on fera avec ».
- Célébrez les détections : une attaque bloquée par l'active response = à raconter en réunion d'équipe. Ça entretient la vigilance.

### 74.5 Articulation avec l'équipe systèmes

Le SOC ne patch pas, ne redémarre pas les serveurs de prod : il **détecte et qualifie**, puis transmet avec un niveau de sévérité et un délai (tableau section 40). Formalisez ce contrat : qui fait quoi, sous quel délai, avec quelle escalade.

---

## 75. Cas pratique 1 : intrusion SSH par force brute

**Contexte :** `srv-web-01` (10.0.1.15) exposé en SSH sur Internet (à corriger !).

### Ce que Wazuh voit

```
1. Règle 5716 (niveau 5) : "SSHD: authentication failed." × 40 en 2 min, srcip 203.0.113.7
2. Règle 5763 (niveau 10) : "sshd: brute force attack." → ACTIVE RESPONSE firewall-drop
3. L'IP est bannie 10 min (active-responses.log)
4. 2 h plus tard : règle 5718 (niveau 8) : "SSHD: authentication success." depuis 198.51.100.99 !
```

### Investigation pas à pas

```bash
# 1. Historique de l'IP sur 7 jours : a-t-elle touché d'autres machines ?
# Dashboard Discover : data.srcip : "198.51.100.99"

# 2. Sur srv-web-01 : que s'est-il passé autour du succès ?
sudo grep "198.51.100.99" /var/log/auth.log | tail -20
sudo last -i | head -20

# 3. L'utilisateur compromis a-t-il fait quelque chose ?
sudo grep "jdupont" /var/log/auth.log | grep -E "sudo|su " | tail -20
```

### Réponse

1. **Isoler** : coupez le SSH entrant au pare-feu (ou `firewall-drop` manuel via API, section 58).
2. **Bloquer** l'utilisateur : `usermod -L jdupont`, forcez la rotation des clés/mots de passe.
3. **Chercher la persistance** : FIM sur `/root/.ssh/authorized_keys`, cron, services (règles 550-557).
4. **Corriger la cause** : SSH derrière VPN/bastion, clés uniquement, fail2ban + Wazuh active response en ceinture-bretelles.

### Leçons

- Le niveau 8 « premier succès après échecs » est **plus important** que le brute force lui-même : c'est lui qui doit réveiller l'astreinte.
- Sans allowlist correcte, l'active response aurait pu bannir votre bastion : vérifiez la section 45.

---

## 76. Cas pratique 2 : ransomware simulé (FIM + active response)

**Contexte :** exercice sur une VM de lab `lab-win-01`. On simule un ransomware qui chiffre `C:\Data`.

### Détection attendue

```
1. FIM temps réel : dizaines de fichiers modifiés en quelques secondes (règles 550/551, niveau 7)
   → règle custom "rafale FIM" (fréquence 50 / 60 s) niveau 13
2. Nouveau processus avec extension suspecte (syscollector / command wodle)
3. Active response : quarantine.sh → coupe le réseau de la VM
```

### Règle « rafale FIM » (à préparer AVANT l'exercice)

```xml
<rule id="100800" level="13" frequency="50" timeframe="60">
  <if_matched_sid>550</if_matched_sid>
  <same_field>id</same_field>   <!-- même agent -->
  <description>Rafale FIM : 50+ fichiers modifiés en 60s sur $(agent_name) — ransomware probable</description>
  <mitre><id>T1486</id></mitre>  <!-- Data Encrypted for Impact -->
</rule>
```

```xml
<active-response>
  <command>quarantine-net</command>
  <location>local</location>
  <rules_id>100800</rules_id>
</active-response>
```

### Déroulé de l'exercice

1. Lancez le simulateur (script qui renomme/chiffre des fichiers de test).
2. Chronométrez : temps entre le début du chiffrement et l'alerte niveau 13.
3. Vérifiez la quarantaine réseau : la VM ne parle plus ?
4. **Restaurez** depuis snapshot : mesurez le RTO.
5. Débriefez : qu'est-ce qui a manqué ? (souvent : la règle rafale n'existait pas, ou le FIM n'était pas en temps réel sur `C:\Data`).

> Refaites cet exercice **2×/an**. C'est le meilleur test de votre chaîne détection → réponse.

---

## 77. Cas pratique 3 : serveur web compromis (PHP webshell)

**Contexte :** `srv-web-02` héberge une application PHP. Un webshell `x.php` est déposé via une faille d'upload.

### Signaux Wazuh (dans l'ordre)

```
1. FIM temps réel /var/www/html : "New file" x.php (règle 554, niveau 7)
2. Logs nginx : POST /upload.php puis GET /x.php?cmd=id (règle custom, niveau 10)
3. Command wodle : processus php-fpm lançant /bin/sh (anormal → niveau 12)
```

### Règle custom « accès webshell »

```xml
<rule id="100900" level="12">
  <decoded_as>nginx-access</decoded_as>
  <regex>GET \/\w+\.php\?cmd=</regex>
  <description>Requête type webshell détectée : $(url)</description>
  <mitre><id>T1505.003</id></mitre>
</rule>
```

> Adaptez `decoded_as` à votre decoder nginx réel (testez avec wazuh-logtest, section 31).

### Investigation

```bash
# 1. Dater le dépôt : FIM donne l'heure exacte de création
# 2. Trouver le point d'entrée : logs nginx autour de cette heure
sudo grep "POST" /var/log/nginx/access.log | grep -B2 -A2 "upload"
# 3. Contenu du webshell (copie forensique AVANT suppression)
sudo cp /var/www/html/x.php /root/forensique/x.php.$(date +%F)
sudo head -50 /root/forensique/x.php.*
# 4. Chercher d'autres fichiers déposés ± 24 h (FIM "new file" sur le webroot)
```

### Réponse

1. Isoler le serveur (pare-feu / VLAN quarantaine).
2. **Ne pas juste supprimer le webshell** : considérez la machine compromise → rebuild depuis une image saine.
3. Corriger la faille (upload non filtré), patcher l'application.
4. Ajouter le hash du webshell à vos listes (CDB) pour détecter sa réapparition ailleurs.

---

## 78. Cas pratique 4 : conformité d'un parc Windows (CIS)

**Objectif :** passer le parc `windows-postes` de 60 % à 90 % de contrôles CIS passants en 3 mois.

### Méthode

```
Semaine 1 : activer SCA, laisser tourner un cycle complet (12 h)
Semaine 2 : exporter les fails par contrôle (dashboard SCA → export CSV)
            → trier par occurrence : les 5 contrôles les plus échoués = 80 % du bruit
Semaines 3-10 : corriger par vague via GPO / Intune :
            - mot de passe complexe + verrouillage (CIS 1.x)
            - désactiver les comptes invités (CIS 2.x)
            - activer l'audit avancé (CIS 17.x)
Semaine 11 : re-mesurer, publier le score à la direction
En continu : alerte si le score d'un poste chute brutalement (dérive)
```

### Suivi via API (script hebdo)

```bash
#!/bin/bash
# Extrait : moyenne des scores SCA du groupe windows-postes
for id in $(curl -sk -H "Authorization: Bearer $TOKEN" \
  'https://localhost:55000/agents?group=windows-postes&select=id&limit=500' \
  | grep -o '"id":"[0-9]*"' | cut -d'"' -f4); do
  curl -sk -H "Authorization: Bearer $TOKEN" \
    "https://localhost:55000/sca/$id" | grep -o '"score":[0-9]*' | head -1
done
```

> Présentez le score CIS comme un **KPI sécurité** en comité de direction : c'est concret, mesurable, et ça justifie les budgets.

---

## 79. Cas pratique 5 : vulnérabilité critique sur le parc (CVE)

**Contexte :** CVE-2026-XXXX, CVSS 9.8, RCE sur un composant présent sur 40 serveurs (annonce un mardi).

### Déroulé avec Wazuh

```
Mardi 10h  : l'annonce tombe.
Mardi 10h05 : Dashboard → Vulnerability Detection → recherche "CVE-2026-XXXX"
             → 40 agents concernés, dont 6 en DMZ (exposés Internet).
Mardi 11h  : patch des 6 DMZ (fenêtre d'urgence) + mitigation (pare-feu) sur les autres.
Mercredi   : patch du reste par vagues, vérification : le CVE disparaît de la liste.
Jeudi      : rapport : 40/40 patchés, preuves (exports dashboard) pour l'audit.
```

### Automatiser la veille

```bash
# Cron quotidien : lister les CVE critiques apparues dans les dernières 24 h
# (via l'API indexer : recherche sur wazuh-states-vulnerabilities-*)
curl -sk -u admin:'<MDP>' 'https://localhost:9200/wazuh-states-vulnerabilities-*/_search?pretty' \
  -H 'Content-Type: application/json' -d '{
    "query": { "range": { "vulnerability.published_at": { "gte": "now-1d" } } },
    "size": 50,
    "_source": ["vulnerability.id", "vulnerability.score.base", "agent.name", "package.name"]
  }'
```

> Le module Vulnerability Detector met à jour ses bases **toutes les heures** (section 39) : un CVE publié le matin est visible l'après-midi. C'est votre radar.

---

## 80. Cas pratique 6 : exfiltration de données via DNS

**Contexte :** un poste `pc-compta-03` envoie des requêtes DNS anormalement longues vers un domaine rare — technique classique d'exfiltration (DNS tunneling).

### Détection

Wazuh ne fait pas nativement de la détection DNS fine, mais :

1. Collectez les logs DNS (serveur DNS interne en syslog, section 52, ou logs du resolver du poste).
2. Decoder + règle sur la **longueur** et l'**entropie** des requêtes :

```xml
<!-- Exemple : alerter sur des requêtes DNS > 60 caractères vers l'extérieur -->
<rule id="101000" level="10">
  <decoded_as>dns-query</decoded_as>
  <regex>\w{60,}\.</regex>
  <description>Requête DNS anormalement longue : exfiltration possible ($(query))</description>
  <mitre><id>T1048.003</id></mitre>
</rule>
```

### Investigation

```
1. Volume : combien de requêtes ? quelle taille totale ? (dashboard : agg par query)
2. Domaine : nouvellement enregistré ? (whois, threat intel)
3. Poste : processus à l'origine ? (syscollector processes, EDR)
4. Données : quelles données auraient pu partir ? (DCP / classification)
```

### Réponse

1. Bloquer le domaine au DNS/pare-feu.
2. Isoler le poste, image forensique.
3. Si exfiltration confirmée : procédure incident + notifications légales (données personnelles ?).

> Ce cas montre la limite ET la force de Wazuh : pas de NDR natif, mais avec un bon decoder sur vos logs DNS existants, vous couvrez 80 % du besoin pour 20 % de l'effort.

---

## 81. Checklist quotidienne / hebdomadaire / mensuelle

### Quotidienne (30 min, analyste)

- [ ] Dashboard → *Overview* : alertes niveau ≥ 10 des dernières 24 h qualifiées ?
- [ ] Agents `disconnected` : justifiés (maintenance) ou à investiguer ?
- [ ] Santé : manager, indexer (`_cluster/health`), Filebeat, espace disque
- [ ] Active responses de la nuit : légitimes ? (log `/var/ossec/logs/active-responses.log`)
- [ ] Zéro alerte ≥ 10 restée « Nouveau »

### Hebdomadaire (2 h, responsable SOC)

- [ ] Revue CVE critiques/hautes → avancement patch (section 40)
- [ ] Top 10 règles bruyantes → 1–2 actions de tuning (section 50)
- [ ] Revue des comptes : nouveaux utilisateurs dashboard/API ?
- [ ] Vérifier que les sauvegardes Borg + snapshots se sont bien déroulées
- [ ] Journal d'incidents à jour

### Mensuelle (½ journée)

- [ ] Revue MITRE : couverture par tactique, règles manquantes (section 47)
- [ ] Score SCA moyen par groupe : dérive ? (section 42)
- [ ] Relecture des règles `overwrite` : toujours pertinentes ? (section 32)
- [ ] Rotation des logs/archivage conforme à la rétention légale
- [ ] Test d'une alerte de bout en bout (fausse attaque en lab → alerte → notification)
- [ ] Mise à jour planifiée si version corrective disponible (section 65)

### Trimestrielle / semestrielle

- [ ] Test de restauration (section 64)
- [ ] Exercice incident type ransomware (section 76)
- [ ] Revue des accès (comptes dashboard, API, clés d'agents)
- [ ] Rotation des clés d'agents critiques (section 21)

---

## 82. Pense-bête de poche : commandes essentielles

```bash
# ── Services ──────────────────────────────────────────────
sudo systemctl status wazuh-manager wazuh-indexer wazuh-dashboard filebeat
sudo systemctl restart wazuh-manager        # après modif de conf
sudo /var/ossec/bin/wazuh-control status    # statut interne du manager

# ── Agents ────────────────────────────────────────────────
sudo /var/ossec/bin/agent_control -l        # liste + états
sudo /var/ossec/bin/agent_control -i 007    # détail d'un agent
sudo /var/ossec/bin/agent_groups -l         # groupes
sudo /var/ossec/bin/manage_agents -l        # clés

# ── Règles / decoders ─────────────────────────────────────
sudo /var/ossec/bin/wazuh-logtest           # tester une ligne de log
sudo /var/ossec/bin/wazuh-analysisd -t      # tester la conf (avec -t)

# ── Cluster ───────────────────────────────────────────────
sudo /var/ossec/bin/cluster_control -l      # nœuds
sudo /var/ossec/bin/cluster_control -s      # statut synchro

# ── Indexer (remplacer <MDP>) ─────────────────────────────
curl -k -u admin:'<MDP>' 'https://localhost:9200/_cluster/health?pretty'
curl -k -u admin:'<MDP>' 'https://localhost:9200/_cat/indices/wazuh-alerts-*?v&s=index'

# ── API (remplacer <MDP>, TOKEN exporté en variable) ──────
TOKEN=$(curl -sk -u wazuh-wui:'<MDP>' https://localhost:55000/security/user/authenticate \
  | python3 -c "import sys,json; print(json.load(sys.stdin)['data']['token'])")
curl -sk -H "Authorization: Bearer $TOKEN" 'https://localhost:55000/agents/summary/status?pretty=true'

# ── Logs à connaître par cœur ─────────────────────────────
sudo tail -f /var/ossec/logs/ossec.log                 # manager
sudo tail -f /var/ossec/logs/alerts/alerts.json        # alertes JSON
sudo tail -f /var/ossec/logs/active-responses.log      # réponses actives
sudo tail -f /var/ossec/logs/ossec.log                 # agent (sur l'agent)
```

---

## 83. Ports et flux réseau : tableau de référence

| Port | Proto | Source → Destination | Service | Exposer |
|---|---|---|---|---|
| 1514 | UDP (TCP opt.) | Agents → Manager | Remontée événements | Parc supervisé |
| 1515 | TCP | Agents → Manager | Inscription (authd) | Parc (ou via provisioning) |
| 1516 | TCP | Manager ↔ Manager | Cluster | Inter-managers uniquement |
| 443 | TCP | Admins → Dashboard | IHM web | Admins / VPN |
| 9200 | TCP | Manager/Dashboard → Indexer | OpenSearch API | Inter-composants + admins |
| 9300 | TCP | Indexer ↔ Indexer | Cluster OpenSearch | Inter-indexers uniquement |
| 55000 | TCP | Admins/scripts → Manager | API Wazuh | Admins / supervision |
| 514 | UDP | Équipements → Manager/relais | Syslog | Équipements réseau |
| 22 | TCP | Admins → tous | SSH admin | Admins / bastion |

> Principe : **1514/1515** vers le parc, **tout le reste** réservé aux admins et aux interconnexions. Quand on hésite, on restreint.

---

## 84. Fichiers importants : où est quoi

### Manager (`/var/ossec/`)

| Fichier | Rôle |
|---|---|
| `/var/ossec/etc/ossec.conf` | Configuration principale du manager |
| `/var/ossec/etc/rules/local_rules.xml` | **Vos** règles |
| `/var/ossec/etc/rules/soc_tuning.xml` | Vos surcharges (suggéré) |
| `/var/ossec/etc/decoders/local_decoder.xml` | **Vos** decoders |
| `/var/ossec/etc/lists/` | Vos CDB lists |
| `/var/ossec/etc/shared/<groupe>/agent.conf` | Conf poussée aux agents |
| `/var/ossec/etc/client.keys` | Clés des agents |
| `/var/ossec/ruleset/` | Ruleset officiel (**ne pas modifier**) |
| `/var/ossec/logs/ossec.log` | Log principal |
| `/var/ossec/logs/alerts/alerts.json` | Alertes (source de Filebeat) |
| `/var/ossec/logs/active-responses.log` | Réponses actives |
| `/var/ossec/active-response/bin/` | Scripts de réponse |
| `/var/ossec/integrations/` | Scripts d'intégration (TheHive, webhooks) |
| `/var/ossec/api/configuration/api.yaml` | Configuration de l'API |
| `/var/ossec/queue/` | Files d'attente, DB agents |

### Indexer / Dashboard

| Fichier | Rôle |
|---|---|
| `/etc/wazuh-indexer/opensearch.yml` | Configuration de l'indexer |
| `/etc/wazuh-indexer/certs/` | Certificats TLS |
| `/var/lib/wazuh-indexer/` | **Données** (à mettre sur volume dédié) |
| `/etc/wazuh-dashboard/opensearch_dashboards.yml` | Configuration du dashboard |
| `/usr/share/wazuh-dashboard/data/wazuh/config/wazuh.yml` | Connexion dashboard → API manager |

### Agent Linux (`/var/ossec/`)

| Fichier | Rôle |
|---|---|
| `/var/ossec/etc/ossec.conf` | Configuration de l'agent |
| `/var/ossec/etc/client.keys` | Clé d'inscription |
| `/var/ossec/logs/ossec.log` | Log de l'agent |
| `/var/ossec/queue/syscheck/` | Base FIM locale |
| `/var/ossec/queue/sca/` | Résultats SCA |

---

## 85. Glossaire

| Terme | Définition |
|---|---|
| **Active Response** | Exécution automatique d'un script (blocage IP, etc.) sur déclenchement d'une règle. |
| **Agent** | Programme installé sur la machine surveillée ; collecte et transmet. |
| **Alerte** | Événement ayant déclenché une règle de niveau > seuil ; stockée dans `alerts.json`. |
| **API** | Interface REST du manager (port 55000) pour automatiser. |
| **authd** | Service d'inscription des agents (port 1515). |
| **CDB list** | Liste de valeurs (IP, utilisateurs...) utilisable dans les règles. |
| **Cluster** | Ensemble de managers synchronisés (1 master + N workers). |
| **CVE** | Référence publique d'une vulnérabilité (*Common Vulnerabilities and Exposures*). |
| **CVSS** | Score de gravité d'un CVE (0.0 à 10.0). |
| **Dashboard** | Interface web (OpenSearch Dashboards) de visualisation. |
| **Decoder** | Règle de parsing : transforme un log brut en champs structurés. |
| **EPS** | *Events Per Second* : débit d'événements, unité de dimensionnement. |
| **Filebeat** | Agent qui envoie `alerts.json` du manager vers l'indexer. |
| **FIM** | *File Integrity Monitoring* : surveillance des modifications de fichiers (syscheck). |
| **Indexer** | Composant de stockage/recherche basé sur OpenSearch (port 9200). |
| **ISM** | *Index State Management* : politiques de cycle de vie des index (rétention). |
| **MITRE ATT&CK** | Référentiel de tactiques et techniques d'attaque ; Wazuh mappe ses règles dessus. |
| **Manager** | Cœur de Wazuh : analyse (decoders, règles), FIM, SCA, réponses actives. |
| **Niveau (level)** | Sévérité d'une règle, de 0 (ignoré) à 16 (attaque en cours). |
| **Règle** | Condition sur des champs décodés → déclenche une alerte. |
| **SCA** | *Security Configuration Assessment* : audit de conformité (CIS...). |
| **SIEM** | *Security Information and Event Management* : centralisation et corrélation des logs. |
| **SLM** | *Snapshot Lifecycle Management* : snapshots automatiques de l'indexer. |
| **SOAR** | *Security Orchestration, Automation and Response* (ex. Shuffle). |
| **Syscheck** | Nom historique du module FIM dans Wazuh/OSSEC. |
| **Syscollector** | Module d'inventaire (logiciels, matériel, réseau, processus). |
| **Tuning** | Ajustement continu des règles pour réduire les faux positifs. |
| **Vulnerability Detector** | Module croisant l'inventaire logiciel avec les bases CVE. |
| **Wodle** | Module « wodle » : exécution périodique de commandes ou tâches (ex. `command`, `sca`). |
| **WPK** | Paquet Wazuh pour la mise à jour distante des agents. |
| **XDR** | *Extended Detection and Response* : détection + réponse sur les endpoints. |

---

## 86. Quiz : 10 questions + réponses

**Q1. Quels sont les 3 composants centraux de Wazuh et leurs ports par défaut ?**
> R : Le **manager** (1514/UDP agents, 1515/TCP inscription, 55000/TCP API), l'**indexer** (9200/TCP, basé sur OpenSearch) et le **dashboard** (443/TCP, basé sur OpenSearch Dashboards). Filebeat fait le lien manager → indexer.

**Q2. Pourquoi ne faut-il jamais modifier les fichiers de `/var/ossec/ruleset/` ?**
> R : C'est le ruleset officiel, **écrasé à chaque mise à jour**. Le custom va dans `/var/ossec/etc/rules/local_rules.xml` et `/var/ossec/etc/decoders/local_decoder.xml`.

**Q3. Dans quel ordre sont évalués decoders et règles, et pourquoi cet ordre compte ?**
> R : **Decoder d'abord** (parse le log brut en champs), **règles ensuite** (conditions sur les champs). Un log non décodé ne déclenchera jamais une règle fine.

**Q4. Quelle est la plage d'IDs réservée aux règles locales, et pourquoi ?**
> R : **100000–199999**. Elle évite toute collision avec les IDs du ruleset officiel lors des mises à jour.

**Q5. Citez les 3 modes de FIM et quand utiliser chacun.**
> R : **Planifié** (`frequency` : simple, détection différée), **temps réel** (`realtime` : immédiat, pour binaires/confs critiques), **who-data** (via auditd Linux : identifie *qui* a modifié, pour les confs sensibles). Jamais de temps réel sur `/var/log` ou `/tmp`.

**Q6. Avant d'activer l'active response `firewall-drop`, quelle configuration est obligatoire ?**
> R : La **allowlist** (`<white_list>` : LAN admin, VPN, supervision, IP publiques du SOC). Sans elle, on risque de se bannir soi-même ou de bloquer un équipement légitime.

**Q7. L'indexer affiche `status: red`. Que faites-vous en premier ?**
> R : C'est la **priorité absolue** : identifier les shards non assignés (`_cat/shards`, `_cat/allocation`), vérifier l'espace disque (flood stage à 95 % → lever le bloc `read_only_allow_delete` après avoir libéré de la place, section 69). Sans indexer, le SIEM est aveugle.

**Q8. Le dashboard est vide alors que les agents sont verts. Citez 3 maillons à vérifier dans l'ordre.**
> R : 1) `alerts.json` se remplit-il sur le manager ? 2) **Filebeat** tourne-t-il et envoie-t-il (TLS, 401) ? 3) Le *doc count* des index augmente-t-il côté indexer ? (+ vérifier la plage de temps et le pattern d'index dans le dashboard.)

**Q9. Quelle est la règle d'or des mises à jour Wazuh (ordre et précautions) ?**
> R : **Sauvegarde + changelog + fenêtre de maintenance** d'abord ; ordre : **indexer → manager → dashboard** ; manager toujours avant les agents ; `apt-mark hold` en temps normal ; mise à jour des agents **par vagues** (WPK), jamais tout le parc d'un coup.

**Q10. En tant que petite équipe, quel rituel hebdomadaire est le plus important et pourquoi ?**
> R : Le **tuning** (top 10 des règles bruyantes → 1–2 actions) couplé à la **revue des CVE critiques**. Sans tuning, le volume d'alertes sature l'équipe et les vraies attaques passent inaperçues ; sans revue CVE, le patch management dérive. Objectif : < 50 alertes/jour/analyste à qualifier.

---

## 87. Pour aller plus loin

### Documentation officielle (à mettre en favori)

- Documentation Wazuh 4.x : `https://documentation.wazuh.com/current/` — la référence, à jour à chaque version.
- Référence des règles : `https://documentation.wazuh.com/current/user-manual/ruleset/rules/` — descriptif de chaque règle officielle.
- API reference : `https://documentation.wazuh.com/current/user-manual/api/reference.html` — tous les endpoints.

### Communauté

- Slack Wazuh (`wazuh.slack.com`) : entraide rapide, canal `#general` et `#questions`.
- GitHub `wazuh/wazuh` : issues, propositions de règles, contributions.
- Forum et blog Wazuh : retours d'expérience, cas d'usage.

### Sujets d'approfondissement (par ordre de valeur pour une petite équipe)

1. **Règles de corrélation avancées** (`if_matched_sid`, `frequency`, `timeframe`, `same_source_ip`) : passer de la détection unitaire à la détection de campagnes.
2. **Threat intelligence** : intégrer des flux (MISP, listes d'IOC) via CDB lists dynamiques.
3. **Déploiement distribué + HA** (section 12) quand le parc dépasse ~150 agents.
4. **SOAR avec Shuffle** (section 55) : automatiser l'enrichissement et la réponse de niveau 1.
5. **Surveillance des conteneurs** : agent Wazuh + inventaire Docker/Kubernetes (wodle `docker-listener`).
6. **Conformité avancée** : écrire des politiques SCA métier complètes (section 43) et les présenter en audit.
7. **Sauvegarde 3-2-1 complète** : intégrer les snapshots Wazuh dans votre PRA existant (voir guide Proxmox : PBS, réplication).
8. **Exercices d'équipe** : rejouez les cas pratiques 1–6 (sections 75–80) chaque semestre ; mesurez le temps de détection et de réponse, et améliorez.

### Derniers conseils

- **Commencez petit** : 5 agents pilotes, 2 semaines de tuning, puis généralisez. Un déploiement « big bang » sur 200 agents sans tuning = 3 mois de bruit.
- **Le SIEM est un organisme vivant** : règles, tuning, mises à jour, exercices. Prévoyez ~0,5 ETP pour le faire vivre sur un parc de 100–200 machines.
- **Mesurez** : temps moyen de détection, temps moyen de qualification, % de faux positifs, score CIS. Ce qui se mesure s'améliore — et se défend en budget.

---

*Fin du guide. Bon SOC !*
