# Guide Grafana — Dashboards, visualisation et alerting

> **Public :** chefs de service systèmes, sysadmins, équipes d'exploitation.
> **Objectif :** installer, configurer, exploiter et superviser Grafana en production :
> dashboards lisibles, alerting fiable, provisioning versionné, sauvegardes qui restaurent.
> **Versions couvertes :** Grafana 10.x / 11.x (OSS), sur Debian 12 / Ubuntu 22.04 / 24.04.
> **Convention :** les commandes sont à exécuter en root sauf mention contraire.
> Aucun mot de passe réel dans ce guide : remplacez les valeurs `<A_COMPLETER>`.

---

## Sommaire

1. Introduction et périmètre du guide
2. Concepts : datasources
3. Concepts : dashboards, panels, variables
4. Concepts : alerting unifié
5. Architecture de Grafana
6. Installation sur Debian/Ubuntu (dépôt officiel)
7. Installation via Docker (aperçu)
8. Premier lancement et première connexion
9. Tour d'horizon de l'interface
10. Configuration `grafana.ini` — serveur et chemins
11. Configuration — base de données interne
12. Configuration — journalisation
13. Configuration — sécurité applicative (secret, cookies, sessions)
14. Configuration — HTTPS/TLS natif
15. Configuration — SMTP (notifications e-mail)
16. Datasource Prometheus — ajout et réglages
17. PromQL de base pour vos dashboards
18. PromQL avancé (rate, histogrammes, label_replace)
19. Datasource Loki — ajout et réglages
20. LogQL de base
21. LogQL avancé (parsing, filtres, métriques issues des logs)
22. Datasource Zabbix (plugin)
23. Datasource MySQL — métriques métier
24. Datasource PostgreSQL — métriques métier
25. Datasource InfluxDB (introduction)
26. Datasources : tests et bonnes pratiques
27. Panels : time series
28. Panels : gauge (jauge)
29. Panels : stat (grandes valeurs)
30. Panels : table
31. Panels : heatmap
32. Panels : bar chart, pie chart, state timeline, status history
33. Panels : text, dashboard list, alert list
34. Options de panel : unités, min/max, seuils, couleurs
35. Overrides de champs (field overrides)
36. Variables et templates — le principe
37. Variables : les 7 types en détail
38. Chaînage de variables, regex et multi-sélection
39. Transformations (merge, join, filter, calculate field…)
40. Annotations (événements superposés aux courbes)
41. Liens et drill-down (data links, dashboard links)
42. Alerting unifié : concepts (règles, instances, états)
43. Créer une règle d'alerte — exemple commenté pas à pas
44. Expressions d'alerte (reduce, math, resample, classic condition)
45. Contact points : e-mail, Slack, webhook, Telegram
46. Notification policies : routage, regroupement, répétition
47. Silences et mute timings (plages de maintenance)
48. Bonnes pratiques d'alerting (ce qui évite la fatigue d'alerte)
49. Provisioning as code : le principe
50. Provisioning des datasources (YAML)
51. Provisioning des dashboards (YAML)
52. Provisioning de l'alerting (contact points, policies, règles)
53. Workflow Git pour le provisioning
54. Organisations, équipes et dossiers
55. Rôles : Viewer, Editor, Admin, Server Admin
56. RBAC fin (permissions granulaires)
57. Authentification : basique, LDAP, OAuth (aperçu)
58. Service accounts et tokens d'API
59. HTTPS en production via reverse proxy Nginx
60. Durcissement sécurité — checklist
61. Superviser Grafana lui-même (métriques internes)
62. Sauvegarde : base SQLite
63. Sauvegarde : base PostgreSQL
64. Sauvegarde : provisioning, plugins, stratégie 3-2-1
65. Restauration — procédure complète
66. Mise à jour de Grafana — procédure sans casse
67. Usage quotidien — routine de l'exploitant
68. Bonnes pratiques de dashboarding (lisibilité)
69. Exemple : dashboard supervision serveurs (node_exporter)
70. Exemple : dashboard supervision réseau
71. Exemple : dashboard supervision onduleur (lien métier)
72. Kiosque / TV murale (playlists, kiosk mode)
73. Dépannage : la datasource ne répond pas
74. Dépannage : les alertes ne partent pas
75. Dépannage : 10 cas concrets résolus
76. Les 10 erreurs classiques (et comment les éviter)
77. Cas pratiques commentés (4 scénarios d'exploitation)
78. Pense-bête de poche (commandes, requêtes, raccourcis)
79. Glossaire
80. Quiz — 10 questions + réponses
81. Pour aller plus loin
82. Annexe A — `grafana.ini` de référence commenté
83. Annexe B — checklist de mise en production

---

## 1. Introduction et périmètre du guide

Grafana est un outil open source de **visualisation et d'observabilité** : il ne collecte
pas les données lui-même, il les interroge là où elles vivent (Prometheus, Loki,
Zabbix, MySQL, InfluxDB…) et les présente sous forme de dashboards, avec un
moteur d'alerting unifié.

Ce guide couvre la chaîne complète, de l'installation sur Debian/Ubuntu jusqu'à
l'exploitation quotidienne en production :

- installer proprement depuis le dépôt officiel (pas de binaire téléchargé à la main) ;
- configurer `grafana.ini` de façon sûre et reproductible ;
- brancher les datasources les plus courantes en entreprise (Prometheus, Loki,
  Zabbix, MySQL/PostgreSQL, InfluxDB) ;
- construire des dashboards **lisibles** (panels, variables, transformations) ;
- mettre en place un **alerting fiable** (règles, contact points, routage, silences) ;
- gérer Grafana **as code** (provisioning YAML versionné dans Git) ;
- sécuriser (rôles, HTTPS, durcissement), sauvegarder, mettre à jour, dépanner.

**Ce que ce guide ne couvre pas :** le développement de plugins maison, Grafana
Cloud (offre SaaS), ni l'administration avancée des backends eux-mêmes
(cluster Prometheus Thanos/Mimir, cluster Loki) — ceux-ci sont mentionnés en
piste dans la section 81.

**Pré-requis :** un serveur Debian 12 ou Ubuntu 22.04/24.04, 2 vCPU / 4 Go RAM
minimum pour un usage d'équipe, un accès réseau vers vos datasources, et des
bases de Linux (systemd, apt, nginx).

---

## 2. Concepts : datasources

Une **datasource** est une connexion nommée vers un backend de données.
Grafana ne stocke pas vos métriques : chaque panel exécute une requête vers la
datasource au moment de l'affichage.

| Datasource | Type de données | Langage de requête | Usage typique |
|---|---|---|---|
| Prometheus | Métriques (séries temporelles) | PromQL | Supervision serveurs, conteneurs, applicatif |
| Loki | Logs | LogQL | Corrélation logs ↔ métriques |
| Zabbix (plugin) | Métriques + événements | Requêtes Zabbix API | Parcs déjà sous Zabbix |
| MySQL / PostgreSQL | Tables relationnelles | SQL | Métriques métier (commandes, tickets, SLA) |
| InfluxDB | Métriques (séries temporelles) | InfluxQL / Flux | IoT, historique existant |
| Elasticsearch | Logs / documents | Lucene / PPL | Logs centralisés existants |
| TestData | Données factices | — | Maquettes, formation |

Points clés à retenir :

- **Une datasource = un backend + des droits.** Grafana interroge le backend avec
  les identifiants configurés dans la datasource, pas avec ceux de l'utilisateur
  connecté (sauf configuration spécifique).
- **Le mode d'accès** : `Server` (le backend Grafana interroge la datasource —
  recommandé, les secrets restent côté serveur) ou `Browser` (le navigateur de
  l'utilisateur interroge directement — à éviter sauf cas particulier, car cela
  expose l'URL et potentiellement les identifiants).
- **La datasource par défaut** (`default: true`) est pré-sélectionnée dans les
  nouveaux panels.
- Une datasource peut être **provisionnée** (définie en YAML, versionnée dans
  Git) : c'est la pratique recommandée en production (voir sections 49-50).

---

## 3. Concepts : dashboards, panels, variables

- **Dashboard** : une page composée de panels. Il répond idéalement à **une
  seule question** (« l'état de santé de mes serveurs ce matin ? »).
- **Panel** : un bloc de visualisation (courbe, jauge, tableau…). Chaque panel
  exécute une ou plusieurs requêtes et affiche le résultat.
- **Row** (ligne) : regroupement visuel de panels, repliable.
- **Variable (template)** : un paramètre du dashboard (`$serveur`, `$datacenter`).
  L'utilisateur choisit des valeurs en haut du dashboard, les requêtes des
  panels s'adaptent. C'est ce qui rend un dashboard **réutilisable** au lieu
  d'en cloner un par serveur.
- **Annotation** : un événement ponctuel (déploiement, incident) affiché comme
  une ligne verticale sur les graphes.
- **Playlist** : une rotation automatique de dashboards (TV murale).
- **Snapshot** : une copie figée d'un dashboard (données incluses), partageable
  sans accès aux datasources.

Cycle de vie conseillé : dashboard construit à la main → exporté en JSON →
versionné en Git → **provisionné** automatiquement au démarrage de Grafana.

---

## 4. Concepts : alerting unifié

Depuis Grafana 9, l'**alerting unifié** remplace l'ancien système d'alertes par
panel. Vocabulaire :

- **Règle d'alerte (alert rule)** : une ou plusieurs requêtes + des expressions
  (reduce, math) + une condition. Évaluée périodiquement (ex. toutes les minutes).
- **Instance d'alerte** : une alerte concrète produite par la règle (ex. « CPU >
  90 % sur srv-web-03 »). Une règle peut produire N instances (une par série).
- **États** : `Normal` → `Pending` (la condition est vraie depuis moins que la
  durée `for`) → `Firing` (alerte déclenchée) → `Resolved`.
- **Contact point** : la destination d'une notification (e-mail, Slack, webhook…).
- **Notification policy** : l'arbre de routage qui décide, selon les labels,
  quel contact point reçoit quoi, avec quel regroupement (`group_by`) et quels
  délais (`group_wait`, `group_interval`, `repeat_interval`).
- **Silence** : met en sourdine des notifications correspondant à des labels,
  pour une durée donnée (maintenance planifiée).
- **Mute timing** : plage récurrente de silence (ex. « pas de notifications
  non critiques le week-end »).

L'alerting unifié gère aussi les alertes **externes** (ex. Alertmanager
Prometheus) et les règles **enregistrées dans les datasources compatibles**
(Prometheus, Loki, Mimir) : Grafana peut les afficher et les router sans les
réévaluer lui-même.

---

## 5. Architecture de Grafana

```
┌─────────────┐      HTTP(S)      ┌──────────────┐      ┌────────────────┐
│  Navigateur │ ◄──────────────► │    Grafana   │ ───► │  Datasources   │
│ (frontend)  │                  │   (backend   │      │ Prometheus,    │
└─────────────┘                  │    Go + API) │ ◄─── │ Loki, Zabbix,  │
                                 └──────┬───────┘      │ SQL, InfluxDB… │
                                        │ SQLite /     └────────────────┘
                                        │ Postgres
                                 ┌──────▼───────┐
                                 │ Base interne │
                                 │ (dashboards, │
                                 │ users, alert │
                                 │  rules…)     │
                                 └──────────────┘
```

- **Backend Go** : sert l'API HTTP, exécute les requêtes datasource en mode
  `Server`, évalue les règles d'alerte, envoie les notifications.
- **Frontend** : application web (React) affichée dans le navigateur.
- **Base interne** (SQLite par défaut, PostgreSQL/MySQL recommandé en
  production) : stocke dashboards, utilisateurs, permissions, règles d'alerte,
  historique. **Ce n'est pas là que vivent vos métriques.**
- **Provisioning** : au démarrage, Grafana lit `/etc/grafana/provisioning/` et
  crée/met à jour datasources, dashboards, contact points, etc.

Conséquence pratique : **sauvegarder Grafana = sauvegarder sa base interne +
le dossier provisioning** (sections 62-64). Les métriques elles, se
sauvegardent côté backends (Prometheus, Loki…).

---

## 6. Installation sur Debian/Ubuntu (dépôt officiel)

On installe depuis le **dépôt APT officiel** de Grafana : mises à jour via
`apt upgrade`, signatures vérifiées, pas de binaire téléchargé à la main.

```bash
# 1. Pré-requis
sudo apt-get install -y apt-transport-https software-properties-common wget gnupg

# 2. Clé GPG officielle Grafana
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | \
  sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null

# 3. Dépôt stable
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | \
  sudo tee /etc/apt/sources.list.d/grafana.list

# 4. Installation
sudo apt-get update
sudo apt-get install -y grafana

# 5. Service systemd : démarrage auto + lancement
sudo systemctl daemon-reload
sudo systemctl enable --now grafana-server

# 6. Vérification
systemctl status grafana-server --no-pager
ss -ltnp | grep 3000
curl -s http://localhost:3000/login | head -c 200; echo
```

Fichiers installés par le paquet :

| Chemin | Rôle |
|---|---|
| `/usr/sbin/grafana-server` | Binaire |
| `/etc/grafana/grafana.ini` | Configuration (à personnaliser) |
| `/etc/grafana/provisioning/` | Provisioning as code |
| `/var/lib/grafana/` | Base SQLite + plugins + données |
| `/var/log/grafana/grafana.log` | Journal applicatif |
| `/usr/share/grafana/` | Frontend statique |

> **Note :** le paquet crée l'utilisateur système `grafana` (sans shell).
> Ne lancez jamais Grafana en root.

Vérifier la version installée :

```bash
grafana-server -v
# ou via l'API (après premier login) :
curl -s http://admin:<A_COMPLETER>@localhost:3000/api/health | python3 -m json.tool
```

---

## 7. Installation via Docker (aperçu)

Utile pour un test ou un labo. En production d'équipe, préférez le paquet APT
(intégration systemd, chemins standards, sauvegardes simples).

```bash
docker volume create grafana-data

docker run -d --name grafana \
  --restart unless-stopped \
  -p 3000:3000 \
  -e GF_SECURITY_ADMIN_USER=admin \
  -e GF_SECURITY_ADMIN_PASSWORD='<A_COMPLETER>' \
  -e GF_INSTALL_PLUGINS=grafana-clock-panel \
  -v grafana-data:/var/lib/grafana \
  grafana/grafana:11.5.2
```

Points d'attention Docker :

- Les variables d'environnement `GF_<SECTION>_<CLE>` surchargent `grafana.ini`
  (ex. `GF_SERVER_HTTP_PORT`, `GF_DATABASE_PATH`). Pratique, mais documentez-les.
- Sans volume, **toute la configuration est perdue** à la suppression du
  conteneur.
- Épinglez un tag de version précis (`:11.5.2`), jamais `:latest` en production.

---

## 8. Premier lancement et première connexion

1. Ouvrez `http://<serveur>:3000` dans un navigateur.
2. Identifiants initiaux : `admin` / `admin`.
3. Grafana **impose** le changement du mot de passe admin à la première
   connexion. Choisissez un mot de passe fort, stocké dans votre coffre.
4. L'assistant de bienvenue propose d'ajouter une datasource : cliquez
   **« Add your first data source »** et choisissez (ex. Prometheus).

Premiers réglages immédiats (roue dentée → Administration) :

- **General → Default preferences** : thème (Dark/Light), fuseau horaire
  (`Europe/Paris` si vos équipes sont en France — voir section 10).
- **Users** : créez les comptes nominatifs, ne partagez pas le compte `admin`.
- **Authentication** : désactivez l'inscription publique si elle est active
  (`allow_sign_up = false`, voir section 13).

Vérification santé via l'API :

```bash
curl -s http://localhost:3000/api/health | python3 -m json.tool
# {"commit":"...","database":"ok","version":"11.5.2"}
```

Si `database` vaut autre chose que `ok`, regardez `/var/log/grafana/grafana.log`
avant toute chose.

---

## 9. Tour d'horizon de l'interface

Barre latérale gauche (icônes) :

| Icône / Menu | Contenu |
|---|---|
| **Dashboards** | Parcourir, créer, importer, playlists |
| **Explore** | Requêtage ad hoc (PromQL/LogQL/SQL) sans créer de dashboard |
| **Alerting** | Règles, contact points, policies, silences |
| **Connections → Data sources** | Ajouter/configurer/tester les datasources |
| **Administration → Users / Teams** | Comptes, équipes, organisations |
| **Administration → Settings** | Lecture seule de la config effective |

Le menu **Explore** mérite d'être connu : c'est l'établi du sysadmin.
On y teste une requête en 10 secondes, avec complétion, avant de l'intégrer
dans un panel. Réflexe : *Explore d'abord, dashboard ensuite.*

Raccourcis utiles : `/` ou `Ctrl+K` pour la recherche globale de dashboards.
Le sélecteur de plage de temps (en haut à droite) s'applique à tout le
dashboard ; apprenez `Ctrl+Z`… non, il n'y a pas d'annulation globale :
**pensez à sauvegarder** (icône disquette ou `Ctrl+S` dans l'éditeur).

---

## 10. Configuration `grafana.ini` — serveur et chemins

Le fichier `/etc/grafana/grafana.ini` est en format INI (`[section]` puis
`clé = valeur`). Toute ligne commençant par `;` est commentée. **Après chaque
modification : `systemctl restart grafana-server`.**

```ini
[server]
# Protocole et port d'écoute
protocol = http
http_port = 3000

# Adresse d'écoute : localhost si un reverse proxy est devant,
# 0.0.0.0 si Grafana est exposé directement (alors mettez HTTPS, section 14)
http_addr = 127.0.0.1

# URL publique vue par les navigateurs (utilisée dans les liens des alertes !)
# INDISPENSABLE : sans elle, les liens "voir l'alerte" pointent vers localhost
root_url = https://grafana.mondomaine.fr/

# Domaine utilisé pour les cookies (à ajuster si sous-domaine)
;domain = grafana.mondomaine.fr

[paths]
# Rarement à toucher avec le paquet APT
data = /var/lib/grafana
logs = /var/log/grafana
plugins = /var/lib/grafana/plugins
provisioning = /etc/grafana/provisioning

[users]
# Page d'accueil par défaut après login
default_theme = dark
```

> **Piège classique :** `root_url` mal renseignée = liens cassés dans les
> e-mails d'alerte et redirections OAuth en échec. C'est le réglage n°1 à
> vérifier quand « les liens des alertes ne marchent pas ».

Fuseau horaire par défaut des dashboards (pratique pour une équipe en France) :

```ini
[date_formats]
# Laisser défaut sauf besoin spécifique

[dashboards]
# Fuseau par défaut proposé à la création (l'utilisateur peut le changer)
default_timezone = Europe/Paris
```

---

## 11. Configuration — base de données interne

Par défaut Grafana utilise **SQLite** (`/var/lib/grafana/grafana.db`).
C'est suffisant pour : une petite équipe, un seul nœud, < 50 dashboards.

Passez à **PostgreSQL** (ou MySQL) quand : plusieurs dizaines d'utilisateurs,
alerting intensif, besoin de sauvegarde à chaud fiable, ou haute
disponibilité en perspective.

```ini
[database]
# SQLite (défaut) :
#path = /var/lib/grafana/grafana.db

# PostgreSQL (recommandé en production) :
path =
host = 127.0.0.1:5432
name = grafana
user = grafana
password = '<A_COMPLETER>'
ssl_mode = require
# Options utiles :
#max_open_conn = 10
#max_idle_conn = 2
#conn_max_lifetime = 14400

# MySQL/MariaDB (alternative) :
#host = 127.0.0.1:3306
#name = grafana
#user = grafana
#password = '<A_COMPLETER>'
```

Création de la base côté PostgreSQL :

```sql
CREATE USER grafana WITH PASSWORD '<A_COMPLETER>';
CREATE DATABASE grafana OWNER grafana;
```

Grafana crée et migre le schéma **tout seul** au démarrage (`migrations`).
Ne touchez jamais aux tables à la main.

**Migration SQLite → PostgreSQL :** il n'existe pas d'outil officiel
automatique. Procédure fiable : exporter les dashboards en JSON (ou via le
provisioning), basculer la config, réimporter. D'où l'intérêt de gérer les
dashboards **as code dès le départ** (section 49).

---

## 12. Configuration — journalisation

```ini
[log]
# Niveaux : debug, info, warn, error, critical
mode = console file
level = info

[log.file]
log_rotate = true
max_lines = 1000000
max_size_shift = 28        # 256 Mo par fichier (2^28)
daily_rotate = true
max_days = 7               # rétention 7 jours
```

En dépannage, passez temporairement en `debug` :

```bash
# Sans redémarrer : via l'API admin (pratique !)
curl -s -X POST -H "Content-Type: application/json" \
  -d '{"level":"debug"}' \
  http://admin:<A_COMPLETER>@localhost:3000/api/admin/log/level
# Revenir en info ensuite :
curl -s -X POST -H "Content-Type: application/json" \
  -d '{"level":"info"}' \
  http://admin:<A_COMPLETER>@localhost:3000/api/admin/log/level
```

Les logs contiennent aussi les **requêtes datasource en échec** et les
**évaluations d'alertes** : c'est le premier endroit à regarder quand
« ça ne marche plus » (voir section 73-75).

> Si vous centralisez vos logs (Loki, rsyslog), ajoutez `/var/log/grafana/*.log`
> à votre collecte : superviser l'outil de supervision, c'est la section 61.

---

## 13. Configuration — sécurité applicative (secret, cookies, sessions)

```ini
[security]
# Identifiants admin créés au premier démarrage si la base est vide.
# Mettez un mot de passe fort ; Grafana force de toute façon son changement.
admin_user = admin
admin_password = '<A_COMPLETER>'

# INTERDIT en production : l'auto-inscription crée des comptes Viewer publics
allow_sign_up = false

# Clé de chiffrement des secrets (datasources, tokens). CRITIQUE :
# - générez-la une fois, sauvegardez-la avec la base (section 62-64)
# - si vous la perdez/changez, les secrets provisionnés deviennent illisibles
secret_key = '<A_COMPLETER_32_CARACTERES_ALEATOIRES>'

# Durée de vie des sessions de login
login_maximum_inactive_lifetime_duration = 7d
login_maximum_lifetime_duration = 30d
token_rotation_interval_minutes = 10

# Désactiver la page de login basique si vous utilisez uniquement OAuth/LDAP
;disable_login_form = true

# Masquer la version dans les en-têtes et l'API (hygiène de base)
;hide_version = true

[auth]
# Interdire le login anonyme sauf besoin kiosque (voir section 72)
disable_login_form = false

[auth.anonymous]
enabled = false
# Si activé pour un écran mural : limitez STRICTEMENT l'organisation et le rôle
#enabled = true
#org_name = Kiosque
#org_role = Viewer
```

Générer une `secret_key` robuste :

```bash
python3 -c "import secrets; print(secrets.token_hex(32))"
```

Cookies (à durcir si HTTPS, voir sections 14 et 59) :

```ini
[security]
cookie_secure = true      # cookie transmis uniquement en HTTPS
cookie_samesite = lax     # 'lax' par défaut ; 'strict' si pas d'iframe
```

---

## 14. Configuration — HTTPS/TLS natif

Deux options : TLS natif dans Grafana, ou TLS terminé par un reverse proxy
(Nginx). **En production, préférez le reverse proxy** (section 59) : gestion
centralisée des certificats, Let's Encrypt, en-têtes de sécurité.

TLS natif (utile en labo ou sans proxy) :

```ini
[server]
protocol = https
http_port = 3000
cert_file = /etc/grafana/cert.pem
cert_key = /etc/grafana/key.pem
# Pour forcer TLS 1.2 minimum :
;min_tls_version = "1.2"
```

```bash
# Permissions : le binaire tourne en tant que 'grafana'
sudo chown root:grafana /etc/grafana/key.pem
sudo chmod 640 /etc/grafana/key.pem
sudo systemctl restart grafana-server
```

Vérification :

```bash
curl -vk https://localhost:3000/login 2>&1 | grep -E "SSL|subject|issuer"
```

---

## 15. Configuration — SMTP (notifications e-mail)

Indispensable pour les alertes e-mail et les invitations d'utilisateurs.

```ini
[smtp]
enabled = true
host = smtp.mondomaine.fr:587
user = grafana@mondomaine.fr
password = '<A_COMPLETER>'
from_address = grafana@mondomaine.fr
from_name = Grafana Supervision
# Si votre relais exige STARTTLS (cas général sur le port 587) :
startTLS_policy = MandatoryStartTLS
#skip_verify = false   # ne jamais passer à true en production
```

Test depuis l'interface : **Alerting → Contact points → votre contact point
e-mail → Test** (envoie un e-mail de test immédiat).

Test en ligne de commande (diagnostic hors Grafana) :

```bash
# Vérifier que le relais répond
nc -vz smtp.mondomaine.fr 587
# Dialogue SMTP manuel minimal
openssl s_client -starttls smtp -connect smtp.mondomaine.fr:587
```

> **Erreur fréquente :** `Failed to send notification: dial tcp: i/o timeout`.
> Dans 90 % des cas c'est un pare-feu sortant (port 25/587 bloqué) ou un
> `host` mal saisi, pas un problème Grafana. Testez avec `nc`/`openssl`
> **depuis le serveur Grafana** avant d'accuser la configuration.

---

## 16. Datasource Prometheus — ajout et réglages

**Connections → Data sources → Add data source → Prometheus.**

Réglages essentiels :

| Champ | Valeur conseillée | Commentaire |
|---|---|---|
| Name | `Prometheus` (ou `Prom-Prod`) | Nom affiché dans les panels |
| URL | `http://prometheus.mondomaine.fr:9090` | URL vue **depuis le serveur Grafana** (mode Server) |
| Access | `Server (default)` | Les secrets restent côté serveur |
| Default | coché | Datasource pré-sélectionnée |
| Scrape interval | `15s` | Doit correspondre au `scrape_interval` de Prometheus (sert aux calculs `$__rate_interval`) |
| Query timeout | `60s` | À augmenter si requêtes lourdes |
| HTTP method | `POST` | Recommandé (URL moins longues, cache) |

Authentification si Prometheus est protégé :

- **Basic auth** : cocher, saisir user/password (stockés chiffrés avec `secret_key`).
- **TLS / CA perso** : coller le certificat CA dans « TLS/SSL Auth Details ».
- **Custom HTTP headers** : ex. `Authorization: Bearer <A_COMPLETER>` pour un
  reverse proxy avec token.

Bouton **Save & test** : Grafana exécute `GET /api/v1/status/buildinfo`.
« Successfully queried the Prometheus API » = la connexion fonctionne.

En provisioning YAML (voir section 50) :

```yaml
apiVersion: 1
datasources:
  - name: Prometheus
    type: prometheus
    access: proxy
    url: http://prometheus.mondomaine.fr:9090
    isDefault: true
    jsonData:
      httpMethod: POST
      timeInterval: 15s
      queryTimeout: 60s
```

---

## 17. PromQL de base pour vos dashboards

Dans un panel Time series, datasource Prometheus, onglet **Code** (pas Builder)
dès que vous êtes à l'aise : le code est versionnable et copiable.

```promql
# CPU : pourcentage d'utilisation moyen par instance (5 min)
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

# Mémoire : % utilisé par instance
100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes))

# Charge système (load1)
node_load1

# Espace disque : % utilisé par point de montage (hors tmpfs)
100 * (1 - (node_filesystem_avail_bytes{fstype!~"tmpfs|overlay"}
            / node_filesystem_size_bytes{fstype!~"tmpfs|overlay"}))

# Réseau : débit entrant par interface (bits/s)
rate(node_network_receive_bytes_total{device!~"lo|veth.*|docker.*"}[5m]) * 8

# Uptime d'un nœud (secondes)
time() - node_boot_time_seconds

# Nombre d'instances UP vues par Prometheus
count(up == 1)

# Disponibilité d'une cible en % sur 1h
100 * avg_over_time(up[1h])
```

Astuces d'affichage :

- **Legend** : `{{instance}}` ou `{{device}}` pour des légendes lisibles
  (sinon vous aurez la série brute illisible).
- **Min step** : laissez `$__interval` par défaut ; augmentez sur les longues
  plages pour alléger Prometheus.
- Utilisez **`$__rate_interval`** dans `rate(...[$__rate_interval])` : Grafana
  calcule l'intervalle optimal selon la plage affichée et le scrape interval.

---

## 18. PromQL avancé (rate, histogrammes, label_replace)

```promql
# --- Quantile de latence HTTP (histogramme) ---
# p95 de la durée des requêtes par handler
histogram_quantile(0.95,
  sum by (handler, le) (rate(http_request_duration_seconds_bucket[5m]))
)

# --- Taux d'erreur HTTP 5xx en % ---
100 * sum(rate(http_requests_total{code=~"5.."}[5m]))
    / sum(rate(http_requests_total[5m]))

# --- Top 5 des consommateurs CPU (par job) ---
topk(5, sum by (job) (rate(process_cpu_seconds_total[5m])))

# --- Prédiction de saturation disque à 7 jours (régression linéaire) ---
# Nombre de jours avant saturation du /var sur chaque instance
(node_filesystem_avail_bytes{mountpoint="/var",fstype!="tmpfs"}
  / deriv(node_filesystem_size_bytes{mountpoint="/var"}[1h]) ) / 86400
# Variante simple : alerte si < 10 % libre ET décroît
(node_filesystem_avail_bytes / node_filesystem_size_bytes) < 0.1
  and deriv(node_filesystem_avail_bytes[1h]) < 0

# --- Renommer / nettoyer un label ---
label_replace(up, "serveur", "$1", "instance", "([^:]+):.*")

# --- Joindre une info (ex. ajouter le datacenter depuis un autre metric) ---
node_cpu_seconds_total * on(instance) group_left(dc) node_dc_info

# --- Compter les redémarrages de conteneurs (increase sur 1h) ---
increase(kube_pod_container_status_restarts_total[1h]) > 3

# --- Exclure proprement du bruit ---
sum by (instance) (rate(node_cpu_seconds_total{mode!="idle"}[5m]))
```

> **Règle d'or PromQL :** `rate()` s'applique **toujours** à un compteur
> (`_total`). Sur une gauge (température, niveau), utilisez `avg_over_time`,
> `max_over_time`, `deriv`. Appliquer `rate()` à une gauge = chiffres absurdes.

---

## 19. Datasource Loki — ajout et réglages

**Connections → Data sources → Add data source → Loki.**

| Champ | Valeur conseillée |
|---|---|
| Name | `Loki` |
| URL | `http://loki.mondomaine.fr:3100` |
| Access | `Server (default)` |
| Default | selon votre usage |

Options spécifiques Loki :

- **Maximum lines** : `1000` (limite de lignes retournées par requête).
- **Derived fields** : extrayez un `traceID` des logs pour sauter vers Tempo
  (si vous avez du tracing) — optionnel.
- **Alerting** : si vous voulez des règles d'alerte Loki gérées par Grafana,
  activez « Manage alerts via Alerting UI » (nécessite le ruler Loki configuré).

**Save & test** : Grafana interroge `/loki/api/v1/labels`.

Provisioning YAML :

```yaml
apiVersion: 1
datasources:
  - name: Loki
    type: loki
    access: proxy
    url: http://loki.mondomaine.fr:3100
    jsonData:
      maxLines: 1000
```

> **Corrélation Loki ↔ Prometheus :** dans la datasource Loki, renseignez le
> champ **Derived fields** ou utilisez les **exemplars** côté Prometheus pour
> passer d'une courbe à « voir les logs de cette instance à ce moment » en un
> clic (data link, section 41).

---

## 20. LogQL de base

LogQL ressemble à PromQL mais s'applique aux **flux de logs** (sélectionnés par
labels, ex. `{job="syslog"}`).

```logql
# Tous les logs d'un job sur la plage affichée
{job="syslog"}

# Filtrer : lignes contenant "error" (insensible à la casse avec (?i))
{job="syslog"} |= "error"
{job="syslog"} |~ "(?i)error|fail|timeout"

# Exclure : tout sauf le bruit
{job="syslog"} != "DEBUG" |!= "healthcheck"

# Plusieurs labels
{job="nginx", instance="srv-web-03"} |= "500"

# Compter les lignes d'erreur par minute (métrique issue des logs !)
count_over_time({job="syslog"} |= "error" [1m])

# Taux d'erreurs nginx par seconde
sum by (instance) (rate({job="nginx"} |= " 500 " [5m]))
```

Dans Grafana, le panel **Logs** affiche les lignes avec coloration par niveau
(`level=error` détecté automatiquement si présent). Les labels extraits sont
cliquables pour filtrer.

**Bon réflexe :** commencez large (`{job="syslog"}`), ajoutez les filtres
`|=` un par un, puis transformez en métrique (`count_over_time`, `rate`) pour
les dashboards et les alertes.

---

## 21. LogQL avancé (parsing, filtres, métriques issues des logs)

```logql
# --- Parser des logs JSON ---
{job="app"} | json | level="error" | line_format "{{.msg}} (user={{.user}})"
# Compter les erreurs par utilisateur
sum by (user) (count_over_time({job="app"} | json | level="error" [5m]))

# --- Parser des logs syslog/nginx (regexp nommée) ---
{job="nginx"}
  | regexp `(?P<ip>\\S+) - - \\[.*?\\] "(?P<method>\\S+) (?P<path>\\S+).*?" (?P<code>\\d+)`
  | code="500"
  | line_format "{{.ip}} -> {{.method}} {{.path}}"

# --- Extraire un nombre et en faire une métrique ---
{job="app"} | json | unwrap duration_ms | __error__=""
# p95 de la durée par endpoint :
quantile_over_time(0.95, {job="app"} | json | unwrap duration_ms [5m]) by (endpoint)

# --- Détecter un pattern d'attaque (brute force SSH) ---
sum by (instance) (
  count_over_time({job="sshd"} |= "Failed password" [5m])
) > 10

# --- Logs d'un conteneur précis sur les dernières 24h, triés ---
{container="api"} | logfmt | level="error"
```

> **Performance :** les requêtes LogQL scannent les chunks. Limitez la plage
> de temps, soyez précis sur les labels (`{job=...}`), évitez `|~` en début de
> pipeline sur de gros volumes. Pour des alertes fréquentes, préférez des
> **règles d'enregistrement** côté Loki (ruler) plutôt qu'une requête ad hoc
> évaluée chaque minute.

---

## 22. Datasource Zabbix (plugin)

Le plugin **Zabbix** (`alexanderzobnin-zabbix-app`) n'est pas inclus par défaut :
il s'installe depuis le catalogue.

```bash
# Installation du plugin (redémarrage nécessaire)
sudo grafana-cli plugins install alexanderzobnin-zabbix-app
sudo systemctl restart grafana-server

# Vérification
sudo grafana-cli plugins ls | grep zabbix
```

Configuration de la datasource :

| Champ | Valeur |
|---|---|
| Name | `Zabbix` |
| URL | `http://zabbix.mondomaine.fr/api_jsonrpc.php` |
| Access | `Server (default)` |
| Username / Password | compte API Zabbix dédié `grafana` (rôle lecture seule) |
| Trends | coché (utilise les trends pour les longues plages — **beaucoup** plus rapide) |
| Cache | `1h` pour les longues plages |

Bonnes pratiques Zabbix + Grafana :

- Créez un **utilisateur Zabbix dédié** avec uniquement les droits de lecture
  sur les groupes d'hôtes concernés. Jamais le compte Admin.
- Activez **Trends** : sans ça, un dashboard sur 30 jours scanne l'historique
  brut et met Zabbix à genoux.
- Les **variables** fonctionnent avec les groupes d'hôtes / hôtes / items
  Zabbix (`$groupe`, `$hote`) : requêtes de type « Zabbix hosts in group ».
- **Problèmes Zabbix → Grafana** : le panel natif du plugin affiche les
  triggers en cours ; pour l'alerting unifié, préférez recréer des règles
  Grafana sur les métriques plutôt que d'importer les triggers.

---

## 23. Datasource MySQL — métriques métier

C'est ici que Grafana devient un outil **métier** : chiffre d'affaires,
tickets ouverts, SLA, taux de disponibilité applicative — en SQL direct.

**Connections → Data sources → Add data source → MySQL.**

| Champ | Valeur |
|---|---|
| Host | `bdd.mondomaine.fr:3306` |
| Database | `metier` |
| User / Password | compte lecture seule `grafana_ro` |
| Max open connections | `10` |
| Max idle | `2` |

Créez le compte en lecture seule côté MySQL :

```sql
CREATE USER 'grafana_ro'@'%' IDENTIFIED BY '<A_COMPLETER>';
GRANT SELECT, SHOW VIEW ON metier.* TO 'grafana_ro'@'%';
FLUSH PRIVILEGES;
```

Exemples de requêtes (macros Grafana `$__timeFilter`, `$__timeGroup`) :

```sql
-- Commandes par jour sur la plage du dashboard
SELECT
  $__timeGroup(created_at, '1d') AS "time",
  COUNT(*) AS "Commandes",
  SUM(total_ht) AS "CA HT"
FROM commandes
WHERE $__timeFilter(created_at)
GROUP BY 1
ORDER BY 1;

-- Tickets ouverts par priorité (panel Table ou Bar chart)
SELECT
  priorite AS "Priorite",
  COUNT(*) AS "Tickets ouverts"
FROM tickets
WHERE statut NOT IN ('clos','resolu')
GROUP BY priorite;

-- Taux de disponibilité mensuel (panel Stat)
SELECT
  ROUND(100 * SUM(CASE WHEN statut='OK' THEN 1 ELSE 0 END) / COUNT(*), 2)
  AS "Dispo %"
FROM sondes_dispo
WHERE $__timeFilter(horodatage);
```

> **Sécurité :** le compte SQL de Grafana doit être **lecture seule**, sur les
> seules bases nécessaires. Grafana n'a aucune raison d'écrire dans votre base
> métier.

---

## 24. Datasource PostgreSQL — métriques métier

Même principe que MySQL, avec le dialecte PostgreSQL.

| Champ | Valeur |
|---|---|
| Host | `bdd.mondomaine.fr:5432` |
| Database | `metier` |
| User / Password | `grafana_ro` (lecture seule) |
| SSL Mode | `require` (ou `verify-full` avec CA) |
| Version | `14+` (active les bonnes macros) |

```sql
-- Rôle lecture seule côté PostgreSQL
CREATE USER grafana_ro WITH PASSWORD '<A_COMPLETER>';
GRANT CONNECT ON DATABASE metier TO grafana_ro;
GRANT USAGE ON SCHEMA public TO grafana_ro;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO grafana_ro;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO grafana_ro;
```

Exemples :

```sql
-- Latence médiane d'API par endpoint (Time series)
SELECT
  $__timeGroup(horodatage, '5m') AS "time",
  endpoint AS metric,
  percentile_cont(0.5) WITHIN GROUP (ORDER BY duree_ms) AS "p50 ms"
FROM api_logs
WHERE $__timeFilter(horodatage)
GROUP BY 1, 2
ORDER BY 1;

-- File d'attente : jobs en échec dernières 24h (Stat)
SELECT COUNT(*) AS "Jobs en echec"
FROM jobs
WHERE statut = 'failed'
  AND horodatage > NOW() - INTERVAL '24 hours';

-- Heatmap des connexions par heure et par jour (Heatmap)
SELECT
  $__timeGroup(horodatage, '1h') AS "time",
  EXTRACT(DOW FROM horodatage)::int AS "jour_semaine",
  COUNT(*) AS "connexions"
FROM sessions
WHERE $__timeFilter(horodatage)
GROUP BY 1, 2;
```

> **Astuce :** la colonne temporelle doit s'appeler `time` (ou être la
> première colonne de type timestamp). Les autres colonnes deviennent des
> séries (mode « Time series ») ou des champs (mode « Table ») selon le
> **Format** choisi dans le panel : `Time series` ou `Table`.

---

## 25. Datasource InfluxDB (introduction)

InfluxDB reste présent dans beaucoup d'entreprises (historique, IoT, sondes
maison). Deux dialectes selon la version :

| Version | Langage | Réglage Grafana |
|---|---|---|
| InfluxDB 1.x | InfluxQL (proche SQL) | Query language = `InfluxQL` |
| InfluxDB 2.x / 3.x | Flux | Query language = `Flux` |

Configuration (InfluxDB 2.x) :

| Champ | Valeur |
|---|---|
| URL | `http://influxdb.mondomaine.fr:8086` |
| Organization | `mon-org` |
| Token | `<A_COMPLETER>` (token lecture seule sur le bucket) |
| Default Bucket | `supervision` |

Exemples :

```flux
// Flux : température moyenne par sonde sur 1h
from(bucket: "supervision")
  |> range(start: v.timeRangeStart, stop: v.timeRangeStop)
  |> filter(fn: (r) => r._measurement == "temperature" and r._field == "value")
  |> aggregateWindow(every: 1h, fn: mean)
```

```sql
-- InfluxQL : même idée
SELECT mean("value") FROM "temperature"
WHERE $timeFilter GROUP BY time(1h), "sonde"
```

> **Conseil de migration :** si vous démarrez un nouveau projet métriques,
> préférez **Prometheus** (écosystème, PromQL, alerting unifié). Gardez
> InfluxDB pour l'existant et l'IoT léger.

---

## 26. Datasources : tests et bonnes pratiques

Checklist à appliquer à **chaque** datasource :

- [ ] Mode d'accès `Server` (sauf besoin explicite du mode Browser, documenté).
- [ ] **Save & test** au vert, depuis le serveur Grafana (pas depuis votre PC).
- [ ] `Scrape interval` / `timeInterval` cohérent avec la réalité du backend.
- [ ] Compte dédié, droits minimaux (lecture seule partout où c'est possible).
- [ ] Secrets chiffrés (provisioning : via variables d'environnement ou
      gestionnaire de secrets, jamais en clair dans Git — section 53).
- [ ] Timeout adapté (60 s par défaut ; 120 s pour les requêtes SQL lourdes).
- [ ] Une seule datasource **par défaut** ; nommez les autres explicitement
      (`Prom-Prod`, `Prom-Lab`, `Loki-Prod`).
- [ ] **Health check** : le panel « datasource qui ne répond pas » (section 73)
      décrit quoi faire quand le test échoue.

Diagnostic réseau depuis le serveur Grafana (à faire **avant** d'accuser Grafana) :

```bash
# Le backend est-il joignable depuis le serveur Grafana ?
curl -s -o /dev/null -w "%{http_code}\n" http://prometheus.mondomaine.fr:9090/-/healthy
curl -s http://loki.mondomaine.fr:3100/ready
nc -vz bdd.mondomaine.fr 5432

# Résolution DNS vue par Grafana
getent hosts prometheus.mondomaine.fr
```

---

## 27. Panels : time series

Le panel **Time series** est le cheval de labour : 80 % de vos graphes.

Réglages qui font la différence :

- **Style → Line width / Fill opacity** : `1` et `10` pour des courbes
  lisibles ; `Fill opacity 0` + `Line width 2` pour comparer peu de séries.
- **Connect null values → Never** : un trou dans les données doit se **voir**
  (un collecteur en panne qui affiche une belle courbe continue, c'est un
  mensonge).
- **Show points → Never** (sauf séries clairsemées).
- **Gradient mode → Scheme** : joli mais trompeur sur les comparaisons ;
  réservez-le aux dashboards de présentation.
- **Stacking → Normal** : pour des composants qui s'additionnent
  (CPU user + system + iowait). **Jamais** de stacking sur des pourcentages
  indépendants.
- **Tooltip mode → All** : voir toutes les séries au survol.

Exemple : CPU par mode, stacké, avec légende propre :

```promql
sum by (mode) (rate(node_cpu_seconds_total{instance="$serveur"}[$__rate_interval]))
```

- Legend : `{{mode}}`
- Unit : `percent (0-100)` → non, ici ce sont des secondes/s : laissez
  `short`, ou convertissez en % avec la requête de la section 17.
- Seuils : `80` orange, `95` rouge (section 34).

---

## 28. Panels : gauge (jauge)

La **jauge** répond à « où en est-on par rapport à une limite ? ».
Un seul nombre, un arc de cercle, des seuils colorés.

Cas d'usage : % disque utilisé, température salle, charge onduleur,
niveau d'un bac, progression d'un quota.

Réglages :

- **Show threshold labels / markers** : coché (on voit où sont les seuils).
- **Orientation → Auto**, **Show unfilled area** : coché.
- **Value → Calculation → Last** (la valeur actuelle).
- **Min/Max** : renseignez-les **toujours** (une jauge sans max ment :
  75 % de quoi ?).
- **Thresholds** : ex. disque : `70` orange, `90` rouge. **Base color** verte.

Exemple (disque le plus plein du serveur sélectionné) :

```promql
100 * max(1 - (node_filesystem_avail_bytes{fstype!~"tmpfs|overlay",instance="$serveur"}
              / node_filesystem_size_bytes{fstype!~"tmpfs|overlay",instance="$serveur"}))
```

> **Anti-pattern :** 12 jauges identiques pour 12 serveurs. Utilisez une
> **variable** `$serveur` (section 36) ou un panel **Stat** répété
> (option Repeat, section 38).

---

## 29. Panels : stat (grandes valeurs)

Le panel **Stat** affiche un grand chiffre + un libellé + une mini-tendance
(sparkline). Idéal pour les indicateurs « d'un coup d'œil ».

Bandeau type en haut d'un dashboard serveurs :

| Panel | Requête | Calculation | Couleur |
|---|---|---|---|
| Serveurs UP | `count(up == 1)` | Last | verte si > 0 |
| Alertes firing | `sum(ALERTS{alertstate="firing"})` | Last | rouge si > 0 |
| CPU moyen | `100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[5m]))*100)` | Last | seuils 70/90 |
| Dispo 30 j | `100 * avg_over_time(up[30d])` | Last | seuils 99/99.9 |

Réglages :

- **Graph mode → Area** (sparkline derrière le chiffre).
- **Color mode → Background** pour les indicateurs critiques (toute la tuile
  devient rouge : visible à 5 mètres sur un écran mural).
- **Text mode → Value and name**.
- **Unit** : toujours renseignée (`percent`, `bytes`, `s`, `none`…).

---

## 30. Panels : table

Le panel **Table** affiche des données tabulaires, avec tri, filtre et
mise en forme par colonne.

Cas d'usage : inventaire (serveur, OS, version), top des filesystems,
tickets ouverts, dernières alertes.

Exemple : filesystems > 70 % d'utilisation :

```promql
100 * (1 - (node_filesystem_avail_bytes{fstype!~"tmpfs|overlay"}
            / node_filesystem_size_bytes{fstype!~"tmpfs|overlay"})) > 70
```

Mise en forme :

- **Column Settings** : renommez `Value` → `Utilisation %`, alignez à droite.
- **Cell options → Cell type → Colored background** + seuils 80/90 : la
  colonne devient un indicateur visuel.
- **Cell type → Data links** : lien vers le dashboard détaillé du serveur
  (`/d/<uid>/detail-serveur?var-serveur=${__data.fields.instance}`).
- Désactivez les colonnes parasites (`Time`) via **Organize fields**
  (transformation, section 39).

> **Table ou Time series ?** Si la question est « combien / qui / quoi
> maintenant » → Table. Si c'est « comment ça évolue » → Time series.

---

## 31. Panels : heatmap

La **heatmap** montre la densité d'une distribution dans le temps : chaque
colonne = un intervalle, chaque ligne = un « bucket », la couleur = le nombre
d'observations. Parfait pour les **latences** et les répartitions.

Source idéale : un histogramme Prometheus.

```promql
sum by (le) (rate(http_request_duration_seconds_bucket[5m]))
```

Réglages :

- **Calculate from histogram → Yes** (Grafana reconstruit les buckets depuis
  les séries `_bucket`).
- **Color scheme → Oranges/Reds** (ou `Spectrum`).
- **Y axis → Unit** : `s` (secondes).
- **Tooltip → Show histogram** : coché.

Lecture : une tache chaude qui monte = la latence se dégrade ; deux taches
distinctes = **distribution bimodale** (ex. cache hit vs cache miss) —
information qu'une simple moyenne ne montre jamais.

Autre usage : heatmap des connexions par heure (requête SQL, section 24) ou
des erreurs par code et par heure.

---

## 32. Panels : bar chart, pie chart, state timeline, status history

**Bar chart** (barres) : comparer des catégories à un instant T.

```promql
# Top 5 filesystems par utilisation
topk(5, 100 * (1 - (node_filesystem_avail_bytes / node_filesystem_size_bytes)))
```
- Orientation → Horizontal, tri décroissant : lecture immédiate.

**Pie chart** (camembert) : répartition d'un tout. À utiliser avec parcimonie
(5 parts max, sinon illisible). Ex. : répartition des OS du parc, des
tickets par statut.

**State timeline** : états discrets dans le temps (up/down, open/closed).
Idéal pour « l'historique de disponibilité des services » :

```promql
up{job="blackbox"}
```
- **Merge equal consecutive values** : coché. Couleurs : vert = 1, rouge = 0.

**Status history** : comme State timeline mais en blocs annotés (ex. statut
des jobs de sauvegarde par nuit : OK / WARNING / FAILED).

> **Règle :** un panel = un type de question. Ne mélangez pas des gauges, des
> courbes et des camemberts pour dire la même chose : choisissez la forme la
> plus directe.

---

## 33. Panels : text, dashboard list, alert list

**Text** (Markdown/HTML) : documentation **dans** le dashboard. Utilisez-le
pour :

- expliquer ce que montre le dashboard et qui le maintient ;
- documenter la procédure (« si cette alerte est rouge, faire… ») ;
- afficher les liens utiles (wiki, runbook).

```markdown
## Dashboard Supervision Serveurs
**Question :** mes serveurs sont-ils sains ?
**Mainteneur :** Équipe Systèmes — <A_COMPLETER>
**Runbook :** https://wiki.mondomaine.fr/runbooks/serveurs
```

**Dashboard list** : affiche les dashboards d'un dossier ou taggés — utile
pour une page d'accueil / un sommaire par équipe.

**Alert list** : liste les alertes en cours (firing/pending), filtrable par
dossier ou dashboard. Parfait en haut d'un dashboard « tour de contrôle ».

---

## 34. Options de panel : unités, min/max, seuils, couleurs

Ces réglages (onglet **Field** / **Standard options**) s'appliquent à presque
tous les panels : c'est 50 % de la lisibilité.

- **Unit** : `percent (0-100)` pour un %, `bytes (IEC)` pour du disque/RAM
  (affiche Go automatiquement), `s` / `ms` pour les durées, `ops` / `reqps`
  pour les débits. **Un graphe sans unité est un graphe ambigu.**
- **Min / Max** : fixez-les quand la borne a un sens (0-100 % pour un taux).
  Ne fixez pas de max arbitraire sur une métrique sans borne (trafic réseau).
- **Decimals** : 1 ou 2 suffisent. `99.87 %`, pas `99.8712345678 %`.
- **Thresholds** : mode `Absolute` (valeurs) ou `Percentage`. La **couleur de
  base** = l'état normal (souvent vert). Ajoutez les seuils dans l'ordre
  croissant.
- **Color scheme** : `Single color` (sobre, recommandé), `From thresholds`
  (la couleur suit les seuils), `Classic palette` (une couleur par série).

Convention d'équipe conseillée (à écrire dans votre wiki) :

| Couleur | Sens |
|---|---|
| Vert | OK / normal |
| Orange | Dégradé / warning (agir sous 4 h) |
| Rouge | Critique (agir maintenant) |
| Bleu | Information (pas d'action) |
| Gris | Pas de données / désactivé |

---

## 35. Overrides de champs (field overrides)

Les **overrides** permettent d'appliquer des réglages différents à **certaines
séries** d'un même panel, sans le dupliquer.

Exemple : sur un graphe « trafic réseau par interface », afficher `eth0` en
ligne épaisse et masquer les interfaces `veth*` :

1. Panel → onglet **Overrides** → **Add field override**.
2. **Fields with name matching regex** : `/^veth/` → **Hide in → Tooltip,
   Viz, Legend** (ou filtrez déjà dans la requête, c'est mieux).
3. **Fields with name** : `eth0` → **Graph styles → Line width = 3**.

Autres usages :

- Une série en **barres** au milieu de courbes (ex. déploiements) :
  override → **Graph styles → Draw mode = Bars**.
- Unités différentes par série : override → **Standard options → Unit**.
  (Mieux : deux axes Y via **Axis → Placement = Right**.)
- Renommer proprement : override → **Standard options → Display name**.

> **Priorité :** les réglages panel s'appliquent d'abord, puis les overrides
> dans l'ordre de la liste. En cas de conflit, le **dernier** override gagne.

---

## 36. Variables et templates — le principe

Une **variable** rend un dashboard réutilisable : au lieu de coder en dur
`instance="srv-web-03"`, on écrit `instance="$serveur"` et l'utilisateur
choisit le serveur dans un menu déroulant en haut du dashboard.

Cycle de vie d'une variable :

1. **Dashboard settings → Variables → New** : nom (`serveur`), type, requête.
2. Utilisation dans les requêtes : `$serveur` ou `${serveur}` (forme longue
   obligatoire si le nom est suivi de texte : `${serveur}_suffixe`).
3. Dans les titres de panels : `$serveur` fonctionne aussi.

Exemple minimal (Prometheus) :

- Nom : `serveur`
- Type : **Query**
- Data source : `Prometheus`
- Query : `label_values(node_uname_info, instance)`
- Refresh : **On Dashboard Load**
- Multi-value : oui, Include All option : oui

Requête du panel :

```promql
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle",instance=~"$serveur"}[5m])) * 100)
```

Notez `=~` (regex match) au lieu de `=` : indispensable quand la variable
peut valoir plusieurs instances ou `.*` (All).

> **Règle :** dès qu'un dashboard est décliné plus de 2 fois pour des cibles
> différentes, il doit utiliser des variables. Le copier-coller de dashboards
> est une dette qui se paie en maintenance.

---

## 37. Variables : les 7 types en détail

| Type | Usage | Exemple |
|---|---|---|
| **Query** | Liste dynamique depuis une datasource | `label_values(up, job)` |
| **Custom** | Liste fixe saisie à la main | `prod,staging,lab` |
| **Interval** | Pas de temps (`$pas`) | `1m,5m,15m,1h` |
| **Data source** | Choisir la datasource | Type `prometheus` → `Prom-Prod`, `Prom-Lab` |
| **Constant** | Valeur invisible figée | `seuil_critique = 90` |
| **Textbox** | Saisie libre | Rechercher un hostname |
| **Ad hoc filters** | Filtres ajoutés à la volée | (Prometheus/Loki/ES uniquement) |

Requêtes utiles par datasource (type Query) :

```promql
# Prometheus : valeurs d'un label, éventuellement filtrées par une autre variable
label_values(node_uname_info{datacenter="$dc"}, instance)
# Tous les jobs
label_values(up, job)
```

```logql
# Loki : valeurs d'un label
label_values({job="syslog"}, instance)
```

```sql
-- MySQL/PostgreSQL : liste depuis une table métier
SELECT DISTINCT datacenter FROM sites ORDER BY 1;
```

```yaml
# Zabbix (plugin) : groupes d'hôtes, hôtes, applications…
# Dans le champ Query du plugin : choisir "Groupes", "Hôtes", etc.
```

Options importantes :

- **Refresh** : `On Dashboard Load` (défaut raisonnable) ; `On Time Range
  Change` si la liste dépend de la plage.
- **Sort** : alphabétique ou numérique selon le cas.
- **Include All option** : ajoute `All` (valeur `.*` en regex). Custom All
  value possible (ex. `.+` pour exclure le vide).
- **Hide** : `Variable` pour masquer une variable technique (ex. constante
  de seuil) tout en l'utilisant.

---

## 38. Chaînage de variables, regex et multi-sélection

**Chaînage** : la requête d'une variable peut référencer une autre variable.
Ordre de définition = ordre d'évaluation (Grafana trie automatiquement les
dépendances, mais gardez un ordre logique : `dc` → `serveur`).

```
Variable dc      : label_values(node_uname_info, datacenter)
Variable serveur : label_values(node_uname_info{datacenter="$dc"}, instance)
```

Quand `$dc` change, la liste des serveurs se rafraîchit. C'est le pattern
« drill-down » : datacenter → cluster → serveur → interface.

**Regex** (champ Regex de la variable) : filtrer/transformer les valeurs.

```
# Ne garder que les vrais serveurs (pas les conteneurs éphémères)
/^(srv|db|web)-.*/

# Extraire le hostname sans le port
/([^:]+):.*/
```

**Multi-value + All** : quand l'utilisateur sélectionne plusieurs valeurs,
Grafana les injecte comme une regex `(a|b|c)`. D'où l'obligation d'utiliser
`=~` dans les requêtes PromQL/LogQL :

```promql
# CORRECT avec variable multi
node_cpu_seconds_total{instance=~"$serveur"}
# FAUX (ne marchera qu'avec une seule valeur)
node_cpu_seconds_total{instance="$serveur"}
```

**Répétition de panels/rows** (Repeat options) : répéter un panel ou une row
**pour chaque valeur** de la variable. Ex. : une row « Serveur » répétée pour
chaque `$serveur` sélectionné → un dashboard qui affiche N serveurs en
parallèle sans duplication manuelle.

> **Limite :** ne répétez pas plus de ~12 panels par dashboard ; au-delà, la
> page devient lourde et les requêtes explosent. Pour de gros parcs, préférez
> le drill-down (un serveur à la fois) + un dashboard « vue d'ensemble ».

---

## 39. Transformations (merge, join, filter, calculate field…)

Les **transformations** (onglet Transform) modifient les données **après**
requête, sans toucher au backend. Les plus utiles :

| Transformation | Effet | Cas d'usage |
|---|---|---|
| **Merge** | Fusionne plusieurs requêtes en une table | Combiner CPU + RAM + disque |
| **Join by field** | Jointure sur un champ (ex. instance) | Enrichir avec une table métier |
| **Filter by name** | Ne garde que certains champs | Nettoyer une table SQL |
| **Organize fields** | Renomme, réordonne, masque | Tables propres |
| **Calculate field** | Nouveau champ calculé | `utilisation = 100 * (1 - libre/total)` |
| **Group by** | Agrégation | Moyenne par datacenter |
| **Sort by** | Tri | Top N |
| **Limit** | Tronque | Top 10 |
| **Series to rows** | Séries → lignes | Alimenter une table depuis PromQL |
| **Labels to fields** | Labels → colonnes | Table d'inventaire depuis Prometheus |
| **Config from query** | Pilote seuils/unités depuis une requête | Seuils dynamiques par client |

Exemple : table « serveurs les plus chargés » depuis Prometheus :

1. Requête A : `avg by (instance) (rate(node_cpu_seconds_total{mode!="idle"}[5m]))`
2. Requête B : `node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes`
3. Transform **Merge** → **Labels to fields** → **Organize fields**
   (renommer, masquer `Time`) → **Sort by** CPU décroissant → **Limit** 10.

> **Transformations vs requêtes :** si le backend peut le faire (SQL,
> PromQL), faites-le côté backend (plus rapide, moins de données
> transférées). Les transformations servent à la **présentation** et aux
> combinaisons impossibles côté backend.

---

## 40. Annotations (événements superposés aux courbes)

Les **annotations** affichent des événements (déploiements, incidents,
maintenances) comme des lignes verticales sur tous les panels time series.
Corréler « la latence a explosé » avec « déploiement à 14h32 » devient immédiat.

**Annotation manuelle** : `Ctrl+clic` (ou `Cmd+clic`) sur un graphe →
« Add annotation ». Visible par tous les viewers du dashboard.

**Annotation par requête** (recommandée) — exemples :

```promql
# Prometheus : changements détectés via Alertmanager ou un metric maison
ALERTS{alertstate="firing", alertname="DeploiementEnCours"}
```

```logql
# Loki : lignes de log marquant un déploiement
{job="deploy"} |= "DEPLOY OK"
```

```sql
-- PostgreSQL : table des maintenances planifiées
SELECT
  debut AS "time",
  CONCAT(titre, ' — ', description) AS "text",
  'maintenance' AS "tags"
FROM maintenances
WHERE $__timeFilter(debut);
```

Réglages : Dashboard settings → Annotations → New. Choisissez la datasource,
la requête, et mappez les champs `time` / `text` / `tags`.

> **Bonne pratique d'équipe :** imposez une annotation à chaque changement
> en production (via une table SQL alimentée par votre outil de déploiement,
> ou un webhook). Le MTTR chute quand la cause est visible sur la courbe.

---

## 41. Liens et drill-down (data links, dashboard links)

**Data links** (par point de donnée) : clic droit sur un point → ouvrir un
lien construit avec les valeurs du point.

Exemples (Panel → Data links) :

```
# Voir les logs Loki de cette instance au moment du point
http://grafana.mondomaine.fr/explore?orgId=1&left={"datasource":"Loki",
"queries":[{"expr":"{instance=\"${__data.fields.instance}\"}",
"range":{"from":"${__value.time}","to":"${__value.time}"}}]}

# Simple : ouvrir le dashboard détaillé du serveur
/d/UID_DASHBOARD_DETAIL/serveur-detail?var-serveur=${__data.fields.instance}&from=${__from}&to=${__to}
```

Variables disponibles : `${__data.fields.<nom>}`, `${__value.time}`,
`${__value.raw}`, `${__from}`, `${__to}`.

**Dashboard links** (en haut du dashboard) : liens vers des dashboards liés
(même tag, ou URL externe vers wiki/runbook). Ex. : depuis « Vue d'ensemble »,
liens vers « Détail serveur », « Détail réseau », « Logs ».

> Le drill-down bien conçu remplace 10 dashboards : **vue d'ensemble →
> détail par entité → logs/traces**. Trois niveaux suffisent dans 95 % des cas.

---

## 42. Alerting unifié : concepts (règles, instances, états)

Rappel du vocabulaire (section 4), avec le cycle de vie précis :

```
Requêtes (A, B, C...) → Expressions (reduce, math) → Condition (seuil)
        │ évaluée toutes les N minutes (evaluation interval)
        ▼
   ┌─────────┐   condition vraie    ┌─────────┐   pendant 'for'   ┌─────────┐
   │ Normal  │ ──────────────────► │ Pending │ ─────────────────► │ Firing  │
   └─────────┘                     └─────────┘                   └────┬────┘
        ▲                                                            │ résolu
        └────────────────────────────────────────────────────────────┘
```

- **Evaluation group** : les règles sont regroupées ; le groupe définit
  l'intervalle d'évaluation commun (ex. `1m`). Mettez les règles critiques
  dans un groupe à intervalle court, les règles lentes (taux journaliers)
  dans un groupe à `10m` ou `1h`.
- **For** : durée pendant laquelle la condition doit rester vraie avant de
  passer en Firing. `for = 0` (ou vide) = alerte immédiate (à réserver aux
  cas vraiment urgents) ; `for = 5m` évite les faux positifs sur les pics.
- **Labels** : `severity=critical`, `equipe=systemes`… Ils servent au
  **routage** (section 46) et au **regroupement** des notifications.
- **Annotations** : `summary` (titre court) et `description` (détail, avec
  template `{{ $values.B.Value }}`) — c'est ce que reçoit l'astreinte.
- **NoData / Error handling** : que faire si la requête ne renvoie rien
  (`NoData` → Normal / Alerting / KeepLast / Ok) ou échoue (`Error` →
  Alerting / Ok / KeepLast). **À régler consciemment** : le défaut
  (`NoData = NoData`, `Error = Error`) peut laisser une panne silencieuse.

---

## 43. Créer une règle d'alerte — exemple commenté pas à pas

**Alerting → Alert rules → New alert rule.** Exemple : « Disque > 85 % depuis
10 minutes sur un serveur ».

**Étape 1 — Requêtes :**

```
A (Prometheus) :
100 * (1 - (node_filesystem_avail_bytes{fstype!~"tmpfs|overlay",mountpoint!~"/snap.*"}
            / node_filesystem_size_bytes{fstype!~"tmpfs|overlay",mountpoint!~"/snap.*"}))
```

**Étape 2 — Expressions :**

```
B : Reduce — Input A, Function Last, Mode Strict
    → réduit chaque série à sa dernière valeur
C : Math — $B > 85
    → condition : vrai/faux par série (instance × mountpoint)
```

(Alternative : **Classic condition** `WHEN last() OF A IS ABOVE 85` — plus
simple mais moins flexible que Reduce+Math.)

**Étape 3 — Détails :**

| Champ | Valeur |
|---|---|
| Rule name | `Disque plein imminent` |
| Folder | `Systemes` |
| Evaluation group | `systemes-1m` (interval `1m`) |
| For | `10m` |
| Labels | `severity=warning`, `equipe=systemes` |
| Annotations | `summary = "Disque à {{ $values.C.Value }}% sur {{ $labels.instance }} ({{ $labels.mountpoint }})"`, `description = "…"` |
| NoData | `NoData` (créer une alerte NoData visible) |
| Error | `Error` |

**Étape 4 — Aperçu** : le bouton **Preview** montre les instances qui
seraient créées **maintenant** avec les valeurs actuelles. Utilisez-le
systématiquement avant de sauvegarder.

**Étape 5 — Contact point** : laisser la policy par défaut au début, vérifier
la réception, puis router finement (section 46).

---

## 44. Expressions d'alerte (reduce, math, resample)

| Expression | Rôle | Exemple |
|---|---|---|
| **Reduce** | Agrège chaque série en un nombre | `Last`, `Mean`, `Max`, `Min`, `Sum`, `Count` |
| **Math** | Calculs et comparaisons | `$B > 85`, `$A + $B`, `$B / $C * 100` |
| **Resample** | Rééchantillonne sur une fenêtre | Moyenne sur 10 min pour lisser |
| **Classic condition** | Ancien modèle simple | `WHEN avg() OF A IS ABOVE 5` |
| **Threshold** | Variante visuelle du seuil | Équivalent Math lisible |

Exemples commentés :

```
# Alerte si le p95 de latence dépasse 2s pendant 5 min (avec lissage)
A : histogram_quantile(0.95, sum by (le) (rate(http_request_duration_seconds_bucket[5m])))
B : Resample — Input A, Window 10m, Downsampler Mean, Upsampler Fill
C : Reduce — Input B, Function Last
D : Math — $C > 2

# Alerte si AUCUNE donnée reçue depuis 15 min (mort du collecteur)
A : up{job="node"}
B : Reduce — Input A, Function Last
C : Math — $B < 1        # up vaut 0 ou 1 ; absent = NoData (géré séparément)

# Comparer deux requêtes : erreurs en % du trafic
A : sum(rate(http_requests_total{code=~"5.."}[5m]))
B : sum(rate(http_requests_total[5m]))
C : Math — ($A / $B) * 100 > 5
```

> **Piège :** dans une expression Math, `$B` désigne la **valeur réduite** de
> B. Si B renvoie plusieurs séries, la comparaison s'applique série par série
> et chaque série devient une **instance d'alerte** distincte (avec ses
> labels). C'est voulu : une alerte par disque plein, pas une alerte globale.

---

## 45. Contact points : e-mail, Slack, webhook, Telegram

**Alerting → Contact points → New contact point.** Un contact point peut
contenir **plusieurs intégrations** (e-mail + Slack + webhook).

**E-mail :**

```
Name: equipe-systemes-mail
To: systemes@mondomaine.fr
Subject: [{{ .Status | toUpper }}] {{ .GroupLabels.SortedPairs.Values | join " " }}
```
Nécessite le SMTP configuré (section 15). Testez avec le bouton **Test**.

**Slack :**

```
Webhook URL: https://hooks.slack.com/services/<A_COMPLETER>
Channel: #alertes-systemes
Title: {{ template "slack.title" . }}
Text: {{ template "slack.text" . }}
```
(Créez le webhook dans les paramètres de votre workspace Slack : Apps →
Incoming WebHooks.)

**Webhook générique** (vers votre ITSM, GLPI, script maison…) :

```
URL: https://itsm.mondomaine.fr/api/grafana-alerts
HTTP Method: POST
Authorization Header: Bearer <A_COMPLETER>
```
Grafana envoie un JSON avec `alerts[]` (status, labels, annotations,
`startsAt`, `endsAt`, `values`). Documentez le format dans votre wiki.

**Telegram :**

```
Bot Token: <A_COMPLETER>   (via @BotFather)
Chat ID: <A_COMPLETER>     (via @userinfobot ou l'API getUpdates)
```

> **Règle d'or :** chaque contact point critique doit être **testé de bout en
> bout** (bouton Test + vraie règle de test) avant la mise en production.
> Une alerte qui part dans le vide est pire que pas d'alerte.

---

## 46. Notification policies : routage, regroupement, répétition

**Alerting → Notification policies.** C'est un **arbre** : la policy racine
(`default`) puis des sous-policies qui matchent sur les labels.

Exemple d'arbre pour une équipe systèmes :

```
default: contact=defaut-mail, group_by=[alertname], group_wait=30s,
         group_interval=5m, repeat_interval=4h
├── matcher severity=critical → contact=astreinte-telephone
│   (group_wait=0s : notification immédiate)
├── matcher equipe=systemes,severity=warning → contact=systemes-slack
│   (group_by=[alertname, instance], repeat_interval=12h)
└── matcher equipe=reseau → contact=reseau-mail
```

Sémantique des temporisations :

| Paramètre | Rôle | Valeur conseillée |
|---|---|---|
| `group_wait` | Attente avant le 1er envoi (pour grouper les alertes qui arrivent ensemble) | `30s` (0s si critique) |
| `group_interval` | Délai entre deux envois pour le même groupe | `5m` |
| `repeat_interval` | Rappel tant que l'alerte est active | `4h` (critique), `12-24h` (warning) |

**Bonnes pratiques :**

- `group_by: [alertname]` au minimum : une notification « 12 disques pleins »
  plutôt que 12 e-mails.
- Ne routez vers l'astreinte téléphonique/SMS **que** `severity=critical`.
- Faites relire l'arbre à quelqu'un d'autre : une erreur de matcher et les
  alertes critiques partent dans le vide (voir section 74).

---

## 47. Silences et mute timings (plages de maintenance)

**Silence** (ponctuel) : **Alerting → Silences → New silence.**

```
Matchers: instance=srv-web-03, alertname=Disque plein imminent
Starts: 2026-09-27 02:00   Ends: 2026-09-27 06:00
Comment: "Migration disque — ticket CHG-1234"
```

- Un silence n'arrête pas l'évaluation : l'alerte passe en Firing mais la
  **notification est supprimée**. L'historique reste visible.
- **Ne créez jamais de silence permanent** « en attendant » : mettez une date
  de fin et un ticket de référence dans le commentaire.

**Mute timing** (récurrent) : **Alerting → Notification policies → Mute timings.**

```
Name: week-end-non-critique
Time intervals:
  - weekdays: [saturday, sunday]
    times: [{start_time: "00:00", end_time: "23:59"}]
```
Puis associez-le à une policy (`mute_time_intervals`). Cas d'usage : pas de
`warning` le week-end, mais les `critical` passent toujours (ne mutez jamais
les critiques sans validation écrite du responsable).

> **Audit :** la liste des silences actifs est visible dans Alerting →
> Silences. En revue d'exploitation hebdo, vérifiez qu'aucun silence
> « temporaire » ne traîne depuis 3 semaines.

---

## 48. Bonnes pratiques d'alerting (ce qui évite la fatigue d'alerte)

La fatigue d'alerte tue l'alerting : trop de bruit → on ignore tout → on rate
la vraie panne. Règles d'hygiène :

1. **Alertez sur des symptômes, pas sur des causes** : « le site ne répond
   pas » (blackbox) avant « CPU à 92 % ».
2. **Chaque alerte doit être actionnable** : si personne ne sait quoi faire
   quand elle sonne, c'est un dashboard, pas une alerte. Ajoutez un lien
   runbook dans l'annotation `description`.
3. **`for` adapté** : 5-10 min pour les warnings, 0-2 min pour les critiques
   vitales. Un `for` à 0 partout = bruit.
4. **Seuils avec marge** : alerte warning à 85 %, critique à 95 % — pas
   l'inverse, pas 99 % (trop tard).
5. **Pas d'alerte sur des métriques qui n'existent pas encore** : testez la
   requête dans Explore d'abord.
6. **Nommez explicitement** : `Disque plein imminent` > `Alerte 12`.
7. **Labels homogènes** : `severity` (critical/warning/info) et `equipe`
   sur **toutes** les règles — c'est la clé du routage.
8. **Limitez le nombre de règles** : 20 règles pertinentes > 200 règles
   copiées d'un template générique jamais triées.
9. **Revoyez trimestriellement** : quelles alertes ont sonné ? Lesquelles
   étaient des faux positifs ? Ajustez ou supprimez.
10. **Testez la chaîne complète** 1 fois par mois : déclenchez une alerte de
    test et vérifiez la réception (e-mail/Slack/téléphone).

---

## 49. Provisioning as code : le principe

Le **provisioning** permet de définir datasources, dashboards, contact points
et règles d'alerte dans des **fichiers YAML/JSON versionnés dans Git**,
appliqués automatiquement au démarrage de Grafana.

Dossier : `/etc/grafana/provisioning/` :

```
provisioning/
├── dashboards/
│   └── dashboards.yaml        # où trouver les JSON de dashboards
├── datasources/
│   └── datasources.yaml       # les datasources
└── alerting/
    ├── contact-points.yaml
    ├── policies.yaml
    └── rules.yaml
```

Avantages :

- **Reproductibilité** : un nouveau serveur Grafana se reconstruit en
  minutes depuis Git.
- **Revue** : chaque changement de dashboard passe par une merge request.
- **Sauvegarde** : Git **est** une partie de la sauvegarde (mais pas la
  seule : la base interne contient aussi utilisateurs, historique, versions —
  voir sections 62-64).

Comportement : au démarrage, Grafana **crée ou met à jour** les objets
provisionnés. Il ne supprime pas ce qui a été retiré du YAML sauf option
explicite (`deleteDatasources`, dossiers dashboards avec `purge`).

> **Règle d'équipe :** tout dashboard « officiel » (supervision, métier) est
> provisionné. Les dashboards personnels/brouillons restent éditables à la
> main dans l'interface.

---

## 50. Provisioning des datasources (YAML)

`/etc/grafana/provisioning/datasources/datasources.yaml` :

```yaml
apiVersion: 1

# Optionnel : supprime les datasources qui ne sont plus dans ce fichier
deleteDatasources:
  - name: Old-Prometheus
    orgId: 1

datasources:
  - name: Prometheus
    type: prometheus
    access: proxy
    url: http://prometheus.mondomaine.fr:9090
    isDefault: true
    editable: false                 # verrouille l'édition dans l'UI
    jsonData:
      httpMethod: POST
      timeInterval: 15s
      queryTimeout: 60s
      manageAlerts: true            # alerting unifié sur cette datasource
    # Secrets : via variables d'environnement (jamais en clair dans Git !)
    secureJsonData:
      basicAuthPassword: $PROM_BASIC_PASSWORD

  - name: Loki
    type: loki
    access: proxy
    url: http://loki.mondomaine.fr:3100
    jsonData:
      maxLines: 1000

  - name: MySQL-Metier
    type: mysql
    url: bdd.mondomaine.fr:3306
    user: grafana_ro
    jsonData:
      database: metier
      maxOpenConns: 10
      maxIdleConns: 2
      connMaxLifetime: 14400
    secureJsonData:
      password: $MYSQL_GRAFANA_PASSWORD
```

Les variables `$VAR` sont résolues depuis l'environnement du processus
`grafana-server` (fichier `/etc/default/grafana-server` ou `Environment=` dans
l'override systemd). Appliquer :

```bash
sudo systemctl restart grafana-server
sudo tail -n 50 /var/log/grafana/grafana.log | grep -i provisioning
```

---

## 51. Provisioning des dashboards (YAML)

Principe en deux temps : le YAML dit **où** sont les dashboards, les fichiers
JSON sont les dashboards eux-mêmes.

`/etc/grafana/provisioning/dashboards/dashboards.yaml` :

```yaml
apiVersion: 1

providers:
  - name: 'Systemes'
    orgId: 1
    folder: 'Supervision'          # dossier cible dans Grafana
    type: file
    disableDeletion: false
    updateIntervalSeconds: 30      # relecture des JSON toutes les 30 s
    allowUiUpdates: false          # false = la source de vérité est Git
    options:
      path: /etc/grafana/dashboards/systemes
```

Workflow :

```bash
# 1. Construire le dashboard dans l'UI, l'exporter (Share → Export → JSON)
# 2. Le déposer versionné :
sudo mkdir -p /etc/grafana/dashboards/systemes
sudo cp supervision-serveurs.json /etc/grafana/dashboards/systemes/
# 3. Git :
cd /srv/git/grafana && git add . && git commit -m "Dashboard serveurs v3" && git push
# 4. Appliquer (updateIntervalSeconds le fait tout seul sous 30 s,
#    sinon restart pour forcer)
```

> `allowUiUpdates: false` + édition dans l'UI = modifications **écrasées**
> au prochain rechargement. Si vous voulez autoriser des ajustements locaux,
> mettez `true`, mais documentez que Git reste la référence.

---

## 52. Provisioning de l'alerting (contact points, policies, règles)

`/etc/grafana/provisioning/alerting/contact-points.yaml` :

```yaml
apiVersion: 1
contactPoints:
  - orgId: 1
    name: equipe-systemes
    receivers:
      - uid: systemes-mail
        type: email
        settings:
          addresses: systemes@mondomaine.fr
          subject: '[{{ .Status | toUpper }}] {{ .GroupLabels.alertname }}'
      - uid: systemes-slack
        type: slack
        settings:
          url: $SLACK_WEBHOOK_URL
          channel: '#alertes-systemes'
```

`/etc/grafana/provisioning/alerting/policies.yaml` :

```yaml
apiVersion: 1
policies:
  - orgId: 1
    receiver: equipe-systemes
    group_by: ['alertname']
    group_wait: 30s
    group_interval: 5m
    repeat_interval: 4h
    routes:
      - receiver: astreinte-tel
        object_matchers:
          - [severity, =, critical]
        group_wait: 0s
        repeat_interval: 1h
```

`/etc/grafana/provisioning/alerting/rules.yaml` (format unifié) :

```yaml
apiVersion: 1
groups:
  - orgId: 1
    name: systemes-1m
    folder: Systemes
    interval: 1m
    rules:
      - uid: disque-plein-imminent
        title: Disque plein imminent
        condition: C
        data:
          - refId: A
            relativeTimeRange: {from: 600, to: 0}
            datasourceUid: prometheus
            model:
              expr: >-
                100 * (1 - (node_filesystem_avail_bytes{fstype!~"tmpfs|overlay"}
                / node_filesystem_size_bytes{fstype!~"tmpfs|overlay"}))
          - refId: B
            datasourceUid: __expr__
            model: {type: reduce, expression: A, reducer: last}
          - refId: C
            datasourceUid: __expr__
            model: {type: math, expression: '$B > 85'}
        for: 10m
        labels: {severity: warning, equipe: systemes}
        annotations:
          summary: 'Disque à {{ $values.C.Value }}% sur {{ $labels.instance }}'
```

> **Le YAML d'alerting est verbeux** : construisez la règle dans l'UI,
> vérifiez-la avec Preview, puis exportez-la (Alerting → règle → Export) pour
> obtenir le YAML exact à versionner.

---

## 53. Workflow Git pour le provisioning

Structure de dépôt conseillée :

```
grafana-config/
├── provisioning/
│   ├── datasources/datasources.yaml
│   ├── dashboards/dashboards.yaml
│   ├── alerting/contact-points.yaml
│   ├── alerting/policies.yaml
│   └── alerting/rules.yaml
├── dashboards/
│   ├── systemes/supervision-serveurs.json
│   ├── reseau/supervision-reseau.json
│   └── onduleurs/supervision-onduleur.json
├── grafana.ini                # modèle (sans secrets)
└── README.md                  # procédure d'install + conventions
```

Secrets : **jamais en clair dans Git**. Trois options, par ordre de
préférence :

1. **Gestionnaire de secrets** (Vault, Bitwarden, etc.) qui écrit
   `/etc/default/grafana-server` au déploiement.
2. **Variables d'environnement** sur le serveur (`$PROM_BASIC_PASSWORD`),
   renseignées à la main une fois.
3. **SOPS/age** pour chiffrer les YAML dans le dépôt (si vous devez absolument
   versionner les secrets).

Déploiement :

```bash
# Sur le serveur Grafana
cd /etc/grafana
sudo git -C /srv/git/grafana-config pull   # ou rsync/ansible
sudo cp -r /srv/git/grafana-config/provisioning/* /etc/grafana/provisioning/
sudo cp -r /srv/git/grafana-config/dashboards/* /etc/grafana/dashboards/
sudo chown -R root:grafana /etc/grafana/provisioning /etc/grafana/dashboards
sudo systemctl restart grafana-server
```

---

## 54. Organisations, équipes et dossiers

**Organisation** : cloisonnement de haut niveau (dashboards, datasources,
utilisateurs). Cas d'usage : une orga `Production`, une orga `Kiosque`
(écrans muraux, voir section 72), voire une orga par grande entité.

**Équipe (Team)** : groupe d'utilisateurs au sein d'une orga. On donne les
permissions aux **équipes**, pas aux individus.

**Dossier (Folder)** : regroupe des dashboards et porte des permissions.

Schéma conseillé pour un service systèmes :

```
Orga "Main Org."
├── Équipe "Systemes" (rôle Editor)
├── Équipe "Reseau"   (rôle Editor)
├── Équipe "Direction" (rôle Viewer)
├── Dossier "Supervision"      → Systemes: Edit, Reseau: View
├── Dossier "Metier"          → Direction: View, Systemes: Edit
└── Dossier "Brouillons"      → Systemes: Edit (pas de provisioning)
```

Commandes utiles (via l'API) :

```bash
# Créer une équipe
curl -s -X POST -H "Content-Type: application/json" \
  -d '{"name":"Systemes","email":"systemes@mondomaine.fr"}' \
  http://admin:<A_COMPLETER>@localhost:3000/api/teams | python3 -m json.tool
```

---

## 55. Rôles : Viewer, Editor, Admin, Server Admin

| Capacité | Viewer | Editor | Admin (orga) | Server Admin |
|---|---|---|---|---|
| Voir les dashboards | ✅ | ✅ | ✅ | ✅ |
| Utiliser Explore | ✅ | ✅ | ✅ | ✅ |
| Créer/modifier ses dashboards | ❌ | ✅ | ✅ | ✅ |
| Créer des datasources | ❌ | ❌ | ✅ | ✅ |
| Gérer équipes/dossiers de l'orga | ❌ | ❌ | ✅ | ✅ |
| Gérer utilisateurs, orgas, plugins | ❌ | ❌ | ❌ | ✅ |
| Voir/modifier l'alerting* | lecture | selon conf | ✅ | ✅ |

\* L'alerting a ses propres permissions fines (section 56).

Règles d'attribution :

- **Viewer** par défaut pour tout nouvel utilisateur.
- **Editor** : aux exploitants qui construisent des dashboards.
- **Admin** : 1-2 personnes par orga (vous, votre adjoint).
- **Server Admin** : vous seul (et un suppléant documenté dans le coffre).

> **Le compte `admin` générique** : changez son mot de passe, stockez-le dans
> le coffre, et **ne l'utilisez pas au quotidien**. Créez-vous un compte
> nominatif Server Admin.

---

## 56. RBAC fin (permissions granulaires)

Depuis Grafana 9, le **RBAC** permet des permissions plus fines que les
rôles de base, via des **rôles personnalisés**.

Cas d'usage typiques :

- **Éditeurs d'alertes sans droit datasource** : rôle avec
  `alert.rules:read/write`, `alert.notifications:read/write` mais sans
  `datasources:write`.
- **Lecteurs d'un seul dossier** : Viewer global + permission `dashboards:read`
  limitée au dossier « Metier ».
- **Gestionnaires d'équipes** : `teams:write` sans être Admin de l'orga.

Création (Administration → Users and access → Roles), exemple en API :

```bash
curl -s -X POST -H "Content-Type: application/json" \
  -d '{
    "name": "editeur-alertes",
    "description": "Gère les règles et notifications, sans toucher aux datasources",
    "permissions": [
      {"action": "alert.rules:read", "scope": "folders:*"},
      {"action": "alert.rules:write", "scope": "folders:*"},
      {"action": "alert.notifications:read", "scope": "*"},
      {"action": "alert.notifications:write", "scope": "*"}
    ]
  }' \
  http://admin:<A_COMPLETER>@localhost:3000/api/access-control/roles | python3 -m json.tool
```

> **Principe du moindre privilège** : commencez restrictif, élargissez sur
> demande justifiée. C'est plus facile que l'inverse.

---

## 57. Authentification : basique, LDAP, OAuth (aperçu)

**Basique (base interne)** : suffisant pour une petite équipe. Activez le
renforcement :

```ini
[auth.basic]
enabled = true

[security]
# Verrouiller après échecs répétés (anti brute-force)
disable_brute_force_login_protection = false
```

**LDAP / Active Directory** (entreprise avec annuaire) :

```ini
[auth.ldap]
enabled = true
config_file = /etc/grafana/ldap.toml
allow_sign_up = true
```

```toml
# /etc/grafana/ldap.toml (extrait)
[[servers]]
host = "ad.mondomaine.fr"
port = 636
use_ssl = true
bind_dn = "CN=grafana_svc,OU=Comptes de service,DC=mondomaine,DC=fr"
bind_password = '<A_COMPLETER>'
search_base_dns = ["OU=Utilisateurs,DC=mondomaine,DC=fr"]

[servers.group_mappings]
group_dn = "CN=G_Grafana_Admins,OU=Groupes,DC=mondomaine,DC=fr"
org_role = "Admin"
[servers.group_mappings]
group_dn = "CN=G_Grafana_Editeurs,OU=Groupes,DC=mondomaine,DC=fr"
org_role = "Editor"
# Tout le reste tombe en Viewer via :
# [[servers]] ... group_dn = "*" → org_role = "Viewer"
```

**OAuth générique** (Keycloak, Azure AD, Google…) : via `[auth.generic_oauth]`
(client_id, client_secret, auth_url, token_url, api_url). Le `root_url`
(section 10) doit être exact, sinon la redirection échoue.

---

## 58. Service accounts et tokens d'API

Les **service accounts** remplacent les anciennes API keys (dépréciées) pour
l'automatisation : provisioning, CI, scripts d'export.

Création : **Administration → Users and access → Service accounts → Add**.

```bash
# Créer un token pour le compte de service (id 2, rôle Viewer)
curl -s -X POST -H "Content-Type: application/json" \
  -d '{"name":"token-ci","secondsToLive":0}' \
  http://admin:<A_COMPLETER>@localhost:3000/api/serviceaccounts/2/tokens \
  | python3 -m json.tool
# → {"key": "glsa_<A_COMPLETER>..."}  # affiché UNE SEULE FOIS
```

Usage :

```bash
# Lister les dashboards via le token
curl -s -H "Authorization: Bearer glsa_<A_COMPLETER>" \
  http://localhost:3000/api/search?query=supervision | python3 -m json.tool

# Exporter un dashboard en JSON (pour Git)
curl -s -H "Authorization: Bearer glsa_<A_COMPLETER>" \
  http://localhost:3000/api/dashboards/uid/<UID> | python3 -m json.tool > dashboard.json
```

Bonnes pratiques :

- Un service account **par usage** (CI, export, script de health check), avec
  le rôle minimal.
- `secondsToLive` : mettez une **expiration** (ex. 90 jours) sauf besoin
  permanent documenté.
- Stockez les tokens dans le coffre, jamais dans les scripts en clair.
- Révoquez les tokens des comptes de service inutilisés (revue trimestrielle).

---

## 59. HTTPS en production via reverse proxy Nginx

Architecture recommandée : Nginx termine TLS, Grafana écoute en HTTP sur
`127.0.0.1:3000`.

```ini
# /etc/grafana/grafana.ini
[server]
protocol = http
http_addr = 127.0.0.1
http_port = 3000
root_url = https://grafana.mondomaine.fr/
```

```nginx
# /etc/nginx/sites-available/grafana
upstream grafana {
    server 127.0.0.1:3000;
}

server {
    listen 443 ssl http2;
    server_name grafana.mondomaine.fr;

    ssl_certificate     /etc/letsencrypt/live/grafana.mondomaine.fr/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/grafana.mondomaine.fr/privkey.pem;

    # En-têtes de sécurité
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Content-Type-Options nosniff always;
    add_header X-Frame-Options SAMEORIGIN always;
    add_header Referrer-Policy no-referrer always;

    # WebSocket (nécessaire pour les live dashboards)
    location / {
        proxy_pass http://grafana;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }

    # API : timeouts un peu plus généreux pour les grosses requêtes
    location /api/ds/query {
        proxy_pass http://grafana;
        proxy_read_timeout 120s;
        proxy_set_header Host $host;
    }
}

server {
    listen 80;
    server_name grafana.mondomaine.fr;
    return 301 https://$host$request_uri;
}
```

```bash
sudo nginx -t && sudo systemctl reload nginx
# Certificat Let's Encrypt :
sudo certbot --nginx -d grafana.mondomaine.fr
```

> Si Grafana est servi sous un **sous-chemin** (`https://intranet/grafana/`),
> renseignez `root_url = https://intranet/grafana/` **et**
> `serve_from_sub_path = true`. Mais préférez un sous-domaine dédié : moins
> de surprises (cookies, OAuth, websockets).

---

## 60. Durcissement sécurité — checklist

- [ ] `allow_sign_up = false`, pas d'anonyme sauf kiosque dédié (section 72).
- [ ] `admin_password` fort, stocké au coffre ; compte `admin` non utilisé
      au quotidien.
- [ ] `secret_key` générée aléatoirement, sauvegardée avec la base.
- [ ] HTTPS partout (Nginx + HSTS) ; `cookie_secure = true`.
- [ ] Grafana **non exposé directement** sur Internet sans nécessité ;
      derrière VPN/SSO si possible.
- [ ] Comptes nominatifs ; revue trimestrielle des comptes et tokens.
- [ ] Datasources en mode `Server`, comptes lecture seule côté backends.
- [ ] `hide_version = true` (hygiène).
- [ ] Pare-feu : n'ouvrir que 443 (et 22 admin) ; 3000 en localhost uniquement.
- [ ] Mises à jour suivies (section 66) ; alertes CVE Grafana surveillées.
- [ ] Logs d'audit : qui a modifié quoi (table `annotation` / logs en `info`).
- [ ] Sauvegardes chiffrées, testées (sections 62-65).

```bash
# Vérification rapide d'exposition
ss -ltnp | grep -E "3000|443"
# Le port 3000 ne doit être visible que sur 127.0.0.1
curl -s -o /dev/null -w "%{http_code}\n" http://localhost:3000/api/health
```

---

## 61. Superviser Grafana lui-même (métriques internes)

Grafana expose ses propres métriques Prometheus sur `/metrics` (à activer) :

```ini
[metrics]
enabled = true
# Restreindre l'accès : ne pas exposer publiquement
basic_auth_username = '<A_COMPLETER>'
basic_auth_password = '<A_COMPLETER>'
```

Job Prometheus :

```yaml
scrape_configs:
  - job_name: grafana
    static_configs:
      - targets: ['grafana.mondomaine.fr:3000']
    metrics_path: /metrics
    basic_auth:
      username: '<A_COMPLETER>'
      password: '<A_COMPLETER>'
```

Métriques clés à surveiller :

```promql
# Disponibilité de Grafana (blackbox ou scrape up)
up{job="grafana"}

# Requêtes API en erreur (5xx)
sum(rate(grafana_api_response_status_total{code=~"5.."}[5m]))

# Latence p95 des réponses API
histogram_quantile(0.95, sum by (le) (rate(grafana_api_response_time_seconds_bucket[5m])))

# Évaluations d'alertes en échec
sum(rate(grafana_alerting_rule_evaluation_failures_total[5m])) > 0

# Notifications en échec
sum(rate(grafana_alerting_notification_failed_total[5m])) > 0

# Nombre de sessions actives (ordre de grandeur d'usage)
grafana_stat_total_users
grafana_stat_total_dashboards
grafana_stat_total_alerts
```

**Règles d'alerte minimales sur Grafana lui-même :**

1. `up{job="grafana"} == 0` depuis 2 min → critical (l'outil de supervision
   est en panne !).
2. `sum(rate(grafana_alerting_notification_failed_total[5m])) > 0` → warning
   (les alertes ne partent plus — voir section 74).
3. Latence API p95 > 2 s depuis 10 min → warning.

> **Paradoxe du superviseur :** si Grafana tombe, qui vous prévient ?
> Prévoyez un **canal externe** : une sonde blackbox indépendante (un second
> Prometheus, un service externe type health check) qui surveille
> `https://grafana.mondomaine.fr/api/health`.

---

## 62. Sauvegarde : base SQLite

Avec SQLite (défaut), la base est un simple fichier : `/var/lib/grafana/grafana.db`.
**Ne copiez jamais le fichier à chaud avec `cp`** : risque de base corrompue.

```bash
#!/bin/bash
# /usr/local/bin/backup-grafana.sh
set -euo pipefail
DATE=$(date +%F)
DEST="/srv/backups/grafana/$DATE"
mkdir -p "$DEST"

# 1. Dump cohérent de la base SQLite (verrouille proprement)
sqlite3 /var/lib/grafana/grafana.db ".backup '$DEST/grafana.db'"

# 2. Configuration + provisioning + dashboards versionnés
tar -czf "$DEST/grafana-etc.tar.gz" -C /etc grafana

# 3. Plugins installés (réinstallables, mais pratique)
tar -czf "$DEST/grafana-plugins.tar.gz" -C /var/lib/grafana plugins 2>/dev/null || true

# 4. Rétention : 30 jours
find /srv/backups/grafana -maxdepth 1 -type d -mtime +30 -exec rm -rf {} +

echo "Sauvegarde Grafana OK : $DEST"
```

```bash
sudo chmod +x /usr/local/bin/backup-grafana.sh
# Cron quotidien à 2h
echo "0 2 * * * root /usr/local/bin/backup-grafana.sh >> /var/log/grafana-backup.log 2>&1" \
  | sudo tee /etc/cron.d/grafana-backup
```

Vérifiez que `sqlite3` est installé (`apt install sqlite3`). Testez le dump
restauré **une fois par trimestre** (section 65) : une sauvegarde non testée
n'est pas une sauvegarde.

---

## 63. Sauvegarde : base PostgreSQL

Avec PostgreSQL, utilisez les outils natifs (dump cohérent à chaud) :

```bash
#!/bin/bash
# /usr/local/bin/backup-grafana-pg.sh
set -euo pipefail
DATE=$(date +%F_%H%M)
DEST="/srv/backups/grafana/$DATE"
mkdir -p "$DEST"

# 1. Dump logique (format custom, parallélisable à la restauration)
pg_dump -h 127.0.0.1 -U grafana -Fc -f "$DEST/grafana.dump" grafana

# 2. Configuration + provisioning
tar -czf "$DEST/grafana-etc.tar.gz" -C /etc grafana

# 3. Rétention : 30 jours
find /srv/backups/grafana -maxdepth 1 -type d -mtime +30 -exec rm -rf {} +

echo "Sauvegarde Grafana (PG) OK : $DEST"
```

Authentification sans mot de passe interactif : fichier `/root/.pgpass`
(`chmod 600`) :

```
127.0.0.1:5432:grafana:grafana:<A_COMPLETER>
```

Alternative robuste : `pg_basebackup` pour une sauvegarde physique complète
du cluster, en complément du dump logique.

> **Point de vigilance :** la `secret_key` (section 13) **doit** être
> sauvegardée avec la base. Sans elle, les secrets des datasources
> provisionnées (mots de passe, tokens) sont illisibles après restauration.
> Stockez-la dans le coffre **et** dans la sauvegarde chiffrée.

---

## 64. Sauvegarde : provisioning, plugins, stratégie 3-2-1

Ce que la base ne contient pas (ou pas bien) :

| Élément | Où | Sauvegarde |
|---|---|---|
| `grafana.ini` | `/etc/grafana/grafana.ini` | Git + tar quotidien |
| Provisioning YAML | `/etc/grafana/provisioning/` | **Git** (source de vérité) |
| Dashboards JSON | `/etc/grafana/dashboards/` | **Git** |
| Plugins | `/var/lib/grafana/plugins/` | Liste (`grafana-cli plugins ls`) + tar |
| `ldap.toml`, certificats | `/etc/grafana/` | Git privé ou tar chiffré |
| `secret_key` | `grafana.ini` | Coffre + sauvegarde chiffrée |

**Stratégie 3-2-1** : 3 copies, 2 supports différents, 1 hors site.

```bash
# Exemple : copie chiffrée vers un stockage distant (après le backup local)
tar -czf - -C /srv/backups/grafana "$(date +%F)" | \
  gpg --encrypt --recipient supervision@mondomaine.fr | \
  ssh backup@nas.mondomaine.fr "cat > /backups/grafana-$(date +%F).tar.gz.gpg"
```

Checklist sauvegarde :

- [ ] Backup quotidien automatisé (base + /etc/grafana).
- [ ] Git poussé à chaque changement de provisioning/dashboard.
- [ ] `secret_key` et mots de passe dans le coffre.
- [ ] Copie hors site (NAS, S3, autre datacenter).
- [ ] **Test de restauration trimestriel** (section 65) avec compte-rendu.

---

## 65. Restauration — procédure complète

Scénario : serveur Grafana perdu, on reconstruit depuis les sauvegardes.

```bash
# 1. Réinstaller (même version majeure !)
# Voir section 6, puis NE PAS démarrer le service tout de suite.

# 2. Restaurer la configuration
sudo tar -xzf /srv/backups/grafana/<DATE>/grafana-etc.tar.gz -C /

# 3a. SQLite : restaurer le fichier (service arrêté)
sudo systemctl stop grafana-server
sudo cp /srv/backups/grafana/<DATE>/grafana.db /var/lib/grafana/grafana.db
sudo chown grafana:grafana /var/lib/grafana/grafana.db

# 3b. PostgreSQL : recréer la base et restaurer le dump
sudo -u postgres psql -c "DROP DATABASE IF EXISTS grafana;"
sudo -u postgres psql -c "CREATE DATABASE grafana OWNER grafana;"
pg_restore -h 127.0.0.1 -U grafana -d grafana /srv/backups/grafana/<DATE>/grafana.dump

# 4. Plugins
sudo tar -xzf /srv/backups/grafana/<DATE>/grafana-plugins.tar.gz -C /var/lib/grafana
# ou réinstallation propre :
sudo grafana-cli plugins install alexanderzobnin-zabbix-app

# 5. Démarrer et vérifier
sudo systemctl start grafana-server
sleep 5
curl -s http://localhost:3000/api/health | python3 -m json.tool
sudo tail -n 30 /var/log/grafana/grafana.log
```

Points de contrôle post-restauration :

- [ ] `/api/health` → `database: ok`.
- [ ] Login admin OK.
- [ ] Datasources : **Save & test** au vert sur chacune.
- [ ] Dashboards présents (compter : `grafana_stat_total_dashboards`).
- [ ] Règles d'alerte présentes et évaluées (Alerting → état).
- [ ] Test d'envoi d'une notification (contact point → Test).

> **Restaurer sur une version différente** : Grafana migre le schéma à la
> montée de version, jamais à la descente. Restaurez toujours sur une version
> **égale ou supérieure** à celle de la sauvegarde.

---

## 66. Mise à jour de Grafana — procédure sans casse

```bash
# 0. Sauvegarde complète AVANT (sections 62-64) — non négociable
sudo /usr/local/bin/backup-grafana.sh

# 1. Lire la release note (breaking changes, migrations)
# https://grafana.com/docs/grafana/latest/breaking-changes/

# 2. Mise à jour via APT
sudo apt-get update
sudo apt-get install -y --only-upgrade grafana

# 3. Redémarrage + vérification
sudo systemctl restart grafana-server
sleep 10
curl -s http://localhost:3000/api/health | python3 -m json.tool
sudo tail -n 50 /var/log/grafana/grafana.log | grep -iE "migrat|error|fail"

# 4. Contrôles fonctionnels
# - ouvrir 2-3 dashboards clés, vérifier les panels
# - Alerting → vérifier que les règles s'évaluent (état Normal/Pending)
# - envoyer un Test sur chaque contact point critique
```

Bonnes pratiques :

- **Jamais de mise à jour un vendredi après-midi** ni avant une astreinte.
- Testez d'abord sur une **instance de labo** (ou un snapshot VM).
- Ne sautez pas plus d'une version majeure d'un coup (10 → 11 OK, 9 → 11 :
  passez par 10).
- Les plugins tiers peuvent casser : vérifiez leur compatibilité
  (`grafana-cli plugins ls`, page du plugin).
- Après mise à jour majeure, rouvrez les dashboards provisionnés : le
  schéma JSON migre automatiquement, mais vérifiez visuellement.

Rollback : réinstallez la version précédente (`apt install grafana=11.4.0`)
**et** restaurez la base sauvegardée avant (le schéma a pu migrer).

---

## 67. Usage quotidien — routine de l'exploitant

**Le matin (5 min) — tour de contrôle :**

1. Ouvrir le dashboard « Vue d'ensemble » : tout est vert ?
2. **Alerting → Alert list** : alertes firing/pending non acquittées ?
3. **Silences actifs** : un silence a-t-il expiré cette nuit sans résolution ?
4. Logs Grafana : erreurs inhabituelles (`grep -i error` sur les dernières
   24 h) ?

**En intervention :**

1. **Explore** pour investiguer (requête ad hoc, pas de dashboard jetable).
2. Annoter le dashboard (`Ctrl+clic`) : « début d'investigation 09:12 ».
3. Si maintenance planifiée : créer le **silence** avec ticket de référence.
4. Après résolution : vérifier que l'alerte repasse en Normal, clore le
   silence, noter la cause dans le ticket.

**Hebdomadaire (30 min) :**

- Revoir les alertes de la semaine : faux positifs à ajuster ?
- Vérifier les sauvegardes (log du cron, taille des dumps).
- Nettoyer les dashboards « brouillon » abandonnés.

**Mensuel :**

- Test de la chaîne d'alerte de bout en bout (alerte de test → réception).
- Revue des comptes/tokens (départs, tokens expirés).
- Vérifier l'espace disque de `/var/lib/grafana` et `/var/log/grafana`.

---

## 68. Bonnes pratiques de dashboarding (lisibilité)

1. **Un dashboard = une question.** « Mes serveurs sont-ils sains ? », pas
   « tout l'infra en 48 panels ».
2. **Pas de spaghetti** : 6-8 séries max par graphe. Au-delà, agrégez
   (`sum by`), filtrez, ou divisez.
3. **Hiérarchie visuelle** : en haut les indicateurs (Stat), au milieu les
   tendances (Time series), en bas le détail (Table, Logs).
4. **Seuils et couleurs sobres** : vert/orange/rouge, pas d'arc-en-ciel.
   La couleur doit signifier quelque chose (section 34).
5. **Intervalles adaptés** : `$__rate_interval` dans les `rate()`, Min step
   cohérent avec la plage (pas de point par seconde sur 30 jours).
6. **Titres qui disent la question** : « % CPU par serveur (5 min) », pas
   « Graph 1 ». Description du panel : unité, source, seuil d'alerte.
7. **Variables en haut, dans l'ordre logique** : datacenter → serveur →
   interface. Valeurs par défaut sensées (All ou le périmètre de l'équipe).
8. **Zéro panel vide** : un « No data » permanent = une requête cassée à
   réparer ou un panel à supprimer.
9. **Documentez dans le dashboard** (panel Text) : question, mainteneur,
   runbook, date de dernière revue.
10. **Testez sur écran mural** : lisible à 3 mètres ? Si non, augmentez les
    tailles, réduisez le nombre de panels (voir section 72).

**Anti-patterns à bannir :**

| Anti-pattern | Pourquoi c'est mal | Alternative |
|---|---|---|
| 20 dashboards quasi identiques | Maintenance impossible | Variables + 1 dashboard |
| Jauges sans max | Chiffre sans référence | Min/Max renseignés |
| Courbes qui relient les trous | Masque les pannes de collecte | Connect null → Never |
| Seuils décoratifs jamais alertés | Bruit visuel | Seuil = alerte ou rien |
| `rate()` sur une gauge | Chiffres absurdes | `avg_over_time` |

---

## 69. Exemple : dashboard supervision serveurs (node_exporter)

**Question :** « mes serveurs Linux sont-ils sains ? »

Structure (dossier `Supervision`, variables `$dc`, `$serveur`) :

**Row « Vue d'ensemble » (Stat) :**

```promql
# Serveurs UP
count(up{job="node"} == 1)
# Serveurs DOWN
count(up{job="node"} == 0)
# CPU moyen du parc
100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
# Alertes firing
sum(ALERTS{alertstate="firing"})
```

**Row « Serveur : $serveur » (répétée par serveur si besoin) :**

```promql
# CPU % par mode (Time series, stacked)
sum by (mode) (rate(node_cpu_seconds_total{instance=~"$serveur"}[$__rate_interval]))

# Mémoire (Time series)
node_memory_MemTotal_bytes{instance=~"$serveur"} - node_memory_MemAvailable_bytes{instance=~"$serveur"}
# + ligne de référence : node_memory_MemTotal_bytes

# Disque % par mountpoint (Time series + seuils 80/90)
100 * (1 - (node_filesystem_avail_bytes{fstype!~"tmpfs|overlay",instance=~"$serveur"}
            / node_filesystem_size_bytes{fstype!~"tmpfs|overlay",instance=~"$serveur"}))

# Réseau (Time series, 2 axes ou 2 panels)
rate(node_network_receive_bytes_total{device!~"lo|veth.*",instance=~"$serveur"}[$__rate_interval]) * 8
rate(node_network_transmit_bytes_total{device!~"lo|veth.*",instance=~"$serveur"}[$__rate_interval]) * 8

# Load (Time series)
node_load1{instance=~"$serveur"}
node_load5{instance=~"$serveur"}
node_load15{instance=~"$serveur"}
```

**Row « Détail » (Table) :** top filesystems, processus top CPU via
`topk(5, ...)`, dernières alertes (Alert list).

Annotations : déploiements (section 40). Liens : drill-down vers les logs
Loki du serveur (section 41).

---

## 70. Exemple : dashboard supervision réseau

**Question :** « mon réseau transporte-t-il correctement ? »

Sources : SNMP via snmp_exporter → Prometheus, ou flows, ou Zabbix.

**Row « Liens WAN/LAN » :**

```promql
# Débit par interface (bits/s), top 8
topk(8, rate(ifHCInOctets[$__rate_interval]) * 8)
# Légende : {{ifDescr}} ({{instance}})

# Utilisation % vs capacité (nécessite la vitesse en label ou en metric)
100 * rate(ifHCInOctets[$__rate_interval]) * 8 / ifHighSpeed * 1e6

# Erreurs/discards (signe d'un lien dégradé)
rate(ifInErrors[$__rate_interval]) + rate(ifOutErrors[$__rate_interval])
rate(ifInDiscards[$__rate_interval]) + rate(ifOutDiscards[$__rate_interval])
```

**Row « Disponibilité » (State timeline) :**

```promql
up{job="snmp"}
# ou ping : probe_success{job="blackbox-icmp"}
```

**Row « Inventaire » (Table) :** équipement, interface, description,
débit actuel, erreurs — via transformations (Merge + Organize fields).

Bonnes pratiques réseau :

- **Nommez les interfaces** (`ifDescr`/`ifAlias` en légende, pas `ifIndex`).
- Seuils : utilisation > 80 % sustained = capacité à planifier ; erreurs >
  0 = investiguer (un lien sain n'a quasiment pas d'erreurs).
- **Dépendances** : un switch d'agrégation down explique 20 alertes
  serveurs — regroupez les notifications par site (`group_by: [site]`).

---

## 71. Exemple : dashboard supervision onduleur (lien métier)

**Question :** « mes onduleurs protègent-ils correctement la charge ? »
*Pour un chef de service systèmes & énergies, c'est un dashboard vital.*

Sources possibles : carte réseau de l'onduleur (SNMP → snmp_exporter →
Prometheus), NUT (`nut_exporter`), ou supervision constructeur (Eaton IPP,
Schneider/APC PowerChute, Vertiv) qui expose SNMP ou Modbus.

**Row « État instantané » (Stat + Gauge) :**

```promql
# Charge de l'onduleur en % (Gauge, seuils 70/90)
ups_load_percent{ups="$onduleur"}

# Tension d'entrée / sortie (Stat)
ups_input_voltage{ups="$onduleur"}
ups_output_voltage{ups="$onduleur"}

# État : sur secteur / sur batterie (Stat avec mapping de valeur)
ups_on_battery{ups="$onduleur"}   # 0 = secteur, 1 = batterie
# Value mappings : 0 → "SECTEUR" (vert), 1 → "BATTERIE" (rouge)

# Autonomie restante estimée en minutes (Gauge, min 0)
ups_battery_runtime_minutes{ups="$onduleur"}
```

**Row « Tendances » (Time series) :**

```promql
# Charge % sur 7 jours (dimensionnement : dérive-t-elle ?)
ups_load_percent{ups="$onduleur"}

# Température batterie (dérive = vieillissement accéléré, cf. loi d'Arrhenius)
ups_battery_temperature{ups="$onduleur"}

# Tension batterie par bloc (écart entre blocs = bloc faible)
ups_battery_voltage{ups="$onduleur"}
```

**Row « Événements » :**

- **State timeline** : `ups_on_battery` (passages sur batterie = coupures
  secteur vues par l'onduleur).
- **Logs** (si syslog de la carte réseau dans Loki) :
  `{job="snmp-ups"} |= "on battery"`.

**Alertes recommandées :**

| Règle | Condition | Severity |
|---|---|---|
| Onduleur sur batterie | `ups_on_battery == 1` for 1m | critical |
| Autonomie faible | `ups_battery_runtime_minutes < 10` | critical |
| Charge élevée | `ups_load_percent > 80` for 15m | warning |
| Batterie à remplacer | `ups_battery_status != "ok"` ou âge > 4 ans | warning |
| Température batterie | `ups_battery_temperature > 30` for 30m | warning |

> **Lien avec l'exploitation :** ce dashboard se combine avec le scénario
> d'arrêt automatique des serveurs (NUT + script de shutdown) : l'alerte
> « autonomie < 10 min » doit arriver **avant** l'arrêt auto, pas après.
> Documentez la procédure dans un panel Text du dashboard.

---

## 72. Kiosque / TV murale (playlists, kiosk mode)

Un écran mural affiche en rotation les dashboards clés, sans interaction.

**1. Organisation et compte dédiés :**

- Créez une organisation `Kiosque`, un utilisateur `kiosque` (rôle **Viewer**).
- Ne partagez que les dossiers nécessaires avec cette orga.
- Alternative sans login : `auth.anonymous` limité à l'orga Kiosque en Viewer
  (section 13) — pratique mais à réserver à un réseau d'affichage isolé.

**2. Playlist :** Dashboards → Playlists → New.

- Ajoutez les dashboards (« Vue d'ensemble », « Réseau », « Onduleurs »),
  par tag ou sélection.
- Intervalle : `1m` par dashboard (assez pour lire, pas assez pour s'ennuyer).
- Cochez **Autofit** si les dashboards ont des tailles différentes.

**3. Kiosk mode :** ouvrez la playlist avec `?kiosk` dans l'URL :

```
https://grafana.mondomaine.fr/playlists/play/<ID>?kiosk
```

- `?kiosk` : masque menus et barre de navigation.
- `?kiosk&autofitpanels` : ajuste les panels à l'écran.
- Pour un navigateur en plein écran au démarrage (Raspberry Pi / mini-PC) :

```bash
chromium-browser --kiosk --incognito \
  "https://grafana.mondomaine.fr/playlists/play/<ID>?kiosk"
```

**4. Bonnes pratiques d'écran mural :**

- Dashboards **dédiés** (pas les dashboards d'investigation) : gros chiffres,
  peu de panels, `Color mode → Background` sur les Stat critiques.
- Refresh auto : `5m` ou `10m` (pas `5s` : ça fatigue et charge les backends).
- **Ne jamais afficher d'identifiants** ni de panel avec des données
  sensibles sur un écran visible par tous.
- Prévoyez le plan B : si Grafana est down, l'écran affiche une erreur —
  c'est aussi un signal (voir section 61).

---

## 73. Dépannage : la datasource ne répond pas

Symptôme : panels en « Error », **Save & test** rouge, ou
`Data source error` dans les logs.

**Arbre de décision (dans l'ordre) :**

1. **Le backend est-il vivant ?** Depuis le **serveur Grafana** :
   ```bash
   curl -s -o /dev/null -w "%{http_code}\n" http://prometheus.mondomaine.fr:9090/-/healthy
   curl -s http://loki.mondomaine.fr:3100/ready
   nc -vz bdd.mondomaine.fr 5432
   ```
   Si ça échoue ici, le problème est réseau/backend, **pas** Grafana.

2. **DNS** : `getent hosts prometheus.mondomaine.fr` — comparez avec ce que
   Grafana résout (un `/etc/hosts` différent ou un DNS menteur arrive plus
   souvent qu'on ne croit).

3. **Pare-feu** : le port est-il ouvert **depuis** le serveur Grafana
   (règles sortantes du FW Grafana + entrantes du FW backend) ?

4. **Identifiants** : testez-les à la main :
   ```bash
   curl -s -u user:<A_COMPLETER> http://prometheus.mondomaine.fr:9090/api/v1/query?query=up
   PGPASSWORD='<A_COMPLETER>' psql -h bdd.mondomaine.fr -U grafana_ro -d metier -c "SELECT 1;"
   ```

5. **URL dans la datasource** : `http` vs `https`, port, path (`/api_jsonrpc.php`
   pour Zabbix). Copiez l'URL exacte qui marche en `curl`.

6. **Logs Grafana** : `grep -i "datasource\|proxy" /var/log/grafana/grafana.log`
   — le message d'erreur exact est souvent explicite
   (`dial tcp: connection refused`, `x509: certificate signed by unknown authority`…).

7. **TLS** : certificat expiré ou CA inconnue → `curl -vk` pour voir, puis
   ajoutez la CA dans la datasource ou corrigez le certificat côté backend.

8. **Timeout** : requêtes lourdes (SQL sur 1 an) → augmentez `queryTimeout`
   dans la datasource.

> **Réflexe :** 8 fois sur 10, c'est le réseau ou les identifiants, pas
> Grafana. Le test `curl`/`nc` **depuis le serveur Grafana** tranche en
> 30 secondes.

---

## 74. Dépannage : les alertes ne partent pas

Symptôme : la règle est en Firing mais personne ne reçoit rien.

**Checklist dans l'ordre :**

1. **La règle s'évalue-t-elle ?** Alerting → Alert rules → état de la règle.
   `Error` ou `NoData` permanent = requête cassée (testez-la dans Explore).
   Vérifiez aussi `grafana_alerting_rule_evaluation_failures_total`.

2. **Le contact point fonctionne-t-il ?** Bouton **Test** sur le contact
   point. Si le test échoue : SMTP (section 15), webhook URL, token Slack —
   corrigez d'abord ça.

3. **Le routage est-il correct ?** Alerting → Notification policies →
   **« View notification routing »** (aperçu du routage pour des labels
   donnés). Erreurs classiques : matcher `severity = Critical` (casse ! les
   labels sont sensibles à la casse) alors que la règle envoie
   `severity="critical"` ; faute de frappe dans le nom du label
   (`equipe` vs `équipe`).

4. **Un silence ou mute timing actif ?** Alerting → Silences : un silence
   oublié mange les notifications en silence (c'est son travail).

5. **`group_wait` / `repeat_interval`** : avec `group_wait: 5m`, la première
   notification arrive 5 min après le Firing — c'est normal, pas un bug.

6. **Notifications en échec côté Grafana :**
   ```promql
   sum(rate(grafana_alerting_notification_failed_total[5m])) > 0
   ```
   + logs : `grep -i "notify\|webhook\|smtp" /var/log/grafana/grafana.log`.

7. **L'alerte est-elle vraiment en Firing ?** `Pending` ≠ notifié (selon
   `for`). Vérifiez l'historique de l'instance (Alerting → instance →
   History).

> **Test de non-régression mensuel :** une règle de test
> (`Test notification — toujours firing` sur une métrique constante) routée
> vers chaque contact point critique, avec un silence permanent… non !
> Sans silence : elle doit notifier à chaque `repeat_interval`. Si le
> téléphone ne sonne plus, vous le saurez.

---

## 75. Dépannage : 10 cas concrets résolus

**Cas 1 — « Data source error: dial tcp: connection refused »**
Cause : Prometheus arrêté ou firewall. Fix : `systemctl status prometheus`
sur le backend, `curl` depuis Grafana (section 73, étape 1).

**Cas 2 — Panels vides après changement d'heure / de fuseau**
Cause : plage de temps relative vs données horodatées en UTC. Fix : vérifier
le fuseau du dashboard (en haut à droite) et celui du backend ; les données
doivent être en UTC côté stockage.

**Cas 3 — `rate()` renvoie des valeurs absurdes**
Cause : `rate()` appliqué à une gauge, ou intervalle < 2× scrape interval.
Fix : n'utiliser `rate()` que sur les compteurs `_total`, avec
`[$__rate_interval]`.

**Cas 4 — Légendes illisibles (`{instance="...", job="..."}`)**
Cause : champ Legend vide. Fix : `{{instance}}` ou `{{ifDescr}}` dans Legend.

**Cas 5 — L'alerte sonne pour « All » mais pas pour un serveur précis**
Cause : la variable `$serveur` vaut `All` dans la règle d'alerte — les règles
d'alerte **n'ont pas accès** aux variables de dashboard ! Fix : réécrire la
requête sans variable (ou avec une regex explicite).

**Cas 6 — « Template variables are not supported in alert queries »**
Cause : voir cas 5. C'est une limitation **voulue** : une alerte doit être
déterministe. Fix : dupliquer la logique sans `$var`.

**Cas 7 — Dashboard très lent (> 10 s)**
Causes fréquentes : trop de panels (> 20), requêtes sur 30 j sans downsampling,
`maxLines` Loki trop élevé. Fix : réduire la plage par défaut, activer les
trends Zabbix, paginer les tables, utiliser des enregistrements (recording
rules) côté Prometheus.

**Cas 8 — « User not found » après passage à LDAP**
Cause : `allow_sign_up = false` avec LDAP, ou `search_base_dns` incorrect.
Fix : `allow_sign_up = true` pour l'auto-provisionnement, tester avec
`ldapsearch` depuis le serveur Grafana avant d'accuser Grafana.

**Cas 9 — Webhook d'alerte reçu mais JSON incompréhensible**
Cause : format non documenté côté récepteur. Fix : logger le payload brut
une fois, écrire le parseur, documenter le format dans le wiki (section 45).

**Cas 10 — Après mise à jour, des panels affichent « Panel plugin not found »**
Cause : plugin tiers incompatible avec la nouvelle version. Fix :
`grafana-cli plugins update-all`, vérifier la compatibilité sur la page du
plugin, ou remplacer par un panel natif.

---

## 76. Les 10 erreurs classiques (et comment les éviter)

| # | Erreur | Conséquence | Prévention |
|---|---|---|---|
| 1 | `root_url` non renseignée | Liens d'alerte cassés, OAuth en échec | Section 10 : la renseigner dès l'install |
| 2 | Dashboards non versionnés | Perdus à la première fausse manip | Provisioning + Git (sections 49-53) |
| 3 | `secret_key` perdue/changée | Secrets illisibles après restauration | Coffre + sauvegarde (sections 13, 64) |
| 4 | Alertes avec variables `$var` | Règles refusées ou silencieuses | Pas de variables dans les alertes (cas 5-6) |
| 5 | `for: 0` partout | Fatigue d'alerte, bruit | `for` adapté (section 48) |
| 6 | Datasource en mode Browser | Secrets exposés, CORS | Mode Server par défaut (section 26) |
| 7 | Compte SQL avec droits d'écriture | Risque d'altération des données métier | Lecture seule (sections 23-24) |
| 8 | Pas de sauvegarde testée | Découverte le jour de la panne | Test trimestriel (section 65) |
| 9 | Grafana non supervisé | Panne de l'outil invisible | Métriques internes + sonde externe (section 61) |
| 10 | 50 dashboards copiés-collés | Maintenance infernale | Variables + templates (sections 36-38) |

---

## 77. Cas pratiques commentés (4 scénarios d'exploitation)

### Cas A — Mise en service d'un nouveau site distant

1. Ajouter le site dans Prometheus (scrape) et Loki (labels `site=lyon`).
2. Variables existantes `$site` : le site apparaît **automatiquement** dans
   les dashboards (aucune modification de dashboard !).
3. Dupliquer la règle d'alerte « Disque plein » ? **Non** : la règle existante
   sans filtre de site couvre déjà le nouveau site (vérifier avec Preview).
4. Ajouter une annotation « Mise en service site Lyon » sur le dashboard réseau.
5. Vérifier la réception d'une alerte de test depuis le nouveau site.

### Cas B — Coupure électrique un dimanche à 3h

1. L'alerte `Onduleur sur batterie` (critical) part vers l'astreinte
   (téléphone, pas e-mail).
2. L'astreinte ouvre le dashboard Onduleurs : autonomie restante 22 min,
   charge 45 %.
3. Procédure (panel Text du dashboard) : si autonomie < 15 min → arrêt
   ordonné des VMs non critiques (runbook lié).
4. Le retour secteur est visible sur la State timeline ; l'alerte passe en
   Resolved → notification de résolution.
5. Lundi : revue — pourquoi la coupure ? Le groupe électrogène a-t-il pris
   le relais ? Ajuster les seuils si besoin.

### Cas C — Lenteurs applicatives signalées par les utilisateurs

1. **Explore** : `histogram_quantile(0.95, sum by (le) (rate(http_request_duration_seconds_bucket[5m])))`
   → le p95 a triplé à 10h12.
2. Heatmap des latences : distribution bimodale → piste cache.
3. Annotations : un déploiement à 10h10 → corrélation immédiate.
4. Logs Loki du service à 10h10-10h15 : erreurs de connexion au cache.
5. Rollback du déploiement, annotation « rollback 10h40 », la courbe
   redescend → incident documenté en 20 min sans dashboard jetable.

### Cas D — Audit : « prouvez que la supervision est sauvegardée »

1. Montrer le cron de backup + les 30 derniers dumps (`ls -lh /srv/backups/grafana`).
2. Montrer le dépôt Git du provisioning (historique des changements).
3. **Test de restauration** sur une VM de labo (section 65), chronométré :
   objectif < 1 h pour un Grafana fonctionnel.
4. Compte-rendu signé : ce qui a été restauré, ce qui a été vérifié
   (datasources au vert, alertes évaluées, test de notification).

---

## 78. Pense-bête de poche (commandes, requêtes, raccourcis)

**Services :**

```bash
systemctl status grafana-server
systemctl restart grafana-server
tail -f /var/log/grafana/grafana.log
grafana-server -v
grafana-cli plugins ls
```

**Santé :**

```bash
curl -s http://localhost:3000/api/health | python3 -m json.tool
curl -s -o /dev/null -w "%{http_code}\n" http://prometheus.mondomaine.fr:9090/-/healthy
```

**PromQL express :**

```promql
100 - (avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)
100 * (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes))
topk(5, sum by (instance) (rate(node_cpu_seconds_total{mode!="idle"}[5m])))
histogram_quantile(0.95, sum by (le) (rate(http_request_duration_seconds_bucket[5m])))
```

**LogQL express :**

```logql
{job="syslog"} |= "error"
count_over_time({job="syslog"} |= "error" [1m])
{job="app"} | json | level="error"
```

**Raccourcis interface :**

| Touche | Action |
|---|---|
| `Ctrl+K` ou `/` | Recherche globale |
| `Ctrl+S` | Sauvegarder le dashboard (dans l'éditeur) |
| `Ctrl+clic` sur un graphe | Ajouter une annotation |
| `E` | Basculer en édition (dashboard) |
| `Esc` | Quitter l'édition / fermer |

**Checklist « ça ne marche pas » :** backend joignable ? → identifiants ? →
URL exacte ? → logs Grafana → test dans Explore → (alerting) routage/silences.

---

## 79. Glossaire

| Terme | Définition |
|---|---|
| Alerting unifié | Moteur d'alertes de Grafana 9+ (règles, contact points, policies) |
| Annotation | Événement affiché sur les graphes (déploiement, incident) |
| Contact point | Destination des notifications (e-mail, Slack, webhook…) |
| Dashboard | Page composée de panels répondant à une question |
| Datasource | Connexion nommée vers un backend (Prometheus, Loki, SQL…) |
| Explore | Mode de requêtage ad hoc sans créer de dashboard |
| For | Durée pendant laquelle une condition doit persister avant Firing |
| Gauge | Panel jauge (valeur vs seuils) |
| Heatmap | Densité d'une distribution dans le temps |
| Instance (alerte) | Occurrence concrète d'une règle (ex. par serveur) |
| Kiosk (mode) | Affichage plein écran sans menus (TV murale) |
| Label | Paire clé/valeur attachée à une série ou une alerte |
| LogQL | Langage de requête de Loki |
| Mute timing | Plage récurrente de suppression des notifications |
| Notification policy | Arbre de routage des alertes vers les contact points |
| Panel | Bloc de visualisation dans un dashboard |
| Playlist | Rotation automatique de dashboards |
| PromQL | Langage de requête de Prometheus |
| Provisioning | Configuration as code via YAML/JSON versionnés |
| RBAC | Contrôle d'accès basé sur les rôles (permissions fines) |
| Recording rule | Règle Prometheus pré-calculant une métrique |
| Reduce | Expression réduisant une série à un nombre |
| Resample | Expression rééchantillonnant une série |
| Row | Ligne regroupant des panels dans un dashboard |
| Scrape | Collecte périodique de métriques par Prometheus |
| Service account | Compte technique pour l'API (remplace les API keys) |
| Silence | Suppression temporaire des notifications (maintenance) |
| Snapshot | Copie figée d'un dashboard avec ses données |
| Stat | Panel affichant un grand chiffre + tendance |
| Threshold | Seuil déclenchant un changement de couleur/état |
| Transformation | Traitement des données après requête (merge, join…) |
| Variable (template) | Paramètre de dashboard (ex. `$serveur`) |

---

## 80. Quiz — 10 questions + réponses

**Q1.** Où sont stockées les métriques affichées par Grafana ?
**R1.** Nulle part dans Grafana : elles restent dans les backends (Prometheus,
Loki, SQL…). Grafana ne stocke que sa configuration (dashboards,
utilisateurs, règles) dans sa base interne.

**Q2.** Pourquoi faut-il renseigner `root_url` dans `grafana.ini` ?
**R2.** Parce que les liens insérés dans les notifications d'alerte et les
redirections OAuth sont construits à partir de cette URL. Sans elle, ils
pointent vers `localhost` et sont inutilisables.

**Q3.** Quelle différence entre le mode d'accès `Server` et `Browser` d'une
datasource ?
**R3.** En mode Server, le backend Grafana interroge la datasource (les
secrets restent côté serveur — recommandé). En mode Browser, c'est le
navigateur de chaque utilisateur qui interroge directement (expose l'URL et
les identifiants).

**Q4.** Pourquoi une règle d'alerte ne peut-elle pas utiliser les variables
de dashboard (`$serveur`) ?
**R4.** Parce qu'une alerte doit être déterministe : elle s'évalue en tâche
de fond, sans dashboard ouvert ni sélection d'utilisateur. Il faut écrire la
requête sans variable (regex explicite si besoin).

**Q5.** À quoi sert le paramètre `for` dans une règle d'alerte ?
**R5.** À exiger que la condition reste vraie pendant une durée donnée avant
de passer en Firing, ce qui filtre les pics transitoires et réduit les faux
positifs.

**Q6.** Que signifie `group_by: [alertname]` dans une notification policy ?
**R6.** Que les instances d'alerte partageant le même `alertname` sont
regroupées en **une seule** notification (ex. « 12 disques pleins » au lieu
de 12 e-mails).

**Q7.** Quelle commande sauvegarde proprement la base SQLite de Grafana ?
**R7.** `sqlite3 /var/lib/grafana/grafana.db ".backup '/chemin/sauvegarde.db'"`.
Un simple `cp` à chaud risque de produire une base corrompue.

**Q8.** Citez deux choses à vérifier en premier quand une datasource ne
répond plus.
**R8.** (1) La joignabilité du backend **depuis le serveur Grafana**
(`curl`/`nc`) ; (2) les identifiants, testés à la main hors Grafana. Dans
80 % des cas, c'est le réseau ou les identifiants, pas Grafana.

**Q9.** Pourquoi ne faut-il jamais appliquer `rate()` à une gauge ?
**R9.** Parce que `rate()` calcule une variation par seconde en supposant un
**compteur** qui ne fait qu'augmenter. Sur une gauge (température, niveau),
le résultat est absurde. On utilise `avg_over_time`, `max_over_time`, etc.

**Q10.** Que doit contenir une sauvegarde complète de Grafana pour permettre
une restauration ?
**R10.** La base interne (SQLite/PostgreSQL), `/etc/grafana` (grafana.ini,
provisioning, ldap.toml), les dashboards JSON versionnés (Git), la
`secret_key`, et la liste des plugins. Sans la `secret_key`, les secrets des
datasources sont illisibles après restauration.

---

## 81. Pour aller plus loin

**Écosystème Grafana :**

- **Grafana Mimir** : backend Prometheus scalable (long terme, multi-tenant).
- **Grafana Tempo** : tracing distribué (corrélation métriques → logs → traces).
- **Grafana Pyroscope** : profiling continu (pourquoi ce CPU ?).
- **Grafana OnCall** : gestion d'astreinte (planning, escalades).
- **Grafana Alloy** : collecteur unifié (successeur de l'agent Grafana).

**Sujets à creuser :**

- Recording rules Prometheus pour les dashboards lents (pré-calcul).
- Exemplars : du graphe au log à la trace en un clic.
- LBC (library panels) : panels réutilisables entre dashboards.
- Tests de charge des dashboards (k6) avant une démo à la direction.
- Haute disponibilité de Grafana (plusieurs instances + base partagée).

**Documentation officielle :** https://grafana.com/docs/grafana/latest/
**Communauté :** https://community.grafana.com/

---

## 82. Annexe A — `grafana.ini` de référence commenté

```ini
#################### Serveur ####################
[server]
protocol = http
http_addr = 127.0.0.1
http_port = 3000
root_url = https://grafana.mondomaine.fr/
;serve_from_sub_path = false

#################### Chemins ####################
[paths]
data = /var/lib/grafana
logs = /var/log/grafana
plugins = /var/lib/grafana/plugins
provisioning = /etc/grafana/provisioning

#################### Base interne ####################
[database]
# SQLite par défaut ; PostgreSQL recommandé en production :
#host = 127.0.0.1:5432
#name = grafana
#user = grafana
#password = '<A_COMPLETER>'
#ssl_mode = require

#################### Logs ####################
[log]
mode = console file
level = info
[log.file]
log_rotate = true
daily_rotate = true
max_days = 7

#################### Sécurité ####################
[security]
admin_user = admin
admin_password = '<A_COMPLETER>'
allow_sign_up = false
secret_key = '<A_COMPLETER_32_CARACTERES_ALEATOIRES>'
login_maximum_inactive_lifetime_duration = 7d
login_maximum_lifetime_duration = 30d
cookie_secure = true
cookie_samesite = lax
;hide_version = true

[auth.anonymous]
enabled = false

#################### Utilisateurs ####################
[users]
default_theme = dark
allow_sign_up = false

#################### SMTP ####################
[smtp]
enabled = true
host = smtp.mondomaine.fr:587
user = grafana@mondomaine.fr
password = '<A_COMPLETER>'
from_address = grafana@mondomaine.fr
from_name = Grafana Supervision
startTLS_policy = MandatoryStartTLS

#################### Métriques internes ####################
[metrics]
enabled = true
basic_auth_username = '<A_COMPLETER>'
basic_auth_password = '<A_COMPLETER>'

#################### Dashboards ####################
[dashboards]
default_timezone = Europe/Paris

#################### Alerting ####################
[unified_alerting]
enabled = true
# Évaluation et rétention :
#evaluation_timeout = 30s
#max_attempts = 3
```

> Après toute modification : `systemctl restart grafana-server`, puis
> `Administration → Settings` pour vérifier la configuration effective.

---

## 83. Annexe B — checklist de mise en production

- [ ] Installé depuis le dépôt APT officiel, version épinglée et notée.
- [ ] `grafana.ini` relu (Annexe A), `root_url` correcte, restart OK.
- [ ] `secret_key` générée, sauvegardée au coffre.
- [ ] Compte `admin` : mot de passe fort au coffre, non utilisé au quotidien.
- [ ] Comptes nominatifs créés, rôles attribués (Viewer par défaut).
- [ ] HTTPS via Nginx (HSTS, redirection 80→443), `cookie_secure = true`.
- [ ] SMTP configuré et testé (e-mail de test reçu).
- [ ] Datasources en mode Server, comptes lecture seule, Save & test au vert.
- [ ] Dashboards clés construits, variables fonctionnelles, provisioning en Git.
- [ ] Alerting : règles avec `for` et labels `severity`/`equipe`, contact
      points testés, arbre de routage relu, test mensuel planifié.
- [ ] Grafana supervisé : métriques internes + sonde externe sur `/api/health`.
- [ ] Sauvegarde quotidienne automatisée + copie hors site + test de
      restauration trimestriel planifié.
- [ ] Documentation : runbooks liés dans les dashboards, mots de passe au
      coffre, procédure de restauration écrite (section 65).
- [ ] Écran mural (si besoin) : orga/compte dédiés, playlist, kiosk mode.

---

*Fin du guide — bon dashboarding !*
