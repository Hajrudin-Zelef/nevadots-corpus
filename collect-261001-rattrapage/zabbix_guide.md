# Guide Zabbix complet — Supervision d'infrastructure en production

> **Public** : Zelef, chef de service systèmes & énergies, sysadmin.
> **Angle** : pratique, production, contexte Afrique de l'Ouest (coupures électriques fréquentes, liens réseau instables, onduleurs au cœur du métier).
> **Versions couvertes** : Zabbix **6.0 LTS**, **6.4**, **7.0 LTS**, **7.2/7.4** (les différences sont signalées par des ⚠️).
> **OS cibles** : Debian 12/13, Ubuntu 22.04 LTS / 24.04 LTS.
> **Avertissement** : les valeurs de dimensionnement sont des ordres de grandeur à valider sur votre parc réel. Testez toujours en préproduction avant d'appliquer en production.

---

## Table des matières

1. Pourquoi Zabbix (et pas une autre solution)
2. Comment utiliser ce guide
3. Conventions utilisées dans ce guide
4. Architecture générale de Zabbix
5. Les composants en détail : Server
6. Les composants en détail : Agent 2 (et Agent 1)
7. Les composants en détail : Proxy
8. Les composants en détail : Frontend web
9. Les composants en détail : base de données (MySQL vs PostgreSQL)
10. Ports réseau et flux — tableau de référence
11. Choisir sa version : LTS vs standard
12. Dimensionnement : CPU, RAM, disque, IOPS
13. Prérequis système et réseau
14. Installation pas à pas : Zabbix Server + PostgreSQL sur Debian/Ubuntu
15. Installation pas à pas : variante MySQL/MariaDB
16. Installation pas à pas : le frontend web (Nginx + PHP-FPM)
17. Installation pas à pas : Zabbix Agent 2 sur Linux
18. Installation pas à pas : Zabbix Agent sur Windows
19. Installation pas à pas : Zabbix Proxy
20. Sécuriser la communication : PSK et certificats TLS
21. Premier démarrage : assistant de configuration du frontend
22. Configuration initiale : réglages globaux essentiels
23. Utilisateurs, groupes, rôles et permissions
24. Audit et traçabilité
25. Concepts clés : hôtes, groupes d'hôtes, interfaces
26. Ajouter son premier hôte pas à pas
27. Templates : principes et bonnes pratiques
28. Templates officiels essentiels (Linux, Windows, SNMP, réseau)
29. Créer et lier un template personnalisé
30. Items : actifs vs passifs, trapper, dépendants
31. Les clés d'items de l'agent : les indispensables
32. Items calculés et agrégés
33. Prétraitement des valeurs (preprocessing)
34. Intervalles, périodes de collecte et flex intervals
35. Triggers : syntaxe et logique
36. Fonctions de trigger : le catalogue indispensable (last, avg, nodata…)
37. Hystérésis et seuils intelligents
38. Sévérités et bonnes pratiques de nommage
39. Dépendances entre triggers (éviter les tempêtes d'alertes)
40. Actions : le moteur de notification
41. Médias d'alerte : email
42. Médias d'alerte : SMS via passerelle (contexte Afrique de l'Ouest)
43. Médias d'alerte : Telegram
44. Webhooks : Slack, Teams, et autres
45. Escalades et acquittements
46. Découverte réseau automatique
47. Low-Level Discovery (LLD) : disques, interfaces, processus
48. Écrire ses propres règles LLD (UserParameter + JSON)
49. Supervision SNMP : principes (v2c vs v3)
50. Superviser un switch via SNMP pas à pas
51. Superviser un onduleur via SNMP (cas métier — le cœur de ce guide)
52. Template onduleur : items, triggers et dashboard dédiés
53. Supervision JMX (Java/Tomcat)
54. Supervision IPMI (températures, ventilateurs, alimentations)
55. Supervision VMware (intégration native)
56. Supervision web : scénarios et checks HTTP
57. Maintenance planifiée (sans alerte parasite)
58. Haute disponibilité native (Zabbix 6.0+)
59. Housekeeping : comprendre la rétention des données
60. Partitionnement de la base (PostgreSQL/MySQL) pour les gros parcs
61. Performance et tuning du server/proxy
62. Usage quotidien : la routine de l'exploitant
63. Dashboards et vues : construire des écrans d'exploitation
64. Inventaire automatique des hôtes
65. Cartes réseau (maps) et écrans de supervision
66. Bonnes pratiques d'alerting : la chasse au bruit
67. Intégration avec Grafana
68. Sécurité et durcissement en production
69. Superviser Zabbix lui-même (qui surveille le surveillant ?)
70. Sauvegarde : base de données, fichiers, stratégie 3-2-1
71. Restauration : procédure testée
72. Mise à jour de version : méthode sans stress
73. Dépannage : méthodologie générale
74. Erreur n°1 à n°12 : les classiques commentés
75. Cas pratique n°1 : supervision d'une baie complète
76. Cas pratique n°2 : supervision d'un onduleur 40 kVA de A à Z
77. Cas pratique n°3 : alerte coupure électrique avec escalade SMS
78. Cas pratique n°4 : supervision d'un lien VSAT/fibre instable
79. Cas pratique n°5 : capacity planning disque avec prédiction
80. Cas pratique n°6 : supervision d'une salle serveur (température, hygrométrie)
81. Cas pratique n°7 : déploiement multi-sites avec proxies
82. Cas pratique n°8 : supervision d'un cluster Proxmox VE
83. Cas pratique n°9 : blackout test — valider toute la chaîne d'alerte
84. Cas pratique n°10 : rapport mensuel de disponibilité pour la direction
85. Pense-bête de poche (cheat sheet imprimable)
86. Glossaire
87. Quiz : 10 questions pour valider (avec réponses)
88. Pour aller plus loin

---

## 1. Pourquoi Zabbix (et pas une autre solution)

Zabbix est une plateforme de supervision **open source**, mature (depuis 2001), sans coût de licence, capable de superviser **plusieurs dizaines de milliers d'équipements** avec un seul serveur correctement dimensionné. Pour un service systèmes & énergies en Afrique de l'Ouest, ses atouts sont concrets :

- **Zéro coût de licence** : budget préservé pour le matériel (sondes, serveurs, onduleurs).
- **SNMP natif et puissant** : switchs, routeurs, onduleurs, sondes environnementales parlent SNMP ; Zabbix les interroge sans agent.
- **Proxy** : supervise des sites distants même avec un lien intermittent (le proxy stocke localement et envoie quand le lien revient — vital avec des liaisons VSAT ou 4G instables).
- **LLD (Low-Level Discovery)** : découvre automatiquement disques, interfaces réseau, partitions — fini la configuration manuelle machine par machine.
- **Alerting flexible** : email, SMS via passerelle GSM locale, Telegram — adapté aux réalités du terrain où l'email n'est pas toujours lu à temps.
- **Pas de dépendance cloud obligatoire** : tout tourne on-premise, vos données restent chez vous.

Comparaison rapide (ordre de grandeur, à valider selon votre contexte) :

| Solution | Licence | Agent | SNMP | Proxy/offline | Courbe d'apprentissage |
|---|---|---|---|---|---|
| Zabbix | Gratuite (GPL) | Oui (Agent 2) | Natif | Oui (proxy) | Moyenne |
| Nagios / Centreon | Gratuite / payante | NRPE/NSClient | Via plugins | Partielle | Moyenne à forte |
| Prometheus | Gratuite | Exporters | Via snmp_exporter | Non natif | Forte (PromQL) |
| PRTG | Payante au capteur | Oui | Oui | Sondes distantes | Faible |

> 💡 **Conseil de terrain** : si votre équipe est petite (1 à 3 personnes), Zabbix offre le meilleur ratio puissance/simplicité d'exploitation au quotidien.

## 2. Comment utiliser ce guide

- **Débutant Zabbix** : lisez dans l'ordre, de la section 4 à la section 47, en montant une maquette en parallèle (une VM suffit).
- **Déjà utilisateur** : piochez dans les sections 48+ (SNMP, onduleur, HA, tuning, dépannage).
- **En astreinte** : imprimez la section 85 (pense-bête) et gardez la section 74 (erreurs classiques) sous la main.
- Tous les exemples de configuration sont **testés syntaxiquement** pour Zabbix 6.x/7.x. Les mots de passe sont fictifs (`mot_de_passe_ici`) : remplacez-les.

## 3. Conventions utilisées dans ce guide

| Convention | Signification |
|---|---|
| `code en chasse fixe` | Commande, fichier, valeur à saisir |
| ⚠️ | Dépend de la version (6.x vs 7.x) ou point de vigilance |
| 💡 | Conseil de terrain / retour d'expérience |
| 🖨️ | Checklist imprimable |
| `mot_de_passe_ici` | Mot de passe fictif à remplacer |
| `#` en début de ligne shell | Commande exécutée en root |
| `$` en début de ligne shell | Commande exécutée en utilisateur normal |

Les exemples utilisent ces noms fictifs (adaptez à votre plan de nommage) :

| Rôle | Nom d'hôte | IP |
|---|---|---|
| Serveur Zabbix | `zabbix-srv` | `192.168.10.50` |
| Proxy site distant | `zabbix-proxy-sud` | `192.168.20.50` |
| Serveur supervisé | `srv-fichiers-01` | `192.168.10.61` |
| Onduleur 40 kVA | `ups-salle-01` | `192.168.10.90` |
| Switch cœur | `sw-coeur-01` | `192.168.10.2` |

## 4. Architecture générale de Zabbix

```
                                    ┌─────────────────────────┐
                                    │      FRONTEND WEB       │
                                    │  (Nginx + PHP-FPM)      │
                                    └────────────┬────────────┘
                                                 │ SQL (lecture/écriture)
┌──────────┐   ┌──────────┐   ┌──────────────────▼──────────────────┐   ┌──────────┐
│ Agent 2  │   │  Proxy   │   │          ZABBIX SERVER              │   │  SNMP    │
│(actif/   │──▶│(site     │──▶│  - collecte (pollers, trappers)     │◀──│(switch,  │
│ passif)  │   │ distant) │   │  - calcul triggers                  │   │ onduleur)│
└──────────┘   └──────────┘   │  - actions / alertes                │   └──────────┘
                              │  - housekeeper                      │
                              └────────────┬──────────────────┘
                                           │ SQL
                              ┌────────────▼────────────┐
                              │   BASE DE DONNÉES       │
                              │ PostgreSQL ou MySQL     │
                              │ (history, trends, events)│
                              └─────────────────────────┘
```

**Flux de données simplifié** :

1. Le **server** (ou le **proxy**) collecte les métriques : il interroge (mode passif), reçoit (mode actif/trapper), ou scrute (SNMP, IPMI, JMX, HTTP).
2. Les valeurs sont stockées dans la **base** : `history` (données brutes, courte rétention) et `trends` (agrégats horaires, longue rétention).
3. Le server **évalue les triggers** à chaque nouvelle valeur.
4. Si un trigger passe à PROBLEM, le server déclenche les **actions** (email, SMS, Telegram…).
5. Le **frontend** lit la base pour afficher dashboards, graphes, événements.

> 💡 Retenez ce mantra : **collecte → stockage → évaluation → alerte → visualisation**. 90 % des problèmes de dépannage se situent sur l'une de ces 5 étapes.

## 5. Les composants en détail : Server

Le **Zabbix Server** est le chef d'orchestre. C'est un démon (`zabbix_server`) écrit en C, multi-processus :

| Processus | Rôle | Paramètre de dimensionnement |
|---|---|---|
| `poller` | Interroge les agents en mode passif, SNMP, etc. | `StartPollers=` |
| `unreachable poller` | Réessaie les hôtes injoignables | `StartPollersUnreachable=` |
| `trapper` | Reçoit les données envoyées (actif, trapper, proxy) | `StartTrappers=` |
| `preprocessing` | Applique le prétraitement des items | `StartPreprocessors=` |
| `history syncer` | Écrit les valeurs en base | `StartHistorySyncers=` / `HistoryStorageDateIndex` |
| `escalator` | Gère les escalades d'actions | `StartEscalators=` |
| `alert manager` | Dispatche vers les médias | `StartAlerters=` |
| `housekeeper` | Supprime les vieilles données | `HousekeepingFrequency=` |
| `discoverer` | Découverte réseau | `StartDiscoverers=` |
| `http poller` | Checks web/HTTP | `StartHTTPPollers=` |
| `snmp trapper` | Reçoit les SNMP traps | `StartSNMPTrapper=` (désactivé par défaut) |

⚠️ **Zabbix 7.x** : le server sait écrire l'historique dans un ** TimescaleDB** ? Non — restons précis : Zabbix supporte officiellement **PostgreSQL + TimescaleDB** (recommandé pour les gros volumes, compression native) et **MySQL/MariaDB**. Le support TimescaleDB existe depuis la 5.0.

Points clés à retenir :

- Le server est **le seul** à évaluer les triggers et à envoyer les alertes (le proxy ne fait que collecter et relayer).
- Le server peut fonctionner en **cluster HA natif** depuis la 6.0 (section 58).
- Fichier de config : `/etc/zabbix/zabbix_server.conf`.

## 6. Les composants en détail : Agent 2 (et Agent 1)

Deux agents coexistent :

| Critère | Zabbix Agent (1) | Zabbix Agent 2 |
|---|---|---|
| Langage | C | Go |
| Plugins | Limités | **Modulaires** (natif : PostgreSQL, MySQL, Redis, Docker, systemd, MQTT, SNMP…) |
| Checks actifs/passifs | Oui | Oui |
| Exécution persistante | Non | Oui (connexions maintenues) |
| Recommandé pour | Parc existant | ✅ **Tout nouveau déploiement** |

> 💡 **Déployez Agent 2 partout** sur les nouveaux serveurs. L'agent 1 reste supporté mais ne reçoit plus les nouveautés.

**Modes de fonctionnement** :

- **Passif** : le server/proxy **demande** la valeur à l'agent (port 10050/TCP sur l'agent). Simple, mais le server porte la charge de planification.
- **Actif** : l'agent **envoie** les valeurs au server/proxy (port 10051/TCP côté server). L'agent récupère sa liste d'items à surveiller, puis pousse. Idéal derrière un NAT ou un pare-feu (seul le flux sortant est nécessaire), et recommandé pour les gros parcs.

```
PASSIF :  server ──(demande :10050)──▶ agent ──(réponse)──▶ server
ACTIF  :  agent ──(récupère liste :10051)──▶ server
          agent ──(envoie valeurs :10051)──▶ server
```

Fichier de config : `/etc/zabbix/zabbix_agent2.conf`. Clés utiles : `Server=` (qui peut interroger), `ServerActive=` (à qui envoyer en actif), `Hostname=` (doit matcher le nom dans le frontend !).

## 7. Les composants en détail : Proxy

Le **proxy** (`zabbix_proxy`) collecte pour le compte du server sur un site distant ou un segment isolé :

- **Stocke localement** (SQLite par défaut, MySQL/PostgreSQL possible) puis **transfère** au server. Si le lien tombe, **aucune donnée n'est perdue** (dans la limite du disque et de `ProxyOfflineBuffer`).
- **Modes** : actif (le proxy se connecte au server — à privilégier derrière un NAT) ou passif (le server se connecte au proxy).
- Le proxy **ne calcule pas les triggers** et **n'envoie pas d'alertes** : il relaie les données brutes.

```
Site distant (liaison 4G instable)
┌─────────────┐      ┌──────────────┐              ┌──────────────┐
│  Agents     │─────▶│    PROXY     │────(quand le──▶│    SERVER    │
│  SNMP       │      │ (buffer local│   lien est up) │  (central)   │
└─────────────┘      └──────────────┘              └──────────────┘
```

> 💡 **Cas d'usage typique en Afrique de l'Ouest** : un proxy par site distant (agence, site technique) avec `ProxyMode=0` (actif). Le proxy encaisse les coupures de liaison sans perdre l'historique local.

Paramètres clés : `ProxyMode=`, `Server=`, `Hostname=` (doit matcher le nom du proxy créé dans le frontend), `DBName/DBUser/DBPassword`, `ProxyOfflineBuffer=` (défaut 1h — montez à 24h+ sur lien instable), `HeartbeatFrequency=`.

## 8. Les composants en détail : Frontend web

Interface PHP (`zabbix-web`) : dashboards, configuration, graphes, cartes, inventaire, audit. Elle **ne collecte rien** : elle lit/écrit uniquement en base.

- Serveur web : **Nginx (recommandé)** ou Apache + PHP-FPM.
- ⚠️ **Zabbix 7.x exige PHP 8.0+** ; Zabbix 6.0 LTS accepte PHP 7.2.5+. Vérifiez la matrice officielle pour votre version exacte.
- Le frontend peut être installé **sur une machine dédiée** (bonne pratique : séparer web et server en production).
- Fichier de config : `/etc/zabbix/web/zabbix.conf.php` (généré par l'assistant d'installation).

## 9. Les composants en détail : base de données (MySQL vs PostgreSQL)

| Critère | PostgreSQL (+ TimescaleDB) | MySQL / MariaDB |
|---|---|---|
| Recommandation Zabbix | ✅ **Recommandé officiellement** | Supporté |
| Gros volumes | Excellent (TimescaleDB : compression, retention policies) | Correct avec partitionnement manuel |
| Partitionnement | Natif via TimescaleDB | Manuel (section 60) |
| Simplicité | Un peu plus exigeant | Plus familier pour beaucoup d'équipes |
| HA base | Patroni / streaming replication | Galera / réplication |

> 💡 **Choix recommandé pour un nouveau déploiement** : **PostgreSQL 15/16 + TimescaleDB**. La compression des historiques divise l'espace disque par 5 à 10 sur les métriques numériques — décisif quand on supervise des onduleurs et des switchs avec des pas de 30 s.

Tailles indicatives (à valider) :

| Parc | history (90 j) | trends (2 ans) | Total indicatif |
|---|---|---|---|
| 100 hôtes / 5 000 items | ~30 Go | ~5 Go | ~40 Go |
| 500 hôtes / 30 000 items | ~200 Go | ~30 Go | ~250 Go |
| 1 000 hôtes / 80 000 items | ~500 Go | ~80 Go | ~650 Go |

## 10. Ports réseau et flux — tableau de référence

🖨️ **À afficher en salle d'exploitation / à fournir à l'équipe réseau pour les ouvertures de flux.**

| Source → Destination | Port | Protocole | Usage |
|---|---|---|---|
| Server → Agent | 10050 | TCP | Checks passifs |
| Agent → Server/Proxy | 10051 | TCP | Checks actifs, envoi proxy |
| Proxy → Server | 10051 | TCP | Transfert données (proxy actif) |
| Server → Proxy | 10050 | TCP | Proxy passif (rare) |
| Server → équipement | 161 | UDP | SNMP poll |
| Équipement → Server | 162 | UDP | SNMP traps |
| Server → équipement | 623 | UDP | IPMI |
| Server → équipement | 12345 | TCP | JMX (variable) |
| Navigateur → Frontend | 80/443 | TCP | Interface web |
| Frontend → Base | 5432 / 3306 | TCP | PostgreSQL / MySQL |
| Server → Base | 5432 / 3306 | TCP | Écriture données |
| Server → SMTP | 25/587 | TCP | Alertes email |
| Server → API Telegram | 443 | TCP | Alertes Telegram |

> ⚠️ N'ouvrez que les flux nécessaires. Un agent en **mode 100 % actif** n'a besoin d'aucun flux entrant : seul le 10051/TCP sortant vers le server/proxy.

## 11. Choisir sa version : LTS vs standard

| Version | Type | Fin de support (indicative — vérifiez le site officiel) |
|---|---|---|
| 6.0 | LTS | Déjà/ancien — ne plus déployer en 2026 |
| 6.4 | Standard | Support court — éviter en production |
| **7.0** | **LTS** | ✅ **Choix recommandé en 2026** |
| 7.2 / 7.4 | Standard | Nouveautés, support court |

**Règle d'or** : en production, déployez une **LTS** (7.0 en 2026). Les versions standard servent à tester les nouveautés en labo.

⚠️ Ce guide couvre 6.x et 7.x ; les différences notables sont signalées. Si vous êtes encore en 6.0 LTS, planifiez la migration (section 72).

## 12. Dimensionnement : CPU, RAM, disque, IOPS

Formule de base : comptez en **valeurs par seconde (vps)** = nombre d'items / intervalle moyen.

```
Exemple : 10 000 items à 60 s  →  ~167 vps
          30 000 items à 60 s  →  ~500 vps
```

| Charge | vCPU server | RAM server | Disque base | IOPS |
|---|---|---|---|---|
| < 100 vps (petit parc) | 2 | 4 Go | 100 Go SSD | 500 |
| 100–500 vps (moyen) | 4 | 8–16 Go | 300 Go SSD/NVMe | 2 000 |
| 500–2 000 vps (gros) | 8 | 32 Go | 1 To NVMe | 5 000+ |
| > 2 000 vps | Cluster HA + base dédiée + TimescaleDB | | | |

> 💡 Le **disque de la base** est presque toujours le goulot : privilégiez du **SSD/NVMe local** plutôt qu'un SAN lent. `CacheSize`, `HistoryCacheSize`, `TrendCacheSize` (section 61) absorbent les pics mais ne remplacent pas des IOPS corrects.

## 13. Prérequis système et réseau

🖨️ **Checklist pré-installation :**

- [ ] Debian 12/13 ou Ubuntu 22.04/24.04 à jour (`apt update && apt full-upgrade`)
- [ ] Nom d'hôte FQDN résolu (`hostname -f` répond)
- [ ] NTP/chrony synchronisé (`chronyc tracking` : `Leap status : Normal`)
- [ ] Partition `/var/lib/postgresql` (ou `/var/lib/mysql`) dimensionnée
- [ ] Flux réseau ouverts (section 10)
- [ ] Accès internet pour les dépôts (ou miroir local)
- [ ] Sauvegarde existante du système (snapshot VM avant l'installation)

```bash
# Vérifications de base
hostname -f
timedatectl status | grep -i "synchronized\|NTP"
df -h /var/lib/postgresql
free -h
```

> ⚠️ **Le temps est critique** : un décalage NTP fausse les timestamps des données et casse les triggers `nodata()`. Synchronisez **tous** les supervisés et le server sur la même source NTP.

## 14. Installation pas à pas : Zabbix Server + PostgreSQL sur Debian/Ubuntu

> Procédure pour **Zabbix 7.0 LTS** sur **Debian 12** (adaptable à Ubuntu 22.04/24.04 — seuls les noms de dépôts changent). ⚠️ Vérifiez toujours la [page de téléchargement officielle](https://www.zabbix.com/download) : les URL de dépôts ci-dessous suivent le schéma officiel.

### 14.1. Ajouter le dépôt officiel Zabbix

```bash
# En root
wget https://repo.zabbix.com/zabbix/7.0/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.0+debian12_all.deb
dpkg -i zabbix-release_latest_7.0+debian12_all.deb
apt update
```

Pour Ubuntu 22.04, remplacez `debian` par `ubuntu` et `debian12` par `ubuntu22.04` dans l'URL (schéma officiel).

### 14.2. Installer PostgreSQL et TimescaleDB

```bash
apt install -y postgresql-15 postgresql-client-15
# TimescaleDB (fortement recommandé) — dépôt officiel Timescale :
apt install -y gnupg postgresql-common apt-transport-https lsb-release wget
/usr/share/postgresql-common/pgdg/apt.postgresql.org.sh
echo "deb https://packagecloud.io/timescale/timescaledb/debian/ $(lsb_release -c -s) main" \
  > /etc/apt/sources.list.d/timescaledb.list
wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | apt-key add -
apt update
apt install -y timescaledb-2-postgresql-15
```

Activez l'extension dans `postgresql.conf` :

```bash
echo "shared_preload_libraries = 'timescaledb'" >> /etc/postgresql/15/main/postgresql.conf
systemctl restart postgresql
```

### 14.3. Créer la base et l'utilisateur Zabbix

```bash
sudo -u postgres psql <<'EOF'
CREATE USER zabbix WITH PASSWORD 'mot_de_passe_ici';
CREATE DATABASE zabbix OWNER zabbix;
\c zabbix
CREATE EXTENSION IF NOT EXISTS timescaledb;
EOF
```

> 💡 Notez ce mot de passe dans votre coffre (KeePass, Vault…) — il sera réutilisé dans `zabbix_server.conf`.

### 14.4. Installer le server et importer le schéma

```bash
apt install -y zabbix-server-pgsql zabbix-sql-scripts
# Import du schéma (long : plusieurs minutes sur petite VM)
zcat /usr/share/zabbix-sql-scripts/postgresql/server.sql.gz \
  | sudo -u zabbix psql zabbix
# Avec TimescaleDB : activez la compression des hypertables
zcat /usr/share/zabbix-sql-scripts/postgresql/timescaledb/schema.sql.gz \
  | sudo -u zabbix psql zabbix
```

⚠️ **Zabbix 7.x** : le script `timescaledb/schema.sql` crée les hypertables et les policies de compression. Ne l'oubliez pas, sinon vous perdez le bénéfice TimescaleDB.

### 14.5. Configurer `zabbix_server.conf`

```bash
cp /etc/zabbix/zabbix_server.conf /etc/zabbix/zabbix_server.conf.orig
```

Éditez `/etc/zabbix/zabbix_server.conf` (extrait des paramètres essentiels) :

```ini
# --- Base de données ---
DBHost=localhost
DBName=zabbix
DBUser=zabbix
DBPassword=mot_de_passe_ici
DBPort=5432

# --- Caches (à ajuster selon RAM, voir section 61) ---
CacheSize=256M
HistoryCacheSize=128M
HistoryIndexCacheSize=64M
TrendCacheSize=128M
ValueCacheSize=256M

# --- Pollers (point de départ raisonnable) ---
StartPollers=20
StartPollersUnreachable=5
StartTrappers=10
StartPreprocessors=10
StartHistorySyncers=8
StartEscalators=5
StartAlerters=5
StartDiscoverers=3

# --- Housekeeping ---
HousekeepingFrequency=1
MaxHousekeeperDelete=5000

# --- Divers ---
LogFile=/var/log/zabbix/zabbix_server.log
LogFileSize=50
PidFile=/run/zabbix/zabbix_server.pid
SocketDir=/run/zabbix
Timeout=10
FpingLocation=/usr/sbin/fping
```

### 14.6. Démarrer et activer le service

```bash
systemctl enable --now zabbix-server
systemctl status zabbix-server --no-pager
tail -n 50 /var/log/zabbix/zabbix_server.log
```

Le log doit se terminer par quelque chose comme `Zabbix Server started. Zabbix 7.0.x`. Si ce n'est pas le cas → section 74 (dépannage).

## 15. Installation pas à pas : variante MySQL/MariaDB

Si votre équipe maîtrise mieux MySQL, voici la variante (Debian 12) :

```bash
apt install -y mariadb-server zabbix-server-mysql zabbix-sql-scripts
systemctl enable --now mariadb
mysql_secure_installation   # répondez aux questions (root, suppression anonyme...)
```

Créez la base avec le bon charset (**obligatoire** : `utf8mb4`) :

```sql
CREATE DATABASE zabbix CHARACTER SET utf8mb4 COLLATE utf8mb4_bin;
CREATE USER 'zabbix'@'localhost' IDENTIFIED BY 'mot_de_passe_ici';
GRANT ALL PRIVILEGES ON zabbix.* TO 'zabbix'@'localhost';
SET GLOBAL log_bin_trust_function_creators = 1;
```

Importez le schéma :

```bash
zcat /usr/share/zabbix-sql-scripts/mysql/server.sql.gz | mysql --default-character-set=utf8mb4 -u zabbix -p zabbix
# Puis remettez la sécurité :
mysql -e "SET GLOBAL log_bin_trust_function_creators = 0;"
```

Dans `/etc/zabbix/zabbix_server.conf` : `DBHost=localhost`, `DBName=zabbix`, `DBUser=zabbix`, `DBPassword=mot_de_passe_ici`.

> ⚠️ Oublier `utf8mb4` provoque des erreurs d'import et des problèmes d'affichage (accents, emojis dans les noms d'hôtes). C'est l'erreur classique n°3 (section 74).

## 16. Installation pas à pas : le frontend web (Nginx + PHP-FPM)

```bash
apt install -y zabbix-frontend-php zabbix-nginx-conf php8.2-fpm php8.2-pgsql \
  php8.2-bcmath php8.2-mbstring php8.2-xml php8.2-gd php8.2-ldap
# (adaptez php8.2-* à la version PHP de votre OS : php8.1-* sur Ubuntu 22.04)
```

⚠️ **Versions PHP** : Zabbix 7.0 exige **PHP 8.0 minimum**. Debian 12 fournit PHP 8.2, Ubuntu 22.04 PHP 8.1, Ubuntu 24.04 PHP 8.3 — tous compatibles 7.0.

Configuration Nginx fournie : `/etc/zabbix/nginx.conf`. Copiez-la ou incluez-la :

```bash
cp /etc/zabbix/nginx.conf /etc/nginx/conf.d/zabbix.conf
# Éditez : server_name, listen 80/443, root /usr/share/zabbix
systemctl enable --now nginx php8.2-fpm
```

Extrait type de `/etc/nginx/conf.d/zabbix.conf` :

```nginx
server {
    listen 80;
    server_name zabbix.votre-domaine.local;
    root /usr/share/zabbix;
    index index.php;

    location = /favicon.ico { log_not_found off; }
    location / {
        try_files $uri $uri/ =404;
    }
    location /assets {
        access_log off;
        expires 10d;
    }
    location ~ /\.ht {
        deny all;
    }
    location ~ /(api\/|conf[^\.]|include|locale) {
        deny all;
        return 404;
    }
    location ~ [^/]\.php(/|$) {
        fastcgi_pass unix:/var/run/php/php8.2-fpm.sock;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

> 💡 **Passez en HTTPS** dès que possible (section 68) : les identifiants transitent sinon en clair.

## 17. Installation pas à pas : Zabbix Agent 2 sur Linux

```bash
# Dépôt déjà ajouté (section 14.1)
apt install -y zabbix-agent2 zabbix-agent2-plugin-*
```

`/etc/zabbix/zabbix_agent2.conf` — mode **actif** (recommandé derrière NAT/pare-feu) :

```ini
PidFile=/run/zabbix/zabbix_agent2.pid
LogFile=/var/log/zabbix/zabbix_agent2.log
LogFileSize=20

# Le server/proxy à qui envoyer les données actives
ServerActive=192.168.10.50:10051
# Autoriser aussi les checks passifs depuis le server (optionnel)
Server=192.168.10.50
# DOIT correspondre exactement au "Host name" dans le frontend
Hostname=srv-fichiers-01

Include=/etc/zabbix/zabbix_agent2.d/*.conf
```

```bash
systemctl enable --now zabbix-agent2
systemctl status zabbix-agent2 --no-pager
```

Vérification depuis le server :

```bash
# Test d'un check passif
zabbix_get -s 192.168.10.61 -k agent.hostname
zabbix_get -s 192.168.10.61 -k system.cpu.num
```

## 18. Installation pas à pas : Zabbix Agent sur Windows

1. Téléchargez l'archive `zabbix_agent2-7.0.x-windows-amd64-openssl.zip` depuis zabbix.com/download.
2. Décompressez dans `C:\Program Files\Zabbix Agent 2\`.
3. Éditez `conf\zabbix_agent2.conf` :

```ini
ServerActive=192.168.10.50:10051
Server=192.168.10.50
Hostname=SRV-WIN-01
```

> ⚠️ Le `Hostname` doit correspondre **exactement** (casse comprise) au nom d'hôte déclaré dans le frontend, sinon les données actives sont rejetées (`"host not found"` dans le log).

4. Installez le service (PowerShell en administrateur) :

```powershell
cd "C:\Program Files\Zabbix Agent 2"
.\zabbix_agent2.exe --config conf\zabbix_agent2.conf --install
Start-Service "Zabbix Agent 2"
Get-Service "Zabbix Agent 2"
```

5. Ouvrez le pare-feu Windows pour le 10050/TCP entrant (si checks passifs) :

```powershell
New-NetFirewallRule -DisplayName "Zabbix Agent" -Direction Inbound `
  -Protocol TCP -LocalPort 10050 -Action Allow
```

## 19. Installation pas à pas : Zabbix Proxy

Sur la machine du site distant (Debian/Ubuntu, dépôt ajouté) :

```bash
# Proxy actif + SQLite (simple) ou PostgreSQL (recommandé si > 2000 items)
apt install -y zabbix-proxy-sqlite3
# ou : apt install -y zabbix-proxy-pgsql
```

`/etc/zabbix/zabbix_proxy.conf` (proxy **actif**, SQLite) :

```ini
ProxyMode=0
Server=192.168.10.50
Hostname=zabbix-proxy-sud
LogFile=/var/log/zabbix/zabbix_proxy.log
PidFile=/run/zabbix/zabbix_proxy.pid
DBName=/var/lib/zabbix/zabbix_proxy.db
DBUser=zabbix

# Buffer offline : encaisse 24 h de coupure de liaison
ProxyOfflineBuffer=24h
HeartbeatFrequency=60
ConfigFrequency=300
DataSenderFrequency=5

# Dimensionnement collecte locale
StartPollers=15
StartTrappers=5
CacheSize=128M
Timeout=10
```

```bash
mkdir -p /var/lib/zabbix && chown zabbix:zabbix /var/lib/zabbix
systemctl enable --now zabbix-proxy
```

Puis **créez le proxy dans le frontend** : *Administration → Proxies → Create proxy*, nom **exact** `zabbix-proxy-sud`, mode *Active*. Sans cette déclaration côté server, le proxy reste en `no data`.

> 💡 `ProxyOfflineBuffer=24h` : avec une liaison 4G capricieuse, c'est la différence entre "trou dans les graphes" et "données complètes au retour du lien".

## 20. Sécuriser la communication : PSK et certificats TLS

Par défaut, les échanges server ↔ agent/proxy sont **en clair**. En production : chiffrez.

### 20.1. PSK (Pre-Shared Key) — simple et efficace

Générez une clé par hôte (ou par groupe) :

```bash
openssl rand -hex 32 > /etc/zabbix/psk_srv-fichiers-01.key
chmod 640 /etc/zabbix/psk_srv-fichiers-01.key
chown root:zabbix /etc/zabbix/psk_srv-fichiers-01.key
cat /etc/zabbix/psk_srv-fichiers-01.key
# exemple : 9f8a7b6c5d4e3f2a1b0c9d8e7f6a5b4c3d2e1f0a9b8c7d6e5f4a3b2c1d0e
```

Côté agent (`zabbix_agent2.conf`) :

```ini
TLSConnect=psk
TLSAccept=psk
TLSPSKFile=/etc/zabbix/psk_srv-fichiers-01.key
TLSPSKIdentity=psk-srv-fichiers-01
```

Côté frontend, sur l'hôte : *Configuration → Hosts → srv-fichiers-01 → Encryption* : cocher PSK pour les connexions, renseigner **la même** identité PSK et la clé.

Pour le proxy : paramètres `TLSConnect`, `TLSAccept`, `TLSPSKFile`, `TLSPSKIdentity` dans `zabbix_proxy.conf`, et déclaration PSK dans *Administration → Proxies*.

### 20.2. Certificats — pour les environnements exigeants

```ini
# zabbix_agent2.conf (extrait)
TLSConnect=cert
TLSAccept=cert
TLSCAFile=/etc/zabbix/ca.crt
TLSCertFile=/etc/zabbix/agent.crt
TLSKeyFile=/etc/zabbix/agent.key
```

> 💡 **Recommandation pragmatique** : PSK suffit dans 90 % des cas internes. Réservez les certificats aux flux traversant des réseaux non maîtrisés, avec une CA interne (section "Pour aller plus loin").

## 21. Premier démarrage : assistant de configuration du frontend

Ouvrez `http://zabbix.votre-domaine.local` (ou l'IP du server). L'assistant en 6 étapes :

1. **Welcome** → Next.
2. **Check of pre-requisites** : tout doit être vert (version PHP, extensions, `max_execution_time`, fuseau horaire). ⚠️ Si `Time zone` est rouge : réglez `date.timezone` dans `/etc/php/8.2/fpm/php.ini` (ex. `Africa/Abidjan`, `Africa/Dakar`… selon votre pays) puis `systemctl restart php8.2-fpm`.
3. **Configure DB connection** : type PostgreSQL (ou MySQL), host, port, nom, user, `mot_de_passe_ici`.
4. **Settings** : nom du server Zabbix (ex. `Zabbix-Production`), fuseau horaire par défaut, thème.
5. **Pre-installation summary** → Next.
6. **Install** → Finish.

Identifiants par défaut : **Admin / zabbix**. ⚠️ **Changez ce mot de passe immédiatement** (*Administration → Users → Admin → Password*) et créez un compte nominatif par exploitant (section 23).

🖨️ **Checklist post-installation immédiate :**

- [ ] Mot de passe `Admin` changé
- [ ] Fuseau horaire correct (`Africa/...`)
- [ ] `zabbix_server.log` sans erreur
- [ ] Frontend accessible, page *Monitoring → Dashboard* affichée
- [ ] Snapshot VM / sauvegarde de la base faite (base vide = restauration triviale)

## 22. Configuration initiale : réglages globaux essentiels

*Administration → General* — les réglages à passer en revue dès le premier jour :

### 22.1. GUI et fuseau horaire

- *Administration → General → GUI* : thème par défaut, limite de recherche, périodes de graphes.
- Chaque utilisateur peut surcharger son fuseau horaire (*User settings → Time zone*). Imposez le fuseau du site principal par défaut.

### 22.2. Housekeeping (rétention) — à régler AVANT de collecter

*Administration → General → Housekeeping* :

| Donnée | Réglage conseillé (départ) | Commentaire |
|---|---|---|
| History (données brutes) | 90 jours | Au-delà, les trends suffisent |
| Trends (agrégats horaires) | 730 jours (2 ans) | Pour les rapports annuels |
| Events / alertes | 365 jours | Traçabilité |
| Audit | 365 jours | Exigences internes |

⚠️ **Ne mettez jamais History à 365 jours "au cas où"** : c'est le premier facteur d'explosion de la base. Les trends (min/max/avg/count par heure) suffisent pour l'analyse long terme.

### 22.3. Autres réglages globaux importants

- *Administration → General → Other* : `Refresh unsupported items` (défaut 10 min — mettez 1 h pour limiter le bruit), groupes d'hôtes par défaut pour la découverte.
- *Administration → Media types* : vérifiez que les médias Email/Telegram sont configurés (sections 41-43).
- *Administration → General → Images / Icon mapping* : pour de belles cartes réseau (section 65).

## 23. Utilisateurs, groupes, rôles et permissions

> 💡 **Règle d'or** : jamais de compte partagé `admin` au quotidien. Un compte nominatif par personne, avec le rôle minimal.

### 23.1. Rôles (⚠️ Zabbix 5.4+/6.x : le système de rôles remplace les anciens "user types")

| Rôle | Usage type |
|---|---|
| Super admin | Vous, et votre second. Configuration complète. |
| Admin | Chefs d'équipe : gèrent hôtes/templates de leur périmètre. |
| User (opérateur) | Exploitation : dashboards, acquittements, maintenance. |
| Guest (désactivé par défaut) | À laisser désactivé. |

Créez des rôles sur mesure (*Administration → User roles*) : par exemple un rôle "Opérateur N1" qui peut acquitter et mettre en maintenance mais pas modifier les templates.

### 23.2. Groupes d'utilisateurs et permissions par groupe d'hôtes

1. *Administration → User groups → Create* : `Exploitation`, `Energie`, `Direction` (lecture seule).
2. Pour chaque groupe : onglet *Permissions*, ajoutez les **groupes d'hôtes** avec le niveau *Read* ou *Read-write*.
3. Onglet *Users* : rattachez les comptes nominatifs.

Exemple de matrice :

| Groupe d'hôtes | Exploitation | Energie | Direction |
|---|---|---|---|
| `Serveurs` | Read-write | Read | Read |
| `Onduleurs` | Read | Read-write | Read |
| `Réseau` | Read-write | Deny | Read |

### 23.3. Authentification

- *Administration → Authentication* : interne par défaut. Activez **LDAP/AD** si disponible (moins de mots de passe à gérer), et imposez le **2FA (TOTP)** pour les Super admins (⚠️ natif depuis 6.4 ; en 6.0 utilisez un plugin MFA).
- Verrouillez les tentatives : *Administration → Authentication → Login attempts* (ex. 5 essais → blocage 30 s).

## 24. Audit et traçabilité

Tout est journalisé : *Administration → Audit* (qui a modifié quel trigger, quand). Le *Audit log* frontend et la table `auditlog` en base sont vos alliés en cas d'incident de configuration ("qui a désactivé cette alerte ?").

> 💡 En production, conservez l'audit **365 jours minimum** et exportez-le avant toute mise à jour (section 72).

## 25. Concepts clés : hôtes, groupes d'hôtes, interfaces

- **Host (hôte)** : tout équipement supervisé (serveur, switch, onduleur, imprimante…).
- **Host group** : regroupement logique (`Serveurs/Linux`, `Onduleurs`, `Réseau/Switchs`). Un hôte appartient à **au moins un** groupe. Les permissions et les actions s'appuient dessus.
- **Interface** : comment joindre l'hôte — Agent (10050), SNMP (161), IPMI (623), JMX. Un hôte peut avoir plusieurs interfaces.
- **Template** : paquet réutilisable d'items/triggers/graphes (sections 27-29).

**Convention de nommage conseillée** (à adapter) :

```
<type>-<fonction>-<numéro>   ex. : srv-fichiers-01, sw-coeur-01, ups-salle-01
```

> 💡 Un nom d'hôte **stable et sans espace** évite 80 % des problèmes : c'est la clé de jointure entre l'agent actif (`Hostname=`) et le frontend.

## 26. Ajouter son premier hôte pas à pas

Exemple : `srv-fichiers-01` (192.168.10.61, Agent 2 en mode actif).

1. *Configuration → Hosts → Create host*.
2. Onglet **Host** :
   - *Host name* : `srv-fichiers-01` (exactement le `Hostname=` de l'agent !)
   - *Visible name* : `Serveur fichiers 01 (bureaux)` (libellé lisible)
   - *Groups* : `Serveurs/Linux`
   - *Interfaces* : *Add → Agent*, IP `192.168.10.61`, port `10050`
   - *Monitored by* : `Server` (ou le proxy du site)
3. Onglet **Templates** : *Link new templates* → `Linux by Zabbix agent` → *Add*.
4. *Add* en bas de page.
5. Vérifiez : la colonne **Availability** doit passer au vert (icône ZBX) sous 2-3 minutes.

Dépannage express si ça reste gris/rouge : section 74, erreur n°2.

### Ajout en masse : l'import CSV et l'API

Pour 50 serveurs, oubliez le clic : utilisez l'**API** (exemple minimal, à adapter) :

```bash
# Obtenir un token : Administration → General → API tokens (Zabbix 6.4+/7.x : Bearer token)
TOKEN="votre_token_api_ici"

curl -s -H "Content-Type: application/json" -H "Authorization: Bearer $TOKEN" \
  -d '{
    "jsonrpc": "2.0", "method": "host.create", "id": 1,
    "params": {
      "host": "srv-fichiers-02",
      "name": "Serveur fichiers 02",
      "groups": [{"groupid": "12"}],
      "interfaces": [{"type": 1, "main": 1, "useip": 1, "ip": "192.168.10.62", "dns": "", "port": "10050"}],
      "templates": [{"templateid": "10001"}]
    }
  }' http://192.168.10.50/api_jsonrpc.php
```

> 💡 Récupérez les `groupid`/`templateid` via `hostgroup.get` / `template.get`, ou lisez-les dans l'URL du frontend.

## 27. Templates : principes et bonnes pratiques

Un template = **items + triggers + graphes + dashboards + règles LLD** réutilisables. Principes :

1. **Ne modifiez jamais les templates officiels directement** : clonez-les (`Clone`) puis modifiez la copie. Sinon la prochaine mise à jour écrase vos changements.
2. **Un template = une fonction** : `Linux by Zabbix agent`, `Onduleur SNMP générique`, `Switch Cisco SNMP`. Empilez-les sur les hôtes (un hôte peut avoir N templates).
3. **Macros de template** `{$SEUIL_CPU_CRIT}` : définissez les seuils en macros au niveau template, surchargez-les au niveau hôte. Changer un seuil = changer une macro, pas 50 triggers.
4. **Versionnez** : exportez vos templates en YAML (*Configuration → Templates → Export*) et commitez dans Git.

```
Template "Onduleur SNMP générique"
├── Macros : {$UPS_LOAD_WARN}="80", {$UPS_LOAD_CRIT}="95", {$UPS_BATT_WARN}="30"
├── Items  : ups.load, ups.battery.charge, ups.input.voltage ...
├── Triggers : "Charge onduleur > {$UPS_LOAD_CRIT}"
└── Dashboards : vue onduleur
         │
         ├── Hôte ups-salle-01  (macros surchargées si besoin)
         └── Hôte ups-agence-sud
```

## 28. Templates officiels essentiels (Linux, Windows, SNMP, réseau)

Livrés avec Zabbix (*Configuration → Templates*, groupe *Templates*) :

| Template | Usage | Prérequis |
|---|---|---|
| `Linux by Zabbix agent` | Serveurs Linux | Agent 2 |
| `Windows by Zabbix agent` | Serveurs Windows | Agent 2 Windows |
| `Generic SNMP` | Base pour tout équipement SNMP | SNMP v2c/v3 |
| `Cisco IOS SNMP` / `Cisco Catalyst...` | Switchs/routeurs Cisco | SNMP |
| `UPS SNMP` (selon version) | Onduleurs génériques | SNMP, MIB UPS |
| `VMware Hypervisor / Guest` | ESXi, vCenter | Compte vSphere |
| `Apache by Zabbix agent`, `Nginx…`, `PostgreSQL…`, `MySQL…` | Applicatifs | Plugin Agent 2 |
| `ICMP Ping` | Disponibilité réseau pure | fping |

> 💡 Le template officiel est un **point de départ**, pas une fin : ajustez intervalles et seuils à votre contexte (un ping toutes les 30 s vers un site VSAT = bruit ; passez à 5 min).

## 29. Créer et lier un template personnalisé

Exemple : template `Energie — Salle serveur` regroupant sondes de température.

1. *Configuration → Templates → Create template* : nom `Salle serveur générique`, groupe `Templates/Energie`.
2. Onglet **Macros** (héritées) :

```
{$TEMP_WARN} = 27
{$TEMP_CRIT} = 32
{$HUMI_WARN} = 70
```

3. Créez les items (ici via SNMP, section 49), les triggers référençant les macros, un graphe.
4. Liez aux hôtes : *Configuration → Hosts → [hôte] → Templates → Link new templates*.
5. **Testez sur un hôte pilote** 48 h avant déploiement massif.

## 30. Items : actifs vs passifs, trapper, dépendants

| Type | Qui initie ? | Cas d'usage |
|---|---|---|
| **Zabbix agent (passif)** | Server → Agent | Parc simple, server dimensionné |
| **Zabbix agent (actif)** | Agent → Server | NAT, pare-feu, gros parcs ✅ recommandé |
| **SNMP agent** | Server → équipement | Switchs, onduleurs, imprimantes |
| **SNMP trap** | Équipement → Server | Alertes temps réel (défaut onduleur) |
| **Trapper** | Script externe → Server | `zabbix_sender` depuis vos scripts |
| **Calculé** | Server (formule) | Ratios, différences |
| **Agrégé** | Server (agrégation) | Moyenne d'un groupe, somme |
| **Dépendant** | Préprocessing d'un item maître | Découper un JSON/XML en plusieurs items |
| **HTTP agent** | Server → URL | API REST, sondes web |
| **JMX / IPMI** | Server → équipement | Java, hardware |

Anatomie d'un item : **clé** (`system.cpu.load[all,avg1]`), **intervalle** (`1m`), **période de rétention history/trends** (surcharge possible par item), **type d'information** (numérique, texte, log…), **prétraitement**.

### Envoyer des données avec zabbix_sender (trapper)

```bash
# Depuis un script de sauvegarde, un onduleur sans SNMP, etc.
zabbix_sender -z 192.168.10.50 -s "srv-fichiers-01" \
  -k backup.status -o "OK - 42 Go"
zabbix_sender -z 192.168.10.50 -s "srv-fichiers-01" \
  -k backup.size -o 45056
```

Côté frontend, créez un item de type **Zabbix trapper** avec la clé `backup.status` sur l'hôte `srv-fichiers-01`.

> 💡 `zabbix_sender` est votre **couteau suisse** : tout script maison (sauvegarde, relevé de compteur, sonde DIY) peut alimenter Zabbix en 1 ligne.

## 31. Les clés d'items de l'agent : les indispensables

🖨️ **Référence rapide** (Agent 2, testez avec `zabbix_get -s <ip> -k <clé>`) :

| Clé | Ce qu'elle mesure |
|---|---|
| `agent.hostname` | Nom déclaré par l'agent |
| `agent.ping` | 1 si l'agent répond |
| `system.cpu.num` | Nombre de vCPU |
| `system.cpu.load[all,avg1]` | Load average 1 min |
| `system.cpu.util` | % CPU total |
| `vm.memory.size[total]` / `[available]` | RAM totale / disponible |
| `vfs.fs.size[/,total]` / `[pused]` | Espace disque / % utilisé |
| `net.if.in[eth0]` / `net.if.out[eth0]` | Octets entrants/sortants |
| `net.if.discovery` | LLD des interfaces |
| `vfs.dev.discovery` | LLD des disques |
| `system.uptime` | Uptime en secondes |
| `proc.num[]` | Nombre de processus |
| `systemd.unit.is_active[ssh]` | État d'un service systemd (Agent 2) |
| `system.boottime` | Timestamp du boot |

### UserParameter : vos propres clés

Dans `/etc/zabbix/zabbix_agent2.d/custom.conf` :

```ini
# Température CPU (nécessite lm-sensors)
UserParameter=cpu.temp,sensors -u 2>/dev/null | awk '/Package id 0/{print $4}' | tr -d '+°C' | head -1
# Taille d'un dossier applicatif en Mo
UserParameter=app.dir.size[/var/lib/app],du -sm /var/lib/app 2>/dev/null | cut -f1
```

```bash
systemctl restart zabbix-agent2
zabbix_get -s 127.0.0.1 -k cpu.temp
```

> ⚠️ Les `UserParameter` s'exécutent avec les droits de l'utilisateur `zabbix` : prévoyez les droits sudo ciblés si besoin (visudo, `NOPASSWD` sur la commande exacte).

## 32. Items calculés et agrégés

**Item calculé** (formule sur le même hôte ou d'autres) :

```
# % de RAM utilisée (plus lisible que "available")
100 * (last(//vm.memory.size[total]) - last(//vm.memory.size[available])) / last(//vm.memory.size[total])
```

Syntaxe : `last(/hôte/clé)` — `/` seul = hôte courant. Fonctions dispo : `last`, `min`, `max`, `avg`.

**Item agrégé** (across hosts) :

```
# Charge CPU moyenne de tous les serveurs du groupe "Serveurs/Linux"
grpsum["Serveurs/Linux",system.cpu.load[all,avg1],last,]
grpavg["Serveurs/Linux",vm.memory.size[pavailable],last,]
```

> 💡 Les agrégés sont parfaits pour les **vues direction** : "charge moyenne du parc", "nombre d'onduleurs sur batterie".

## 33. Prétraitement des valeurs (preprocessing)

Transformez la donnée **avant stockage** (onglet *Preprocessing* de l'item). Étapes exécutées dans l'ordre :

| Étape | Usage |
|---|---|
| Multiplier / diviser | Octets → Go, ms → s |
| JSONPath / XML XPath | Extraire un champ d'une API |
| Regex | Extraire un motif d'un texte |
| Plage → valeur (Value mapping) | 1→"On battery", 2→"On mains" |
| Différence par seconde | Compteurs SNMP (ifInOctets) → débit |
| Jeter les doublons / les inchangés | Réduire le volume stocké |

Exemple : un onduleur expose la charge en dixièmes de pourcent (`850` = 85,0 %) :

```
Étape 1 : Custom multiplier → 0.1
Résultat : 85 (%)
```

Exemple SNMP : compteur d'octets → Mbit/s :

```
Étape 1 : Change per second
Étape 2 : Custom multiplier → 8
Étape 3 : Custom multiplier → 0.000001   (→ Mbit/s)
```

> 💡 **Toujours** vérifier le prétraitement avec *Test* (bouton dans l'item) avant de sauvegarder.

## 34. Intervalles, périodes de collecte et flex intervals

- **Intervalle** (`1m`, `30s`, `5m`) : fréquence de collecte. Plus c'est court, plus c'est précis — et plus ça coûte (base, réseau, CPU).
- **Flex intervals** : collectez plus souvent à certaines heures (`Interval: 30s, Period: 1-5,08:00-18:00`) et moins la nuit.
- **Scheduling** (`wd{1-5}h8-18`) : syntaxe avancée pour des créneaux complexes (⚠️ 6.0+).

Repères :

| Métrique | Intervalle conseillé |
|---|---|
| Ping / disponibilité | 1 min (5 min sur lien VSAT) |
| CPU / RAM | 1 min |
| Espace disque | 5 min (avec LLD) |
| Température salle | 2–5 min |
| Charge onduleur | 30 s–1 min (critique pour vous) |
| Inventaire (packages) | 1 jour |

> 💡 **Règle** : l'intervalle doit être ≤ 1/3 du délai d'alerte souhaité. Alerte "disque plein dans l'heure" → collecte toutes les 5 min minimum.

## 35. Triggers : syntaxe et logique

Un trigger = **expression** évaluée à chaque nouvelle valeur → états `OK` / `PROBLEM`.

```
{<server>:<clé>.<fonction>(<paramètres>)}<opérateur><seuil>
```

Exemples :

```
# Load > 4 sur 5 min
{serveur:system.cpu.load[all,avg1].avg(5m)}>4

# Disque / > 90 %
{srv:vfs.fs.size[/,pused].last()}>90

# Pas de données depuis 10 min (agent mort ?)
{srv:agent.ping.nodata(10m)}=1

# Service SSH arrêté
{srv:systemd.unit.is_active[ssh].last()}=0
```

**Opérateurs** : `> < >= <= = <>`, `and`, `or`, `not`. **Sévérités** : Not classified, Information, Warning, Average, High, Disaster.

### Assistant d'expression (recommandé)

Dans le frontend, *Configuration → Triggers → Create* : le bouton **Add** ouvre le constructeur (item + fonction + paramètres + seuil). **Utilisez-le** : il évite 99 % des erreurs de syntaxe.

### Nommage

```
# Bon : précis, avec macros
"Charge CPU > {$CPU_LOAD_CRIT} sur {HOST.NAME} depuis 5 min"
# Mauvais : vague
"Alerte CPU"
```

> 💡 Incluez `{HOST.NAME}` et `{ITEM.VALUE}` dans le nom : l'email d'alerte devient lisible sans ouvrir le frontend.

## 36. Fonctions de trigger : le catalogue indispensable

🖨️ **Fiche mémo** — les fonctions à connaître par cœur :

| Fonction | Signification | Exemple d'usage |
|---|---|---|
| `last()` | Dernière valeur | Seuil simple |
| `avg(5m)` | Moyenne sur 5 min | Lisser les pics CPU |
| `min(15m)` / `max(15m)` | Min/max sur période | Pics mémoire |
| `nodata(10m)` | =1 si aucune donnée depuis 10 min | Agent/équipement muet |
| `change()` | Différence avec la valeur précédente | Compteur qui bouge |
| `diff()` | =1 si la valeur a changé (0 sinon) | Détection de reboot via uptime |
| `date()` / `dayofweek()` | Date/jour courant | Alertes ouvrées uniquement |
| `time()` | Heure courante HHMMSS | Plages horaires |
| `count(10m,,"gt")` | Nombre de valeurs > seuil sur 10 min | "3 dépassements en 10 min" |
| `forecast()` | Prédiction linéaire | "Disque plein dans 7 jours" |
| `trendavg(1h,7d)` | Moyenne des trends | Comparaison semaine/semaine |

Exemples commentés :

```
# CPU > 90 % pendant 10 minutes d'affilée (évite les pics)
{srv:system.cpu.util.avg(10m)}>90

# Au moins 5 dépassements de 95 % CPU en 15 min
{srv:system.cpu.util.count(15m,95,"gt")}>5

# Reboot détecté (uptime a diminué)
{srv:system.uptime.change()}<0

# Pas de ping depuis 5 min → hôte injoignable
{sw:icmpping.nodata(5m)}=1

# Prédiction : /var plein dans moins de 7 jours
{srv:vfs.fs.size[/var,pfree].forecast(#10,7d,,avg)}<10
```

> ⚠️ `forecast()` et les fonctions sur trends exigent assez d'historique : ne vous étonnez pas d'un `Unknown` les premiers jours.

## 37. Hystérésis et seuils intelligents

**Problème** : un seuil unique à 90 % fait osciller l'alerte (PROBLEM/OK/PROBLEM…) quand la valeur oscille autour de 90 — c'est du **flapping**, source n°1 du bruit.

**Solution — hystérésis** : seuil de déclenchement ≠ seuil de résolution.

```
# Déclenche à 90, ne se résout qu'en dessous de 80
Expression problème : {srv:vfs.fs.size[/,pused].last()}>90
Expression de récupération : {srv:vfs.fs.size[/,pused].last()}<80
```

Dans le frontend : *Trigger → Recovery expression*. Autres techniques :

- **Durée** : `.avg(10m)>90` au lieu de `.last()>90`.
- **Occurrences** : `.count(15m,90,"gt")>=3`.
- **Macros** : `{$DISK_PUSED_CRIT}` = 90, `{$DISK_PUSED_CRIT_RECOVERY}` = 80.

> 💡 Chaque trigger critique de production devrait avoir une expression de récupération. C'est 30 secondes de travail qui divisent le bruit par deux.

## 38. Sévérités et bonnes pratiques de nommage

| Sévérité | Quand l'utiliser | Exemple |
|---|---|---|
| Disaster | Service vital coupé, action immédiate | Onduleur sur batterie + charge > 95 % |
| High | Dégradation forte | Disque > 95 %, site injoignable |
| Average | À traiter sous 24 h | Disque > 85 %, température > 27 °C |
| Warning | Informatif, tendance | Certificat expire dans 30 j |
| Information | Purement informatif | Reboot détecté |

Règles de nommage :

```
[Équipement] Métrique > seuil (contexte)
"Charge onduleur ups-salle-01 > 95 % depuis 2 min"
```

- Toujours `{HOST.NAME}` (ou `{HOST.HOST}`) dans le nom.
- Toujours l'unité et le seuil.
- **Jamais** de "ALERTE !!!" en majuscules : la sévérité porte déjà l'urgence.

## 39. Dépendances entre triggers (éviter les tempêtes d'alertes)

**Scénario** : le switch `sw-coeur-01` tombe → 40 hôtes derrière deviennent "injoignables" → 40 emails. **Inutile.**

**Solution** : *Trigger → Dependencies* : le trigger "hôte injoignable" **dépend** du trigger "switch injoignable". Zabbix **masque** les dépendants (ils passent quand même en PROBLEM mais ne déclenchent pas d'action).

```
Trigger "Switch sw-coeur-01 injoignable"   (cause racine)
    └── Trigger "srv-fichiers-01 injoignable" dépend du précédent
    └── Trigger "ups-salle-01 injoignable" dépend du précédent
```

Configuration : sur le trigger dépendant, onglet *Dependencies → Add*, sélectionnez le trigger du switch.

> 💡 Dessinez votre **arbre de dépendances** par site : routeur → switch → serveurs/onduleurs. 10 minutes de réflexion = des nuits d'astreinte paisibles.

Alternative/complément : les **actions avec conditions** sur les groupes d'hôtes (section 40).

## 40. Actions : le moteur de notification

*Configuration → Actions → Trigger actions*. Une action = **conditions** (quand) + **opérations** (quoi faire) + **escalades** (quand répéter/monter en puissance).

### Exemple : action "Alertes critiques énergie"

**Conditions** (toutes doivent matcher — logique AND entre types différents) :

```
Trigger severity >= High
AND Host group = Onduleurs
AND Trigger value = PROBLEM
```

**Opérations** :

```
Step 1 (0s)      : Send to user group "Energie" via Email + Telegram
Step 2 (15 min)  : Send to user "Chef de service" via SMS   (escalade)
Step 3 (1h, répéter toutes les 30 min) : répéter l'escalade
```

**Recovery operation** : envoyer "RÉSOLU" aux mêmes destinataires (toujours configurer !).

Paramètres clés d'une opération :

- *Default message* : personnalisez le sujet/corps (voir 41).
- *Pause* entre étapes, *durée d'escalade*.
- *Conditions d'acquittement* : ne pas escalader si acquitté.

> 💡 **Une action par criticité/métier**, pas une action par trigger. 3-5 actions bien pensées valent mieux que 50 actions bricolées.

## 41. Médias d'alerte : email

*Administration → Media types → Email* :

| Paramètre | Valeur type |
|---|---|
| SMTP server | `smtp.votre-domaine.local` ou relais interne |
| SMTP port | `587` (STARTTLS) ou `25` |
| Connection security | STARTTLS / SSL/TLS |
| Authentication | Username + password (compte dédié `zabbix@votre-domaine.local`) |

**Testez** avec *Test* (envoie un vrai email). Modèles de message (*Message templates*) :

```
Sujet : [{TRIGGER.SEVERITY}] {HOST.NAME} : {TRIGGER.NAME} ({EVENT.STATUS})
Corps :
  Alerte : {TRIGGER.NAME}
  Hôte : {HOST.NAME} ({HOST.IP})
  Sévérité : {TRIGGER.SEVERITY}
  Heure : {EVENT.DATE} {EVENT.TIME}
  Valeur : {ITEM.VALUE} ({ITEM.NAME})
  Détails : {TRIGGER.DESCRIPTION}
  Acquitter : {EVENT.ACKNOWLEDGE.URL}  (⚠️ 7.x : macro disponible)
```

> 💡 Utilisez un **relais SMTP interne** (Postfix) plutôt que le SMTP du FAI : en cas de coupure internet, les emails internes continuent de partir… si votre relais est local. Pour l'externe, le SMS reste roi (section suivante).

## 42. Médias d'alerte : SMS via passerelle (contexte Afrique de l'Ouest)

L'email ne suffit pas en astreinte. Options classées par fiabilité terrain :

### Option A — Passerelle GSM locale (recommandée)

Un modem/routeur GSM (clé 4G, routeur Teltonika…) + outil d'envoi :

```bash
# Exemple avec gammu + clé USB Huawei
apt install -y gammu
# /etc/gammurc
[gammu]
port = /dev/ttyUSB0
connection = at115200

# Envoi
echo "ZABBIX [High] ups-salle-01 : sur batterie depuis 5 min" \
  | gammu sendsms TEXT +2250700000000
```

Média Zabbix de type **Script** : *Administration → Media types → Create → Type: Script*, script `/usr/lib/zabbix/alertscripts/sendsms.sh` :

```bash
#!/bin/bash
# $1 = numéro, $2 = sujet, $3 = message
TO="$1"; MSG="$2 - $3"
echo "$MSG" | gammu sendsms TEXT "$TO"
```

```bash
chmod +x /usr/lib/zabbix/alertscripts/sendsms.sh
chown zabbix:zabbix /usr/lib/zabbix/alertscripts/sendsms.sh
```

> 💡 Gardez la clé 4G d'alerte sur un **opérateur différent** de votre lien principal, avec du crédit en permanence. Testez l'envoi **chaque mois** (section 62).

### Option B — API d'un agrégateur SMS

Script utilisant `curl` vers l'API de votre fournisseur (Orange/MTN/Wave API, etc.) :

```bash
#!/bin/bash
TO="$1"; SUBJECT="$2"; BODY="$3"
curl -s -X POST "https://api.fournisseur-sms.local/send" \
  -H "Authorization: Bearer CLE_API_ICI" \
  -d "to=$TO" -d "message=$(echo "$SUBJECT $BODY" | cut -c1-160)"
```

> ⚠️ Dépend d'internet : prévoyez l'option A en secours pour les alertes Disaster.

### Option C — SMS via email-to-SMS de l'opérateur

Si votre opérateur offre une passerelle `numero@sms.operateur.xx`, réutilisez le média Email avec une adresse par destinataire.

## 43. Médias d'alerte : Telegram

Gratuit, instantané, avec accusé de lecture — parfait pour une équipe technique.

1. Créez un bot via **@BotFather** → récupérez le **token** (`123456:ABC-DEF...`).
2. Créez un groupe d'astreinte, ajoutez le bot, récupérez le **chat_id** (`https://api.telegram.org/bot<TOKEN>/getUpdates`).
3. *Administration → Media types → Telegram* (média natif depuis 5.x) :

| Paramètre | Valeur |
|---|---|
| Bot token | `123456:ABC-DEF...` (le vôtre) |

4. Dans chaque utilisateur : *Media → Add → Type: Telegram*, *Send to* : le chat_id (ou `@pseudo`).
5. Message template adapté (court !) :

```
[{TRIGGER.SEVERITY}] {HOST.NAME}
{TRIGGER.NAME}
{EVENT.DATE} {EVENT.TIME} — {ITEM.VALUE}
```

> 💡 Un groupe Telegram "Astreinte Zabbix" où le bot poste : toute l'équipe voit qui a acquitté quoi. Zéro coût, historique intégré.

## 44. Webhooks : Slack, Teams, et autres

*Administration → Media types* : des webhooks pré-packagés existent (Slack, Teams, PagerDuty, Jira…). Principe : un script JavaScript embarqué appelle l'API distante.

Exemple minimal (webhook générique, à adapter) :

```javascript
// Paramètres du média : URL, Token
var params = JSON.parse(value);
var req = new HttpRequest();
req.addHeader('Content-Type: application/json');
req.addHeader('Authorization: Bearer ' + params.Token);
var body = JSON.stringify({
    text: params.Subject + "\n" + params.Message
});
var resp = req.post(params.URL, body);
if (req.getStatus() != 200) {
    throw 'Echec webhook: ' + resp;
}
return 'OK';
```

> ⚠️ Les webhooks sortants exigent internet : ne faites **jamais** reposer une alerte Disaster uniquement sur un webhook.

## 45. Escalades et acquittements

### Escalade type (à configurer dans l'action, section 40)

```
Étape 1 — immédiate : Email + Telegram → groupe "Exploitation"
Étape 2 — après 15 min sans acquittement : SMS → groupe "Exploitation"
Étape 3 — après 45 min sans acquittement : SMS + appel → "Chef de service"
Récupération : Email + Telegram → tous ("RÉSOLU")
```

### Acquittements

Depuis *Monitoring → Problems* : *Acknowledge* → message ("Pris en charge, intervention à 14h"), éventuellement **changer la sévérité** ou **mettre en maintenance**. L'acquittement **stoppe l'escalade** (si l'action est configurée ainsi) mais ne résout pas le problème.

> 💡 Règle d'équipe : **tout PROBLEM High/Disaster est acquitté sous 15 min** avec un commentaire. Un problème non acquitté qui escalade, c'est le système qui fonctionne.

## 46. Découverte réseau automatique

*Configuration → Discovery → Create discovery rule*. Exemple : balayer `192.168.10.1-254` :

| Check | Port | Règle |
|---|---|---|
| ICMP ping | — | `icmpping` |
| SNMPv2 agent | 161 | `snmpget` OID `1.3.6.1.2.1.1.1.0` (sysDescr) |
| Zabbix agent | 10050 | `system.hostname` |

Puis *Configuration → Actions → Discovery actions* : "si SNMP détecté → ajouter au groupe `Réseau/Auto`, lier le template `Generic SNMP`".

> ⚠️ La découverte, c'est bien ; la **découverte + action auto** non relue, c'est 300 imprimantes dans votre inventaire. Limitez les plages, relisez la file *Monitoring → Discovery*, et **désactivez** la règle après le déploiement initial.

## 47. Low-Level Discovery (LLD) : disques, interfaces, processus

La LLD découvre **à l'intérieur** d'un hôte : partitions, interfaces réseau, disques, processus… et crée items/triggers/graphes par entité.

### Exemple : LLD des systèmes de fichiers (déjà dans le template Linux)

Règle `vfs.fs.discovery` → prototypes d'items :

```
Item prototype : vfs.fs.size[{#FSNAME},pused]
Trigger prototype : "{#FSNAME} plein à > {$VFS_FS_PUSED_CRIT}%"
Graph prototype : espace disque
```

**Macros LLD** `{#FSNAME}`, `{#IFNAME}`… : remplacées par entité découverte.

### Filtres LLD (indispensables)

Sans filtre, vous supervisez `/snap/*`, `docker0`, `veth*`… :

```
Filtre : {#FSNAME} matches ^/(|var|home|opt)$      (regex)
     ET  {#FSTYPE} matches ^(ext4|xfs)$
```

> 💡 **Toujours** filtrer les LLD réseau (`{#IFNAME} not matches ^(lo|docker|veth)`) et disque. Une LLD non filtrée = des centaines d'items parasites et des alertes absurdes.

## 48. Écrire ses propres règles LLD (UserParameter + JSON)

Une règle LLD attend un **JSON** : `{"data":[{"{#MACRO1}":"val1",...}, ...]}`.

Exemple : découvrir les onduleurs USB branchés (ou tout parc maison) :

```bash
# /etc/zabbix/zabbix_agent2.d/llb_custom.conf
UserParameter=custom.ups.discovery,python3 /usr/local/bin/ups_discovery.py
```

```python
#!/usr/bin/env python3
"""Découverte des onduleurs : sort un JSON LLD."""
import json, subprocess

result = {"data": []}
try:
    out = subprocess.run(["upsc", "-l"], capture_output=True, text=True, timeout=10)
    for name in out.stdout.split():
        name = name.strip()
        if name:
            result["data"].append({"{#UPSNAME}": name})
except Exception:
    pass
print(json.dumps(result))
```

Test :

```bash
zabbix_get -s 127.0.0.1 -k custom.ups.discovery
# {"data": [{"{#UPSNAME}": "ups-salle-01"}, {"{#UPSNAME}": "ups-agence"}]}
```

Côté template : règle de découverte de type *Zabbix agent*, clé `custom.ups.discovery`, puis prototypes :

```
Item prototype : upsc[{#UPSNAME},battery.charge]
Trigger prototype : "Batterie {#UPSNAME} < {$UPS_BATT_WARN}%"
```

> 💡 La LLD maison + `zabbix_sender`/`upsc` : c'est comme ça qu'on supervise proprement ce que Zabbix ne connaît pas nativement.

## 49. Supervision SNMP : principes (v2c vs v3)

SNMP = le langage commun des équipements réseau et énergie. Deux versions à connaître :

| Critère | SNMP v2c | SNMP v3 |
|---|---|---|
| Authentification | Communauté (mot de passe en clair, ex. `public`) | Utilisateur + auth (SHA) + priv (AES) |
| Chiffrement | Non | Oui (authPriv) |
| Simplicité | Très simple | Plus complexe |
| Recommandation | Labo / réseau de gestion isolé | ✅ **Production** |

**OID** : adresse d'une donnée, ex. `1.3.6.1.2.1.1.3.0` (uptime). **MIB** : dictionnaire qui traduit les OID en noms (`SNMPv2-MIB::sysUpTime.0`).

Outils de diagnostic (paquet `snmp`) :

```bash
apt install -y snmp snmp-mibs-downloader
# Tester un équipement v2c
snmpwalk -v2c -c public 192.168.10.2 1.3.6.1.2.1.1.1.0
# Tester v3
snmpwalk -v3 -l authPriv -u zabbix-mon -a SHA -A 'mot_de_passe_ici' \
  -x AES -X 'mot_de_passe_ici' 192.168.10.90 1.3.6.1.2.1.33
```

> 💡 **Toujours valider avec `snmpwalk` en ligne de commande AVANT de créer l'item Zabbix.** Si `snmpwalk` ne répond pas, Zabbix ne répondra pas non plus — inutile de chercher côté Zabbix.

Dans Zabbix : interface **SNMP** sur l'hôte (port 161/UDP), items de type **SNMP agent**, clé = OID (`1.3.6.1.2.1.1.3.0`) ou nom MIB (`sysUpTime.0` si les MIB sont chargées).

## 50. Superviser un switch via SNMP pas à pas

Exemple : `sw-coeur-01` (192.168.10.2), SNMP v2c communauté `communaute_lecture_ici`.

1. Créez l'hôte : groupe `Réseau/Switchs`, interface **SNMP** `192.168.10.2:161`.
2. Onglet **Macros** (héritées) : `{$SNMP_COMMUNITY}` = `communaute_lecture_ici`.
3. Liez le template `Cisco IOS SNMP` (ou `Generic SNMP` + LLD maison).
4. Items essentiels à vérifier/créer :

| Item | OID / clé | Prétraitement |
|---|---|---|
| sysDescr | `1.3.6.1.2.1.1.1.0` | — |
| sysUpTime | `1.3.6.1.2.1.1.3.0` | diviser par 100 (centisecondes) |
| CPU 5 min | `1.3.6.1.4.1.9.9.109.1.1.1.1.8.1` (Cisco) | — |
| ifInOctets (LLD) | `IF-MIB::ifInOctets[{#IFINDEX}]` | change per second × 8 |
| ifOperStatus (LLD) | `IF-MIB::ifOperStatus[{#IFINDEX}]` | value mapping |

5. Triggers :

```
# Interface down (en excluant les ports administrativement down)
{sw:ifOperStatus[{#IFINDEX}].last()}=2 and {sw:ifAdminStatus[{#IFINDEX}].last()}=1
# CPU switch > 80 %
{sw:cpu.5min.avg(5m)}>80
# Reboot du switch
{sw:sysUpTime.change()}<0
```

> 💡 Sur les LLD d'interfaces : filtrez `{#IFOPERSTATUS}` et excluez les ports non connectés durablement, sinon chaque brassage génère une alerte.

## 51. Superviser un onduleur via SNMP (cas métier — le cœur de ce guide)

C'est **votre** cas d'usage prioritaire : un onduleur non supervisé = une coupure subie au lieu d'une coupure gérée.

### 51.1. La MIB UPS standard (RFC 1628) — `1.3.6.1.2.1.33`

La plupart des onduleurs (APC, Eaton, Vertiv, Schneider, Riello…) exposent la **UPS-MIB** standard. OID clés :

| Donnée | OID (suffixe de 1.3.6.1.2.1.33) | Nom MIB |
|---|---|---|
| Identité / modèle | `1.1.1.1.2.0` | `upsIdentModel.0` |
| État batterie | `1.2.1.1.0` | `upsBatteryStatus.0` (1=unknown, 2=normal, 3=low, 4=depleted) |
| Charge restante batterie | `1.2.3.1.1.2.1` | `upsEstimatedChargeRemaining.1` (%) |
| Tension batterie | `1.2.2.7.0` | `upsBatteryVoltage.0` (dixièmes de V) |
| Température batterie | `1.2.7.0` | `upsBatteryTemperature.0` (°C) |
| Source d'alimentation | `1.1.4.1.1.2.1` | `upsOutputSource.1` (3=normal/mains, 5=battery) |
| Charge en sortie | `1.4.4.1.1.5.1` | `upsOutputPercentLoad.1` (%) |
| Tension d'entrée | `1.3.3.1.1.3.1` | `upsInputVoltage.1` (V) |
| Fréquence d'entrée | `1.3.3.1.1.2.1` | `upsInputFrequency.1` (dixièmes de Hz) |
| Dernier test batterie | `1.2.6.4.0` | `upsTestResultsSummary.0` |
| Autonomie estimée | `1.2.3.1.1.3.1` | `upsEstimatedMinutesRemaining.1` (min) |

Vérifiez d'abord en CLI :

```bash
# L'onduleur parle-t-il la MIB standard ?
snmpwalk -v2c -c communaute_lecture_ici 192.168.10.90 1.3.6.1.2.1.33.1.2.3.1.1.2.1
# Réponse attendue : INTEGER: 100  (batterie à 100 %)
snmpwalk -v2c -c communaute_lecture_ici 192.168.10.90 1.3.6.1.2.1.33.1.4.1.1.2.1
# Réponse attendue : INTEGER: 3  (3 = sur secteur)
```

### 51.2. Valeurs pièges à connaître

- `upsOutputSource` : **3** = secteur normal, **5** = sur batterie, 4 = bypass. Le trigger critique porte sur `=5`.
- `upsBatteryStatus` : **2** = normale, **3** = basse, **4** = épuisée.
- Tensions/fréquences souvent en **dixièmes** : prétraitement `× 0.1`.
- Certains constructeurs (Eaton, Huawei) ont leurs **MIB propriétaires** en plus : chargez-les pour le détail (courants par phase, etc.), mais la RFC 1628 suffit pour l'essentiel.

### 51.3. SNMP traps de l'onduleur (temps réel)

En plus du polling, configurez l'onduleur pour envoyer des **traps** vers le server Zabbix (port 162/UDP). Côté Zabbix :

```bash
# /etc/zabbix/zabbix_server.conf
StartSNMPTrapper=1
SNMPTrapperFile=/var/log/zabbix/zabbix_traps.log
```

```bash
# Réception des traps (snmptrapd)
apt install -y snmptrapd
```

`/etc/snmp/snmptrapd.conf` :

```
authCommunity log,execute,net public
perl do "/usr/share/zabbix/bin/zabbix_trap_receiver.pl";
```

Item de type **SNMP trap** sur l'hôte onduleur, clé `snmptrap["linkDown"]` ou `snmptrap.fallback` pour tout intercepter.

> 💡 Polling (toutes les 30-60 s) + traps (instantané) = le meilleur des deux mondes pour un onduleur.

## 52. Template onduleur : items, triggers et dashboard dédiés

Construisez (ou clonez/adaptez) un template `Onduleur SNMP générique (RFC1628)` :

**Macros** :

```
{$UPS_LOAD_WARN} = 75
{$UPS_LOAD_CRIT} = 90
{$UPS_BATT_WARN} = 50
{$UPS_BATT_CRIT} = 30
{$UPS_BATT_CRIT_MIN} = 15
{$UPS_TEMP_CRIT} = 40
{$SNMP_COMMUNITY} = communaute_lecture_ici
```

**Items** (type SNMP agent, intervalle 1m ; 30s pour les critiques) :

| Nom | Clé (OID) | Prétraitement |
|---|---|---|
| Charge sortie % | `1.3.6.1.2.1.33.1.4.4.1.1.5.1` | — |
| Source alimentation | `1.3.6.1.2.1.33.1.1.4.1.1.2.1` | value mapping |
| Batterie % | `1.3.6.1.2.1.33.1.2.3.1.1.2.1` | — |
| Autonomie restante (min) | `1.3.6.1.2.1.33.1.2.3.1.1.3.1` | — |
| Tension batterie | `1.3.6.1.2.1.33.1.2.2.7.0` | × 0.1 |
| Température batterie | `1.3.6.1.2.1.33.1.2.7.0` | — |
| Tension entrée | `1.3.6.1.2.1.33.1.3.3.1.1.3.1` | — |
| État batterie | `1.3.6.1.2.1.33.1.2.1.1.0` | value mapping |

**Value mappings** (*Administration → General → Value mapping*) :

```
UPS output source : 1→other, 2→none, 3→normal (secteur), 4→bypass, 5→BATTERIE, 6→booster, 7→reducer
UPS battery status : 1→unknown, 2→normale, 3→basse, 4→épuisée
```

**Triggers** (les plus importants de votre supervision) :

```
# 🔴 DISASTER — Onduleur sur batterie
{ups:upsOutputSource.last()}=5
  Nom : "COUPURE SECTEUR : {HOST.NAME} sur batterie"

# 🔴 DISASTER — Batterie critique sur batterie (< 15 %)
{ups:upsOutputSource.last()}=5 and {ups:battery.charge.last()}<{$UPS_BATT_CRIT_MIN}

# 🟠 HIGH — Batterie faible
{ups:battery.charge.last()}<{$UPS_BATT_CRIT}

# 🟠 HIGH — Charge > 90 %
{ups:output.load.avg(2m)}>{$UPS_LOAD_CRIT}

# 🟡 AVERAGE — Température batterie > 40 °C (vieillissement accéléré)
{ups:battery.temp.avg(10m)}>{$UPS_TEMP_CRIT}

# 🟡 AVERAGE — Pas de données onduleur depuis 3 min
{ups:agent.ping.nodata(3m)}=1   (via icmpping sur l'hôte)

# 🔵 INFO — Retour sur secteur (recovery du trigger batterie)
  Recovery expression : {ups:upsOutputSource.last()}=3
```

> 💡 **L'autonomie restante** (`upsEstimatedMinutesRemaining`) est l'item le plus précieux en coupure : affichez-la en grand sur le dashboard d'astreinte (section 63) et utilisez-la dans l'escalade SMS ("il reste ~22 min").

## 53. Supervision JMX (Java/Tomcat)

Pour les applications Java (Tomcat, Kafka, Elasticsearch…) : activez JMX côté applicatif, puis items **JMX agent** dans Zabbix (interface JMX, port 12345 par défaut).

```bash
# Exemple : Tomcat avec JMX distant
CATALINA_OPTS="$CATALINA_OPTS -Dcom.sun.management.jmxremote \
  -Dcom.sun.management.jmxremote.port=12345 \
  -Dcom.sun.management.jmxremote.authenticate=false \
  -Dcom.sun.management.jmxremote.ssl=false \
  -Djava.rmi.server.hostname=192.168.10.70"
```

Item JMX : clé `jmx["java.lang:type=Memory",HeapMemoryUsage.used]`, type *JMX agent*.

> ⚠️ JMX sans auth ni SSL = à réserver au réseau de gestion isolé. En production, activez l'authentification JMX.

## 54. Supervision IPMI (températures, ventilateurs, alimentations)

Pour le **hardware** des serveurs (Dell iDRAC, HPE iLO, Supermicro…) : interface **IPMI** sur l'hôte, items de type *IPMI agent*.

Prérequis server : `apt install -y libopenipmi0` (souvent déjà dépendance).

Clés IPMI : le nom du capteur tel que remonté (`System Temp`, `FAN 1`, `PS1 Status`) — découvrez-les via la LLD IPMI du template `IPMI` officiel, ou :

```bash
ipmitool -I lanplus -H 192.168.10.61 -U ADMIN -P 'mot_de_passe_ici' sensor list
```

Items types :

```
Capteur : System Temp → trigger si > 75 °C
Capteur : PS1 Status / PS2 Status → trigger si <> "OK" (redondance d'alim !)
Capteur : FAN 1..N → trigger si vitesse = 0
```

> 💡 Pour vous, la **redondance d'alimentation** (`PS1/PS2 Status`) est un item à mettre en HIGH : une alim redondante en panne = plus de redondance, et la prochaine panne = coupure.

## 55. Supervision VMware (intégration native)

Zabbix supervise **vCenter/ESXi** sans agent : *Configuration → Hosts → Create host*, template `VMware Hypervisor` ou `VMware Guest`, macros :

```
{$VMWARE.URL} = https://vcenter.votre-domaine.local/sdk
{$VMWARE.USERNAME} = zabbix-mon@votre-domaine.local
{$VMWARE.PASSWORD} = mot_de_passe_ici
```

La LLD découvre hyperviseurs, VMs, datastores. Surveillez : CPU ready, ballooning mémoire, espace datastore, snapshots oubliés.

> ⚠️ Créez un compte vSphere **lecture seule** dédié. Intervalle de collecte VMware : 5 min minimum (l'API n'aime pas le polling agressif).

## 56. Supervision web : scénarios et checks HTTP

*Configuration → Hosts → [hôte] → Web scenarios*. Exemple : vérifier le portail interne toutes les 5 min :

```
Nom : Portail intranet
Étapes :
  1. Page d'accueil : URL https://intranet.local/, code attendu 200, contient "Bienvenue"
  2. Login : POST https://intranet.local/login, variables user/pass, contient "Déconnexion"
```

Triggers auto : `web.test.fail[Portail intranet]` > 0, `web.test.time[...]` > 5 s.

L'**HTTP agent** (item) permet d'interroger une API REST avec JSONPath en prétraitement — idéal pour les sondes modernes.

## 57. Maintenance planifiée (sans alerte parasite)

*Configuration → Maintenance → Create* : pendant une maintenance, les triggers ne génèrent **pas d'actions** (mais les données continuent d'être collectées — les graphes restent complets).

Cas types :

| Maintenance | Période | Avec collecte de données |
|---|---|---|
| MCO onduleur (bypass) | Ponctuelle, 4 h | Non (pauses) — les tests faussent les mesures |
| Mise à jour serveurs | Hebdo, dimanche 02:00-06:00 | Oui |
| Travaux électriques site | Ponctuelle | Non |

> 💡 **Toujours** poser une maintenance avant une intervention : une alerte "onduleur sur batterie" pendant VOTRE bypass = bruit + SMS inutiles + perte de crédibilité du système d'alerte.

Astuce : maintenance **par tag** (⚠️ 6.x+) — taggez les hôtes `maintenance:elec` et ciblez la maintenance sur le tag.

## 58. Haute disponibilité (HA natif Zabbix 6+)

Depuis la 6.0, le **cluster HA natif** : plusieurs nœuds server en actif/passif automatique, sans outil tiers.

```ini
# /etc/zabbix/zabbix_server.conf — sur chaque nœud
HANodeName=zabbix-node-1      # unique par nœud !
NodeAddress=192.168.10.50:10051
```

1. Installez `zabbix-server-pgsql` sur 2 nœuds (même version !), **même base partagée**.
2. `HANodeName` différent par nœud, `NodeAddress` = IP:port d'écoute de chaque nœud.
3. Démarrez les deux : l'un devient **active**, l'autre **standby** (bascule auto en ~10-30 s).

Vérification : *Administration → General → HA cluster* (⚠️ 6.4+/7.x : *Administration → High availability*).

```
zabbix-node-1   active    last access 5s ago
zabbix-node-2   standby   last access 8s ago
```

> ⚠️ Le HA Zabbix ne couvre **que le server** : la base (PostgreSQL Patroni/réplication) et le frontend (2 instances + reverse proxy) doivent être redondés séparément. Et les **proxies** : déployez-en 2 par site critique avec bascule manuelle des hôtes, ou acceptez le buffer offline.

## 59. Housekeeping : comprendre la rétention des données

Le **housekeeper** supprime périodiquement : history > X jours, trends > Y jours, events, sessions, audit.

Tables concernées : `history`, `history_uint`, `history_str`, `history_text`, `history_log`, `trends`, `trends_uint`, `events`, `alerts`, `auditlog`.

Réglages (*Administration → General → Housekeeping*) — rappelez-vous la section 22 :

```
History storage period : 90d
Trend storage period   : 730d
```

Surveillez le housekeeper lui-même : si `zabbix[housekeeper,...]` (clé interne, section 69) montre qu'il ne suit plus, la base grossit indéfiniment.

> 💡 Alternative moderne : **désactiver le housekeeping interne** et laisser **TimescaleDB** gérer la rétention par policy (compression + drop des chunks). Plus performant sur gros volumes (section 60).

## 60. Partitionnement de la base (PostgreSQL/MySQL) pour les gros parcs

### Avec TimescaleDB (recommandé)

Le script `timescaledb/schema.sql` (section 14.4) a déjà converti `history*` et `trends*` en hypertables. Ajoutez les policies :

```sql
-- Compression après 7 jours, suppression après 90 jours (history)
SELECT add_compression_policy('history', INTERVAL '7 days');
SELECT add_retention_policy('history', INTERVAL '90 days');
SELECT add_compression_policy('history_uint', INTERVAL '7 days');
SELECT add_retention_policy('history_uint', INTERVAL '90 days');
-- Trends : compression après 30 j, rétention 2 ans
SELECT add_compression_policy('trends', INTERVAL '30 days');
SELECT add_retention_policy('trends', INTERVAL '730 days');
```

Vérifiez le gain :

```sql
SELECT hypertable_name,
       pg_size_pretty(hypertable_size(hypertable_name)) AS taille
FROM timescaledb_information.hypertables;
```

### Avec MySQL/MariaDB (partitionnement manuel)

Zabbix fournit un exemple : `/usr/share/doc/zabbix-server-mysql*/create.sql` (script de partitionnement par jour). Principe :

```sql
-- Exemple de partition mensuelle sur history_uint (à adapter)
ALTER TABLE history_uint PARTITION BY RANGE (clock)
(PARTITION p202609 VALUES LESS THAN (UNIX_TIMESTAMP('2026-10-01')),
 PARTITION p202610 VALUES LESS THAN (UNIX_TIMESTAMP('2026-11-01')));
```

> ⚠️ Le partitionnement MySQL manuel exige une **procédure stockée + event scheduler** pour créer/supprimer les partitions. C'est une raison de plus de choisir PostgreSQL + TimescaleDB pour un nouveau déploiement.

## 61. Performance et tuning du server/proxy

### Caches (dans `zabbix_server.conf`)

| Paramètre | Défaut | Parc moyen | Rôle |
|---|---|---|---|
| `CacheSize` | 32M | 256M–1G | Config (hosts, items, triggers) |
| `HistoryCacheSize` | 16M | 128–512M | Valeurs en attente d'écriture |
| `TrendCacheSize` | 4M | 128M | Agrégats |
| `ValueCacheSize` | 8M | 256M–1G | ⚠️ Le plus critique pour les triggers |
| `HistoryIndexCacheSize` | — | 64M | Index (7.x) |

Symptômes d'un cache trop petit : `zabbix[wcache,...]` > 80 %, triggers en retard, "value cache is fully used" dans les logs.

### Pollers

Règle : augmentez `StartPollers` tant que la file (`zabbix[queue]`, *Administration → Queue*) ne se résorbe pas. Repères :

```
< 1 000 items SNMP : StartPollers=20 suffit
5 000+ items       : 40-60
```

Ne dépassez pas ~2× le nombre de cœurs sans mesurer : trop de pollers = contention base.

### Base de données

```ini
# postgresql.conf (serveur dédié 16 Go RAM, à ajuster)
shared_buffers = 4GB
effective_cache_size = 12GB
work_mem = 64MB
maintenance_work_mem = 512MB
max_connections = 200
```

```bash
# Surveillez les requêtes lentes
# postgresql.conf :
log_min_duration_statement = 1000   # log > 1 s
```

### Clés internes de monitoring (section 69)

```
zabbix[queue]              # items en attente (> 10 = sous-dimensionné)
zabbix[wcache,values,all]  # % cache valeurs
zabbix[process,poller,avg,busy]  # % occupation pollers
```

> 💡 **Tunez par la mesure** : dashboard "Zabbix server health" (template `Zabbix server health` officiel) affiché en permanence pendant 1 semaine avant de toucher aux paramètres.

## 62. Usage quotidien : la routine de l'exploitant

🖨️ **Routine quotidienne (15 min)** :

- [ ] *Monitoring → Problems* : 0 problème non acquitté en High/Disaster ?
- [ ] *Monitoring → Dashboard* "Santé Zabbix" : queue < 10, caches < 80 % ?
- [ ] *Reports → Availability* : un hôte < 99 % sur 24 h ?
- [ ] Vérifier les acquittements de la veille (commentaires renseignés ?)
- [ ] *Administration → Queue* : aucun pic anormal ?

🖨️ **Routine hebdomadaire (1 h)** :

- [ ] *Monitoring → Discovery* : nouveaux équipements à intégrer ?
- [ ] Items **Not supported** (*Configuration → Hosts → Items*, filtre) : corriger ou désactiver
- [ ] Test d'envoi SMS mensuel (section 42) — noter le résultat
- [ ] Relecture d'un trigger bruyant : ajuster seuil/hystérésis (section 37)
- [ ] Vérifier l'espace disque de la base (`df -h`, taille des tables)
- [ ] Contrôler la sauvegarde (restauration test trimestrielle, section 71)

🖨️ **Routine mensuelle** :

- [ ] Rapport de disponibilité pour la direction (section 84)
- [ ] Revue des maintenances planifiées du mois suivant
- [ ] Mise à jour mineure Zabbix si disponible (section 72)
- [ ] Test de bout en bout de la chaîne d'alerte (section 83)

## 63. Dashboards et vues : construire des écrans d'exploitation

*Monitoring → Dashboards → Create dashboard*. Widgets essentiels :

| Widget | Usage |
|---|---|
| Problems | Les problèmes en cours (filtre par sévérité/groupe) |
| Graph / Item value | Courbes et valeurs temps réel |
| Top hosts | Top 10 CPU/mémoire/disque |
| Geomap | Sites distants sur carte |
| URL / Clock | Contexte |

### Dashboard "Énergie" (votre écran prioritaire)

```
┌─────────────────────────────────────────────────────┐
│ COUPURES EN COURS (Problems, filtre groupe=Onduleurs)│
├──────────────┬──────────────┬───────────────────────┤
│ ups-salle-01 │ ups-salle-02 │ ups-agence-sud        │
│ Source: BAT. │ Source: sect.│ Source: secteur       │
│ Batterie: 68%│ Batterie:100%│ Batterie: 100%        │
│ Autonomie: 24│ Autonomie: -- │ Autonomie: --         │
│ min          │              │                       │
├──────────────┴──────────────┴───────────────────────┤
│ Charge des onduleurs (graphe multi-hôtes, 24h)       │
│ Température salle (graphe)                           │
└─────────────────────────────────────────────────────┘
```

> 💡 **Un dashboard par public** : "Énergie" (vous), "Réseau" (équipe réseau), "Direction" (disponibilité %, SLA, sans jargon). Le dashboard direction ne montre JAMAIS de problèmes techniques bruts.

Partage : *Dashboard → Sharing* (public/privé, par groupe d'utilisateurs). ⚠️ 7.x : les dashboards peuvent être définis dans les templates.

## 64. Inventaire automatique des hôtes

*Configuration → Hosts → [hôte] → Inventory → Automatic*. L'agent remplit : OS, version, serial, modèle… (items `system.hw.*`, `system.sw.os`).

*Inventory → Overview* : tableau filtrable ("tous les serveurs Debian 12", "tous les switchs Cisco"). Export CSV pour la CMDB.

> 💡 Renseignez aussi les champs **manuels** utiles en intervention : contact, localisation ("Salle serveur, armoire A3"), contrat de maintenance. En astreinte à 3h du matin, ça vaut de l'or.

## 65. Cartes réseau (maps) et écrans de supervision

*Monitoring → Maps → Create map* : fond de plan (importez le plan de votre salle), icônes par type, liens colorés selon l'état.

- **Liens** : entre switch et serveurs, avec indicateurs de débit (`{sw:ifInOctets[{#IFINDEX}]}` en label).
- **Éléments** : icône = dernier état (OK vert, PROBLEM rouge clignotant).
- **Map imbriquées** : carte "Sites" → clic → carte "Site Sud" détaillée.

> 💡 Affichez la carte des sites sur un **écran mural** en mode kiosque (URL + `&kiosk=1`… vérifiez le paramètre selon version, ou mode plein écran du navigateur avec refresh auto). C'est votre "tour de contrôle".

## 66. Bonnes pratiques d'alerting : la chasse au bruit

Le bruit tue la confiance : une équipe qui reçoit 50 emails/jour **ne lit plus** les alertes. Règles d'or :

1. **Chaque alerte doit exiger une action.** Sinon : dashboard, pas email.
2. **Seuils + durée** : jamais de `.last()` seul sur une métrique bruitée (section 37).
3. **Hystérésis** systématique sur les seuils critiques.
4. **Dépendances** (section 39) : pas d'alerte serveur si le switch parent est down.
5. **Plages horaires** : `and {TRIGGER.VALUE}=1` combiné à `time()`/`dayofweek()` pour les alertes ouvrées.
6. **Dédupliquez** : un trigger "disque > 90 %" par partition via LLD, pas 12 triggers manuels.
7. **Acquittement obligatoire** avec commentaire (section 45).
8. **Revue mensuelle du bruit** : *Reports → Notifications* : quel trigger a envoyé le plus d'emails ? Corrigez le top 3.

### Exemple : alerte disque "propre"

```
Nom : "Espace disque {#FSNAME} > {$VFS_FS_PUSED_WARN}% sur {HOST.NAME}"
Problème  : {srv:vfs.fs.size[{#FSNAME},pused].avg(15m)}>{$VFS_FS_PUSED_WARN}
Recovery  : {srv:vfs.fs.size[{#FSNAME},pused].avg(15m)}<{$VFS_FS_PUSED_WARN}-10
Sévérité  : Average (Warning si > 95 via 2e trigger)
Action    : Email groupe Exploitation, heures ouvrées uniquement
```

### Ce qu'il ne faut JAMAIS alerter par SMS

- CPU > 80 % (bruit)
- Disque > 85 % (email suffit)
- Reboot planifié (maintenance !)

### Ce qui mérite le SMS

- Onduleur sur batterie (Disaster)
- Site injoignable (High)
- Température salle > 35 °C (High)
- Batterie onduleur < 30 % pendant coupure (Disaster)

## 67. Intégration avec Grafana

Zabbix a de beaux dashboards ; Grafana excelle dans les **vues mixtes** (Zabbix + autres sources) et les rapports.

1. Installez Grafana, ajoutez le plugin **Zabbix datasource** (alexanderzobnin-zabbix-app).
2. *Connections → Data sources → Zabbix* : URL `http://192.168.10.50/api_jsonrpc.php`, credentials d'un compte API **lecture seule** dédié.
3. Créez des dashboards : courbes lissées, annotations d'événements, variables `$host`, `$group`.

Cas d'usage typiques :

- **Rapport direction** : disponibilité mensuelle, jolis graphes exportables PDF.
- **Corrélation** : charge onduleur (Zabbix) + température salle (sonde) + incidents (annotations manuelles).
- **Mur d'écrans** : playlists Grafana en rotation.

> 💡 Compte API dédié `grafana-reader` (rôle User, accès Read sur les groupes nécessaires, **pas** d'accès admin). Si le token fuit, l'impact est limité.

## 68. Sécurité et durcissement en production

🖨️ **Checklist durcissement** :

- [ ] **HTTPS** sur le frontend (Let's Encrypt ou CA interne) :
```bash
apt install -y certbot python3-certbot-nginx
certbot --nginx -d zabbix.votre-domaine.local
```
- [ ] **TLS/PSK** sur tous les flux agent/proxy (section 20)
- [ ] **SNMP v3** partout en production (section 49) ; communautés v2c complexes et ACL sur les équipements
- [ ] Compte `Admin` renommé ou désactivé après création des comptes nominatifs
- [ ] **2FA** pour les Super admins (⚠️ natif 6.4+/7.x)
- [ ] LDAP/AD pour l'authentification centralisée
- [ ] Pare-feu sur le server : n'ouvrir que 10051 (agents/proxys), 443 (admins), 162/UDP (traps)
```bash
# Exemple nftables/ufw (à adapter)
ufw allow from 192.168.10.0/24 to any port 10051 proto tcp
ufw allow from 192.168.10.0/24 to any port 443 proto tcp
ufw allow 162/udp
```
- [ ] Base de données : écoute sur localhost ou réseau de gestion uniquement, mot de passe fort, pas de compte `zabbix` superuser
- [ ] `AllowRoot=0` sur les agents (défaut) ; `DenyKey=` pour bloquer les clés dangereuses :
```ini
# zabbix_agent2.conf — bloque l'exécution de commandes arbitraires
DenyKey=system.run[*]
```
- [ ] Mises à jour OS régulières ; Zabbix mineur à jour (section 72)
- [ ] Logs : `LogFileSize` avec rotation, envoi vers syslog centralisé si possible
- [ ] Sauvegarde chiffrée (section 70)

> ⚠️ **Ne jamais** exposer le frontend Zabbix sur internet sans : HTTPS + 2FA + restriction IP (ou VPN). Un Zabbix compromis = cartographie complète de votre SI offerte à l'attaquant.

## 69. Superviser Zabbix lui-même (qui surveille le surveillant ?)

Créez un hôte `zabbix-srv` (interface agent) + liez les templates officiels :

- `Zabbix server health` : queue, caches, pollers busy, housekeeper, valeur par seconde
- `Linux by Zabbix agent` : OS du server
- Base : `PostgreSQL by Zabbix agent 2` ou `MySQL by Zabbix agent 2`

**Triggers vitaux du monitoring** :

```
# File d'attente qui grandit → server sous-dimensionné ou base lente
{zabbix-srv:zabbix[queue].avg(5m)}>50                    → High
# Cache valeurs saturé
{zabbix-srv:zabbix[wcache,values,all].last()}>80         → High
# Pollers saturés
{zabbix-srv:zabbix[process,poller,avg,busy].avg(5m)}>75  → Average
# Housekeeper à la traîne
{zabbix-srv:zabbix[housekeeper,lastrun].fuzzytime(86400)}=0 → Average
# Le service est-il vivant ? (depuis un proxy ou un 2e site)
{zabbix-srv:net.tcp.service[http,,80].last()}=0          → Disaster
```

> 💡 **Règle absolue** : les alertes "Zabbix lui-même en panne" doivent partir par un **canal indépendant** (SMS via la passerelle GSM locale, pas l'email qui dépend du même réseau). Sinon, quand tout tombe, vous n'apprenez rien.

**Supervision croisée** : un 2e Zabbix (ou un simple script cron + `zabbix_sender` depuis un autre site) qui ping le server principal. En version simple :

```bash
# Cron toutes les 5 min sur une machine distante
*/5 * * * * /usr/bin/zabbix_sender -z 192.168.10.50 -s "watchdog-externe" \
  -k zabbix.frontend.http -o $(curl -s -o /dev/null -w "%{http_code}" https://zabbix.votre-domaine.local/)
```

Trigger : `{watchdog:zabbix.frontend.http.last()}<>200`.

## 70. Sauvegarde : base de données, fichiers, stratégie 3-2-1

**Ce qu'il faut sauvegarder** :

| Quoi | Où | Fréquence |
|---|---|---|
| Base Zabbix complète | `pg_dump` / `mysqldump` | Quotidienne |
| `/etc/zabbix/` | Fichiers de config | À chaque changement (+ Git) |
| `/usr/lib/zabbix/alertscripts/` | Scripts d'alerte | À chaque changement |
| `/usr/share/zabbix/` (custom) | Images, cartes personnalisées | Hebdo |
| Templates exportés (YAML) | Git | À chaque changement |

### PostgreSQL

```bash
#!/bin/bash
# /usr/local/bin/backup_zabbix.sh
set -e
DATE=$(date +%F)
DEST=/backup/zabbix
mkdir -p "$DEST"
sudo -u postgres pg_dump -Fc zabbix > "$DEST/zabbix_$DATE.dump"
# Rétention 30 jours
find "$DEST" -name "zabbix_*.dump" -mtime +30 -delete
# Configs
tar czf "$DEST/zabbix_etc_$DATE.tgz" /etc/zabbix /usr/lib/zabbix/alertscripts
find "$DEST" -name "zabbix_etc_*.tgz" -mtime +30 -delete
```

```bash
chmod +x /usr/local/bin/backup_zabbix.sh
# Cron : tous les jours à 02:00
echo "0 2 * * * root /usr/local/bin/backup_zabbix.sh >> /var/log/zabbix_backup.log 2>&1" \
  > /etc/cron.d/zabbix-backup
```

### MySQL/MariaDB

```bash
mysqldump --single-transaction --routines --events -u zabbix -p'mot_de_passe_ici' \
  zabbix | gzip > /backup/zabbix/zabbix_$(date +%F).sql.gz
```

> ⚠️ **3-2-1** : 3 copies, 2 supports différents, 1 hors site. La sauvegarde Zabbix doit partir **hors du server Zabbix** (NAS, autre site via le proxy, stockage objet). Une sauvegarde sur le même disque que la base ne protège de rien.

### Sauvegarde applicative (items, triggers, templates)

```bash
# Export API de la configuration (sans les données)
# À lancer périodiquement, versionné en Git
python3 export_config.py  # via API : template.get, host.get, action.get...
```

## 71. Restauration : procédure testée

> 💡 **Une sauvegarde non testée = pas de sauvegarde.** Testez la restauration **chaque trimestre** sur une VM isolée.

### Restauration PostgreSQL

```bash
# 1. Stopper les services
systemctl stop zabbix-server zabbix-proxy nginx php8.2-fpm
# 2. Recréer la base vide
sudo -u postgres psql -c "DROP DATABASE zabbix;"
sudo -u postgres psql -c "CREATE DATABASE zabbix OWNER zabbix;"
# 3. Restaurer
sudo -u zabbix pg_restore -d zabbix /backup/zabbix/zabbix_2026-09-25.dump
# 4. Restaurer les configs
tar xzf /backup/zabbix/zabbix_etc_2026-09-25.tgz -C /
# 5. Redémarrer
systemctl start zabbix-server nginx php8.2-fpm
# 6. Vérifier
tail -n 30 /var/log/zabbix/zabbix_server.log
```

### Restauration MySQL

```bash
zcat /backup/zabbix/zabbix_2026-09-25.sql.gz | mysql -u zabbix -p zabbix
```

🖨️ **Checklist de test de restauration** :

- [ ] Restauration effectuée sur machine isolée
- [ ] Frontend accessible, login OK
- [ ] Hôtes et derniers événements visibles
- [ ] Un trigger de test se déclenche
- [ ] Durée mesurée : ______ (objectif < 1 h pour un parc moyen)

## 72. Mise à jour de version : méthode sans stress

### Mise à jour mineure (7.0.1 → 7.0.5) : simple

```bash
# 1. Sauvegarde complète (section 70) — NON NÉGOCIABLE
/usr/local/bin/backup_zabbix.sh
# 2. Snapshot VM si possible
# 3. Mise à jour
apt update && apt install --only-upgrade zabbix-server-pgsql zabbix-frontend-php \
  zabbix-agent2 zabbix-sql-scripts
# 4. Redémarrage (le server migre la base automatiquement si besoin)
systemctl restart zabbix-server zabbix-agent2 nginx php8.2-fpm
# 5. Vérification
tail -n 50 /var/log/zabbix/zabbix_server.log | grep -i "database upgrade\|started"
```

### Mise à jour majeure (6.0 LTS → 7.0 LTS) : méthodique

1. **Lisez les release notes** et la section "Upgrade notes" de la doc officielle (breaking changes : PHP, base, templates).
2. **Maquette** : clonez la prod (VM + dump base), faites la montée en version dessus, validez 1 semaine.
3. **Sauvegarde** + snapshot.
4. Changez le dépôt (`zabbix-release` 7.0), `apt update`, `apt install` des paquets.
5. Le server **7.0** migre automatiquement le schéma 6.0 → 7.0 au démarrage (peut prendre 10-60 min sur grosse base — **ne redémarrez pas pendant la migration**).
6. Mettez à jour les **proxies** (un proxy 6.0 ne parle pas à un server 7.0 — même majeure requise).
7. Les **agents** peuvent rester en version N-1 temporairement (compatibilité ascendante).
8. Videz le cache navigateur, vérifiez les dashboards custom.

> ⚠️ **Ordre impératif** : server d'abord, puis proxies, puis agents. Jamais l'inverse. Et **jamais** de saut de 2 majeures (5.0 → 7.0) : passez par les intermédiaires ou réinstallez proprement.

## 73. Dépannage : méthodologie générale

Quand quelque chose ne marche pas, suivez la chaîne **collecte → stockage → évaluation → alerte → visualisation** (section 4) :

```
1. L'item a-t-il des "Latest data" ?  (Monitoring → Latest data)
   NON → problème de COLLECTE : testez avec zabbix_get / snmpwalk depuis le server
   OUI → continuez
2. Le trigger s'évalue-t-il ? (statut Unknown ? → pas assez d'historique / fonction mal paramétrée)
3. L'action matche-t-elle ? (Reports → Audit / Action log : l'action s'est-elle déclenchée ?)
4. Le média fonctionne-t-il ? (Administration → Media types → Test)
5. Le dashboard affiche-t-il ? (problème de droits ? de filtre temporel ?)
```

**Les 3 logs à connaître** :

```bash
tail -f /var/log/zabbix/zabbix_server.log    # le server
tail -f /var/log/zabbix/zabbix_agent2.log    # l'agent (côté supervisé)
tail -f /var/log/zabbix/zabbix_proxy.log     # le proxy
```

Augmentez temporairement la verbosité : `LogLevel=4` (debug) dans la conf + `systemctl restart`, **puis remettez 3** après diagnostic (le debug remplit le disque).

## 74. Erreur n°1 à n°12 : les classiques commentés

### Erreur n°1 — Le server ne démarre pas : connexion base impossible

```
[Z3001] connection to database 'zabbix' failed: [1045] Access denied
```

**Causes** : mauvais `DBPassword`, utilisateur inexistant, base non créée, PostgreSQL n'écoute pas.
**Correctifs** :

```bash
# Tester la connexion manuellement
sudo -u zabbix psql -h localhost -U zabbix zabbix -c "SELECT 1;"
# Vérifier pg_hba.conf : méthode md5/scram pour l'utilisateur zabbix
grep -v "^#" /etc/postgresql/15/main/pg_hba.conf | grep -v "^$"
```

### Erreur n°2 — Hôte en gris/rouge : "Get value from agent failed"

```
ZBX_NOTSUPPORTED / cannot connect to [[192.168.10.61]:10050]: connection refused
```

**Checklist** :
1. L'agent tourne-t-il ? `systemctl status zabbix-agent2` sur le supervisé.
2. Le pare-feu ? `ss -tlnp | grep 10050` ; test depuis le server : `zabbix_get -s 192.168.10.61 -k agent.ping`.
3. `Server=` dans `zabbix_agent2.conf` contient-il l'IP du server ? (sinon l'agent refuse la connexion — silencieux côté agent, timeout côté server).
4. En mode actif : `Hostname=` **exactement** égal au nom dans le frontend (casse, espaces).

### Erreur n°3 — Import du schéma MySQL : charset incorrect

```
ERROR 1366 (HY000): Incorrect string value...
```

**Cause** : base créée sans `utf8mb4`. **Correctif** : recréer la base avec `CHARACTER SET utf8mb4 COLLATE utf8mb4_bin` (section 15). Il n'y a pas de raccourci propre : refaites l'import.

### Erreur n°4 — "No data" partout après ajout d'un proxy

**Cause** : le proxy n'est pas déclaré côté server, ou `Hostname` différent entre `zabbix_proxy.conf` et *Administration → Proxies*.
**Correctif** : les 3 noms doivent matcher : `Hostname=` du proxy, nom du proxy dans le frontend, et les hôtes doivent avoir *Monitored by* = ce proxy. Log proxy : `cannot send list of active checks` = le server ne connaît pas ce proxy.

### Erreur n°5 — Items SNMP "Not supported"

**Causes** : mauvais OID, communauté erronée, pare-feu UDP/161, équipement sans SNMP activé.
**Diagnostic** :

```bash
# Depuis le server Zabbix lui-même :
snmpwalk -v2c -c communaute_lecture_ici 192.168.10.90 1.3.6.1.2.1.33.1.2.3.1.1.2.1
# Timeout → réseau/communauté ; "No Such Object" → mauvais OID
```

> 💡 90 % des problèmes SNMP se résolvent en CLI avant même d'ouvrir le frontend.

### Erreur n°6 — Trigger en "Unknown"

**Causes** : pas encore de données (nouvel item), fonction sur période trop longue (`avg(1h)` avec 5 min d'historique), division par zéro dans un calculé.
**Correctif** : attendez la première collecte ; *Monitoring → Latest data* pour vérifier que l'item reçoit des valeurs.

### Erreur n°7 — Tempête d'emails après une coupure

**Cause** : pas de dépendances (section 39), pas de maintenance (section 57).
**Correctif immédiat** : *Configuration → Maintenance* d'urgence sur le site ; **correctif durable** : arbre de dépendances + action "site down" unique au lieu de N actions par hôte.

### Erreur n°8 — "Value cache is fully used"

```
value cache is fully used
```

**Cause** : `ValueCacheSize` trop petit (section 61). **Correctif** : doublez `ValueCacheSize`, redémarrez le server, surveillez `zabbix[wcache,values,all]`.

### Erreur n°9 — Le frontend affiche "Database error"

**Causes** : base arrêtée, `zabbix.conf.php` avec mauvais mot de passe, base pleine (disque).
**Diagnostic** :

```bash
df -h /var/lib/postgresql
systemctl status postgresql
# Tester les identifiants du frontend :
grep DB_ /etc/zabbix/web/zabbix.conf.php
```

### Erreur n°10 — Les graphes ont des "trous"

**Causes** : collecte plus lente que l'intervalle (pollers saturés), lien instable (normal — le proxy bufferise, section 7), NTP désynchronisé, housekeeper agressif.
**Diagnostic** : *Administration → Queue* (âge des items en retard), `zabbix[queue]`, vérifier chrony des deux côtés.

### Erreur n°11 — L'alerte SMS ne part pas

**Checklist** :
1. *Administration → Media types → [SMS] → Test* : fonctionne ?
2. Le script a-t-il les bons droits ? (`zabbix:zabbix`, +x)
3. La clé 4G a-t-elle du crédit / du réseau ? (`gammu networkinfo`)
4. L'action a-t-elle vraiment déclenché une opération SMS ? (*Reports → Action log*)
5. Le numéro du destinataire est-il au format international ? (`+225...`)

### Erreur n°12 — Après mise à jour : "Database version mismatch"

```
Database version does not match: expected X, got Y
```

**Cause** : le server a été mis à jour mais pas les scripts SQL, ou la migration a été interrompue.
**Correctif** : installez `zabbix-sql-scripts` de la **même version** que le server, relancez le server et **laissez la migration se terminer** (surveillez le log — ne redémarrez pas). En cas d'interruption brutale : restaurez la sauvegarde pré-upgrade (section 71) et recommencez.

## 75. Cas pratique n°1 : supervision d'une baie complète

**Objectif** : 1 switch + 4 serveurs + 1 onduleur dans une armoire, alertes propres.

1. Groupes : `Baie-A3/Réseau`, `Baie-A3/Serveurs`, `Baie-A3/Onduleurs`.
2. Hôtes + templates : switch (SNMP), serveurs (Agent 2), onduleur (template section 52).
3. Dépendances : triggers serveurs/onduleur "injoignable" → dépendent du trigger "switch injoignable".
4. Maintenance récurrente : créneau MCO mensuel.
5. Dashboard "Baie A3" : map de l'armoire + problèmes filtrés.
6. Action : High+ du groupe `Baie-A3/*` → Email + Telegram, escalade SMS à 15 min.

**Résultat attendu** : une coupure du switch = **1 seule alerte** (le switch), pas 6.

## 76. Cas pratique n°2 : supervision d'un onduleur 40 kVA de A à Z

**Contexte** : onduleur `ups-salle-01` (192.168.10.90), 40 kVA, salle serveur principale.

**Étape 1 — Validation SNMP** (depuis le server) :

```bash
snmpwalk -v2c -c communaute_lecture_ici 192.168.10.90 1.3.6.1.2.1.33.1.1.1.1.2.0
# Attendu : STRING: "Easy UPS 3S 40 kVA" (ou modèle réel)
```

**Étape 2 — Hôte** : groupe `Onduleurs`, interface SNMP, macros `{$SNMP_COMMUNITY}`, template `Onduleur SNMP générique (RFC1628)` (section 52).

**Étape 3 — Seuils adaptés au 40 kVA** (macros au niveau hôte) :

```
{$UPS_LOAD_WARN} = 70      # 28 kVA : anticiper l'extension
{$UPS_LOAD_CRIT} = 85      # 34 kVA : proche de la limite
{$UPS_BATT_WARN} = 60
{$UPS_BATT_CRIT} = 30
```

**Étape 4 — Action dédiée** (section 40) : Disaster/High du groupe `Onduleurs` → Email + Telegram immédiats, SMS à 5 min, escalade chef de service à 20 min.

**Étape 5 — Dashboard** : widget "Item value" géant sur `upsEstimatedMinutesRemaining` + courbe charge 24 h + température batterie.

**Étape 6 — Test réel** : passage sur batterie (test programmé avec maintenance !) → vérifier que toute la chaîne se déclenche en < 2 min.

## 77. Cas pratique n°3 : alerte coupure électrique avec escalade SMS

**Scénario** : coupure secteur à 14:03, onduleur sur batterie.

```
14:03:30  Item upsOutputSource → 5 (poll 30 s)
14:03:35  Trigger "COUPURE SECTEUR : ups-salle-01 sur batterie" → PROBLEM (Disaster)
14:03:35  Action : Email + Telegram → groupe Energie  ✓
14:08:35  Pas d'acquittement → SMS → groupe Energie : "COUPURE ups-salle-01, batterie 82 %, autonomie ~35 min"
14:23:35  Toujours pas d'acquittement → SMS → Chef de service
14:41:00  Secteur rétabli → upsOutputSource → 3
14:41:05  Recovery → Email + Telegram "RÉSOLU : retour sur secteur à 14:41"
```

**Points de vigilance** :
- Le SMS contient **l'autonomie restante** (`{ITEM.VALUE}` de l'item autonomie) : l'astreinte sait si elle a 10 ou 40 min.
- Si la coupure touche aussi le server Zabbix : c'est le **watchdog externe** (section 69) qui alerte via la passerelle GSM autonome.
- **Ne jamais** mettre l'envoi SMS sur le même onduleur que la salle supervisée sans batterie de secours pour la clé 4G.

## 78. Cas pratique n°4 : supervision d'un lien VSAT/fibre instable

**Contexte** : site distant relié en VSAT (latence 600 ms, micro-coupures).

1. **Proxy actif** sur site (`ProxyOfflineBuffer=48h`, `HeartbeatFrequency=120`).
2. **Intervalles adaptés** : ping 5 min, SNMP 5 min, pas de LLD agressive.
3. **Triggers tolérants** :
```
# Site down : 3 échecs de ping sur 15 min (pas 1 seul)
{proxy-site:icmpping.count(15m,0)}>2
```
4. **Dépendance** : tout le site dépend du trigger "lien VSAT down".
5. **Dashboard** : disponibilité 30 jours du lien (pour renégocier le SLA opérateur avec des preuves).

> 💡 Les données de disponibilité Zabbix sont des **preuves contractuelles** : exportez le rapport mensuel avant chaque réunion avec l'opérateur.

## 79. Cas pratique n°5 : capacity planning disque avec prédiction

**Objectif** : être alerté **avant** la panne, pas pendant.

```
Trigger "Disque {#FSNAME} plein dans < 14 jours" (Warning) :
  {srv:vfs.fs.size[{#FSNAME},pfree].forecast(#100,14d,,avg)}<10
  and {srv:vfs.fs.size[{#FSNAME},pfree].avg(1h)}<20
```

- `forecast(#100,14d,,avg)` : régression sur les 100 dernières valeurs, projection à 14 jours.
- La 2e condition évite les faux positifs sur disques déjà petits mais stables.
- Dashboard "Capacity" : Top hosts par `pfree` croissant + courbes 90 jours (trends).

**Rituel mensuel** : relecture du dashboard capacity → plan d'extension avant saturation.

## 80. Cas pratique n°6 : supervision d'une salle serveur (température, hygrométrie)

**Équipement** : sonde SNMP (ex. Sonde USB/Modbus via passerelle, ou boîtier type AKCP/APC NetBotz).

1. Hôte `sonde-salle-01`, template custom avec items :
```
salle.temp   (OID sonde, °C)   intervalle 2m
salle.hygro  (OID sonde, %HR)  intervalle 5m
```
2. Triggers :
```
# Warning 27 °C, High 32 °C, Disaster 35 °C (+ hysteresis recovery -2 °C)
{salle:salle.temp.avg(10m)}>32
# Hygrométrie > 70 % : risque condensation/corrosion
{salle:salle.hygro.avg(30m)}>70
# Sonde muette
{salle:salle.temp.nodata(10m)}=1
```
3. Corrélation : superposez température salle + charge onduleur + état climatisation sur un graphe.

> 💡 Pour vous : la température salle est un **indicateur avancé** de problème de climatisation, elle-même souvent liée à l'énergie. Mettez-la sur le dashboard "Énergie".

## 81. Cas pratique n°7 : déploiement multi-sites avec proxies

**Architecture** : 1 server central + 3 proxies (sites Nord, Sud, Est).

```
Site Nord (fibre)     Site Sud (4G)         Site Est (VSAT)
proxy-nord (actif)    proxy-sud (actif)     proxy-est (actif)
Buffer 6h             Buffer 24h            Buffer 48h
```

1. Créez les 3 proxies dans le frontend (*Administration → Proxies*), mode actif, PSK par proxy (section 20).
2. Chaque hôte de site : *Monitored by* = son proxy.
3. **Nommage** : préfixe par site (`sud-srv-01`, `est-ups-01`) → filtres et permissions par groupe `Sites/Sud`.
4. **Supervision des proxies eux-mêmes** : hôtes `proxy-sud` avec template `Zabbix proxy health` (officiel) → alerte si `zabbix[proxy,lastaccess]` > 10 min.
5. **Mise à jour** : planifiez par site (jamais les 3 en même temps), ordre server → proxies → agents.

## 82. Cas pratique n°8 : supervision d'un cluster Proxmox VE

1. Sur chaque nœud : Agent 2 + template `Linux by Zabbix agent`.
2. Items spécifiques (UserParameter ou plugin) :
```ini
# VMs en cours (nécessite droits lecture sur l'API ou qm)
UserParameter=pve.vm.running, qm list 2>/dev/null | grep -c running
UserParameter=pve.node.quorum, pvecm status 2>/dev/null | grep -q "Quorate: Yes" && echo 1 || echo 0
```
3. Triggers :
```
# Perte du quorum → Disaster (le cluster ne migre plus)
{pve:pve.node.quorum.last()}=0
# Stockage Ceph/ZFS > 85 %
```
4. Alternative sans agent : API Proxmox via **HTTP agent** + JSONPath (token API dédié).

> 💡 Le quorum Proxmox et l'état Zabbix HA (section 58) : deux clusters à superviser avec la même rigueur — ce sont vos fondations.

## 83. Cas pratique n°9 : blackout test — valider toute la chaîne d'alerte

**Objectif** : prouver que l'alerte arrive vraiment quand tout s'écroule. À faire **2 fois par an**, en heures ouvrées, avec l'équipe prévenue.

🖨️ **Protocole** :

- [ ] Prévenir l'équipe (ceci est un TEST, pas une vraie panne)
- [ ] Couper le secteur de la salle de test (ou simuler : arrêter l'agent d'un hôte pilote)
- [ ] Chronométrer : T+0 coupure → T+? email → T+? Telegram → T+? SMS
- [ ] Vérifier l'escalade : ne pas acquitter, attendre le SMS chef de service
- [ ] Vérifier le watchdog externe (section 69)
- [ ] Rétablir, vérifier le message "RÉSOLU"
- [ ] Compte-rendu : délais mesurés, points de défaillance, actions correctives

**Objectifs** : email < 2 min, SMS < 10 min, escalade < 30 min. Si ce n'est pas tenu : revoir intervalles et actions **avant** la vraie panne.

## 84. Cas pratique n°10 : rapport mensuel de disponibilité pour la direction

*Reports → Availability* ou dashboard Grafana (section 67) :

```
Disponibilité Septembre 2026
─────────────────────────────────────────
Service / équipement        Dispo    Indisponibilité
Onduleur salle (énergie)    99,98 %  9 min (coupure 12/09)
Lien VSAT site Est          97,20 %  20 h (opérateur)
Serveur fichiers            100 %    —
Infra globale (moyenne)     99,40 %
─────────────────────────────────────────
Incidents majeurs : 2 (détail en annexe)
Alertes traitées : 34 (dont 3 Disaster)
```

> 💡 Un rapport mensuel régulier, même d'une page, **justifie le budget supervision** et objective les discussions avec les opérateurs/fournisseurs. C'est aussi votre mémoire en cas d'audit.

## 85. Pense-bête de poche (cheat sheet imprimable)

🖨️ **Zabbix — Mémo d'astreinte** (une page, à plastifier) :

```
── SERVICES ──────────────────────────────
systemctl status zabbix-server zabbix-agent2 zabbix-proxy
systemctl restart zabbix-server
tail -f /var/log/zabbix/zabbix_server.log

── TESTS RAPIDES ─────────────────────────
zabbix_get -s <IP> -k agent.ping
zabbix_get -s <IP> -k system.cpu.load[all,avg1]
snmpwalk -v2c -c <comm> <IP> 1.3.6.1.2.1.33.1.2.3.1.1.2.1  (batterie %)
snmpwalk -v2c -c <comm> <IP> 1.3.6.1.2.1.33.1.1.4.1.1.2.1 (source: 3=secteur,5=batterie)

── OÙ CHERCHER ───────────────────────────
Pas de données  → Monitoring → Latest data
Trigger inconnu → attendre / vérifier l'item
Pas d'alerte    → Reports → Action log, puis Audit
File d'attente  → Administration → Queue
Santé Zabbix   → Dashboard "Zabbix server health"

── NUMÉROS UTILES ────────────────────────
Passerelle SMS : gammu sendsms TEXT +2250700000000 "message"
Test SMS mensuel : le 1er lundi du mois
Escalade : Exploitation (15 min) → Chef de service (45 min)

── EN CAS DE TEMPÊTE D'ALERTES ───────────
1. Configuration → Maintenance → créer (site concerné)
2. Identifier la cause racine (dépendances, section 39)
3. Acquitter avec commentaire
4. Post-mortem : ajuster seuils/dépendances

── SAUVEGARDE ────────────────────────────
/usr/local/bin/backup_zabbix.sh  (cron 02:00)
Restauration : section 71 — TESTÉE LE : ____/____/________
```

## 86. Glossaire

| Terme | Définition |
|---|---|
| **Action** | Règle "si trigger en PROBLEM alors notifier" (conditions + opérations + escalades) |
| **Agent (actif/passif)** | Programme sur le supervisé ; actif = il envoie, passif = on l'interroge |
| **Agrégat** | Item calculé sur plusieurs hôtes (grpavg, grpsum) |
| **Disponibilité (Availability)** | Icônes ZBX/SNMP/JMX/IPMI vertes/rouges par hôte |
| **Escalade** | Montée en puissance des notifications si pas d'acquittement |
| **Frontend** | Interface web PHP (visualisation + configuration) |
| **Hôte** | Équipement supervisé |
| **Housekeeping** | Nettoyage automatique des vieilles données |
| **Item** | Une métrique collectée (clé + intervalle + type) |
| **LLD** | Low-Level Discovery : découverte automatique (disques, interfaces…) |
| **Macro** | Variable `{$NOM}` (template, hôte, globale) pour seuils et paramètres |
| **Maintenance** | Plage sans notification (mais avec collecte) |
| **Média** | Canal d'alerte (Email, SMS, Telegram, webhook) |
| **MIB** | Dictionnaire SNMP (OID ↔ noms lisibles) |
| **nodata()** | Fonction trigger = 1 si aucune donnée reçue sur la période |
| **OID** | Identifiant numérique d'une donnée SNMP |
| **Poller** | Processus server qui interroge (passif, SNMP…) |
| **Prétraitement** | Transformation de la valeur avant stockage |
| **Proxy** | Collecteur distant avec buffer offline |
| **PSK** | Clé pré-partagée pour chiffrer les flux (TLS) |
| **Template** | Paquet réutilisable (items, triggers, graphes, LLD) |
| **Trapper** | Item alimenté par envoi externe (`zabbix_sender`) |
| **Trends** | Agrégats horaires (min/max/avg/count), longue rétention |
| **Trigger** | Expression logique → OK/PROBLEM |
| **UserParameter** | Clé d'agent personnalisée (script maison) |
| **Value mapping** | Traduction valeur → libellé (5 → "BATTERIE") |
| **vps** | Values per second : valeurs collectées par seconde (dimensionnement) |

## 87. Quiz : 10 questions pour valider (avec réponses)

**Q1.** Quelle est la différence entre un check agent **actif** et **passif** ?
> *R : Passif : le server interroge l'agent (port 10050 côté agent). Actif : l'agent récupère sa liste d'items puis envoie les valeurs au server (port 10051 côté server). L'actif traverse NAT/pare-feu en flux sortant uniquement.*

**Q2.** Un proxy Zabbix calcule-t-il les triggers et envoie-t-il des alertes ?
> *R : Non. Le proxy collecte et bufferise (utile sur lien instable), puis relaie au server. Seul le server évalue les triggers et déclenche les actions.*

**Q3.** Que signifient les valeurs 3 et 5 de `upsOutputSource` (RFC 1628) ?
> *R : 3 = alimentation normale (secteur), 5 = sur batterie. Le trigger Disaster "coupure secteur" teste `=5`.*

**Q4.** Pourquoi faut-il une *recovery expression* différente du seuil de déclenchement ?
> *R : Pour l'hystérésis : éviter le flapping (oscillation PROBLEM/OK) quand la valeur oscille autour du seuil. Ex. déclenche à 90 %, ne se résout qu'à 80 %.*

**Q5.** Dans quel ordre met-on à jour server, proxies et agents lors d'une montée de version majeure ?
> *R : Server d'abord, puis proxies (même version majeure requise), puis agents (peuvent rester en N-1 temporairement). Jamais l'inverse, jamais de saut de 2 majeures.*

**Q6.** À quoi sert `ProxyOfflineBuffer` ?
> *R : Durée pendant laquelle le proxy conserve localement les données quand le lien vers le server est coupé (ex. 24h sur liaison 4G instable). Au retour du lien, les données sont renvoyées : pas de trou dans les graphes.*

**Q7.** Citez 3 moyens de réduire le bruit des alertes.
> *R : Hystérésis (recovery expression), seuils avec durée (avg/count au lieu de last), dépendances entre triggers, maintenances planifiées, plages horaires, acquittement obligatoire. (3 parmi ces réponses.)*

**Q8.** Pourquoi choisir PostgreSQL + TimescaleDB plutôt que MySQL pour un nouveau déploiement ?
> *R : Recommandation officielle Zabbix ; compression native des historiques (÷5 à ÷10 d'espace disque), retention policies automatiques, meilleures performances sur gros volumes. Le partitionnement MySQL est manuel et plus fragile.*

**Q9.** Que vérifiez-vous en premier si un item SNMP est "Not supported" ?
> *R : En CLI depuis le server : `snmpwalk` avec la même communauté et le même OID. Si snmpwalk échoue (timeout → réseau/communauté ; No Such Object → mauvais OID), inutile de chercher côté Zabbix.*

**Q10.** Pourquoi les alertes "Zabbix en panne" doivent-elles partir par un canal indépendant ?
> *R : Parce que si le server ou le réseau principal tombe, l'email classique (qui dépend de la même infrastructure) ne partira pas. Il faut un canal hors dépendance : SMS via passerelle GSM autonome, watchdog externe.*

## 88. Pour aller plus loin

**Documentation officielle** (toujours la référence pour votre version exacte) :
- Doc Zabbix 7.0 : `https://www.zabbix.com/documentation/7.0`
- Matrice de compatibilité (PHP, bases, navigateurs) : page "Requirements"
- Templates officiels : dépôt Git `zabbix/zabbix` (dossier `templates/`)

**À explorer ensuite** :
- **API Zabbix** : automatisez la création d'hôtes (inventaire CMDB → Zabbix), les maintenances, les exports Git.
- **TimescaleDB avancé** : continuous aggregates pour des rapports instantanés sur 2 ans.
- **CA interne + certificats** (plutôt que PSK) si vos flux traversent des réseaux tiers.
- **Zabbix + NUT** : corrélez les événements onduleur Zabbix (SNMP) avec les arrêts automatiques NUT de vos serveurs (voir votre guide onduleurs, section NUT).
- **Tests de charge** : `zabbix_sender` en boucle pour valider le dimensionnement avant la mise en production.
- **Communauté** : forum Zabbix, Zabbix Summit (conférence annuelle), blog officiel.

**Feuille de route conseillée (6 mois)** :
1. Mois 1 : maquette + 10 hôtes pilotes, chaîne d'alerte email/Telegram validée.
2. Mois 2 : templates onduleur/switch, dépendances, premier site avec proxy.
3. Mois 3 : SMS opérationnel, dashboards énergie/réseau/direction, blackout test n°1.
4. Mois 4 : durcissement (TLS, SNMPv3, 2FA), sauvegarde testée (restauration réelle).
5. Mois 5 : Grafana, rapports mensuels automatisés, LLD maison pour le parc spécifique.
6. Mois 6 : revue du bruit, HA si besoin, blackout test n°2, documentation d'exploitation à jour.

> 💡 **Dernier conseil** : la supervision est un organisme vivant. Un Zabbix qu'on n'entretient pas (seuils jamais relus, templates jamais mis à jour, sauvegardes jamais testées) devient en 2 ans un générateur de bruit que plus personne ne regarde. La routine de la section 62 est aussi importante que l'installation.

---

*Fin du guide — bon monitoring !* 🔍
