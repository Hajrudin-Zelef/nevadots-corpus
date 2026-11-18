# Guide Prometheus — Supervision métrique complète

> **Public cible** : chefs de service systèmes, administrateurs systèmes & réseaux,
> responsables d'exploitation. **Version** : Prometheus 2.5x / 3.x (concepts stables
> sur toutes les 2.x). **Date** : septembre 2026.

---

## 1. Pourquoi ce guide

Prometheus est devenu le standard de facto de la supervision métrique dans les
infrastructures modernes : serveurs Linux, équipements réseau, bases de données,
applications. Pour un chef de service systèmes, c'est l'outil qui permet de
passer d'une supervision réactive ("le serveur est tombé, on nous appelle") à
une supervision prédictive ("le disque sera plein dans 12 jours, on planifie").

Ce guide couvre tout le cycle de vie : installation, configuration, PromQL
(le langage de requêtes), les exporters (node, blackbox, SNMP — y compris les
onduleurs), Alertmanager, la haute disponibilité, la sauvegarde, le dépannage,
et les pièges à éviter (la cardinalité !).

> **Convention de lecture** : les blocs `yaml` sont des extraits de configuration,
> les blocs `promql` (ou sans langage) sont des requêtes à tester dans l'interface
> web Prometheus (menu **Graph**). Les ⚠️ signalent les pièges de production.

---

## 2. Architecture générale de Prometheus

Prometheus est un **système de monitoring** qui fonctionne sur un principe
simple : il **va chercher** (pull) les métriques sur les cibles, les **stocke**
dans sa base de données temporelle (TSDB), permet de les **interroger** avec
PromQL, et **déclenche des alertes** via Alertmanager.

```
 ┌──────────────┐   scrape (pull)   ┌──────────────────┐
 │ node_exporter│◄──────────────────┤                  │
 ├──────────────┤                   │                  │
 │blackbox_exp. │◄──────────────────┤    PROMETHEUS    │──► Alertmanager ──► email/Telegram/webhook
 ├──────────────┤                   │  (TSDB + PromQL) │
 │ snmp_exporter│◄──────────────────┤                  │
 ├──────────────┤                   │                  │
 │  application │◄──────────────────┤                  │
 │  (instrument)│                   └──────────────────┘
 └──────────────┘                              │
                                               ▼
                                        Grafana (dashboards)
```

Composants :

| Composant | Rôle |
|---|---|
| **Prometheus server** | Scrape, stockage TSDB, évaluation des règles, API HTTP |
| **Exporters** | Exposent les métriques d'un système au format texte sur `/metrics` |
| **Alertmanager** | Reçoit les alertes, les déduplique, groupe, route vers les bons canaux |
| **Pushgateway** | Point d'entrée pour les jobs batch (push) qui ne vivent pas assez longtemps pour être scrapés |
| **Grafana** | Visualisation (dashboards), s'appuie sur l'API PromQL |

## 3. Le modèle pull : comprendre la philosophie

Contrairement à Nagios/Zabbix (modèle push ou checks actifs centralisés),
Prometheus **initie la connexion** vers chaque cible à intervalle régulier
(par défaut toutes les 15 s) sur un endpoint HTTP `/metrics`.

Avantages du pull :

- **Découverte de service simple** : ajouter une cible = ajouter une ligne de config.
- **Santé intrinsèque** : si le scrape échoue, la métrique `up{job="..."}` passe
  à 0 → on sait immédiatement que la cible ne répond plus, sans agent tiers.
- **Pas d'agent à configurer côté serveur** : l'exporter expose, Prometheus vient.
- **Débogage facile** : `curl http://cible:9100/metrics` montre exactement ce que
  Prometheus voit.

Inconvénients :

- Derrière un NAT ou un firewall strict, il faut ouvrir le flux **vers** la cible
  (ou utiliser le Pushgateway / un remote write).
- Pas adapté aux jobs éphémères (< intervalle de scrape) → Pushgateway.

## 4. La TSDB : comment Prometheus stocke les données

La **TSDB** (Time Series Database) embarquée stocke chaque série temporelle
sous forme d'échantillons `(timestamp, valeur)` compressés.

Concepts clés :

- **Série temporelle** = une métrique + un jeu unique de labels.
  Exemple : `node_cpu_seconds_total{cpu="0",mode="idle"}` est une série.
- **Chunks** : les échantillons récents vivent en mémoire (head chunk), puis
  sont flushés sur disque en blocs de 2 h, compactés ensuite.
- **Rétention** : par défaut **15 jours** (`--storage.tsdb.retention.time=15d`)
  ou 512 Mo (`--storage.tsdb.retention.size`). Le premier atteint déclenche la purge.
- **WAL** (Write-Ahead Log) : journal d'écriture pour rejouer les données
  récentes après un crash.

Répertoire de données (par défaut `./data`, configurable) :

```
data/
├── wal/            # journal d'écriture (données < ~2h)
├── chunks_head/    # chunks en mémoire persistés
└── 01J.../         # blocs TSDB (2h chacun, avec index + chunks)
```

⚠️ **Ne jamais modifier ces fichiers à la main.** Pour sauvegarder, utiliser
l'API de snapshots (voir section 74).

## 5. Modèle de données : métriques et labels

Toute donnée Prometheus est une **série temporelle** identifiée par :

1. Un **nom de métrique** : `http_requests_total`, `node_memory_MemTotal_bytes`.
2. Zéro ou plusieurs **labels** (paires clé=valeur) : `{method="GET",status="200"}`.

Le nom + l'ensemble des labels = **l'identité unique** de la série.

```promql
# Une série précise :
http_requests_total{method="POST", handler="/api/login", status="500"}

# Toutes les séries de cette métrique (quel que soit le label) :
http_requests_total
```

Règles de nommage :

- Noms en `snake_case`, en anglais, avec **unité en suffixe** si applicable :
  `_seconds`, `_bytes`, `_total` (pour les compteurs).
- Labels en `snake_case` : `instance`, `job`, `datacenter`, `host`.
- Les labels `job` et `instance` sont ajoutés automatiquement par Prometheus
  lors du scrape (configurables via relabeling).

## 6. Les 4 types de métriques

### 6.1 Counter (compteur)

Valeur qui **ne fait qu'augmenter** (ou reste stable), remise à zéro au redémarrage
du processus. Exemples : nombre de requêtes HTTP, octets réseau reçus.

```promql
# ❌ MAUVAIS : la valeur brute d'un compteur ne veut rien dire
http_requests_total

# ✅ BON : le débit par seconde sur 5 minutes
rate(http_requests_total[5m])
```

⚠️ Toujours utiliser `rate()`, `irate()` ou `increase()` sur un compteur,
jamais la valeur brute ni `delta()`.

### 6.2 Gauge (jauge)

Valeur qui **peut monter et descendre** : température, mémoire utilisée,
nombre de connexions actives, niveau de batterie d'onduleur.

```promql
# ✅ Directement exploitable
node_memory_MemAvailable_bytes
ups_battery_charge_percent
```

### 6.3 Histogram

Échantillonne des observations (tailles de requêtes, durées) dans des
**buckets** configurables et expose :

- `*_bucket{le="..."}` : compteurs cumulés par borne supérieure (`le` = less or equal),
- `*_sum` : somme des observations,
- `*_count` : nombre d'observations.

```promql
# Quantile p95 de la latence HTTP sur 5 min
histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))
```

⚠️ Les buckets sont **cumulatifs** : `le="0.5"` inclut tout ce qui est ≤ 0,5 s.

### 6.4 Summary

Comme l'histogramme, mais calcule les **quantiles côté client** (dans l'application)
et expose `*_quantile{quantile="0.95"}`, `*_sum`, `*_count`.

| | Histogram | Summary |
|---|---|---|
| Quantiles calculés | côté serveur (PromQL) | côté client |
| Agrégeable (`sum` sur plusieurs instances) | ✅ oui | ❌ non |
| Coût | buckets à dimensionner | calculé en continu |

**Recommandation** : préférer **Histogram** pour tout ce qui doit s'agréger
(par service, par datacenter). Summary uniquement pour du debug local.

## 7. Installation : méthode binaire (Debian/Ubuntu)

La méthode la plus simple et la plus robuste en production : binaire statique
officiel, aucune dépendance.

```bash
# 1. Variables
PROM_VERSION="3.0.1"
ARCH="linux-amd64"   # ou linux-arm64 pour Raspberry Pi / ARM

# 2. Téléchargement et vérification (depuis releases GitHub officielles)
cd /tmp
wget "https://github.com/prometheus/prometheus/releases/download/v${PROM_VERSION}/prometheus-${PROM_VERSION}.${ARCH}.tar.gz"
wget "https://github.com/prometheus/prometheus/releases/download/v${PROM_VERSION}/sha256sums.txt"
grep "prometheus-${PROM_VERSION}.${ARCH}.tar.gz" sha256sums.txt | sha256sum -c -

# 3. Installation
tar xzf "prometheus-${PROM_VERSION}.${ARCH}.tar.gz"
cd "prometheus-${PROM_VERSION}.${ARCH}"
sudo mkdir -p /etc/prometheus /var/lib/prometheus
sudo cp prometheus promtool /usr/local/bin/
sudo cp -r consoles console_libraries /etc/prometheus/
sudo cp prometheus.yml /etc/prometheus/prometheus.yml

# 4. Utilisateur dédié (ne JAMAIS lancer en root en production)
sudo useradd --no-create-home --shell /usr/sbin/nologin prometheus
sudo chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus
sudo chmod 755 /usr/local/bin/prometheus /usr/local/bin/promtool

# 5. Vérification de la config avant tout démarrage
promtool check config /etc/prometheus/prometheus.yml
```

⚠️ `promtool check config` est votre meilleur ami : lancez-le **avant chaque
redémarrage** après modification de la config.

## 8. Installation : service systemd

Fichier `/etc/systemd/system/prometheus.service` :

```ini
[Unit]
Description=Prometheus Server
Documentation=https://prometheus.io/docs/
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
Restart=always
RestartSec=5
# Limites anti-OOM : ajuster selon la RAM disponible
MemoryMax=4G
# Démarrage
ExecStart=/usr/local/bin/prometheus \
  --config.file=/etc/prometheus/prometheus.yml \
  --storage.tsdb.path=/var/lib/prometheus \
  --storage.tsdb.retention.time=30d \
  --storage.tsdb.retention.size=50GB \
  --web.console.templates=/etc/prometheus/consoles \
  --web.console.libraries=/etc/prometheus/console_libraries \
  --web.listen-address=0.0.0.0:9090 \
  --web.enable-lifecycle \
  --query.max-concurrency=20 \
  --query.timeout=2m

[Install]
WantedBy=multi-user.target
```

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now prometheus
sudo systemctl status prometheus
# Vérifier que l'UI répond :
curl -s http://localhost:9090/-/healthy && echo " OK"
```

`--web.enable-lifecycle` active `/-/reload` (rechargement de config sans restart)
et `/-/quit` (arrêt propre). **Indispensable en production.**

## 9. Flags de démarrage importants

| Flag | Défaut | Usage |
|---|---|---|
| `--config.file` | `prometheus.yml` | Chemin du fichier de configuration |
| `--storage.tsdb.path` | `data/` | Répertoire des données TSDB |
| `--storage.tsdb.retention.time` | `15d` | Durée de rétention |
| `--storage.tsdb.retention.size` | `512MB` | Taille max (purge au premier atteint) |
| `--web.listen-address` | `0.0.0.0:9090` | Adresse d'écoute HTTP |
| `--web.external-url` | — | URL publique (derrière reverse proxy) |
| `--web.route-prefix` | — | Préfixe de chemin (ex. `/prometheus`) |
| `--web.enable-lifecycle` | off | `/-/reload` et `/-/quit` |
| `--web.enable-admin-api` | off | API admin (snapshots, delete series) |
| `--query.timeout` | `2m` | Timeout max d'une requête |
| `--query.max-concurrency` | `20` | Requêtes concurrentes max |
| `--query.max-samples` | `50M` | Échantillons max par requête (anti-OOM) |
| `--storage.tsdb.no-lockfile` | off | À activer si le FS ne supporte pas flock (NFS…) |

Recharger la configuration **sans redémarrer** :

```bash
curl -X POST http://localhost:9090/-/reload
# ou, si protégé :
kill -HUP $(pidof prometheus)
```

## 10. Structure de prometheus.yml

```yaml
# /etc/prometheus/prometheus.yml
global:
  scrape_interval: 15s          # fréquence de scrape par défaut
  scrape_timeout: 10s           # doit être < scrape_interval !
  evaluation_interval: 15s      # fréquence d'évaluation des règles
  external_labels:              # labels ajoutés à TOUTES les séries (fédération, HA)
    datacenter: 'dc-paris'
    replica: 'prom-01'

# Fichiers de règles (recording + alerting)
rule_files:
  - '/etc/prometheus/rules/*.yml'

# Destinations d'alertes
alerting:
  alertmanagers:
    - static_configs:
        - targets: ['127.0.0.1:9093']

# Les jobs de scrape
scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['127.0.0.1:9090']
```

⚠️ Règles d'or :

- `scrape_timeout` < `scrape_interval`, toujours.
- `external_labels` : **obligatoire** en HA (distinguer les réplicas) et fédération.
- Après chaque modification : `promtool check config` puis `/-/reload`.

## 11. scrape_configs : le job de base

Chaque `scrape_config` définit **un job** = un ensemble de cibles scrapées
de la même façon.

```yaml
scrape_configs:
  - job_name: 'node-linux'
    scrape_interval: 15s
    scrape_timeout: 10s
    metrics_path: /metrics        # chemin HTTP (défaut)
    scheme: http                  # ou https
    static_configs:
      - targets:
          - 'srv-web01:9100'
          - 'srv-web02:9100'
          - 'srv-db01:9100'
        labels:
          datacenter: 'dc-paris'
          equipe: 'infra'
```

Labels automatiques ajoutés par Prometheus à chaque série scrapée :

- `job` = nom du job (`node-linux`),
- `instance` = `host:port` de la cible (`srv-web01:9100`).

On peut les réécrire via `relabel_configs` (section 19).

## 12. static_configs : quand les cibles sont fixes

`static_configs` = liste de cibles en dur. Parfait pour un parc stable
et de taille modérée (< 50 serveurs).

```yaml
  - job_name: 'onduleurs-snmp'
    static_configs:
      - targets: ['192.168.10.20', '192.168.10.21']
        labels:
          site: 'siege'
          equipement: 'ups-eaton-93ps'
```

Limites : chaque ajout/retrait = modification du fichier + reload.
Pour un parc dynamique, préférer `file_sd_configs` (section suivante).

## 13. file_sd_configs : découverte par fichier

Le fichier JSON/YAML listant les cibles est **relu automatiquement**
(`refresh_interval`), sans reload de Prometheus. Idéal quand un inventaire
(Ansible, CMDB, script) génère la liste.

`/etc/prometheus/file_sd/serveurs.json` :

```json
[
  {
    "targets": ["srv-web01:9100", "srv-web02:9100"],
    "labels": {"datacenter": "dc-paris", "role": "web"}
  },
  {
    "targets": ["srv-db01:9100"],
    "labels": {"datacenter": "dc-paris", "role": "db", "critique": "oui"}
  }
]
```

```yaml
scrape_configs:
  - job_name: 'node-dynamique'
    file_sd_configs:
      - files:
          - '/etc/prometheus/file_sd/*.json'
        refresh_interval: 30s
```

> **Astuce d'exploitation** : générez ce fichier depuis votre inventaire Ansible
> (`ansible-inventory --list`) via un template : une seule source de vérité.

## 14. Découverte DNS (SRV, A, AAAA)

Pour les environnements avec DNS à jour (Consul DNS, DNS d'entreprise) :

```yaml
  - job_name: 'dns-sd'
    dns_sd_configs:
      - names: ['_prometheus._tcp.dc-paris.lan']  # enregistrements SRV
        type: 'SRV'
        port: 9100                                # port par défaut si absent du SRV
        refresh_interval: 30s
      - names: ['exporters.dc-paris.lan']         # enregistrements A/AAAA
        type: 'A'
        port: 9100
```

Le SRV permet de publier `host:port` par service : chaque équipe expose
`_node-exporter._tcp` dans sa zone, Prometheus suit automatiquement.

## 15. Découverte Consul

Si vous utilisez Consul comme registre de services :

```yaml
  - job_name: 'consul-services'
    consul_sd_configs:
      - server: 'consul.dc-paris.lan:8500'
        token: 'votre-token-acl'      # via variable d'env de préférence
        datacenter: 'dc-paris'
        refresh_interval: 30s
    relabel_configs:
      # Ne garder que les services taggés "metrics"
      - source_labels: [__meta_consul_tags]
        regex: '.*,metrics,.*'
        action: keep
      # Utiliser le nom du service comme label "service"
      - source_labels: [__meta_consul_service]
        target_label: service
```

Les méta-labels `__meta_consul_*` (service, node, tags, address…) permettent
un relabeling très fin (voir section 19).

## 16. Découverte Kubernetes

Dans un cluster Kubernetes, Prometheus découvre pods, services, endpoints :

```yaml
  - job_name: 'k8s-pods'
    kubernetes_sd_configs:
      - role: pod
        kubeconfig_file: ''   # in-cluster si vide et RBAC configuré
    relabel_configs:
      # Scraper uniquement les pods annotés prometheus.io/scrape=true
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        regex: 'true'
        action: keep
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
        regex: '(.+)'
        target_label: __metrics_path__
      - source_labels: [__meta_kubernetes_pod_ip, __meta_kubernetes_pod_annotation_prometheus_io_port]
        regex: '([^;]+);([^;]+)'
        replacement: '${1}:${2}'
        target_label: __address__
```

⚠️ En Kubernetes, **toujours** filtrer par annotation/label au relabeling :
scraper tous les pods = explosion de cardinalité.

## 17. Paramètres de scrape avancés

```yaml
  - job_name: 'lent-ou-distant'
    scrape_interval: 60s        # site distant : scraper moins souvent
    scrape_timeout: 50s
    honor_labels: false         # les labels du job écrasent ceux de la cible
    honor_timestamps: true      # faire confiance aux timestamps exposés
    sample_limit: 5000          # coupe le scrape si > 5000 échantillons (anti-cardinalité)
    target_limit: 100           # nombre max de cibles pour ce job
    enable_compression: true    # gzip (défaut true)
```

`sample_limit` est un **garde-fou cardinalité** : si un exporter déraille
(label à haute cardinalité), le scrape est rejeté au lieu de remplir la TSDB.

## 18. Relabeling : principes

Le **relabeling** transforme les labels **avant** le scrape (sur les métadonnées
de découverte) ou **après** (sur les séries ingérées, via `metric_relabel_configs`).

Labels spéciaux (préfixe `__`, supprimés après le scrape sauf réécriture) :

| Label | Signification |
|---|---|
| `__address__` | `host:port` de la cible (modifiable → change la cible scrapée !) |
| `__scheme__` | `http` / `https` |
| `__metrics_path__` | chemin HTTP (défaut `/metrics`) |
| `__meta_*` | métadonnées de la découverte (consul, k8s, ec2…) |

Actions possibles : `replace` (défaut), `keep`, `drop`, `labelmap`,
`labeldrop`, `labelkeep`, `hashmod`, `uppercase`, `lowercase`.

Ordre d'application : les `relabel_configs` sont évaluées **dans l'ordre**,
de haut en bas. L'ordre compte !

## 19. Relabeling : filtrer avec keep et drop

```yaml
    relabel_configs:
      # Ne garder que les cibles dont le label "env" vaut prod ou preprod
      - source_labels: [env]
        regex: 'prod|preprod'
        action: keep

      # Exclure les instances en maintenance (label maintenance=true)
      - source_labels: [maintenance]
        regex: 'true'
        action: drop

      # Exclure un port précis
      - source_labels: [__address__]
        regex: '.*:9115'     # ne pas scraper le blackbox avec ce job
        action: drop
```

> `keep` = liste blanche (tout le reste est jeté), `drop` = liste noire.
> En cas de doute, préférez `keep` : le défaut est de tout scraper.

## 20. Relabeling : réécrire des labels (replace)

```yaml
    relabel_configs:
      # Extraire le hostname depuis __address__ (srv-web01:9100 -> srv-web01)
      - source_labels: [__address__]
        regex: '([^:]+):.*'
        replacement: '${1}'
        target_label: hostname

      # Construire un label "site" depuis le nom DNS
      - source_labels: [__address__]
        regex: '.*\\.([a-z]+)\\.lan:.*'
        replacement: '${1}'
        target_label: site

      # Forcer le port de scrape à 9100 quel que soit le port découvert
      - source_labels: [__address__]
        regex: '([^:]+)(:[0-9]+)?'
        replacement: '${1}:9100'
        target_label: __address__

      # Ajouter un label statique
      - replacement: 'equipe-infra'
        target_label: equipe
```

⚠️ Écrire dans `__address__` **change la cible réellement scrapée** :
c'est la base du pattern blackbox_exporter (section 52).

## 21. metric_relabel_configs : filtrer après scrape

Appliqué **après** le scrape, sur les séries ingérées. Sert à **jeter des
métriques inutiles** avant stockage (économie disque + cardinalité).

```yaml
    metric_relabel_configs:
      # Jeter toutes les métriques Go runtime des exporters (inutiles en supervision)
      - source_labels: [__name__]
        regex: 'go_.*'
        action: drop

      # Jeter les métriques de debug d'un exporter
      - source_labels: [__name__]
        regex: 'process_.*'
        action: drop

      # Ne garder du node_exporter que ce qui sert (exemple restrictif)
      - source_labels: [__name__]
        regex: 'node_cpu_seconds_total|node_memory_.*|node_filesystem_.*|node_network_.*|node_load.*|node_time_seconds|up'
        action: keep
```

⚠️ `metric_relabel_configs` s'applique **après** le scrape réseau : la bande
passante est consommée quand même. Pour réduire aussi le trafic, filtrer côté
exporter (`--collector.disable-defaults`, `--no-collector.*`).

## 22. honor_labels et conflits de labels

Quand une cible expose déjà un label `job` ou `instance` :

```yaml
honor_labels: true   # garde les labels de la CIBLE en cas de conflit
honor_labels: false  # (défaut) les labels du SCRAPE (job/instance) gagnent
```

Cas typique : la **fédération** (section 71) ou le scrape d'un autre Prometheus :
`honor_labels: true` préserve les `job`/`instance` d'origine. Sinon, toutes les
séries fédérées se retrouveraient avec `job="federate"`.

## 23. Vérifier sa configuration : promtool

```bash
# Valider la syntaxe
promtool check config /etc/prometheus/prometheus.yml

# Tester les règles (unit tests !)
promtool check rules /etc/prometheus/rules/*.yml

# Tester une requête PromQL contre une série de test
promtool query instant http://localhost:9090 'up{job="node-linux"}'

# Tester les règles avec des données simulées (fichier test.yml)
promtool test rules test_alertes.yml
```

Exemple de fichier de test `test_alertes.yml` :

```yaml
rule_files:
  - alertes.yml
evaluation_interval: 1m
tests:
  - interval: 1m
    input_series:
      - series: 'up{job="node-linux", instance="srv-web01:9100"}'
        values: 1x10 0x5        # 10x up, puis 5x down
    alert_rule_test:
      - eval_time: 12m
        alertname: InstanceDown
        exp_alerts:
          - exp_labels:
              instance: 'srv-web01:9100'
              severity: 'critical'
```

> Les tests unitaires de règles d'alerte : **peu connu, très précieux**.
> Ils évitent de découvrir une alerte cassée le jour de la panne.

## 24. L'interface web au quotidien

`http://prometheus:9090` — onglets principaux :

| Onglet | Usage |
|---|---|
| **Graph** | Tester des requêtes PromQL (table + graphique) |
| **Alerts** | État des règles d'alerte (pending / firing) |
| **Targets** | État de santé de chaque cible (up/down, dernier scrape, erreur) |
| **Rules** | Recording rules évaluées |
| **Service Discovery** | Cibles découvertes par mécanisme, avant relabeling |
| **TSDB Status** | Cardinalité : top 10 des métriques les plus lourdes |
| **Flags / Config** | Vérifier la config chargée et les flags |

Réflexe quotidien : **Status → Targets** : tout doit être vert (`UP`).
Une cible `DOWN` = alerte `InstanceDown` si vos règles sont bien faites
(section 70).

---

---

# PARTIE II — PromQL : le langage de requêtes

---

## 25. PromQL en 5 minutes

PromQL manipule des **vecteurs** :

- **Vecteur instantané** : un ensemble de séries, une valeur chacune, à un instant T.
- **Vecteur de plage (range vector)** : un ensemble de séries, plusieurs valeurs
  sur une durée `[5m]`. Ne peut pas être affiché directement : il faut lui
  appliquer une fonction (`rate()`, `avg_over_time()`…) qui le transforme en
  vecteur instantané.

```promql
node_cpu_seconds_total                    # vecteur instantané (toutes les séries)
node_cpu_seconds_total[5m]                 # range vector (5 min d'historique)
rate(node_cpu_seconds_total[5m])          # vecteur instantané : débit/sec par série
```

## 26. Sélecteurs et filtres de labels

```promql
# Égalité exacte
node_filesystem_avail_bytes{mountpoint="/", fstype="ext4"}

# Négation
node_filesystem_avail_bytes{mountpoint!="/boot"}

# Regex (opérateur =~)
node_network_receive_bytes_total{device=~"eth[0-9]+"}

# Regex négative
node_network_receive_bytes_total{device!~"lo|docker.*|veth.*"}

# Plusieurs filtres (ET logique implicite)
up{job="node-linux", datacenter="dc-paris"}
```

⚠️ `{job="..."}` sans nom de métrique = toutes les séries ayant ce label.
Utile en exploration, **interdit dans les dashboards** (trop large).

## 27. Offset : regarder dans le passé

```promql
# Valeur il y a 1 heure (comparaison avant/après incident)
node_memory_MemAvailable_bytes offset 1h

# Débit d'il y a 7 jours, même heure (comparaison semaine/semaine)
rate(http_requests_total[5m] offset 7d)

# Offset aussi sur les range vectors
avg_over_time(node_load1[1h] offset 24h)
```

Cas d'usage roi : *"est-ce que c'est pire que la semaine dernière à la même heure ?"*

## 28. Opérateurs arithmétiques

`+ - * / % ^` s'appliquent élément par élément entre vecteurs (appariés par labels)
ou entre vecteur et scalaire.

```promql
# Mémoire utilisée en Gio
(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes) / 1024^3

# Pourcentage d'utilisation disque
100 * (1 - node_filesystem_avail_bytes / node_filesystem_size_bytes)

# Conversion octets -> Gbit/s sur le réseau
rate(node_network_receive_bytes_total[5m]) * 8 / 1e9
```

⚠️ Division par zéro : PromQL retourne **NaN**, pas d'erreur. Filtrer si besoin :
`... / clamp_min(node_filesystem_size_bytes, 1)`.

## 29. Opérateurs de comparaison et `bool`

Par défaut, une comparaison **filtre** (garde les séries où c'est vrai, jette les autres).
Avec `bool`, elle retourne **0 ou 1**.

```promql
# Séries dont l'utilisation disque dépasse 85 % (filtre)
(100 * (1 - node_filesystem_avail_bytes / node_filesystem_size_bytes)) > 85

# Retourne 1 si critique, 0 sinon (utile pour des métriques d'état)
(node_filesystem_avail_bytes / node_filesystem_size_bytes) < bool 0.1
```

Opérateurs : `== != > < >= <=`. Attention, `=` seul n'existe pas en PromQL.

## 30. Opérateurs logiques : and, or, unless

- `and` : intersection (séries présentes des deux côtés, valeur du côté gauche).
- `or` : union (toutes les séries, valeur du côté où elle existe).
- `unless` : séries de gauche absentes à droite.

```promql
# Disques pleins à plus de 85 % ET montés (exclure les pseudo-fs)
(100 * (1 - node_filesystem_avail_bytes / node_filesystem_size_bytes)) > 85
  and on (instance, device, mountpoint) node_filesystem_readonly == 0

# Alertes "disque plein" OU "inode épuisés"
(disque_plein > 85) or (inodes_pleins > 90)

# Cibles down SAUF celles en maintenance planifiée
up == 0 unless on (instance) maintenance_mode == 1
```

## 31. Vector matching : on() et ignoring()

Quand les deux côtés n'ont pas exactement les mêmes labels, précisez la clé
de jointure :

```promql
# Pourcentage de CPU idle par instance : les deux métriques partagent "instance"
avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m]))
  / on (instance) group_left
avg by (instance) (rate(node_cpu_seconds_total[5m]))
```

- `on (labels)` : n'apparier **que** sur ces labels.
- `ignoring (labels)` : apparier sur tous les labels **sauf** ceux-là.
- `group_left` / `group_right` : jointure **plusieurs-vers-un**
  (cardinalité many-to-one), obligatoire sinon erreur.

⚠️ Erreur classique : `vector1 / vector2` sans matching alors que les labels
diffèrent → **résultat vide**, sans message d'erreur. Toujours vérifier qu'on
obtient des données.

## 32. rate(), irate(), increase() : bien les utiliser

```promql
rate(http_requests_total[5m])      # débit moyen/sec sur 5 min (lissé)
irate(http_requests_total[5m])     # débit instantané (2 derniers points)
increase(http_requests_total[1h])  # augmentation totale sur 1 h
```

| Fonction | Usage recommandé |
|---|---|
| `rate()` | Dashboards, alertes (lisse les à-coups) |
| `irate()` | Graphiques volatiles, debug en direct |
| `increase()` | "Combien d'erreurs sur la dernière heure ?" |

```promql
# Erreurs HTTP 5xx par seconde, par service
sum by (service) (rate(http_requests_total{status=~"5.."}[5m]))

# Nombre total de redémarrages de conteneurs sur 24 h
increase(kube_pod_container_status_restarts_total[24h])
```

⚠️ **Fenêtre minimale** : `[5m]` avec un scrape à 15 s = ~20 points, bon lissage.
En dessous de 4× l'intervalle de scrape, `rate()` devient bruité. Pour les
alertes, préférez `[5m]` ou `[10m]`, jamais `[1m]`.

⚠️ `rate()` sur un **compteur qui se réinitialise** (reboot) : Prometheus détecte
la remise à zéro et lisse. `irate()` peut produire un pic artificiel.

## 33. Fonctions sur range vectors (`*_over_time`)

Transforment un range vector en vecteur instantané :

```promql
avg_over_time(node_load1[1h])        # charge moyenne sur 1 h
max_over_time(cpu_temp[30m])         # pic de température sur 30 min
min_over_time(node_memory_MemAvailable_bytes[1h])
sum_over_time(increase_erreurs[24h]) # (rare : préférer increase)
count_over_time(up[5m])              # nombre de scrapes réussis sur 5 min
last_over_time(metrique[5m])         # dernière valeur (utile après absent)
stddev_over_time(latence[1h])        # écart-type : détecter l'instabilité
stdvar_over_time(latence[1h])
quantile_over_time(0.95, latence[1h])# p95 (coûteux : préférer histogram_quantile)
```

```promql
# Disponibilité d'un service sur 30 jours (en %)
100 * avg_over_time(up{job="api"}[30d])
```

## 34. delta(), idelta(), deriv(), predict_linear()

```promql
delta(temperature_celsius[1h])              # variation absolue sur 1 h (gauges !)
idelta(temperature_celsius[5m])             # variation entre 2 derniers points
deriv(temperature_celsius[1h])              # dérivée/sec (tendance)
predict_linear(disk_free_bytes[6h], 86400)  # prévision dans 24 h (86400 s)
```

**Le cas d'usage star** — prédire un disque plein :

```promql
# Heures restantes avant saturation du disque (si tendance linéaire)
(node_filesystem_avail_bytes / deriv(node_filesystem_avail_bytes[6h])) / 3600
# Alerte si < 72 h restantes :
predict_linear(node_filesystem_avail_bytes{fstype!~"tmpfs|overlay"}[6h], 3*24*3600) < 0
```

⚠️ `predict_linear` suppose une tendance **linéaire** : parfait pour un disque
qui se remplit régulièrement, faux pour des à-coups. Fenêtre `[6h]` minimum.

⚠️ `delta()`/`deriv()` sont pour les **gauges**. Sur un compteur, utilisez
`increase()`/`rate()`.

## 35. histogram_quantile() et les latences

```promql
# p50, p95, p99 de la latence HTTP par route, sur 5 min
histogram_quantile(0.50, sum by (route, le) (rate(http_request_duration_seconds_bucket[5m])))
histogram_quantile(0.95, sum by (route, le) (rate(http_request_duration_seconds_bucket[5m])))
histogram_quantile(0.99, sum by (route, le) (rate(http_request_duration_seconds_bucket[5m])))
```

Points critiques :

1. **Toujours** `sum by (le, ...)` **avant** `histogram_quantile` (agréger les buckets).
2. Le label `le` doit être conservé dans le `by`.
3. `rate()` **à l'intérieur**, sur les buckets (ce sont des compteurs).

```promql
# Taux de requêtes lentes (> 1 s) : sans quantile, souvent plus parlant
sum by (route) (rate(http_request_duration_seconds_bucket{le="+Inf"}[5m])
-
sum by (route) (rate(http_request_duration_seconds_bucket{le="1"}[5m]))
```

## 36. Fonctions temporelles

```promql
time()                          # timestamp unix actuel
hour()                          # heure UTC (0-23) — attention au fuseau !
day_of_week()                   # 0=dimanche … 6=samedi (UTC)
days_in_month()
month()  year()  minute()
timestamp(metrique)              # timestamp du dernier échantillon
```

⚠️ Ces fonctions sont en **UTC**. Pour des alertes "heures ouvrées" en Europe/Paris,
décaler manuellement : `hour(time() + 2*3600)` (été) — fragile aux changements
d'heure. Mieux : gérer les horaires dans Alertmanager (time_intervals).

```promql
# Âge des données : détecter un exporter figé
time() - timestamp(node_time_seconds)
```

## 37. Manipulation de labels : label_replace, label_join

```promql
# Extraire le nom court depuis instance="srv-web01:9100" -> host="srv-web01"
label_replace(up, "host", "$1", "instance", "([^:]+):.*")

# Combiner deux labels : src + dst -> paire="src->dst"
label_join(probe_success, "paire", "->", "src", "dst")

# Renommer proprement pour Grafana
label_replace(
  node_filesystem_avail_bytes,
  "serveur", "$1", "instance", "([^:]+):.*"
)
```

Signature : `label_replace(vecteur, "dst", "$1", "src", "regex")`.
Si la regex ne matche pas, le label destination n'est pas créé (pas d'erreur).

## 38. absent() : détecter ce qui n'existe pas

```promql
# Retourne 1 si AUCUNE série ne matche (ex. : plus aucun worker en vie)
absent(up{job="workers"})

# Alerte "aucune sauvegarde réussie depuis 26 h"
absent_over_time(backup_success[26h])

# Combiner : alerter si le job de backup n'émet plus RIEN
absent(up{job="backup"}) or (up{job="backup"} == 0)
```

⚠️ `absent()` retourne un vecteur **vide** (pas de 0) quand des séries existent :
c'est son intérêt (rien à alerter) mais ça surprend dans les dashboards.

## 39. Agrégations : sum, avg, min, max, count, stddev…

```promql
sum by (datacenter) (rate(http_requests_total[5m]))   # trafic par DC
avg by (job) (node_load1)                             # charge moyenne par job
max by (instance) (node_cpu_temperature)              # point chaud par serveur
count by (job) (up)                                   # nombre de cibles par job
count by (job) (up == 0)                              # nombre de cibles DOWN par job
stddev by (service) (latence)                         # dispersion
```

- `by (labels)` : groupe **en gardant** ces labels (tout le reste est jeté).
- `without (labels)` : groupe **en jetant** ces labels (garde tout le reste).

```promql
# Équivalents :
sum by (datacenter, job) (rate(http_requests_total[5m]))
sum without (instance, cpu, mode) (rate(http_requests_total[5m]))
```

⚠️ `without` est dangereux si un nouveau label à haute cardinalité apparaît :
préférez `by` explicite dans les recording rules.

## 40. topk(), bottomk(), count_values(), group()

```promql
topk(5, 100 * (1 - node_filesystem_avail_bytes / node_filesystem_size_bytes))
# -> les 5 filesystems les plus pleins

bottomk(3, node_memory_MemAvailable_bytes)
# -> les 3 serveurs avec le moins de RAM libre

count_values("instances", up{job="node-linux"})
# -> nombre de cibles par valeur de up : {instances="42"} 1, {instances="3"} 0

group by (datacenter) (up)
# -> 1 par datacenter ayant au moins une cible (test d'existence)
```

⚠️ `topk`/`bottomk` : le nombre de séries retournées peut varier → compliqué
pour l'alerting (préférer un seuil avec `>`).

## 41. Subqueries : des requêtes dans des requêtes

Permettent d'appliquer une fonction de range sur le **résultat** d'une requête :

```promql
# Dérivée du débit sur 1 h, calculée sur des points à 5 min
deriv(rate(http_requests_total[5m])[1h:5m])

# Moyenne sur 1 h du p95 glissant à 5 min
avg_over_time(
  histogram_quantile(0.95, sum by (le) (rate(http_request_duration_seconds_bucket[5m])))[1h:5m]
)

# Tendance : le trafic augmente-t-il depuis ce matin ?
rate(http_requests_total[5m])[8h:5m]
```

Syntaxe : `<requête>[durée:pas]`. Le pas (`5m`) doit être ≥ l'intervalle de scrape.

⚠️ Les subqueries sont **coûteuses** : à réserver aux dashboards occasionnels,
jamais dans des alertes évaluées toutes les 15 s.

## 42. Recording rules : pré-calculer pour aller vite

Les **recording rules** évaluent une requête à intervalle régulier et stockent
le résultat comme une **nouvelle métrique**. Indispensable pour :

- Accélérer les dashboards (requêtes lourdes pré-calculées),
- Simplifier les alertes,
- Réduire la cardinalité (agréger tôt).

`/etc/prometheus/rules/recording.yml` :

```yaml
groups:
  - name: node-recording
    interval: 30s
    rules:
      # CPU usage % par instance (pré-calculé)
      - record: instance:cpu_usage_percent:30s
        expr: |
          100 * (1 - avg by (instance) (
            rate(node_cpu_seconds_total{mode="idle"}[5m])
          ))

      # Trafic HTTP par service et par code
      - record: service:http_requests:rate5m
        expr: |
          sum by (service, status) (rate(http_requests_total[5m]))

      # Espace disque libre minimum par nœud
      - record: instance:disk_free_bytes:min
        expr: |
          min by (instance) (node_filesystem_avail_bytes{fstype!~"tmpfs|overlay|squashfs"})
```

Convention de nommage : `niveau:metrique:operation` (`job:`, `instance:`…).
Le suffixe indique la fenêtre si pertinent.

⚠️ Une recording rule **crée des séries** : ne pas en abuser sur des requêtes
à haute cardinalité (voir section 79).

## 43. Alerting rules : la syntaxe des alertes

`/etc/prometheus/rules/alertes.yml` :

```yaml
groups:
  - name: infra-alertes
    interval: 30s
    rules:
      - alert: InstanceDown
        expr: up == 0
        for: 5m                 # doit être vrai pendant 5 min avant de tirer
        labels:
          severity: critical
          equipe: infra
        annotations:
          summary: "Instance {{ $labels.instance }} DOWN (job {{ $labels.job }})"
          description: >
            La cible {{ $labels.instance }} ne répond plus depuis plus de 5 minutes.
            Runbook: https://wiki.lan/runbooks/instance-down
          dashboard: "https://grafana.lan/d/infra?var-instance={{ $labels.instance }}"
```

- `expr` : la condition PromQL (vecteur non vide = alerte).
- `for` : durée pendant laquelle la condition doit rester vraie → évite les
  alertes sur un micro-à-coup. États : `pending` → `firing`.
- `labels` : routage Alertmanager (severity, équipe…).
- `annotations` : texte humain (templating `{{ $labels.x }}`, `{{ $value }}`).

## 44. Tester et débugger ses requêtes

Méthode en 4 étapes :

1. **Vérifier que la métrique existe** : dans Graph, taper le début du nom
   (autocomplétion) ou `count({__name__=~"node_.*"})`.
2. **Simplifier** : partir de la métrique brute, ajouter les filtres un par un.
3. **Vérifier les labels** : cliquer une série → voir ses labels réels
   (souvent la jointure échoue à cause d'un label inattendu).
4. **Expliquer la lenteur** : préfixer par rien de spécial — utiliser l'API
   avec `stats=all` ou le flag `--query.stats-enabled` :

```bash
curl -s 'http://localhost:9090/api/v1/query?query=up&stats=all' | head -c 2000
```

Réflexes :

- Résultat vide ? → vérifier les noms de labels (`instance` vs `hostname`),
  les regex, le `on()`/`ignoring()`.
- Trop lent ? → réduire la fenêtre, ajouter un filtre `job`, créer une
  recording rule.
- Valeurs absurdes sur `rate()` ? → fenêtre trop courte (< 4× scrape_interval).

---

---

# PARTIE III — Les exporters

---

## 45. node_exporter : l'exporter système de référence

`node_exporter` expose ~1500 métriques matérielles/OS sur `:9100/metrics`.
Installation (même méthode que Prometheus : binaire + systemd) :

```bash
NODE_VERSION="1.9.0"
cd /tmp
wget "https://github.com/prometheus/node_exporter/releases/download/v${NODE_VERSION}/node_exporter-${NODE_VERSION}.linux-amd64.tar.gz"
tar xzf "node_exporter-${NODE_VERSION}.linux-amd64.tar.gz"
sudo cp "node_exporter-${NODE_VERSION}.linux-amd64/node_exporter" /usr/local/bin/
sudo useradd --no-create-home --shell /usr/sbin/nologin node_exporter
```

`/etc/systemd/system/node_exporter.service` :

```ini
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
Restart=always
ExecStart=/usr/local/bin/node_exporter \
  --collector.systemd \
  --collector.processes \
  --collector.tcpstat \
  --no-collector.arp \
  --no-collector.bcache \
  --no-collector.infiniband \
  --web.listen-address=127.0.0.1:9100

[Install]
WantedBy=multi-user.target
```

> Activez uniquement les collecteurs utiles : chaque collecteur = des séries
> en plus. `--collector.disable-defaults` + activation sélective = le plus propre
> sur un grand parc.

## 46. node_exporter : CPU et charge

Métriques clés :

| Métrique | Type | Usage |
|---|---|---|
| `node_cpu_seconds_total{cpu,mode}` | counter | % CPU par mode |
| `node_load1`, `node_load5`, `node_load15` | gauge | charge système |
| `node_procs_running`, `node_procs_blocked` | gauge | processus |
| `node_context_switches_total` | counter | changements de contexte |
| `node_forks_total` | counter | forks |

```promql
# Utilisation CPU % (tous modes sauf idle), par instance
100 * (1 - avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])))

# Détail par mode (user/system/iowait...) — dashboard uniquement
100 * avg by (instance, mode) (rate(node_cpu_seconds_total[5m]))

# Charge vs nombre de CPU : alerte si load5 > 2× CPU pendant 15 min
node_load5 > 2 * count by (instance) (node_cpu_seconds_total{mode="idle"})

# iowait élevé = disques ou stockage en souffrance
100 * avg by (instance) (rate(node_cpu_seconds_total{mode="iowait"}[5m])) > 20
```

⚠️ `node_load1` seul ne veut rien dire sans le comparer au nombre de CPU.

## 47. node_exporter : mémoire et swap

| Métrique | Usage |
|---|---|
| `node_memory_MemTotal_bytes` | RAM totale |
| `node_memory_MemAvailable_bytes` | RAM réellement disponible (le bon indicateur) |
| `node_memory_SwapTotal_bytes`, `node_memory_SwapFree_bytes` | swap |
| `node_memory_Cached_bytes`, `node_memory_Buffers_bytes` | cache |

```promql
# % mémoire utilisée (méthode fiable)
100 * (1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)

# Swap utilisé en Mio
(node_memory_SwapTotal_bytes - node_memory_SwapFree_bytes) / 1024 / 1024

# Pression mémoire : disponible < 10 % pendant 10 min → alerte
node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes < 0.1
```

⚠️ Ne jamais utiliser `MemFree` seul : Linux utilise la RAM libre comme cache,
c'est normal. `MemAvailable` est l'indicateur pertinent (noyau ≥ 3.14).

## 48. node_exporter : disques et filesystems

| Métrique | Usage |
|---|---|
| `node_filesystem_size_bytes{device,mountpoint,fstype}` | taille |
| `node_filesystem_avail_bytes` | espace dispo (utilisateur) |
| `node_filesystem_files`, `node_filesystem_files_free` | inodes |
| `node_filesystem_readonly` | 1 si monté en lecture seule |
| `node_disk_io_time_seconds_total{device}` | temps d'occupation disque |
| `node_disk_read_bytes_total`, `node_disk_written_bytes_total` | débits |

```promql
# % utilisé par filesystem (exclure les pseudo-fs)
100 * (1 - node_filesystem_avail_bytes{fstype!~"tmpfs|overlay|squashfs|vfat"}
          / node_filesystem_size_bytes)

# Inodes : le piège classique (% espace OK mais inodes pleins !)
100 * (1 - node_filesystem_files_free / node_filesystem_files) > 80

# Saturation disque : % d'utilisation du disque (0-100)
100 * rate(node_disk_io_time_seconds_total[5m])

# Disque qui va saturer dans moins de 3 jours
predict_linear(node_filesystem_avail_bytes{fstype="ext4"}[6h], 3*24*3600) < 0
```

> **Toujours** superviser les inodes en plus de l'espace : des millions de
> petits fichiers (sessions PHP, mails) remplissent les inodes bien avant l'espace.

## 49. node_exporter : réseau

| Métrique | Usage |
|---|---|
| `node_network_receive_bytes_total{device}` | octets reçus |
| `node_network_transmit_bytes_total{device}` | octets émis |
| `node_network_receive_errors_total` | erreurs RX |
| `node_network_receive_drop_total` | paquets jetés |
| `node_network_carrier` | 1 si lien UP |

```promql
# Débit en Mbit/s par interface (hors loopback/virtuelles)
sum by (instance, device) (rate(node_network_receive_bytes_total{device!~"lo|veth.*|docker.*"}[5m])) * 8 / 1e6

# Erreurs ou drops : doit rester à 0
increase(node_network_receive_errors_total[1h]) > 0
  or increase(node_network_transmit_errors_total[1h]) > 0

# Lien tombé
node_network_carrier{device!~"lo.*"} == 0
```

## 50. node_exporter : systemd et processus

Avec `--collector.systemd` :

```promql
# Service en échec
node_systemd_unit_state{name="nginx.service", state="failed"} == 1

# Tous les services failed, par machine
count by (instance) (node_systemd_unit_state{state="failed"} == 1)

# Service désactivé mais attendu actif (exemple)
node_systemd_unit_state{name="postgresql.service", state="active"} == 0
```

Avec `--collector.processes` : `namedprocess_namegroup_*` (nécessite config).
Alternative légère : `node_procs_running` pour détecter un fork-bomb.

## 51. blackbox_exporter : principe et installation

Le **blackbox_exporter** sonde des cibles **depuis** Prometheus : HTTP(S),
ICMP (ping), TCP, DNS. Pattern : Prometheus scrape le blackbox en lui passant
la cible en paramètre.

Installation :

```bash
BB_VERSION="0.26.0"
cd /tmp
wget "https://github.com/prometheus/blackbox_exporter/releases/download/v${BB_VERSION}/blackbox_exporter-${BB_VERSION}.linux-amd64.tar.gz"
tar xzf "blackbox_exporter-${BB_VERSION}.linux-amd64.tar.gz"
sudo cp "blackbox_exporter-${BB_VERSION}.linux-amd64/blackbox_exporter" /usr/local/bin/
sudo mkdir -p /etc/blackbox_exporter
sudo useradd --no-create-home --shell /usr/sbin/nologin blackbox_exporter
```

`/etc/blackbox_exporter/config.yml` — modules de sondes :

```yaml
modules:
  http_2xx:                      # sonde HTTP(S) : code 2xx attendu
    prober: http
    timeout: 10s
    http:
      valid_status_codes: [200, 301, 302]
      method: GET
      follow_redirects: true
      preferred_ip_protocol: "ip4"

  http_post_api:                 # sonde d'API avec POST + Basic Auth
    prober: http
    timeout: 10s
    http:
      method: POST
      headers:
        Content-Type: application/json
      body: '{"ping": true}'
      basic_auth:
        username: "monitor"
        password: "changeme"
      fail_if_body_not_matches_regexp:
        - '"status":"ok"'

  icmp_ping:                     # ping ICMP (nécessite capabilities)
    prober: icmp
    timeout: 10s
    icmp:
      preferred_ip_protocol: "ip4"

  tcp_connect:                   # simple ouverture TCP (ex. : port 443, 25, 3306)
    prober: tcp
    timeout: 10s

  dns_resolve:                   # résolution DNS
    prober: dns
    dns:
      query_name: "intranet.lan"
      query_type: "A"
      valid_rcodes: ["NOERROR"]
```

Service systemd (note : ICMP nécessite `CAP_NET_RAW`) :

```ini
[Service]
User=blackbox_exporter
ExecStart=/usr/local/bin/blackbox_exporter \
  --config.file=/etc/blackbox_exporter/config.yml \
  --web.listen-address=127.0.0.1:9115
AmbientCapabilities=CAP_NET_RAW
```

## 52. blackbox_exporter : configuration Prometheus

Le pattern clé : `relabel_configs` réécrit `__address__` vers le blackbox et
passe la vraie cible en paramètre `__param_target`.

```yaml
  - job_name: 'blackbox-http'
    metrics_path: /probe
    params:
      module: [http_2xx]          # module par défaut
    file_sd_configs:
      - files: ['/etc/prometheus/file_sd/blackbox.json']
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target     # la vraie cible -> ?target=
      - source_labels: [__param_target]
        target_label: instance          # garder le nom lisible
      - target_label: __address__       # ...et scraper le blackbox lui-même
        replacement: '127.0.0.1:9115'

  - job_name: 'blackbox-icmp'
    metrics_path: /probe
    params:
      module: [icmp_ping]
    static_configs:
      - targets:
          - '192.168.10.1'      # passerelle
          - '192.168.10.20'     # onduleur
          - '8.8.8.8'           # contrôle sortie internet
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: '127.0.0.1:9115'
```

`/etc/prometheus/file_sd/blackbox.json` :

```json
[
  {"targets": ["https://intranet.lan", "https://webmail.lan"],
   "labels": {"module": "http_2xx", "equipe": "infra"}},
  {"targets": ["smtp.lan:25", "imap.lan:993"],
   "labels": {"module": "tcp_connect", "equipe": "infra"}}
]
```

> Pour choisir le module par cible via le fichier SD, ajouter un relabeling
> `source_labels: [module] → target_label: __param_module`.

## 53. blackbox_exporter : métriques et alertes

Métriques principales (préfixe `probe_`) :

| Métrique | Signification |
|---|---|
| `probe_success` | 1 = sonde OK, 0 = échec |
| `probe_duration_seconds` | durée totale de la sonde |
| `probe_http_status_code` | code HTTP retourné |
| `probe_http_duration_seconds{phase}` | détail par phase (connect, tls, processing…) |
| `probe_ssl_earliest_cert_expiry` | timestamp d'expiration du certificat |
| `probe_icmp_duration_seconds` | latence ping |
| `probe_dns_lookup_time_seconds` | temps de résolution DNS |

```promql
# Sonde en échec depuis plus de 5 min
probe_success == 0

# Latence HTTP p95 par cible
histogram_quantile(0.95, sum by (instance, le) (rate(probe_http_duration_seconds_bucket[5m])))

# Certificat qui expire dans moins de 21 jours
(probe_ssl_earliest_cert_expiry - time()) / 86400 < 21

# Détail : où part le temps ? (DNS ? TLS ? serveur ?)
avg by (instance, phase) (probe_http_duration_seconds)
```

> `probe_ssl_earliest_cert_expiry` : **l'alerte certificat** la plus fiable qui
> soit — oubliez les scripts maison.

## 54. snmp_exporter : principe et générateur

Le **snmp_exporter** interroge les équipements SNMP (switchs, routeurs,
onduleurs, imprimantes) et expose le résultat à Prometheus.

Particularité : la configuration (`snmp.yml`) est **générée** par le
`generator` à partir d'un `generator.yml` qui liste les MIBs à parcourir.
On ne l'écrit pas à la main.

```bash
# Générateur : cloner le repo et construire le snmp.yml
git clone https://github.com/prometheus/snmp_exporter.git
cd snmp_exporter/generator
# 1. Placer vos MIBs dans mibs/
# 2. Décrire les modules dans generator.yml :
```

`generator.yml` (extrait) :

```yaml
modules:
  switch_standard:          # module "générique" pour switchs
    walk: [sysUpTime, interfaces, ifXTable, ipNetToMediaTable]
    lookups:
      - source_labels: [ifIndex]
        target_label: ifDescr
    overrides:
      ifDescr:
        ignore: true        # on utilise ifAlias à la place si renseigné

  ups_eaton:                # module onduleur (MIB PowerMIB / XUPS-MIB)
    walk:
      - 1.3.6.1.4.1.534.1  # XUPS-MIB : tout l'arbre Eaton
    version: 2
    auth:
      community: public
```

```bash
go run generator.go generate   # produit snmp.yml
sudo cp snmp.yml /etc/snmp_exporter/snmp.yml
```

> Sans les MIBs du constructeur, le générateur ne peut pas nommer les OID :
> téléchargez-les (Eaton XUPS-MIB, APC PowerNet-MIB, MIBs constructeur du switch)
> et placez-les dans `mibs/`.

## 55. snmp_exporter : supervision des switchs

`prometheus.yml` — pattern identique au blackbox (paramètre `target`) :

```yaml
  - job_name: 'snmp-switch'
    metrics_path: /snmp
    params:
      module: [switch_standard]
      # auth: [v3-user]          # si SNMPv3 (voir section 56)
    static_configs:
      - targets:
          - '192.168.10.2'      # SW-CORE-01
          - '192.168.10.3'      # SW-ACC-01
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: '127.0.0.1:9116'   # snmp_exporter
```

Métriques utiles :

```promql
# Trafic par interface (en Mbit/s), avec le nom d'interface si ifAlias renseigné
sum by (instance, ifDescr) (
  rate(ifHCInOctets[5m]) or rate(ifInOctets[5m])
) * 8 / 1e6

# Interfaces en erreur
increase(ifInErrors[1h]) > 0 or increase(ifOutErrors[1h]) > 0

# Interface down (mais pas administrativement désactivée)
ifOperStatus != 1 and ifAdminStatus == 1

# Uptime de l'équipement (détecte un reboot : chute brutale)
sysUpTime
```

⚠️ Compteurs 32 bits (`ifInOctets`) vs 64 bits (`ifHCInOctets`) : sur les liens
rapides, le 32 bits boucle en quelques minutes → `rate()` fausse les calculs.
Toujours préférer `ifHCInOctets`/`ifHCOutOctets` quand l'équipement les expose.

## 56. snmp_exporter : supervision des onduleurs (UPS)

Les onduleurs exposent l'état batterie, la charge, les tensions via SNMP.
Deux grandes familles de MIB :

| Constructeur | MIB | OID racine typique |
|---|---|---|
| Eaton | XUPS-MIB | `1.3.6.1.4.1.534.1` |
| APC / Schneider | PowerNet-MIB | `1.3.6.1.4.1.318.1.1.1` |
| Standard | UPS-MIB (RFC 1628) | `1.3.6.1.2.1.33` |

Métriques typiques après génération (noms normalisés par le générateur) :

```promql
# Charge de l'onduleur en %
xupsOutputLoad            # Eaton
upsAdvOutputLoad          # APC

# Niveau batterie en %
xupsBatteryCapacity       # Eaton
upsAdvBatteryCapacity     # APC

# Tension batterie (V)
xupsBatteryVoltage

# Temps restant estimé (minutes)
xupsBatteryRunTimeRemaining
upsAdvBatteryRunTimeRemaining

# Température batterie (°C)
xupsBatteryTemperature
upsAdvBatteryTemperature
```

Alertes onduleur critiques :

```promql
# Sur batterie (pas sur secteur) — CRITIQUE
xupsInputSource == 2 or upsAdvInputLineStatus == 2

# Batterie faible (< 30 %)
xupsBatteryCapacity < 30 or upsAdvBatteryCapacity < 30

# Batterie à remplacer (drapeau constructeur)
xupsBatteryNeedsReplacement == 1 or upsAdvBatteryReplaceIndicator == 1

# Surcharge (> 90 %)
xupsOutputLoad > 90

# Température batterie anormale (> 35 °C : vieillissement accéléré)
xupsBatteryTemperature > 35
```

Configuration SNMPv3 (recommandée : SNMPv2c = community en clair) :

```yaml
# dans snmp.yml (généré), section auths :
auths:
  v3-securise:
    version: 3
    username: monitor
    security_level: authPriv
    auth_protocol: SHA
    auth_password: "xxx"
    priv_protocol: AES
    priv_password: "xxx"
```

```yaml
# prometheus.yml
  - job_name: 'snmp-ups'
    metrics_path: /snmp
    params:
      module: [ups_eaton]
      auth: [v3-securise]
    static_configs:
      - targets: ['192.168.10.20', '192.168.10.21']
    relabel_configs:
      - source_labels: [__address__]
        target_label: __param_target
      - source_labels: [__param_target]
        target_label: instance
      - target_label: __address__
        replacement: '127.0.0.1:9116'
```

> **Lien avec le métier** : couplez ces métriques avec le guide onduleurs
> (seuils de floating, loi d'Arrhenius : +10 °C = durée de vie / 2). Une alerte
> `xupsBatteryTemperature > 30` prolongée justifie une visite technique.

## 57. Exporters SQL : MySQL et PostgreSQL

**mysqld_exporter** (`:9104`) :

```bash
# Fichier d'authentification (jamais le mot de passe en ligne de commande !)
sudo tee /etc/mysqld_exporter/.my.cnf > /dev/null <<'EOF'
[client]
user=exporter
password=MotDePasseLong
host=127.0.0.1
EOF
sudo chmod 600 /etc/mysqld_exporter/.my.cnf
sudo chown mysqld_exporter:mysqld_exporter /etc/mysqld_exporter/.my.cnf
```

```sql
-- Utilisateur MySQL dédié (privilèges minimaux)
CREATE USER 'exporter'@'localhost' IDENTIFIED BY 'MotDePasseLong';
GRANT PROCESS, REPLICATION CLIENT, SELECT ON *.* TO 'exporter'@'localhost';
```

```promql
# Connexions utilisées vs max
mysql_global_status_threads_connected / mysql_global_variables_max_connections

# Requêtes lentes (toujours > 0 à surveiller en tendance)
increase(mysql_global_status_slow_queries[1h])

# Replication : retard en secondes (0 = OK)
mysql_slave_status_seconds_behind_master

# InnoDB : lectures disque vs buffer pool
rate(mysql_global_status_innodb_buffer_pool_reads[5m])
```

**postgres_exporter** (`:9187`) — via `DATA_SOURCE_NAME` :

```bash
DATA_SOURCE_NAME="postgresql://exporter:mdp@localhost:5432/postgres?sslmode=disable"
```

```promql
# Connexions vs max
pg_stat_database_numbackends / pg_settings_max_connections

# Taux de hit du cache (doit rester > 99 %)
sum(rate(pg_stat_database_blks_hit[5m])) /
  (sum(rate(pg_stat_database_blks_hit[5m])) + sum(rate(pg_stat_database_blks_read[5m])))

# Transactions wraparound : âge max (alerte si > 1 milliard)
max(pg_stat_database_datfrozenxid_age)

# Locks en attente
pg_locks_count{mode="exclusive", granted="false"} > 0
```

---

---

# PARTIE IV — Alertmanager et l'alerting

---

## 58. Alertmanager : architecture et concepts

Prometheus **évalue** les règles et envoie les alertes **en état firing** à
Alertmanager. C'est Alertmanager qui gère tout le reste :

```
Prometheus ──► Alertmanager ──┬──► email (SMTP)
    ▲                        ├──► Telegram (bot API)
    │ (renvoie tant          ├──► webhook (GLPI, script…)
    │  que firing)           ├──► Slack / Teams / PagerDuty…
                             └──► (silences, inhibitions, regroupements)
```

Concepts :

- **Déduplication** : une alerte identique reçue 10 fois = 1 notification.
- **Regroupement (grouping)** : "42 instances down" → 1 seul message groupé,
  pas 42 SMS à 3 h du matin.
- **Routage (routes)** : arbre de décision selon les labels
  (`severity=critical` → astreinte, `severity=warning` → email équipe).
- **Inhibition** : "si le datacenter est down, ne pas m'alerter sur chaque serveur".
- **Silences** : coupure volontaire et tracée des notifications (maintenance).

⚠️ Alertmanager **ne génère aucune alerte** : sans règles côté Prometheus,
il ne se passe rien. Et Prometheus sans Alertmanager = alertes visibles
uniquement dans l'UI (inutile en production).

## 59. Alertmanager : installation

```bash
AM_VERSION="0.28.0"
cd /tmp
wget "https://github.com/prometheus/alertmanager/releases/download/v${AM_VERSION}/alertmanager-${AM_VERSION}.linux-amd64.tar.gz"
tar xzf "alertmanager-${AM_VERSION}.linux-amd64.tar.gz"
sudo cp "alertmanager-${AM_VERSION}.linux-amd64/alertmanager" /usr/local/bin/
sudo cp "alertmanager-${AM_VERSION}.linux-amd64/amtool" /usr/local/bin/
sudo mkdir -p /etc/alertmanager /var/lib/alertmanager
sudo cp "alertmanager-${AM_VERSION}.linux-amd64/alertmanager.yml" /etc/alertmanager/
sudo useradd --no-create-home --shell /usr/sbin/nologin alertmanager
sudo chown -R alertmanager:alertmanager /etc/alertmanager /var/lib/alertmanager
```

`/etc/systemd/system/alertmanager.service` :

```ini
[Unit]
Description=Alertmanager
Wants=network-online.target
After=network-online.target

[Service]
User=alertmanager
Group=alertmanager
Type=simple
Restart=always
ExecStart=/usr/local/bin/alertmanager \
  --config.file=/etc/alertmanager/alertmanager.yml \
  --storage.path=/var/lib/alertmanager \
  --web.listen-address=127.0.0.1:9093 \
  --cluster.listen-address=""     # vide = pas de cluster (voir section 76)

[Install]
WantedBy=multi-user.target
```

Vérification : `amtool check-config /etc/alertmanager/alertmanager.yml`
puis `curl -X POST http://127.0.0.1:9093/-/reload` après modification.

## 60. Configuration Alertmanager : squelette

`/etc/alertmanager/alertmanager.yml` :

```yaml
global:
  resolve_timeout: 5m            # "résolue" si plus reçue pendant 5 min
  smtp_smarthost: 'smtp.lan:587'
  smtp_from: 'alertes@entreprise.lan'
  smtp_auth_username: 'alertes@entreprise.lan'
  smtp_auth_password: 'xxx'       # idéalement via fichier (voir section 84)
  smtp_require_tls: true

# Arbre de routage (évalué de haut en bas, première route qui matche)
route:
  group_by: ['alertname', 'datacenter']
  group_wait: 30s                 # attend 30 s pour grouper les alertes liées
  group_interval: 5m              # nouvel envoi pour le même groupe toutes les 5 min
  repeat_interval: 4h             # répéter la notification toutes les 4 h si toujours firing
  receiver: 'equipe-infra-email'
  routes:
    - matchers:
        - severity = "critical"
      receiver: 'astreinte-telegram'
      continue: false
    - matchers:
        - severity = "warning"
      receiver: 'equipe-infra-email'

receivers:
  - name: 'equipe-infra-email'
    email_configs:
      - to: 'infra@entreprise.lan'
        headers:
          Subject: '[{{ .Status | toUpper }}] {{ .GroupLabels.alertname }}'

  - name: 'astreinte-telegram'
    telegram_configs:
      - bot_token: 'XXX'
        chat_id: -123456789
        message: |
          {{ if eq .Status "firing" }}🔥{{ else }}✅{{ end }}
          <b>{{ .GroupLabels.alertname }}</b>
          {{ range .Alerts }}{{ .Annotations.summary }}
          {{ end }}
        parse_mode: HTML
```

## 61. Le routage : arbre de décision

Les `routes` forment un arbre : chaque alerte descend jusqu'à la première
feuille qui matche. `continue: true` permet de continuer vers les routes sœurs.

```yaml
route:
  receiver: 'defaut'
  group_by: ['alertname']
  routes:
    # 1. Critiques infra -> Telegram astreinte + email
    - matchers: ['severity = "critical"', 'equipe = "infra"']
      receiver: 'astreinte'
      continue: true            # ...et aussi la route suivante
    - matchers: ['severity = "critical"']
      receiver: 'direction-email'
    # 2. Onduleurs -> équipe énergie (même en warning)
    - matchers: ['equipement =~ "ups.*"']
      receiver: 'equipe-energie'
    # 3. Heures ouvrées uniquement pour les warnings non critiques
    - matchers: ['severity = "warning"']
      receiver: 'equipe-infra-email'
      active_time_intervals: ['heures-ouvrees']

time_intervals:
  - name: heures-ouvrees
    time_intervals:
      - weekdays: ['monday:friday']
        times:
          - start_time: '08:00'
            end_time: '18:00'
```

> Les `time_intervals` (noms au pluriel dans la config !) permettent le
> "pas d'alerte warning la nuit" sans bidouille PromQL dépendante de l'UTC.

## 62. Receiver email : bien le configurer

```yaml
receivers:
  - name: 'equipe-infra-email'
    email_configs:
      - to: 'infra@entreprise.lan'
        from: 'alertes@entreprise.lan'
        smarthost: 'smtp.lan:587'
        auth_username: 'alertes@entreprise.lan'
        auth_password_file: '/etc/alertmanager/smtp_pass'  # mieux que en clair
        require_tls: true
        headers:
          Subject: '[{{ .Status | toUpper }}:{{ .CommonLabels.severity }}] {{ .CommonLabels.alertname }} ({{ .Alerts | len }} instance(s))'
        html: |
          <h2>{{ .CommonLabels.alertname }} — {{ .Status }}</h2>
          <table>
          {{ range .Alerts }}
            <tr><td>{{ .Labels.instance }}</td><td>{{ .Annotations.summary }}</td></tr>
          {{ end }}
          </table>
          <p><a href="{{ .ExternalURL }}">Voir dans Alertmanager</a></p>
```

⚠️ Toujours inclure `{{ .Alerts | len }}` ou la liste des instances : un email
"InstanceDown" sans dire **laquelle** est inutile.

## 63. Receiver Telegram

```yaml
  - name: 'astreinte-telegram'
    telegram_configs:
      - bot_token_file: '/etc/alertmanager/telegram_token'  # fichier, pas en clair
        chat_id: -123456789        # ID du groupe (négatif) ou de l'utilisateur
        message: |
          {{ if eq .Status "firing" }}🔥 <b>ALERTE</b>{{ else }}✅ <b>RÉSOLUE</b>{{ end }}
          <b>{{ .CommonLabels.alertname }}</b> [{{ .CommonLabels.severity }}]
          {{ range .Alerts -}}
          • <code>{{ .Labels.instance }}</code> : {{ .Annotations.summary }}
          {{ end }}
          <a href="{{ .ExternalURL }}">Alertmanager</a>
        parse_mode: HTML
        disable_notifications: false
```

Créer le bot via `@BotFather`, récupérer le token, ajouter le bot au groupe,
récupérer le `chat_id` via `https://api.telegram.org/bot<TOKEN>/getUpdates`.

## 64. Receiver webhook : brancher GLPI, scripts, etc.

```yaml
  - name: 'glpi-webhook'
    webhook_configs:
      - url: 'https://glpi.lan/plugins/alertes/prometheus'
        http_config:
          basic_auth:
            username: 'prometheus'
            password_file: '/etc/alertmanager/glpi_pass'
        send_resolved: true
        max_alerts: 10
```

Le webhook reçoit un JSON avec `status`, `alerts[]` (labels, annotations,
`startsAt`, `endsAt`). Côté GLPI/script : créer un ticket à `firing`, le clore
à `resolved` (corrélation par `alertname` + `instance` = clé du ticket).

Exemple minimal de script récepteur (Python) :

```python
from flask import Flask, request
app = Flask(__name__)

@app.route("/hook", methods=["POST"])
def hook():
    data = request.json
    for alert in data["alerts"]:
        key = (alert["labels"]["alertname"], alert["labels"].get("instance", "?"))
        if data["status"] == "firing":
            creer_ticket(key, alert["annotations"].get("summary", ""))
        else:
            clore_ticket(key)
    return "ok"
```

## 65. Inhibition : couper le bruit en cascade

Si le switch d'un site tombe, inutile de recevoir 30 alertes "serveur down" :
**l'alerte parente inhibe les enfants**.

```yaml
inhibit_rules:
  # Si un site est injoignable, inhiber les alertes unitaires de ce site
  - source_matchers:
      - alertname = "SiteDown"
    target_matchers:
      - severity =~ "critical|warning"
    equal: ['site']              # même site des deux côtés

  # Si Prometheus ne scrape plus, inhiber les alertes "métier" basées sur le scrape
  - source_matchers:
      - alertname = "PrometheusScrapeDown"
    target_matchers:
      - alertname =~ "Instance.*"
    equal: ['job']
```

Logique : `source` (l'alerte grave) **inhibe** `target` (les alertes dérivées)
quand les labels `equal` correspondent. L'alerte inhibée reste visible dans
l'UI (barrée), elle n'est juste pas notifiée.

## 66. Silences : maintenance propre et tracée

Un **silence** coupe les notifications pour un sélecteur de labels, avec début,
fin, auteur et commentaire. À créer dans l'UI (`:9093` → Silences → New Silence)
ou via `amtool` :

```bash
# Silence de 2 h sur toutes les alertes du serveur en maintenance
amtool silence add \
  --alertmanager.url=http://127.0.0.1:9093 \
  --comment="Maintenance planifiée - changement disques" \
  --author="zelef" \
  --duration=2h \
  instance="srv-db01:9100"

# Lister / expirer
amtool silence query --alertmanager.url=http://127.0.0.1:9093
amtool silence expire <id> --alertmanager.url=http://127.0.0.1:9093
```

> **Règle d'équipe** : toute maintenance = un silence **avec commentaire et
> durée limitée**. Un silence "oublié" qui court encore 6 mois après = des
> alertes perdues. Auditez les silences actifs chaque semaine.

## 67. Regroupement : group_by, group_wait, repeat_interval

```yaml
route:
  group_by: ['alertname', 'datacenter', 'severity']
  group_wait: 30s        # à la 1re alerte d'un groupe, attendre 30 s (rafale initiale)
  group_interval: 5m     # si d'autres alertes rejoignent le groupe, notifier toutes les 5 min max
  repeat_interval: 4h    # tant que ça sonne, rappeler toutes les 4 h
```

Bien choisir `group_by` :

- Trop fin (`instance`) → N notifications pour N serveurs (bruit).
- Trop large (rien) → un seul message fourre-tout illisible.
- Bon compromis : `['alertname', 'datacenter']` ou `['alertname', 'equipe']`.

⚠️ `repeat_interval: 4h` sur du critical, `12h` ou `24h` sur du warning.
Un `repeat_interval` trop court = harcèlement ; trop long = on oublie l'alerte.

## 68. Bonnes pratiques d'alerting (l'anti-bruit)

1. **Alerter sur les symptômes, pas sur les causes** : "le site est lent"
   (latence p95) plutôt que "CPU à 82 %". Le CPU à 82 % n'est pas un problème
   en soi.
2. **`for:` toujours** : 5 min minimum pour le critical, 15-30 min pour le warning.
   Zéro `for` = alerte sur un à-coup.
3. **Seuils avec marge** : alerter à 85 % disque (warning) puis 95 % (critical),
   pas à 99 %.
4. **Chaque alerte = une action** : si personne ne sait quoi faire quand elle
   sonne, c'est une mauvaise alerte. Ajouter un **runbook** en annotation.
5. **Severities limitées** : `critical` (astreinte, nuit), `warning` (équipe,
   heures ouvrées). Pas de `info` notifié (dashboard uniquement).
6. **Pas d'alerte sur des compteurs bruts** : toujours `rate()`/`increase()`.
7. **Tester** : `promtool test rules` + déclencher volontairement 1×/trimestre.
8. **Revue trimestrielle** : toute alerte ignorée 3 fois = seuil à revoir ou
   alerte à supprimer. Le bruit tue la vigilance.

## 69. Exemples d'alertes production (prêtes à l'emploi)

`/etc/prometheus/rules/alertes.yml` :

```yaml
groups:
  - name: alertes-infra
    interval: 30s
    rules:
      # --- Disponibilité ---
      - alert: InstanceDown
        expr: up == 0
        for: 5m
        labels: {severity: critical, equipe: infra}
        annotations:
          summary: "Cible DOWN : {{ $labels.instance }} (job {{ $labels.job }})"
          description: "Aucun scrape réussi depuis 5 min. Vérifier réseau/service."
          runbook: "https://wiki.lan/runbooks/instance-down"

      - alert: BlackboxProbeFailed
        expr: probe_success == 0
        for: 5m
        labels: {severity: critical, equipe: infra}
        annotations:
          summary: "Sonde {{ $labels.job }} en échec sur {{ $labels.instance }}"

      # --- Disques ---
      - alert: DisqueBientotPlein
        expr: |
          100 * (1 - node_filesystem_avail_bytes{fstype!~"tmpfs|overlay|squashfs"}
                     / node_filesystem_size_bytes) > 85
        for: 30m
        labels: {severity: warning, equipe: infra}
        annotations:
          summary: "Disque {{ $labels.mountpoint }} à {{ $value | printf \"%.1f\" }} % sur {{ $labels.instance }}"

      - alert: DisqueCritique
        expr: |
          100 * (1 - node_filesystem_avail_bytes{fstype!~"tmpfs|overlay|squashfs"}
                     / node_filesystem_size_bytes) > 95
        for: 10m
        labels: {severity: critical, equipe: infra}
        annotations:
          summary: "Disque {{ $labels.mountpoint }} à {{ $value | printf \"%.1f\" }} % sur {{ $labels.instance }}"

      - alert: DisqueSatureSous3Jours
        expr: predict_linear(node_filesystem_avail_bytes{fstype!~"tmpfs|overlay|squashfs"}[6h], 3*24*3600) < 0
        for: 1h
        labels: {severity: warning, equipe: infra}
        annotations:
          summary: "Tendance : disque {{ $labels.mountpoint }} plein sous 3 j sur {{ $labels.instance }}"

      - alert: InodesEpuises
        expr: 100 * (1 - node_filesystem_files_free / node_filesystem_files) > 85
        for: 30m
        labels: {severity: warning, equipe: infra}
        annotations:
          summary: "Inodes {{ $labels.mountpoint }} à {{ $value | printf \"%.1f\" }} % sur {{ $labels.instance }}"

      # --- Mémoire / CPU ---
      - alert: MemoireCritique
        expr: node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes < 0.1
        for: 10m
        labels: {severity: critical, equipe: infra}
        annotations:
          summary: "RAM disponible < 10 % sur {{ $labels.instance }}"

      - alert: ChargeElevee
        expr: node_load5 > 2 * count by (instance) (node_cpu_seconds_total{mode="idle"})
        for: 15m
        labels: {severity: warning, equipe: infra}
        annotations:
          summary: "Load5 ({{ $value | printf \"%.1f\" }}) > 2× CPU sur {{ $labels.instance }}"

      # --- Certificats ---
      - alert: CertificatExpireBientot
        expr: (probe_ssl_earliest_cert_expiry - time()) / 86400 < 21
        for: 1h
        labels: {severity: warning, equipe: infra}
        annotations:
          summary: "Certificat {{ $labels.instance }} expire dans {{ $value | printf \"%.0f\" }} j"

      # --- Onduleurs ---
      - alert: UpsSurBatterie
        expr: xupsInputSource == 2 or upsAdvInputLineStatus == 2
        for: 2m
        labels: {severity: critical, equipe: energie}
        annotations:
          summary: "Onduleur {{ $labels.instance }} SUR BATTERIE !"
          description: "Coupure secteur détectée. Vérifier groupe électrogène."

      - alert: UpsBatterieFaible
        expr: xupsBatteryCapacity < 30 or upsAdvBatteryCapacity < 30
        for: 5m
        labels: {severity: critical, equipe: energie}
        annotations:
          summary: "Batterie onduleur {{ $labels.instance }} à {{ $value }} %"

      - alert: UpsBatterieAChanger
        expr: xupsBatteryNeedsReplacement == 1 or upsAdvBatteryReplaceIndicator == 1
        for: 1h
        labels: {severity: warning, equipe: energie}
        annotations:
          summary: "Batterie à remplacer sur {{ $labels.instance }} — planifier intervention"
```

## 70. Règles d'alerting : les 10 erreurs à ne pas commettre

| # | Erreur | Correction |
|---|---|---|
| 1 | Pas de `for:` | Toujours `for: 5m` minimum |
| 2 | Seuil sur compteur brut (`http_requests_total > 1000`) | `rate(...[5m]) > X` |
| 3 | Fenêtre trop courte (`rate(...[1m])`) | `[5m]` minimum en alerte |
| 4 | Alerte sans label `severity` | Le routage en dépend |
| 5 | Pas de `summary`/`description` | Annotations obligatoires |
| 6 | Même seuil warning/critical | warning < critical, `for` plus long en warning |
| 7 | Alerter sur `up` sans `for` | Un scrape raté isolé ≠ panne |
| 8 | Oublier `or` entre deux MIB (Eaton/APC) | Les deux variantes dans l'expr |
| 9 | Comparaison UTC non décalée | Utiliser les time_intervals d'Alertmanager |
| 10 | Jamais tester | `promtool test rules` + exercice trimestriel |

---

---

# PARTIE V — Fédération, stockage distant, sauvegarde, HA

---

## 71. Fédération : agréger plusieurs Prometheus

La **fédération** permet à un Prometheus "global" de scraper les métriques
(agrégées) d'autres Prometheus "locaux" (par site, par datacenter).

Sur le Prometheus **global** :

```yaml
  - job_name: 'federate-dc-paris'
    honor_labels: true            # garder job/instance d'origine !
    metrics_path: '/federate'
    params:
      'match[]':
        - '{job="node-linux"}'           # quelles séries rapatrier…
        - '{__name__=~"job:.*"}'         # …dont les recording rules
        - '{__name__=~"instance:.*"}'
    static_configs:
      - targets: ['prom-dc-paris.lan:9090']
```

Bonnes pratiques :

- Ne fédérer que des **métriques agrégées** (recording rules `job:...`,
  `instance:...`), jamais les séries brutes → sinon le global explose.
- `honor_labels: true` **obligatoire** (sinon tout devient `job="federate"`).
- `external_labels: {datacenter: 'dc-paris'}` côté local pour distinguer.

```
 [Prometheus dc-paris] ──┐
 [Prometheus dc-lyon]  ──┼──federate──► [Prometheus GLOBAL] ──► Grafana
 [Prometheus usine]    ──┘                    │
                                              └──► Alertmanager global
```

## 72. Remote write : envoyer vers un stockage distant

`remote_write` envoie les échantillons **au fil de l'eau** vers un stockage
long terme (Thanos, Mimir/Cortex, VictoriaMetrics, InfluxDB…) :

```yaml
remote_write:
  - url: 'https://thanos-receive.lan/api/v1/receive'
    name: 'thanos-central'
    queue_config:
      capacity: 10000
      max_shards: 50
      min_shards: 1
      max_samples_per_send: 5000
      batch_send_deadline: 5s
    write_relabel_configs:       # n'envoyer que l'utile !
      - source_labels: [__name__]
        regex: 'go_.*|process_.*'
        action: drop
```

Points d'attention :

- `write_relabel_configs` : **filtrer avant envoi** (coût réseau/stockage).
- Le remote_write a un **buffer local** (WAL) : en cas de coupure réseau,
  il rejoue (jusqu'à la taille du buffer, puis il jette).
- Métrique de santé : `prometheus_remote_storage_shards`,
  `prometheus_remote_storage_samples_dropped_total`.

## 73. Remote read : lire depuis le stockage distant

```yaml
remote_read:
  - url: 'https://thanos-query.lan/api/v1/read'
    name: 'thanos-central'
    read_recent: true     # les données récentes sont lues en local (plus rapide)
```

`read_recent: true` : Prometheus lit d'abord sa TSDB locale pour le récent,
puis le distant pour l'ancien. Sans ça, tout passe par le réseau.

⚠️ Remote read = **dépendance réseau** pour Grafana. En cas de coupure,
prévoir un dashboard "local only" ou accepter la dégradation.

## 74. Sauvegarde : snapshots TSDB via l'API

Activer l'API admin au démarrage :

```bash
--web.enable-admin-api
```

Créer un snapshot (cohérent, sans arrêter Prometheus) :

```bash
curl -X POST http://localhost:9090/api/v1/admin/tsdb/snapshot
# {"status":"success","data":{"name":"20260926T233000Z-abcdef1234"}}
# -> snapshot dans /var/lib/prometheus/snapshots/20260926T233000Z-abcdef1234
```

Script de sauvegarde quotidienne (`/usr/local/bin/backup-prometheus.sh`) :

```bash
#!/bin/bash
set -euo pipefail
PROM="http://127.0.0.1:9090"
DEST="/backup/prometheus"
RETENTION_JOURS=7

SNAP=$(curl -s -X POST "$PROM/api/v1/admin/tsdb/snapshot" | grep -o '[0-9TZ-]*-[a-f0-9]*' | head -1)
[ -z "$SNAP" ] && { echo "ERREUR: snapshot vide"; exit 1; }

# Compresser + chiffrer éventuellement, puis copier hors machine
tar -czf "$DEST/prometheus-$SNAP.tar.gz" -C /var/lib/prometheus "snapshots/$SNAP"

# Nettoyer le snapshot source (il reste sur disque sinon !)
curl -s -X POST "$PROM/api/v1/admin/tsdb/delete_series?match[]={__name__=~\".+\"}" > /dev/null || true
# ^ non : delete_series ne supprime pas les snapshots. Nettoyage manuel :
rm -rf "/var/lib/prometheus/snapshots/$SNAP"

# Purge des vieilles archives
find "$DEST" -name 'prometheus-*.tar.gz' -mtime +$RETENTION_JOURS -delete
echo "Sauvegarde OK : $DEST/prometheus-$SNAP.tar.gz"
```

⚠️ **Deux pièges** :

1. Le snapshot **reste dans** `snapshots/` après création : il faut le supprimer
   à la main après archivage, sinon le disque se remplit.
2. `delete_series` ne libère l'espace qu'après **compaction** (~2 h) et ne touche
   pas aux snapshots. Pour vraiment purger, il faut supprimer les blocs.

> **Règle 3-2-1** : 3 copies, 2 supports, 1 hors site. Le snapshot seul sur la
> même machine n'est pas une sauvegarde.

## 75. Rétention : dimensionner le disque

```bash
--storage.tsdb.retention.time=30d
--storage.tsdb.retention.size=50GB
```

Estimation : Prometheus compresse à ~**1 à 2 octets par échantillon**.

```
Espace ≈ nb_séries × (86400 / scrape_interval) × jours × 1,5 octet
```

Exemple : 100 000 séries, scrape 15 s, 30 jours :

```
100 000 × 5760 échantillons/jour × 30 j × 1,5 o ≈ 26 Go
```

Commandes de diagnostic :

```promql
# Nombre de séries actives (l'indicateur n°1 à surveiller)
prometheus_tsdb_head_series

# Taille WAL / blocs
prometheus_tsdb_wal_segment_current
prometheus_tsdb_size_retention_total_bytes
```

⚠️ Si `retention.size` est atteinte, Prometheus **purge les blocs les plus
anciens sans prévenir** (c'est son fonctionnement normal, pas un bug).

## 76. Haute disponibilité : la paire de Prometheus

Le pattern standard : **2 Prometheus identiques** qui scrapent les mêmes cibles
(scrape redondant), avec `external_labels` différents :

```yaml
# prom-01
global:
  external_labels: {replica: 'prom-01', datacenter: 'dc-paris'}

# prom-02 : identique sauf replica: 'prom-02'
```

Côté Alertmanager : **cluster** (gossip) pour dédupliquer :

```bash
# alertmanager-01
--cluster.listen-address=0.0.0.0:9094
--cluster.peer=alertmanager-02.lan:9094

# alertmanager-02 : miroir
```

Et Prometheus pointe vers les deux :

```yaml
alerting:
  alertmanagers:
    - static_configs:
        - targets: ['am-01.lan:9093', 'am-02.lan:9093']
```

Schéma :

```
 [Prometheus 01] ──┐
                   ├──► [Alertmanager cluster] ──► notifications (1 seule fois)
 [Prometheus 02] ──┘
```

⚠️ Sans cluster Alertmanager, chaque Prometheus enverrait ses propres
notifications → **doublons**. Le cluster gossip déduplique.

Limite : les deux Prometheus stockent en double (2× disque). Pour aller plus
loin : Thanos / Mimir.

## 77. Haute disponibilité : Thanos (introduction)

**Thanos** ajoute au-dessus de Prometheus : stockage objet illimité (S3),
requête globale multi-clusters, déduplication des réplicas, downsampling.

```
 [Prometheus 01] ──sidecar──┐
                            ├──► [Thanos Store / S3] ──► [Thanos Query] ──► Grafana
 [Prometheus 02] ──sidecar──┘         (rétention infinie)
```

Composants :

| Composant | Rôle |
|---|---|
| **Sidecar** | Upload les blocs TSDB vers S3, proxy les requêtes |
| **Store Gateway** | Lit l'historique depuis S3 |
| **Compactor** | Compacte + downsample (5 min, 1 h) pour les longues périodes |
| **Query** | Point d'entrée unique : fusionne sidecars + store |
| **Ruler** | Évalue les règles sur les données globales |

Quand l'adopter : multi-sites, besoin de rétention > 90 jours, ou quand la
paire de Prometheus ne suffit plus. Pour un seul site avec 30 jours de
rétention, **la paire suffit** — ne pas sur-ingénierer.

## 78. Haute disponibilité : Mimir / Cortex (introduction)

**Grafana Mimir** (fork de Cortex) : TSDB distribuée, compatible remote_write /
remote_read, multi-tenant, haute dispo native.

```
 [Prometheus × N] ──remote_write──► [Mimir] ──► Grafana (1 an+ de rétention)
```

| | Paire Prometheus | Thanos | Mimir |
|---|---|---|---|
| Complexité | Faible | Moyenne | Élevée |
| Rétention longue | Non | Oui (S3) | Oui (S3) |
| Requête globale | Non (fédération manuelle) | Oui | Oui |
| Idéal pour | 1 site, < 6 mois | Multi-sites, S3 dispo | Gros volumes, multi-tenant |

> **Conseil de chef de service** : commencez par **une paire bien configurée +
> snapshots quotidiens**. Migrez vers Thanos/Mimir quand le besoin (rétention
> longue, multi-sites) est avéré, pas avant.

---

# PARTIE VI — Cardinalité, sécurité

---

## 79. La cardinalité : le piège n°1 de Prometheus

La **cardinalité** = le nombre de séries temporelles uniques. C'est **le**
facteur limitant de Prometheus : chaque série coûte de la RAM (~1-2 Ko en head)
et ralentit les requêtes.

```
1 métrique × 10 valeurs de label A × 10 valeurs de label B = 100 séries
1 métrique × 10 000 user_id × 100 chemins = 1 000 000 000 séries  💥
```

Ordres de grandeur :

| Séries actives | État |
|---|---|
| < 1 M | Confortable |
| 1 – 5 M | OK avec RAM adaptée (8-16 Go) |
| 5 – 10 M | Zone rouge : optimiser |
| > 10 M | OOM probable, requêtes lentes |

Surveiller : `prometheus_tsdb_head_series` + page **Status → TSDB**
(top 10 des métriques par nombre de séries).

## 80. Cardinalité : ce qu'il ne faut JAMAIS faire

```python
# ❌ CATASTROPHIQUE : un label par utilisateur / par email / par IP client
http_requests_total{user_id="48293", email="jean.dupont@x.lan"}
login_attempts_total{client_ip="10.0.4.128"}

# ❌ Très mauvais : chemins d'URL non normalisés (1 série par URL !)
http_requests_total{path="/produit/12893", path="/produit/12894", ...}

# ❌ Mauvais : ID de requête, UUID, timestamp en label
job_duration_seconds{job_id="a3f8c9e2-..."}

# ✅ BON : labels à cardinalité bornée et connue
http_requests_total{method="GET", status="200", route="/produit/:id"}
#                         (5 valeurs)  (10)        (50 routes) = 2 500 séries max
```

**Règle d'or** : un label doit avoir un nombre de valeurs **borné, petit et
prévisible** (< 100 en général). Tout identifiant unique (user_id, IP client,
UUID, chemin brut) est **interdit** en label.

Autres pièges :

- `labelmap` qui promeut tous les labels Kubernetes (`__meta_kubernetes_*`)
  sans filtrer.
- Exporter applicatif qui expose un label `version` incluant le hash de build.
- `info`-métriques (`node_uname_info`, `target_info`) : 1 série par cible,
  c'est OK, mais ne pas les multiplier.

## 81. Cardinalité : diagnostiquer et corriger

Diagnostic :

```promql
# Top 10 des métriques les plus lourdes (par nombre de séries)
topk(10, count by (__name__) ({__name__=~".+"}))

# Nombre de séries d'une métrique suspecte
count({__name__="http_requests_total"})

# Quelles valeurs de label explosent ?
count by (path) ({__name__="http_requests_total"})
```

Corrections (par ordre de préférence) :

1. **Côté application** : supprimer/normaliser le label fautif (vrai fix).
2. **`metric_relabel_configs: drop`** : jeter la métrique ou le label
   avant ingestion (section 21).
3. **`labeldrop`** : supprimer juste le label à haute cardinalité, garder la métrique :

```yaml
metric_relabel_configs:
  - regex: 'user_id|client_ip|job_id'
    action: labeldrop
```

4. **Agrégation en amont** : recording rule qui `sum by` sans le label fautif,
   puis drop de la métrique brute.

⚠️ `labeldrop` après ingestion massive ne récupère pas la RAM immédiatement :
le head se vide au fil des compactions. Le vrai fix reste côté émetteur.

## 82. Sécurité : authentification sur l'interface web

Prometheus natif n'a **pas d'authentification** (voulu : simplicité). Options :

**a) Basic auth native (depuis Prometheus 2.x, fichier web.yml)** :

```yaml
# /etc/prometheus/web.yml
basic_auth_users:
  admin: '$2y$05$...'     # hash bcrypt généré par htpasswd
  lecteur: '$2y$05$...'
```

```bash
htpasswd -nBC 10 "" | tr -d ':\n'   # génère le hash (paquet apache2-utils)
```

```bash
--web.config.file=/etc/prometheus/web.yml
```

**b) Reverse proxy (recommandé)** : nginx/Apache devant, avec auth + TLS +
ACL IP. Voir section 83.

**c) Ne pas exposer** : `web.listen-address=127.0.0.1:9090` + accès via SSH
ou VPN uniquement. Le plus simple est souvent le plus sûr.

## 83. Sécurité : TLS et reverse proxy

Nginx devant Prometheus (TLS + basic auth + restriction IP) :

```nginx
server {
    listen 443 ssl;
    server_name prometheus.lan;

    ssl_certificate     /etc/ssl/prometheus.lan.crt;
    ssl_certificate_key /etc/ssl/prometheus.lan.key;
    ssl_protocols TLSv1.2 TLSv1.3;

    # API reload protégée : seul le réseau admin
    location = /-/reload {
        allow 10.0.0.0/8;
        deny all;
        auth_basic "admin";
        auth_basic_user_file /etc/nginx/.htpasswd;
        proxy_pass http://127.0.0.1:9090;
    }

    location / {
        allow 10.0.0.0/8;       # réseau interne uniquement
        deny all;
        auth_basic "Prometheus";
        auth_basic_user_file /etc/nginx/.htpasswd;
        proxy_pass http://127.0.0.1:9090;
        proxy_set_header Host $host;
    }
}
```

Côté scrape en HTTPS vers les exporters :

```yaml
  - job_name: 'node-tls'
    scheme: https
    tls_config:
      ca_file: '/etc/prometheus/certs/ca.crt'
      # cert_file: '/etc/prometheus/certs/client.crt'   # si mTLS
      # key_file: '/etc/prometheus/certs/client.key'
      insecure_skip_verify: false   # jamais true en production !
    basic_auth:
      username: 'prometheus'
      password_file: '/etc/prometheus/node_pass'  # fichier, pas en clair
    static_configs:
      - targets: ['srv-dmz01:9100']
```

## 84. Sécurité : durcissement en production

Checklist :

- [ ] Utilisateur dédié non-root (`prometheus`, `node_exporter`…).
- [ ] `web.listen-address=127.0.0.1:9090` si accès via reverse proxy/SSH.
- [ ] Mots de passe **jamais en clair** : `password_file`, `*_token_file`.
- [ ] `--web.enable-admin-api` : à n'activer que si besoin (snapshots/delete),
  et protéger via ACL IP.
- [ ] `--web.enable-lifecycle` : protéger `/-/quit` et `/-/reload` (ACL IP).
- [ ] Fichiers de config en `640` propriétaire `prometheus:prometheus`
  (ils contiennent des secrets).
- [ ] SNMP : **v3 authPriv**, jamais v2c en clair sur un réseau non maîtrisé.
- [ ] Alertmanager : `bot_token_file`, `auth_password_file` (pas en clair).
- [ ] Sauvegardes chiffrées si elles quittent le site.
- [ ] Mettre à jour régulièrement (faille Go ou UI possible) — voir section 86.
- [ ] Journaliser les accès admin (nginx access_log sur `/-/reload`, `/api/v1/admin`).

---

---

# PARTIE VII — Supervision de Prometheus, mise à jour, dépannage

---

## 85. Supervision de Prometheus lui-même (méta-monitoring)

"Qui surveille le surveillant ?" — **un second Prometheus** (celui de la paire
HA, section 76) ou, à minima, des checks externes. Métriques vitales que
Prometheus expose sur lui-même :

```promql
# --- Santé du scrape ---
up{job="node-linux"} == 0                          # cibles down
increase(prometheus_target_scrapes_exceeded_sample_limit_total[1h]) > 0   # sample_limit atteinte
rate(prometheus_target_scrape_pool_exceeded_target_limit_total[5m]) > 0

# --- Performance des requêtes ---
histogram_quantile(0.95, rate(prometheus_engine_query_duration_seconds_bucket[5m])) > 1
# p95 des requêtes > 1 s = requêtes trop lourdes

# --- Mémoire / TSDB ---
prometheus_tsdb_head_series                          # séries actives (tendance !)
prometheus_tsdb_head_chunks
rate(prometheus_tsdb_head_gc_duration_seconds_sum[5m])  # GC trop long = mémoire sous pression
prometheus_tsdb_compactions_failed_total

# --- Stockage ---
prometheus_tsdb_retention_limit_bytes - prometheus_tsdb_size_retention_total_bytes < 5e9
# moins de 5 Go de marge avant purge forcée

# --- Règles ---
prometheus_rule_evaluation_failures_total > 0        # règles en erreur
prometheus_rule_group_interval_seconds - prometheus_rule_group_last_duration_seconds < 0
# évaluation plus longue que l'intervalle = règles trop lourdes

# --- Alertmanager ---
prometheus_notifications_dropped_total > 0           # alertes perdues !
rate(prometheus_notifications_failed_total[5m]) > 0
```

Alertes méta-minimales à mettre en place **en priorité** :

```yaml
- alert: PrometheusDown
  expr: up{job="prometheus"} == 0
  for: 5m
  labels: {severity: critical}
- alert: PrometheusSeriesExplosion
  expr: increase(prometheus_tsdb_head_series[1h]) > 100000
  for: 30m
  labels: {severity: warning}
  annotations: {summary: "Explosion du nombre de séries : vérifier la cardinalité"}
- alert: PrometheusNotificationsDropped
  expr: increase(prometheus_notifications_dropped_total[10m]) > 0
  for: 5m
  labels: {severity: critical}
```

> **Règle d'or** : les alertes `PrometheusDown` et `AlertmanagerDown` doivent
> être supervisées par un système **externe** (deuxième site, check HTTP externe)
> : si tout votre monitoring est down, personne ne vous préviendra.

## 86. Mise à jour de Prometheus

Procédure sans perte de données (TSDB compatible ascendante entre mineures) :

```bash
# 1. Sauvegarde : snapshot + config
curl -X POST http://127.0.0.1:9090/api/v1/admin/tsdb/snapshot
cp -a /etc/prometheus /root/prometheus-config-backup-$(date +%F)

# 2. Arrêt propre (flush du WAL)
sudo systemctl stop prometheus

# 3. Remplacement du binaire
cd /tmp
wget "https://github.com/prometheus/prometheus/releases/download/vX.Y.Z/prometheus-X.Y.Z.linux-amd64.tar.gz"
tar xzf prometheus-X.Y.Z.linux-amd64.tar.gz
sudo cp prometheus-X.Y.Z.linux-amd64/prometheus /usr/local/bin/
sudo cp prometheus-X.Y.Z.linux-amd64/promtool /usr/local/bin/

# 4. Vérification config (les flags dépréciés peuvent casser le démarrage !)
promtool check config /etc/prometheus/prometheus.yml

# 5. Redémarrage + vérification
sudo systemctl start prometheus
sleep 5
curl -s http://127.0.0.1:9090/-/healthy && echo OK
curl -s http://127.0.0.1:9090/api/v1/status/buildinfo | grep version
```

Points de vigilance :

- Lire les **release notes** : certains flags sont renommés/supprimés entre
  majeures (ex. 2.x → 3.x : vérifier `--storage.tsdb.*` et la syntaxe des
  `matchers` Alertmanager).
- Ne jamais sauter plus d'une majeure sans étape intermédiaire.
- Tester d'abord sur un Prometheus **de preprod** ou sur le second membre
  de la paire HA (rolling upgrade).
- Après upgrade : vérifier `Targets` (tout UP), `Rules` (0 erreur), et
  `prometheus_tsdb_head_series` (pas d'explosion).

## 87. Dépannage : une cible est DOWN

Symptôme : `up{instance="srv-web01:9100"} == 0`, pastille rouge dans Targets
avec message d'erreur.

| Message d'erreur | Cause probable | Action |
|---|---|---|
| `connection refused` | Exporter arrêté | `systemctl status node_exporter` sur la cible |
| `connection timed out` | Firewall / réseau | `telnet srv-web01 9100`, vérifier iptables/nftables |
| `context deadline exceeded` | Scrape trop lent | Augmenter `scrape_timeout`, alléger l'exporter |
| `server returned HTTP 404` | Mauvais `metrics_path` | Vérifier le chemin (`/metrics` ?) |
| `401 Unauthorized` | Basic auth | Vérifier `basic_auth` / `password_file` |
| `certificate verify failed` | TLS | `ca_file`, nom DNS vs CN du certificat |
| `exceeded sample limit` | Trop de séries | Voir cardinalité (section 81) |

Méthode express depuis le serveur Prometheus :

```bash
# Reproduire le scrape à la main (mêmes paramètres que Prometheus)
curl -s -m 10 http://srv-web01:9100/metrics | head -20
curl -s -m 10 http://srv-web01:9100/metrics | grep -c '^node_'
```

## 88. Dépannage : requêtes lentes ou timeout

Symptômes : Grafana "timeout", `query.timeout` atteint, UI lente.

Diagnostic :

```promql
# Requêtes les plus lentes (p95 par chemin)
histogram_quantile(0.95, sum by (path, le) (rate(prometheus_engine_query_duration_seconds_bucket[5m])))

# File d'attente des requêtes (si > 0 durablement : sous-dimensionné)
prometheus_engine_queries
prometheus_engine_query_log_enabled
```

Remèdes (dans l'ordre) :

1. **Réduire la fenêtre** : `[30d]` → `[7d]`, ou pré-agréger.
2. **Filtrer tôt** : ajouter `{job="..."}` / `{instance="..."}`.
3. **Recording rule** : pré-calculer la requête lourde (section 42).
4. **Augmenter les ressources** : `--query.max-concurrency`, RAM.
5. **Limiter** : `--query.max-samples=50M` (défaut) protège déjà de l'OOM ;
   ne l'augmentez que si vous savez pourquoi.

```promql
# ❌ Lent : 30 jours × toutes les instances × tous les CPU
sum(rate(node_cpu_seconds_total[30d]))

# ✅ Rapide : recording rule pré-calculée
sum(instance:cpu_usage_percent:30s)
```

## 89. Dépannage : disque plein

Symptômes : `No space left on device` dans les logs, WAL qui grossit,
rétention qui ne purge plus (la purge a besoin d'espace pour compacter !).

Plan d'urgence :

```bash
# 1. Vérifier l'espace et ce qui le consomme
df -h /var/lib/prometheus
du -sh /var/lib/prometheus/* | sort -h | tail

# 2. Réduire la rétention TEMPORAIREMENT et recharger (pas de restart)
#    -> passer retention.time de 30d à 7d dans le service systemd, puis :
sudo systemctl daemon-reload && sudo systemctl restart prometheus
#    (le restart est nécessaire ici : les flags ne se rechargent pas via /-/reload)

# 3. Si vraiment bloqué : supprimer manuellement les plus vieux blocs
#    (Prometheus arrêté !) — dernier recours
sudo systemctl stop prometheus
ls -lt /var/lib/prometheus/ | tail -5      # les plus vieux en bas
# rm -rf /var/lib/prometheus/01XXXX...     # bloc précis, JAMAIS wal/ !
sudo systemctl start prometheus
```

⚠️ Ne **jamais** supprimer `wal/` à chaud : perte des ~2 dernières heures
et risque de corruption. Les blocs (`01.../`) sont supprimables à froid.

Prévention : alerte `predict_linear(..._size...)` sur le disque de données
+ dimensionnement section 75 + `retention.size` en garde-fou.

## 90. Dépannage : OOM (mémoire épuisée)

Symptômes : processus tué (`dmesg | grep -i "killed process"`), restart en
boucle, `MemoryMax` atteint sous systemd.

Causes classées par fréquence :

1. **Explosion de cardinalité** : `prometheus_tsdb_head_series` en forte
   hausse → section 81. **Cause n°1, de loin.**
2. **Requêtes trop lourdes** : subqueries sur 30 j, `quantile_over_time`
   sur de longues fenêtres → `--query.max-samples`, recording rules.
3. **Trop de règles** évaluées trop souvent → `interval: 1m` au lieu de 15 s
   sur les groupes lourds.
4. **Rétention énorme + petit disque/RAM** : le head grossit avec la rétention.

Actions immédiates :

```bash
# Limiter la casse : réduire la concurrence des requêtes
--query.max-concurrency=10 --query.max-samples=20000000
# Puis identifier la métrique fautive :
# Status -> TSDB -> top 10, ou :
curl -s http://127.0.0.1:9090/api/v1/status/tsdb | python3 -m json.tool | head -40
```

## 91. Dépannage : mes alertes ne partent pas

Checklist dans l'ordre :

1. **La règle s'évalue-t-elle ?** UI → Rules : état `ok`, pas d'erreur.
   Tester l'expr dans Graph : retourne-t-elle des séries ?
2. **Le `for:` est-il atteint ?** UI → Alerts : `pending` (en attente) vs
   `firing` (envoyée). Une alerte `pending` qui retombe = `for` trop long
   ou expr instable.
3. **Prometheus envoie-t-il à Alertmanager ?**
   ```promql
   rate(prometheus_notifications_sent_total[5m])
   rate(prometheus_notifications_failed_total[5m])   # > 0 = problème d'envoi
   ```
4. **Alertmanager reçoit-il ?** UI `:9093` → Alerts : l'alerte y figure ?
   Sinon : vérifier `alerting.alertmanagers` dans prometheus.yml + firewall :9093.
5. **Le routage matche-t-il ?** `:9093` → Status : voir la config chargée.
   Tester avec `amtool config routes test` :
   ```bash
   amtool config routes test --config.file=/etc/alertmanager/alertmanager.yml \
     severity=critical equipe=infra
   ```
6. **Silence ou inhibition actifs ?** `:9093` → Silences / Inhibitions.
7. **Le receiver fonctionne-t-il ?** Tester le SMTP à la main :
   ```bash
   swaks --to infra@entreprise.lan --server smtp.lan:587 -tls --auth-user alertes@entreprise.lan
   ```

## 92. Dépannage : scrape timeout / cibles lentes

Symptôme : `up=1` en pointillés, `scrape_duration_seconds` proche du timeout,
trous dans les graphiques.

```promql
# Durée de scrape par job (alerte si > 80 % du timeout)
max by (job) (scrape_duration_seconds) > 8   # pour timeout=10s

# Échantillons par scrape (détecte l'exporter qui grossit)
avg by (job) (scrape_samples_scraped)
```

Remèdes :

- Exporter lent (SNMP sur équipement chargé) : `scrape_interval: 60s`,
  `scrape_timeout: 50s` sur ce job uniquement.
- Trop de séries : `sample_limit` + `metric_relabel_configs` (drop).
- Réseau distant : intervalle plus large, ou Prometheus local + fédération.
- Blackbox avec `follow_redirects` sur un site lent : `timeout` du module
  < `scrape_timeout` du job.

## 93. Les 10 erreurs classiques (et comment les éviter)

| # | Erreur | Symptôme | Solution |
|---|---|---|---|
| 1 | Label à haute cardinalité (`user_id`, `path` brut) | RAM qui explose, OOM | Normaliser côté app / `labeldrop` |
| 2 | `rate()` sur une gauge | Valeurs absurdes | `rate`/`increase` = compteurs uniquement |
| 3 | Fenêtre `[1m]` dans les alertes | Alertes yo-yo | `[5m]` minimum, `for: 5m` |
| 4 | Oublier `by (le)` avant `histogram_quantile` | Quantiles faux | Toujours agréger les buckets d'abord |
| 5 | `sum()` sans `by` sur des compteurs multi-labels | Double comptage après agrégation | `sum by (labels utiles)` explicite |
| 6 | `predict_linear` sur fenêtre trop courte | Faux positifs "disque plein" | `[6h]` minimum |
| 7 | Pas de `for:` sur `up == 0` | Page à 3 h pour un micro-blip réseau | `for: 5m` |
| 8 | SNMP en v2c sur réseau non maîtrisé | Community interceptée | SNMPv3 authPriv |
| 9 | Un seul Prometheus sans méta-monitoring | Panne de supervision invisible | Paire HA + check externe |
| 10 | Rétention non dimensionnée | Disque plein un dimanche | Calcul section 75 + alerte disque |

---

---

# PARTIE VIII — Cas pratiques, checklist, pense-bête, glossaire, quiz

---

## 94. Cas pratique 1 : supervision complète d'un serveur Linux

Objectif : en 15 minutes, un serveur supervisé avec alertes pertinentes.

```bash
# Sur le serveur cible : installer node_exporter (section 45)
# Sur Prometheus : ajouter au file_sd (section 13)
```

`/etc/prometheus/file_sd/serveurs.json` :

```json
[
  {"targets": ["srv-web01:9100"],
   "labels": {"datacenter": "dc-paris", "role": "web", "os": "debian12"}}
]
```

Vérifications :

```promql
up{instance="srv-web01:9100"}                                              # == 1 ?
100 * (1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)    # RAM %
100 * (1 - node_filesystem_avail_bytes{mountpoint="/"} / node_filesystem_size_bytes{mountpoint="/"})  # disque /
```

Alertes appliquées : `InstanceDown`, `DisqueBientotPlein`, `MemoireCritique`,
`ChargeElevee` (section 69). Dashboard Grafana : importer le dashboard
officiel **Node Exporter Full (ID 1860)** puis l'adapter.

## 95. Cas pratique 2 : supervision d'un parc de switchs (SNMP)

Objectif : superviser 12 switchs d'accès + 2 cœurs : trafic, erreurs, ports down.

1. Générer le module SNMP avec les MIBs du constructeur (section 54).
2. Configurer le job `snmp-switch` (section 55).
3. Renseigner les `ifAlias` sur les switchs (description des ports) : c'est ce
   qui rend les graphiques lisibles ("Uplink-Fibre-01" plutôt que "ifIndex 27").

Requêtes du dashboard "Réseau" :

```promql
# Top 5 des ports les plus chargés (tous switchs)
topk(5, sum by (instance, ifDescr) (rate(ifHCInOctets[5m]) + rate(ifHCOutOctets[5m])) * 8 / 1e6)

# Ports en erreur sur 1 h
sum by (instance, ifDescr) (increase(ifInErrors[1h]) + increase(ifOutErrors[1h])) > 0

# Ports down non désactivés (le classique "câble débranché ou équipement éteint")
count by (instance) (ifOperStatus == 2 and ifAdminStatus == 1)
```

Alerte :

```yaml
- alert: PortSwitchDown
  expr: ifOperStatus == 2 and ifAdminStatus == 1
  for: 15m
  labels: {severity: warning}
  annotations:
    summary: "Port {{ $labels.ifDescr }} DOWN sur {{ $labels.instance }}"
```

> Exclure les ports d'accès volontairement vides via un label ou un silence
> permanent documenté — sinon : bruit.

## 96. Cas pratique 3 : supervision des onduleurs du site

Objectif : ne plus jamais découvrir une coupure par les utilisateurs.

1. Activer SNMP sur chaque onduleur (v3 authPriv, section 56).
2. Job `snmp-ups` + job `blackbox-icmp` vers les IP des onduleurs
   (si le ping ne répond plus, l'onduleur est injoignable : alerte critique).
3. Alertes : `UpsSurBatterie` (2 min), `UpsBatterieFaible`, `UpsBatterieAChanger`
   (section 69).

Dashboard "Énergie" :

```promql
xupsBatteryCapacity or upsAdvBatteryCapacity                 # % batterie
xupsOutputLoad or upsAdvOutputLoad                            # % charge
xupsBatteryRunTimeRemaining or upsAdvBatteryRunTimeRemaining  # autonomie (min)
xupsBatteryTemperature                                       # température
```

Scénario à tester **trimestriellement** : coupure secteur simulée → vérifier
que l'alerte `UpsSurBatterie` arrive en < 3 min sur le canal d'astreinte, et
que l'autonomie affichée correspond au besoin (lien avec le plan de continuité).

## 97. Cas pratique 4 : supervision d'une application web

Objectif : SLI/SLO sur une application : disponibilité, latence, erreurs.

Instrumentation (exemple Python/prometheus_client) :

```python
from prometheus_client import Counter, Histogram, start_http_server

REQUESTS = Counter("app_requests_total", "Requêtes", ["route", "status"])
LATENCY = Histogram("app_request_duration_seconds", "Latence", ["route"],
                    buckets=[0.05, 0.1, 0.25, 0.5, 1, 2.5, 5])

start_http_server(8000)   # endpoint /metrics pour Prometheus
```

SLI en PromQL :

```promql
# Disponibilité 30 j (objectif 99,9 %)
100 * sum(rate(app_requests_total{status!~"5.."}[30d]))
    / sum(rate(app_requests_total[30d]))

# Latence p95 par route
histogram_quantile(0.95, sum by (route, le) (rate(app_request_duration_seconds_bucket[5m])))

# Budget d'erreur restant (SLO 99,9 % sur 30 j)
1 - (sum(rate(app_requests_total{status=~"5.."}[30d])) / sum(rate(app_requests_total[30d])))
```

Alerte "budget d'erreur qui fond vite" (multi-fenêtres, anti-bruit) :

```yaml
- alert: BudgetErreurCritique
  expr: |
    sum(rate(app_requests_total{status=~"5.."}[1h])) / sum(rate(app_requests_total[1h])) > 0.005
    and
    sum(rate(app_requests_total{status=~"5.."}[5m])) / sum(rate(app_requests_total[5m])) > 0.005
  for: 10m
  labels: {severity: critical}
```

## 98. Cas pratique 5 : capacity planning trimestriel

Objectif : répondre à "quand faudra-t-il acheter du disque / de la RAM ?".

```promql
# Croissance disque : Go consommés par jour, par serveur (sur 30 j)
-1 * deriv(node_filesystem_avail_bytes{fstype!~"tmpfs|overlay"}[30d]) * 86400 / 1e9

# Jours restants avant saturation (tendance 30 j)
node_filesystem_avail_bytes{fstype!~"tmpfs|overlay"} /
  (-1 * deriv(node_filesystem_avail_bytes[30d]) * 86400)

# Top 10 des croissances les plus rapides
topk(10,
  -1 * deriv(node_filesystem_avail_bytes{fstype!~"tmpfs|overlay|squashfs"}[30d]) * 86400 / 1e9
)
```

Méthode :

1. Exporter le résultat chaque trimestre (API `/api/v1/query` → CSV).
2. Croiser avec les projets à venir (nouvelle appli = +X Go).
3. Présenter : "serveur X saturé dans 4 mois au rythme actuel → prévoir extension
   au budget N+1".

> C'est ce cas d'usage qui justifie la rétention 90 j – 1 an sur les métriques
> d'infrastructure (ou un Thanos/Mimir, section 77-78).

## 99. Checklist de mise en production

### Installation
- [ ] Binaires officiels, checksums vérifiés
- [ ] Utilisateurs dédiés non-root
- [ ] Services systemd avec `Restart=always` et `MemoryMax`
- [ ] `promtool check config` OK avant chaque reload
- [ ] `--web.enable-lifecycle` actif (reload sans restart)

### Configuration
- [ ] `scrape_timeout` < `scrape_interval` partout
- [ ] `external_labels` renseignés (`datacenter`, `replica`)
- [ ] `sample_limit` / `target_limit` en garde-fous
- [ ] `metric_relabel_configs` : drop de `go_*`, `process_*` si inutiles
- [ ] `honor_labels: true` sur la fédération

### Alerting
- [ ] Alertmanager en cluster (si paire Prometheus)
- [ ] Chaque alerte a `severity`, `for`, `summary`, `runbook`
- [ ] Routage testé (`amtool config routes test`)
- [ ] Inhibitions configurées (panne site → pas de spam)
- [ ] Silences documentés pour les maintenances
- [ ] Canaux testés de bout en bout (email + Telegram/webhook)

### Données & résilience
- [ ] Rétention dimensionnée (calcul section 75)
- [ ] Snapshots quotidiens + copie hors machine (3-2-1)
- [ ] Paire HA ou plan de restauration testé
- [ ] Méta-monitoring : `PrometheusDown` supervisé de l'extérieur

### Sécurité
- [ ] Écoute locale ou reverse proxy TLS + auth
- [ ] Secrets dans des fichiers (`password_file`), perms 640
- [ ] SNMPv3 authPriv sur les équipements
- [ ] API admin et `/-/quit` restreintes par IP

### Exploitation
- [ ] Dashboard "Santé supervision" (targets, séries, règles)
- [ ] Revue trimestrielle des alertes (bruit → seuils)
- [ ] Exercice : panne simulée 1×/trimestre
- [ ] Documentation : où sont les configs, comment reloader, qui est d'astreinte

## 100. Pense-bête de poche PromQL

```promql
# ── Santé ──────────────────────────────────────────────
up == 0                                                    # cibles down
count by (job) (up == 0)                                   # down par job

# ── CPU ────────────────────────────────────────────────
100 * (1 - avg by (instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])))
node_load5 > 2 * count by (instance) (node_cpu_seconds_total{mode="idle"})

# ── Mémoire ────────────────────────────────────────────
100 * (1 - node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)

# ── Disque ─────────────────────────────────────────────
100 * (1 - node_filesystem_avail_bytes / node_filesystem_size_bytes)
100 * (1 - node_filesystem_files_free / node_filesystem_files)   # inodes !
predict_linear(node_filesystem_avail_bytes[6h], 3*24*3600) < 0

# ── Réseau ─────────────────────────────────────────────
rate(node_network_receive_bytes_total{device!~"lo|veth.*|docker.*"}[5m]) * 8 / 1e6
increase(node_network_receive_errors_total[1h]) > 0

# ── Compteurs génériques ───────────────────────────────
rate(metrique_total[5m])        # débit/sec
increase(metrique_total[1h])    # total sur 1 h
irate(metrique_total[5m])       # débit instantané

# ── Histogrammes ───────────────────────────────────────
histogram_quantile(0.95, sum by (le) (rate(http_request_duration_seconds_bucket[5m])))

# ── Temps ──────────────────────────────────────────────
metrique offset 1h             # il y a 1 h
rate(metrique_total[5m] offset 7d)   # semaine dernière
avg_over_time(metrique[1h])    # moyenne glissante

# ── Absence ────────────────────────────────────────────
absent(up{job="backup"})        # le job n'émet plus rien

# ── Cardinalité ────────────────────────────────────────
prometheus_tsdb_head_series    # séries actives
topk(10, count by (__name__) ({__name__=~".+"}))
```

## 101. Glossaire

| Terme | Définition |
|---|---|
| **Alerte** | Règle dont l'expression est vraie → envoyée à Alertmanager |
| **Alertmanager** | Composant qui déduplique, groupe, route et notifie les alertes |
| **Cardinalité** | Nombre de séries uniques ; le facteur limitant n°1 |
| **Chunk** | Bloc d'échantillons compressés dans la TSDB |
| **Compaction** | Fusion des blocs TSDB de 2 h en blocs plus gros |
| **Counter** | Compteur qui ne fait qu'augmenter (requêtes, octets) |
| **Exporter** | Programme qui expose des métriques sur `/metrics` |
| **Fédération** | Un Prometheus scrape les métriques d'un autre |
| **Gauge** | Jauge qui monte et descend (température, mémoire) |
| **Grouping** | Regroupement d'alertes similaires en une notification |
| **Head** | Partie en mémoire de la TSDB (données récentes) |
| **Histogram** | Distribution d'observations en buckets cumulés |
| **Inhibition** | Une alerte grave coupe les notifications des alertes dérivées |
| **Instance** | Label `host:port` d'une cible scrapée |
| **Job** | Ensemble de cibles scrapées avec la même config |
| **Label** | Paire clé=valeur identifiant une série |
| **mTLS** | TLS mutuel : client et serveur s'authentifient |
| **PromQL** | Langage de requêtes de Prometheus |
| **Pushgateway** | Point d'entrée pour les jobs batch (push) |
| **Range vector** | Série de valeurs sur une durée `[5m]` |
| **Recording rule** | Requête pré-calculée et stockée comme métrique |
| **Relabeling** | Réécriture des labels avant/après scrape |
| **Remote read/write** | Lecture/écriture vers un stockage distant |
| **Rétention** | Durée/taille de conservation des données |
| **Scrape** | Action d'aller chercher les métriques (pull) |
| **Série temporelle** | Métrique + jeu unique de labels |
| **Service Discovery (SD)** | Découverte automatique des cibles |
| **Silence** | Coupure volontaire et tracée des notifications |
| **SLI / SLO** | Indicateur / objectif de niveau de service |
| **SNMP** | Protocole de supervision des équipements réseau |
| **Subquery** | Requête PromQL imbriquée `[durée:pas]` |
| **Summary** | Histogram-like avec quantiles calculés côté client |
| **Target** | Cible à scraper (`host:port`) |
| **TSDB** | Base de données temporelle embarquée |
| **up** | Métrique de santé du scrape (1 = OK, 0 = échec) |
| **Vector matching** | Jointure de vecteurs sur les labels (`on`/`ignoring`) |
| **WAL** | Write-Ahead Log : journal des écritures récentes |

## 102. Quiz : validez vos acquis (10 questions + réponses)

**Q1.** Quelle est la différence fondamentale entre un `counter` et une `gauge` ?
Pourquoi ne doit-on jamais faire `rate()` sur une gauge ?
> **R1.** Le counter ne fait qu'augmenter (remise à zéro au reboot) ; la gauge
> monte et descend. `rate()` suppose un compteur monotone : sur une gauge, les
> baisses "normales" seraient interprétées comme des remises à zéro et fausseraient
> le calcul. Sur une gauge on utilise `delta()`, `deriv()` ou la valeur brute.

**Q2.** Votre alerte `up == 0` sans `for:` vous réveille à 3 h du matin alors que
tout va bien. Que s'est-il passé et comment corriger ?
> **R2.** Un scrape isolé a échoué (micro-coupure réseau, GC de l'exporter) et
> l'alerte est partie immédiatement. Correction : `for: 5m` — l'alerte ne tire
> que si la cible reste down 5 minutes.

**Q3.** Que se passe-t-il si vous ajoutez un label `user_id` (10 000 valeurs) à
`http_requests_total` qui a déjà 3 labels à 10 valeurs chacun ?
> **R3.** Explosion de cardinalité : 10 000 × 10³ = 10 millions de séries
> potentielles → RAM saturée, OOM probable. Il faut normaliser (route générique)
> ou agréger côté application, jamais d'identifiant unique en label.

**Q4.** Dans quel ordre s'appliquent `relabel_configs` et `metric_relabel_configs`,
et à quoi sert chacun ?
> **R4.** `relabel_configs` d'abord (avant le scrape, sur les métadonnées de
> découverte : filtrer/réécrire les cibles). `metric_relabel_configs` ensuite
> (après le scrape, sur les séries : jeter des métriques avant stockage).
> Le premier économise aussi la bande passante réseau, pas le second.

**Q5.** Écrivez la requête du p95 de latence HTTP par route sur 5 minutes.
> **R5.** `histogram_quantile(0.95, sum by (route, le) (rate(http_request_duration_seconds_bucket[5m])))`
> Points clés : `rate()` à l'intérieur sur les buckets (compteurs), `sum by (le)`
> avant le quantile, `le` conservé.

**Q6.** À quoi sert `honor_labels: true` et dans quel cas est-il obligatoire ?
> **R6.** Il conserve les labels (`job`, `instance`…) exposés par la cible en cas
> de conflit avec ceux du scrape. Obligatoire en **fédération** : sans lui,
> toutes les séries fédérées se retrouvent avec `job="federate"` et on perd
> l'origine.

**Q7.** Votre disque Prometheus est plein à 98 %. Donnez les 3 premières actions,
dans l'ordre.
> **R7.** 1) Vérifier ce qui consomme (`df`, `du`) et purger les vieux snapshots
> oubliés dans `snapshots/`. 2) Réduire temporairement la rétention
> (`retention.time`) et redémarrer pour purger. 3) En dernier recours, à froid,
> supprimer les plus vieux blocs TSDB (jamais `wal/` à chaud).

**Q8.** Quelle est la différence entre une inhibition et un silence dans
Alertmanager ? Quand utiliser chacun ?
> **R8.** L'**inhibition** est automatique et permanente (règle de config) : une
> alerte grave coupe les alertes dérivées (panne site → pas d'alerte par serveur).
> Le **silence** est manuel et temporaire : on coupe les notifications pendant
> une maintenance planifiée, avec auteur, commentaire et expiration.

**Q9.** Pourquoi `predict_linear(node_filesystem_avail_bytes[1h], 86400)` est-il
une mauvaise idée en alerte ?
> **R9.** Fenêtre trop courte (1 h) : la prédiction est ultra-sensible aux
> à-coups (gros fichier temporaire) → faux positifs. Il faut au minimum `[6h]`,
> et garder en tête que la tendance est supposée linéaire.

**Q10.** Vous avez deux Prometheus en HA qui scrapent les mêmes cibles. Pourquoi
faut-il des `external_labels` différents et un cluster Alertmanager ?
> **R10.** Les `external_labels` (`replica: prom-01/02`) distinguent les deux
> sources dans la fédération et le debug. Le cluster Alertmanager (gossip)
> déduplique : sans lui, chaque Prometheus enverrait ses propres notifications
> → chaque alerte reçue en double.

## 103. Pour aller plus loin

**Documentation officielle**

- https://prometheus.io/docs/introduction/overview/ — concepts
- https://prometheus.io/docs/prometheus/latest/querying/basics/ — PromQL
- https://prometheus.io/docs/alerting/latest/configuration/ — Alertmanager
- https://prometheus.io/docs/operating/integrations/ — liste des exporters

**Dashboards Grafana (IDs à importer)**

- 1860 — Node Exporter Full
- 11074 — Blackbox Exporter
- 3662 — Prometheus 2.0 Stats (santé du serveur lui-même)

**Livres**

- *Prometheus: Up & Running* (O'Reilly) — la référence, à jour sur les concepts
- *Site Reliability Engineering* (Google, gratuit en ligne) — les chapitres
  alerting et SLO, la doctrine derrière les bonnes pratiques

**Prochaines étapes concrètes**

1. Monter une maquette : 1 Prometheus + 3 node_exporters + 1 blackbox en une
   après-midi (sections 7, 8, 45, 51, 52).
2. Brancher Alertmanager avec email + Telegram (sections 60-63) et tester une
   vraie alerte de bout en bout.
3. Ajouter le SNMP pour 2 switchs et 1 onduleur (sections 54-56).
4. Mettre en place la paire HA + snapshots quotidiens (sections 74, 76).
5. Revue des alertes après 1 mois : supprimer le bruit, ajuster les seuils.

> **Dernier conseil de chef de service** : la supervision est un investissement
> d'équipe, pas un projet solo. Documentez chaque alerte (runbook), faites
> tourner l'astreinte, et revoyez le bruit tous les trimestres. Un Prometheus
> silencieux à 3 h du matin, c'est un Prometheus bien configuré. 🔕

---

*Fin du guide — bon monitoring !*
