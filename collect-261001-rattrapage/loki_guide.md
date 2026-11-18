# Grafana Loki — Le guide complet

> **Centraliser, indexer et exploiter vos logs comme vos métriques.**
> Guide pratique en français pour mettre Loki en production : de l'installation
> sur Debian/Ubuntu à l'alerting, en passant par Promtail, LogQL, la rétention
> et le dépannage.
>
> Versions couvertes : **Loki 2.9 / 3.x**, **Promtail 2.9 / 3.x**, Grafana 10/11.
> Les commandes ciblent **Debian 12 / Ubuntu 22.04 / 24.04**.
>
> ⚠️ Les valeurs de configuration sont des **ordres de grandeur** : adaptez-les
> à votre volumétrie et validez toujours sur un environnement de test avant la
> production.

---

## Table des matières

| # | Section |
|---|---------|
| 1 | Pourquoi Loki ? |
| 2 | Concepts : le flux de logs (log stream) |
| 3 | Les chunks : l'unité de stockage |
| 4 | Pas d'index plein texte — la différence avec Elasticsearch |
| 5 | Architecture : vue d'ensemble |
| 6 | Le distributeur (distributor) |
| 7 | L'ingester |
| 8 | Le querier et le query-frontend |
| 9 | Le stockage : filesystem vs stockage objet |
| 10 | Installation sur Debian/Ubuntu : le binaire |
| 11 | Installation : service systemd |
| 12 | Installation : Docker |
| 13 | Premier fichier `loki.yaml` (mode monolithique) |
| 14 | Vérifier que Loki démarre |
| 15 | Modes de déploiement : monolithique vs microservices |
| 16 | Promtail : installation du binaire |
| 17 | Promtail : configuration de base |
| 18 | `scrape_configs` et `static_configs` |
| 19 | `__path__` : globs, pièges et bonnes pratiques |
| 20 | Parser logfmt |
| 21 | Parser JSON |
| 22 | Parser regex |
| 23 | Multiline : gérer les stack traces Java et Python |
| 24 | `relabel_configs`, `labeldrop`, `labelkeep` |
| 25 | Labels : bonnes pratiques |
| 26 | La cardinalité : l'ennemi numéro 1 |
| 27 | Exemples concrets : labels autorisés vs interdits |
| 28 | Structured metadata (Loki 2.8+) |
| 29 | LogQL : anatomie d'une requête |
| 30 | Les sélecteurs de flux |
| 31 | Opérateurs de filtrage : `\|=`, `!=`, `\|~`, `!~` |
| 32 | Filtrer sur les labels extraits par les parsers |
| 33 | `rate()` : mesurer un taux d'erreur |
| 34 | `count_over_time()`, `sum`, `avg` et compagnie |
| 35 | Agréger par labels : `sum by (...)` |
| 36 | Requêtes métriques depuis les logs |
| 37 | Détecter l'absence de logs (dead man's switch) |
| 38 | Sous-requêtes et fenêtres temporelles |
| 39 | Optimiser les requêtes LogQL lentes |
| 40 | Alerting : les règles du ruler Loki |
| 41 | Alertmanager : alerter sur trop d'erreurs 500 |
| 42 | Alerting : bonnes pratiques |
| 43 | Rétention : le table manager (méthode historique) |
| 44 | Rétention : le compactor (méthode moderne) |
| 45 | Rétention par tenant |
| 46 | Intégration Grafana : déclarer la datasource |
| 47 | Grafana Explore : explorer les logs |
| 48 | Dashboards logs : exemples de panneaux |
| 49 | Variables de dashboard LogQL |
| 50 | Cas pratique 1 : centraliser le syslog de 20 serveurs |
| 51 | Cas pratique 2 : suivre les erreurs nginx |
| 52 | Cas pratique 3 : détecter les attaques SSH (avec fail2ban) |
| 53 | Cas pratique 4 : logs applicatifs au format JSON |
| 54 | Cas pratique 5 : audit des commandes sudo |
| 55 | Sécurité : authentification et multi-tenant |
| 56 | Sécurité : TLS de bout en bout |
| 57 | Sécurité : reverse proxy nginx devant Loki |
| 58 | Sécurité : durcissement |
| 59 | Supervision de Loki : ses propres métriques |
| 60 | Alertes sur la santé de Loki |
| 61 | Sauvegarde : chunks sur filesystem |
| 62 | Sauvegarde : stockage objet (S3) |
| 63 | Restauration après sinistre |
| 64 | Mise à jour de Loki |
| 65 | Mise à jour de Promtail |
| 66 | Migration : changer de schéma de stockage |
| 67 | Dépannage : les logs n'arrivent pas |
| 68 | Dépannage : requêtes trop lentes |
| 69 | Dépannage : disque plein |
| 70 | 10 cas de dépannage concrets |
| 71 | Erreur 1 : `too many outstanding requests` |
| 72 | Erreur 2 : `entry out of order` |
| 73 | Erreur 3 : `per-stream rate limit exceeded` |
| 74 | Erreur 4 : `label value too long` / `max label value length` |
| 75 | Erreur 5 : `schema mismatch` / erreurs de schéma |
| 76 | Erreur 6 : HTTP 429 `too many requests` |
| 77 | Erreur 7 : explosion de cardinalité |
| 78 | Erreur 8 : `query timeout` |
| 79 | Erreur 9 : le compactor ne tourne pas |
| 80 | Erreur 10 : Promtail ne lit pas les fichiers |
| 81 | Checklist de mise en production |
| 82 | Pense-bête de poche LogQL |
| 83 | Glossaire |
| 84 | Quiz : 10 questions + réponses |
| 85 | Pour aller plus loin |

---

## 1. Pourquoi Loki ?

Grafana Loki est un système d'agrégation de logs **inspiré de Prometheus** :
au lieu d'indexer le contenu complet de chaque ligne de log (comme
Elasticsearch), il indexe uniquement un petit ensemble de **labels**
(métadonnées clé=valeur) et stocke les lignes brutes compressées dans des
**chunks**.

Conséquences pratiques :

| Aspect | Loki | Elasticsearch |
|---|---|---|
| Index | labels uniquement (léger) | plein texte (lourd) |
| Requêtes | LogQL (proche de PromQL) | Query DSL / KQL |
| Coût de stockage | faible (chunks compressés) | élevé (index inversé) |
| Recherche plein texte | filtrée après sélection des flux | native et rapide |
| Écosystème | Grafana, Prometheus, Alertmanager | Kibana, Logstash |

**Quand choisir Loki :**
- vous utilisez déjà Prometheus + Grafana (même philosophie, même langage) ;
- vous voulez un coût d'infrastructure maîtrisé ;
- vos requêtes partent d'un périmètre (serveur, application) avant de chercher
  du texte.

**Quand éviter Loki :**
- besoin de recherche plein texte massive et instantanée sur tout le corpus ;
- analytique complexe sur le contenu des logs (dans ce cas, un SIEM ou
  Elasticsearch reste pertinent).

---

## 2. Concepts : le flux de logs (log stream)

Dans Loki, un **flux (stream)** = un ensemble unique de labels + les lignes
de log qui lui sont associées.

Exemple : toutes les lignes suivantes appartiennent au **même flux** car
elles partagent exactement les mêmes labels :

```
{job="nginx", host="web01", level="error"}
```

```
{job="nginx", host="web01", level="error"} 2026-09-26T10:00:01Z GET /api 500
{job="nginx", host="web01", level="error"} 2026-09-26T10:00:02Z GET /api 500
```

En revanche, ceci constitue un **autre flux** (le label `host` diffère) :

```
{job="nginx", host="web02", level="error"}
```

Règles fondamentales :

1. **Le nombre de flux actifs = le nombre de combinaisons uniques de labels.**
   C'est lui qui détermine la charge mémoire des ingesters.
2. Les labels doivent être **stables et peu nombreux** : `job`, `host`,
   `app`, `environment`, `level`.
3. Tout ce qui varie à chaque ligne (ID de requête, IP client, timestamp,
   message d'erreur complet) **ne doit jamais être un label**.

Retenez cette phrase, elle reviendra partout dans ce guide :

> **Les labels décrivent *d'où vient* le log. Le contenu du log décrit
> *ce qui s'est passé*. Ne mélangez jamais les deux.**

---

## 3. Les chunks : l'unité de stockage

Loki regroupe les lignes d'un flux en **chunks** (morceaux) compressés :

- un chunk contient les lignes d'**un seul flux** ;
- taille cible : ~1,5 Mo compressé (paramètre `chunk_target_size`) ;
- un chunk est d'abord conservé en mémoire dans l'ingester, puis **flushé**
  vers le stockage (filesystem ou objet) quand il est plein ou trop vieux
  (`chunk_idle_period`, `max_chunk_age`) ;
- les chunks sont **immuables** une fois flushés : on ne les modifie plus,
  on en crée de nouveaux.

Cycle de vie d'un chunk :

```
lignes de log → ingester (mémoire) → chunk plein/vieux → flush → stockage
                                                              → index (labels)
```

L'**index** (BoltDB, TSDB) ne contient que : *quel chunk contient quel flux
pour quelle période*. C'est pour cela que Loki est si économe : l'index est
minuscule comparé à un index plein texte.

---

## 4. Pas d'index plein texte — la différence avec Elasticsearch

Elasticsearch construit un **index inversé** : pour chaque mot de chaque
ligne, il sait dans quels documents il apparaît. Puissant, mais coûteux en
disque, en RAM et en CPU à l'ingestion.

Loki fait le choix inverse :

1. À l'ingestion : il n'indexe que les **labels**. Le texte brut est
   compressé tel quel (gzip) dans les chunks.
2. À la requête : il sélectionne d'abord les flux via les labels
   (rapide, index petit), **puis** il filtre le texte avec `|=`, `|~`
   en décompressant les chunks concernés.

Implications concrètes :

- **Requête rapide** = sélecteur de labels sélectif
  (`{job="nginx", host="web01"}` sur 1 heure).
- **Requête lente** = sélecteur trop large (`{job=~".+"}` sur 30 jours)
  + filtre texte : Loki doit décompresser des montagnes de chunks.
- **Bonne pratique** : concevez vos labels pour que 90 % des requêtes
  touchent un petit nombre de flux.

> ⚠️ **Avertissement cardinalité** : chaque combinaison unique de labels
> crée un flux en mémoire dans l'ingester. 10 labels à 10 valeurs chacun
> = potentiellement 10 milliards de flux. C'est le crash assuré.
> Voir sections 25 à 27.

---

## 5. Architecture : vue d'ensemble

En mode distribué, Loki se compose de plusieurs composants. En mode
**monolithique** (recommandé pour démarrer), un seul binaire fait tout.

```
                    ┌─────────────┐
  Promtail ────────►│ Distributeur│──┐
  (agents)          └─────────────┘  │
                                     ▼
                    ┌─────────────┐  ┌──────────┐   ┌──────────────┐
                    │Query-frontend│◄─│ Querier  │◄──│  Ingester    │
                    └──────┬──────┘  └──────────┘   └──────┬───────┘
                           │                               │
                           ▼                               ▼
                    ┌─────────────┐                 ┌──────────────┐
                    │   Grafana   │                 │   Stockage   │
                    └─────────────┘                 │ (chunks+index)│
                                                    └──────────────┘
```

| Composant | Rôle |
|---|---|
| Distributeur | reçoit les logs (push depuis Promtail), valide, route vers les ingesters |
| Ingester | écrit les lignes dans les chunks en mémoire, flushe vers le stockage |
| Querier | exécute les requêtes LogQL (chunks en mémoire + stockage) |
| Query-frontend | cache, parallélise et déduplique les requêtes (optionnel mais recommandé) |
| Ruler | évalue les règles d'alerting/enregistrement sur les logs |
| Compactor | compacte l'index, applique la rétention, déduplique |

**Mode monolithique** : un seul processus `loki` avec `-target=all`
fait tourner les 6 rôles. Parfait jusqu'à ~quelques To de logs/jour.

---

## 6. Le distributeur (distributor)

Le distributeur est la **porte d'entrée** des logs :

1. Il reçoit les requêtes HTTP `POST /loki/api/v1/push` de Promtail
   (ou de tout client compatible).
2. Il **valide** : labels présents ? taille des lignes OK ? rate limiting
   par tenant respecté ?
3. Il **répartit** les flux vers les ingesters via un anneau de hachage
   cohérent (hash ring) — le même flux va toujours vers les mêmes ingesters.
4. Il applique le **rate limiting** global et par flux
   (`ingestion_rate_mb`, `per_stream_rate_limit`).

Réglages importants (dans `limits_config`) :

```yaml
limits_config:
  ingestion_rate_mb: 16        # débit max par distributeur (Mo/s)
  ingestion_burst_size_mb: 32  # pic autorisé
  per_stream_rate_limit: 5MB   # par flux (défaut 3 Mo/s en 3.x)
  per_stream_rate_limit_burst: 20MB
  max_streams_per_user: 0      # 0 = illimité ; à fixer en prod (ex: 10000)
  max_line_size: 256kB         # lignes plus longues = rejetées (tronquées si truncate)
  max_line_size_truncate: true # tronquer plutôt que rejeter (recommandé)
```

---

## 7. L'ingester

L'ingester est le **cœur chaud** de Loki :

- il maintient les chunks **en mémoire** pour les flux actifs ;
- il répond aux requêtes sur les données récentes (non encore flushées) ;
- il flushe les chunks vers le stockage selon :
  - `chunk_idle_period` (défaut 30 min) : aucun nouveau log depuis X ;
  - `max_chunk_age` (défaut 2 h) : chunk trop vieux ;
  - `chunk_target_size` (~1,5 Mo) : chunk assez gros.

Points de vigilance :

- **Mémoire** : proportionnelle au nombre de flux actifs. Surveillez
  `loki_ingester_memory_streams`.
- **WAL** (write-ahead log) : depuis Loki 2.4, l'ingester écrit un journal
  sur disque (`wal.dir`) pour survivre à un redémarrage sans perdre les
  chunks en mémoire. **Activez-le toujours en production.**

```yaml
ingester:
  wal:
    enabled: true
    dir: /var/lib/loki/wal
  chunk_idle_period: 30m
  max_chunk_age: 2h
  chunk_target_size: 1572864   # ~1,5 Mo
  max_chunk_age: 2h
```

---

## 8. Le querier et le query-frontend

**Querier** : exécute les requêtes LogQL. Il interroge :
1. les ingesters (données récentes en mémoire),
2. le stockage (chunks flushés + index).

**Query-frontend** (fortement recommandé en production) :
- **met en cache** les résultats (`results_cache`) ;
- **découpe** les grosses requêtes en sous-requêtes parallèles
  (`split_queries_by_interval: 30m`) ;
- **limite** la parallélisation (`max_outstanding_per_tenant`) ;
- **rejette** les requêtes trop gourmandes avant qu'elles ne tuent les queriers.

```yaml
query_range:
  split_queries_by_interval: 30m
  results_cache:
    cache:
      embedded_cache:
        enabled: true
        max_size_mb: 100

limits_config:
  max_query_parallelism: 32
  max_outstanding_per_tenant: 2048
  max_query_length: 721h        # 30 jours max par requête
  max_query_lookback: 721h
```

---

## 9. Le stockage : filesystem vs stockage objet

Loki sépare **chunks** (les logs compressés) et **index** (labels → chunks).

| Backend | Chunks | Index | Usage |
|---|---|---|---|
| `filesystem` | disque local | BoltDB local | lab, petits déploiements |
| `s3` (+ compatible : MinIO) | objet | TSDB ou BoltDB | **production recommandée** |
| `gcs` / `azure` | objet | TSDB | clouds GCP / Azure |
| `tsdb` (index) | — | fichiers TSDB sur objet | **recommandé depuis 2.8** |

**Recommandation production** : chunks sur **S3 (ou MinIO on-premise)** +
index **TSDB**. Le filesystem ne passe pas à l'échelle (un seul nœud,
pas de réplication) et complique la sauvegarde.

Exemple de configuration S3/MinIO :

```yaml
storage_config:
  tsdb_shipper:
    active_index_directory: /var/lib/loki/tsdb-index
    cache_location: /var/lib/loki/tsdb-cache
  aws:
    s3: s3://access_key:secret_key@minio.local:9000/loki-chunks
    s3forcepathstyle: true
```

> 💡 Avec MinIO on-premise, vous gardez un stockage objet S3-compatible
> sans dépendre d'un cloud public. Idéal pour une infra d'entreprise.

---

## 10. Installation sur Debian/Ubuntu : le binaire

Loki est distribué en binaire statique unique. Procédure manuelle propre :

```bash
# 1. Variables
LOKI_VERSION="3.3.2"
ARCH="amd64"   # ou arm64

# 2. Téléchargement (vérifiez la version sur github.com/grafana/loki/releases)
cd /tmp
curl -sSLO "https://github.com/grafana/loki/releases/download/v${LOKI_VERSION}/loki-linux-${ARCH}.zip"
curl -sSLO "https://github.com/grafana/loki/releases/download/v${LOKI_VERSION}/promtail-linux-${ARCH}.zip"

# 3. Vérification du checksum (fortement recommandé)
curl -sSL "https://github.com/grafana/loki/releases/download/v${LOKI_VERSION}/SHA256SUMS" -o SHA256SUMS
sha256sum -c SHA256SUMS --ignore-missing

# 4. Installation
sudo apt-get install -y unzip
unzip -o loki-linux-${ARCH}.zip
unzip -o promtail-linux-${ARCH}.zip
sudo install -m 0755 loki-linux-${ARCH} /usr/local/bin/loki
sudo install -m 0755 promtail-linux-${ARCH} /usr/local/bin/promtail

# 5. Utilisateur dédié + répertoires
sudo useradd --system --no-create-home --shell /usr/sbin/nologin loki
sudo mkdir -p /etc/loki /var/lib/loki /var/log/loki
sudo chown -R loki:loki /var/lib/loki /var/log/loki

# 6. Vérification
loki --version
promtail --version
```

Alternative : paquets `.deb` non officiels ou dépôt APT tiers — le binaire
reste la méthode la plus maîtrisée et la plus documentée.

---

## 11. Installation : service systemd

Fichier `/etc/systemd/system/loki.service` :

```ini
[Unit]
Description=Grafana Loki - log aggregation system
Documentation=https://grafana.com/docs/loki/
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=loki
Group=loki
ExecStart=/usr/local/bin/loki -config.file=/etc/loki/loki.yaml
Restart=on-failure
RestartSec=5
# Durcissement
NoNewPrivileges=true
ProtectSystem=strict
ProtectHome=true
ReadWritePaths=/var/lib/loki /var/log/loki /tmp
PrivateTmp=true
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

Activation :

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now loki
sudo systemctl status loki --no-pager
# Logs du service :
sudo journalctl -u loki -f
```

> 💡 `LimitNOFILE=65536` est important : Loki ouvre beaucoup de fichiers
> (chunks, index, WAL). La limite par défaut (1024) provoque des erreurs
> `too many open files` en charge.

---

## 12. Installation : Docker

Pour un lab ou un déploiement conteneurisé, l'image officielle suffit :

```bash
mkdir -p ~/loki/{config,data}
cat > ~/loki/config/loki.yaml <<'EOF'
auth_enabled: false
server:
  http_listen_port: 3100
common:
  path_prefix: /loki
  storage:
    filesystem:
      chunks_directory: /loki/chunks
      rules_directory: /loki/rules
  replication_factor: 1
  ring:
    instance_addr: 127.0.0.1
    kvstore:
      store: inmemory
schema_config:
  configs:
    - from: 2024-01-01
      store: tsdb
      object_store: filesystem
      schema: v13
      index:
        prefix: index_
        period: 24h
EOF

docker run -d --name loki --restart unless-stopped \
  -p 3100:3100 \
  -v ~/loki/config/loki.yaml:/etc/loki/loki.yaml:ro \
  -v ~/loki/data:/loki \
  grafana/loki:3.3.2 -config.file=/etc/loki/loki.yaml
```

Avec Docker Compose (Loki + Promtail + Grafana), voir la section 50
pour un exemple complet prêt à l'emploi.

---

## 13. Premier fichier `loki.yaml` (mode monolithique)

Voici une configuration **monolithique complète et fonctionnelle** pour
démarrer sur un serveur Debian/Ubuntu avec stockage filesystem.
C'est la base que nous enrichirons dans tout le guide.

```yaml
# /etc/loki/loki.yaml
auth_enabled: false          # true en production multi-tenant (section 55)

server:
  http_listen_port: 3100
  grpc_listen_port: 9096
  log_level: info

common:
  path_prefix: /var/lib/loki
  storage:
    filesystem:
      chunks_directory: /var/lib/loki/chunks
      rules_directory: /var/lib/loki/rules
  replication_factor: 1
  ring:
    instance_addr: 127.0.0.1
    kvstore:
      store: inmemory        # en cluster : consul ou etcd

ingester:
  wal:
    enabled: true
    dir: /var/lib/loki/wal
  lifecycler:
    address: 127.0.0.1
    ring:
      kvstore:
        store: inmemory
      replication_factor: 1

schema_config:
  configs:
    - from: 2024-01-01
      store: tsdb
      object_store: filesystem
      schema: v13
      index:
        prefix: index_
        period: 24h

storage_config:
  tsdb_shipper:
    active_index_directory: /var/lib/loki/tsdb-index
    cache_location: /var/lib/loki/tsdb-cache

compactor:
  working_directory: /var/lib/loki/compactor
  compaction_interval: 10m
  retention_enabled: true
  retention_delete_delay: 2h
  retention_delete_worker_count: 150

limits_config:
  retention_period: 744h     # 31 jours
  ingestion_rate_mb: 16
  ingestion_burst_size_mb: 32
  max_query_parallelism: 16
  max_query_length: 721h
  split_queries_by_interval: 30m

query_range:
  align_queries_with_step: true
  results_cache:
    cache:
      embedded_cache:
        enabled: true
        max_size_mb: 100

ruler:
  alertmanager_url: http://localhost:9093   # si Alertmanager installé
  storage:
    type: local
    local:
      directory: /var/lib/loki/rules
```

> ⚠️ `auth_enabled: false` = **aucune authentification**. OK en lab sur
> réseau de confiance, **interdit** exposé sur Internet. Voir section 55.

---

## 14. Vérifier que Loki démarre

```bash
# Le port d'écoute répond ?
curl -s http://localhost:3100/ready
# → "ready" (peut prendre quelques secondes au premier démarrage)

# Les métriques Prometheus internes :
curl -s http://localhost:3100/metrics | head -20

# Page d'état (ingesters, schémas, build info) :
curl -s http://localhost:3100/loki/api/v1/status/buildinfo
```

Checklist de premier démarrage :

- [ ] `curl localhost:3100/ready` renvoie `ready`
- [ ] aucune erreur `level=error` dans `journalctl -u loki`
- [ ] les répertoires `/var/lib/loki/{chunks,tsdb-index,wal}` se créent
- [ ] le port 3100 n'est exposé que sur le réseau prévu (pare-feu)

Test d'ingestion manuel (sans Promtail) :

```bash
curl -s -X POST http://localhost:3100/loki/api/v1/push \
  -H 'Content-Type: application/json' \
  -d '{
    "streams": [{
      "stream": {"job": "test", "host": "srv01"},
      "values": [["'$(date +%s%N)'", "premier log de test"]]
    }]
  }'
# → HTTP 204 = accepté
```

Puis vérifiez dans Grafana Explore ou via l'API :
`curl -s 'http://localhost:3100/loki/api/v1/query_range?query={job="test"}'`.

---

## 15. Modes de déploiement : monolithique vs microservices

| Critère | Monolithique (`-target=all`) | Microservices |
|---|---|---|
| Processus | 1 seul | 6+ (distributor, ingester, querier…) |
| Complexité ops | faible | élevée (ring, gossip, scaling) |
| Scalabilité | verticale (plus gros serveur) | horizontale (plus de réplicas) |
| Stockage | filesystem possible | objet quasi obligatoire |
| Quand | < ~2 To/jour, équipe réduite | gros volumes, haute dispo |

**Recommandation pour Zelef** : commencez **monolithique** avec stockage
objet (MinIO). C'est simple à opérer, à sauvegarder et à superviser.
Passez en microservices (ou en mode « scalable monolithique » : 3 cibles
`read`, `write`, `backend`) uniquement si un composant devient le goulot.

Le mode intermédiaire « scalable » (Loki 2.9+) :

```bash
# 3 déploiements du même binaire, cibles différentes :
loki -target=read      # query-frontend + querier
loki -target=write     # distributor + ingester
loki -target=backend   # compactor + ruler + index-gateway
```

---

## 16. Promtail : installation du binaire

Promtail est l'**agent** qui lit les fichiers de log sur chaque machine et
les pousse vers Loki. Installez-le **sur chaque serveur supervisé**.

```bash
# Binaire déjà récupéré à la section 10, sinon :
PROMTAIL_VERSION="3.3.2"
cd /tmp
curl -sSLO "https://github.com/grafana/loki/releases/download/v${PROMTAIL_VERSION}/promtail-linux-amd64.zip"
unzip -o promtail-linux-amd64.zip
sudo install -m 0755 promtail-linux-amd64 /usr/local/bin/promtail

sudo useradd --system --no-create-home --shell /usr/sbin/nologin promtail
# Promtail doit LIRE les logs : ajoutez-le aux groupes concernés
sudo usermod -aG adm,systemd-journal promtail

sudo mkdir -p /etc/promtail /var/lib/promtail
sudo chown -R promtail:promtail /var/lib/promtail
```

Service systemd `/etc/systemd/system/promtail.service` :

```ini
[Unit]
Description=Promtail - log shipper for Loki
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=promtail
Group=promtail
ExecStart=/usr/local/bin/promtail -config.file=/etc/promtail/promtail.yaml
Restart=on-failure
RestartSec=5
NoNewPrivileges=true
ProtectSystem=strict
ProtectHome=true
ReadWritePaths=/var/lib/promtail /tmp
# Lecture des logs système :
ReadOnlyPaths=/var/log /var/lib/docker/containers
PrivateTmp=true
LimitNOFILE=65536

[Install]
WantedBy=multi-user.target
```

> ⚠️ Avec `ProtectSystem=strict`, listez explicitement les chemins de logs
> dans `ReadOnlyPaths`, sinon Promtail ne verra aucun fichier.

---

## 17. Promtail : configuration de base

`/etc/promtail/promtail.yaml` minimal et fonctionnel :

```yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /var/lib/promtail/positions.yaml   # où Promtail en est dans chaque fichier

clients:
  - url: http://loki.local:3100/loki/api/v1/push
    # batchwait / batchsize : compromis latence/débit
    batchwait: 1s
    batchsize: 1048576   # 1 Mo

scrape_configs:
  - job_name: system
    static_configs:
      - targets: [localhost]
        labels:
          job: syslog
          host: srv01
          __path__: /var/log/syslog
```

Points clés :

- `positions.yaml` : **ne jamais le supprimer en production** sauf pour
  forcer une relecture complète (il mémorise l'offset de chaque fichier).
- `batchwait` : Promtail attend jusqu'à 1 s pour regrouper les lignes avant
  d'envoyer. Baissez à `500ms` si la latence d'ingestion compte.
- Un `clients` peut pointer vers plusieurs Loki (HA) : ajoutez des entrées.

---

## 18. `scrape_configs` et `static_configs`

Chaque `scrape_config` définit **quoi lire, où l'envoyer, quels labels
ajouter**. C'est le pendant des `scrape_configs` de Prometheus.

```yaml
scrape_configs:
  # 1. Logs système
  - job_name: syslog
    static_configs:
      - targets: [localhost]
        labels:
          job: syslog
          host: srv01
          __path__: /var/log/syslog

  # 2. Plusieurs fichiers dans un seul job (globs)
  - job_name: nginx
    static_configs:
      - targets: [localhost]
        labels:
          job: nginx
          host: srv01
          __path__: /var/log/nginx/*.log

  # 3. Plusieurs chemins avec des labels différents
  - job_name: apps
    static_configs:
      - targets: [localhost]
        labels:
          job: app-paiement
          env: prod
          __path__: /opt/paiement/logs/app.log
      - targets: [localhost]
        labels:
          job: app-facturation
          env: prod
          __path__: /opt/facturation/logs/app.log

  # 4. Journal systemd (journald)
  - job_name: journal
    journal:
      path: /var/log/journal
      max_age: 12h
      labels:
        job: systemd-journal
        host: srv01
    relabel_configs:
      - source_labels: ['__journal__systemd_unit']
        target_label: unit
      - source_labels: ['__journal_priority_keyword']
        target_label: level
```

---

## 19. `__path__` : globs, pièges et bonnes pratiques

`__path__` accepte les globs (`*`, `**`) mais quelques règles évitent les
mauvaises surprises :

| Règle | Exemple | Pourquoi |
|---|---|---|
| Un job = un type de log | `job: nginx` → `/var/log/nginx/*.log` | labels cohérents |
| Éviter `**` trop large | ❌ `/var/log/**/*.log` | ramasse tout, labels incohérents |
| Fichiers stables | ✔ `/var/log/nginx/access.log` | la rotation logrotate garde le nom |
| Attention aux liens symboliques | vérifier `follow_symlinks` | par défaut Promtail ne suit pas toujours |

**Rotation des logs (logrotate)** : Promtail gère nativement la rotation
par renommage (`access.log` → `access.log.1`) grâce au fichier `positions`.
Configurez logrotate en mode `copytruncate` **ou** `create` — les deux
fonctionnent, mais `copytruncate` peut dupliquer quelques lignes lors de
la copie (acceptable pour des logs).

```bash
# /etc/logrotate.d/nginx — exemple compatible Promtail
/var/log/nginx/*.log {
    daily
    rotate 14
    compress
    delaycompress
    missingok
    notifempty
    create 0640 www-data adm
    sharedscripts
    postrotate
        [ -f /var/run/nginx.pid ] && kill -USR1 $(cat /var/run/nginx.pid)
    endscript
}
```

---

## 20. Parser logfmt

Le format **logfmt** (`clé=valeur`) est le format structuré le plus simple.
Beaucoup d'applications modernes (et Loki lui-même) l'utilisent.

Ligne d'exemple :

```
ts=2026-09-26T10:00:01Z level=error msg="connexion refusée" host=db01 retries=3
```

Pipeline Promtail :

```yaml
scrape_configs:
  - job_name: monapp
    static_configs:
      - targets: [localhost]
        labels:
          job: monapp
          __path__: /opt/monapp/app.log
    pipeline_stages:
      - logfmt:
          mapping:
            ts: timestamp
            level: level
            msg: message
      - labels:
          level: level        # promeut 'level' en vrai label Loki
      - timestamp:
          source: timestamp
          format: RFC3339
      - output:
          source: message
```

Ce que fait chaque étape :
1. `logfmt` : découpe la ligne en champs nommés.
2. `labels` : `level` devient un **label indexé** (pratique pour
   `{job="monapp", level="error"}`).
3. `timestamp` : utilise le timestamp du log plutôt que l'heure de réception.
4. `output` : remplace la ligne brute par le champ `message` (optionnel).

> ⚠️ Ne promeuvez en labels que des champs à **faible cardinalité**
> (`level`, `host`). Jamais `msg`, `retries` ou un ID.

---

## 21. Parser JSON

Pour les applications qui loguent en JSON (de plus en plus courant) :

```json
{"ts":"2026-09-26T10:00:01Z","level":"error","msg":"timeout","service":"api","latency_ms":1500}
```

```yaml
    pipeline_stages:
      - json:
          expressions:
            timestamp: ts
            level: level
            message: msg
            service: service
      - labels:
          level:
          service:
      - timestamp:
          source: timestamp
          format: RFC3339
      - output:
          source: message
```

Variante **sans réécriture** (on garde la ligne JSON brute, utile pour
`json` dans les requêtes LogQL à la volée — voir section 32) :

```yaml
    pipeline_stages:
      - json:
          expressions:
            level: level
      - labels:
          level:
      # pas de stage 'output' : la ligne reste le JSON d'origine
```

**Conseil** : parser à l'ingestion (Promtail) = requêtes plus rapides et
moins chères. Parser à la requête (`| json` en LogQL) = flexibilité sans
redéployer Promtail. Pour les champs stables et critiques (`level`,
`service`), parsez à l'ingestion.

---

## 22. Parser regex

Quand le format est fixe mais ni JSON ni logfmt (ex : logs Apache/nginx
personnalisés), la regex fait le travail.

Format d'exemple (nginx) :

```
192.168.1.10 - - [26/Sep/2026:10:00:01 +0000] "GET /api/users HTTP/1.1" 500 1234 "-" "curl/8.0"
```

```yaml
    pipeline_stages:
      - regex:
          expression: '^(?P<ip>\S+) \S+ \S+ \[(?P<time>[^\]]+)\] "(?P<method>\S+) (?P<path>\S+) \S+" (?P<status>\d{3}) (?P<size>\d+|-)'
      - labels:
          method:
          status:
      - timestamp:
          source: time
          format: "02/Jan/2006:15:04:05 -0700"
```

> 💡 Testez vos regex avec `|~` en LogQL ou sur regex101.com **avant** de
> les déployer. Une regex qui ne matche pas = des champs vides silencieux.
> Ajoutez un stage de debug temporaire :
>
> ```yaml
>       - template:
>           source: debug
>           template: 'ip={{ .ip }} status={{ .status }}'
>       - output:
>           source: debug
> ```

---

## 23. Multiline : gérer les stack traces Java et Python

Par défaut, Promtail envoie **une ligne = un log**. Les stack traces
(Java, Python) s'étalent sur plusieurs lignes : il faut les **regrouper**
avec le stage `multiline`.

Exemple Python :

```
2026-09-26 10:00:01 ERROR app: échec du traitement
Traceback (most recent call last):
  File "/opt/app/main.py", line 42, in run
    process()
ValueError: invalid literal
```

Configuration :

```yaml
    pipeline_stages:
      - multiline:
          firstline: '^\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}'
          max_wait_time: 3s
          max_lines: 128
```

- `firstline` : regex qui détecte le **début** d'un nouveau log.
  Tout ce qui ne matche pas est rattaché au log précédent.
- `max_wait_time` : au bout de 3 s sans nouvelle ligne, le bloc est flushé.
- `max_lines` : sécurité contre les blocs infinis.

Variante Java (stack traces indentées) :

```yaml
      - multiline:
          firstline: '^\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2},\d{3}'
          max_wait_time: 3s
```

> ⚠️ `firstline` doit être **très fiable** : si elle matche au milieu d'une
> trace, le bloc est coupé en deux. Testez sur des exemples réels.

---

## 24. `relabel_configs`, `labeldrop`, `labelkeep`

Comme Prometheus, Promtail permet de **réécrire les labels** avant envoi.

```yaml
scrape_configs:
  - job_name: kubernetes-pods
    kubernetes_sd_configs:
      - role: pod
    relabel_configs:
      # Ne garder que le namespace 'prod'
      - source_labels: [__meta_kubernetes_namespace]
        regex: prod
        action: keep
      # Créer un label 'app' depuis une annotation
      - source_labels: [__meta_kubernetes_pod_annotation_app]
        target_label: app
      # Supprimer les labels temporaires __meta_*
      - regex: __meta_.*
        action: labeldrop
    pipeline_stages:
      # Supprimer un label devenu inutile
      - labeldrop:
          - filename
      # Ou ne garder qu'une liste blanche (radical)
      # - labelkeep:
      #     - job
      #     - host
      #     - level
```

Actions `relabel_configs` utiles :

| Action | Effet |
|---|---|
| `keep` / `drop` | filtrer les cibles selon une regex |
| `replace` | (défaut) créer/modifier un label |
| `labeldrop` / `labelkeep` | supprimer/garder par regex sur le **nom** |
| `hashmod` | sharding (rarement utile pour Loki) |

---

## 25. Labels : bonnes pratiques

Le choix des labels est **la décision la plus importante** d'un
déploiement Loki. Règles d'or :

1. **Peu de labels** : 3 à 6 par flux suffisent largement.
   Exemples : `job`, `host`, `env`, `app`, `level`, `unit`.
2. **Valeurs stables et bornées** : la liste des valeurs possibles doit
   être connue et petite (dizaines, centaines — pas millions).
3. **Jamais de données à forte cardinalité** : pas d'IP client, pas d'ID
   de requête, pas de timestamp, pas de message d'erreur complet, pas
   d'URL complète.
4. **Cohérence des noms** : `host` partout (pas `hostname` ici et
   `server` là). Documentez votre convention.
5. **Préférez les labels aux filtres texte** pour ce que vous interrogez
   souvent : `{level="error"}` est infiniment plus rapide que `|= "error"`.

Convention proposée pour une PME / un service systèmes :

| Label | Valeurs exemples |
|---|---|
| `job` | `syslog`, `nginx`, `app-paiement`, `systemd-journal` |
| `host` | `srv01`, `web02` (nom court, stable) |
| `env` | `prod`, `preprod`, `lab` |
| `level` | `debug`, `info`, `warn`, `error` |
| `unit` | `nginx.service`, `sshd.service` (journald) |

---

## 26. La cardinalité : l'ennemi numéro 1

La **cardinalité** d'un label = le nombre de valeurs distinctes qu'il peut
prendre. Le nombre de flux actifs ≈ produit des cardinalités.

Exemple catastrophe :

| Label | Cardinalité |
|---|---|
| `job` | 20 |
| `host` | 20 |
| `request_id` | 1 000 000 / jour |
| **Flux potentiels** | **400 000 000** |

Chaque flux actif consomme de la mémoire dans l'ingester (quelques Ko).
400 millions de flux = **OOM kill garanti** de l'ingester, ingestion
bloquée, trous dans les logs.

**Comment détecter** : métrique `loki_ingester_memory_streams` en
croissance continue, ou requêtes `logql` qui timeout sur
`sum by (...)` avec trop de séries.

**Comment corriger** :
1. Identifier le label coupable : `topk(10, count by (label) (...))` ne
   marche pas directement — utilisez plutôt l'API
   `/loki/api/v1/label/{nom}/values` et comptez.
2. Le **rétrograder** : transformer le label en champ parsé à la requête
   (`| json | request_id="abc"` au lieu d'un label).
3. Ou utiliser la **structured metadata** (section 28) pour les champs à
   cardinalité moyenne.

> 🚨 **Règle simple** : si un label peut dépasser ~quelques milliers de
> valeurs distinctes, ce n'est pas un label. Point.

Limites de garde-fou dans `limits_config` :

```yaml
limits_config:
  max_streams_per_user: 10000      # rejette les nouveaux flux au-delà
  max_global_streams_per_user: 0   # 0 = désactivé ; à régler en cluster
  max_label_names_per_series: 15   # pas plus de 15 labels par flux
  max_label_name_length: 1024
  max_label_value_length: 2048
```

---

## 27. Exemples concrets : labels autorisés vs interdits

### ✅ Labels sains

```yaml
labels:
  job: nginx
  host: web01
  env: prod
  level: error
```
→ 4 labels, cardinalités faibles, flux stables et peu nombreux.

### ❌ Labels dangereux

```yaml
labels:
  job: nginx
  host: web01
  client_ip: 203.0.113.45      # ❌ cardinalité énorme (tous les visiteurs)
  request_id: a3f9c1d2         # ❌ unique par requête = un flux par requête !
  url: /api/users/12345        # ❌ quasi-unique par appel
  user_agent: "Mozilla/5.0..." # ❌ des milliers de variantes
```

### 🔧 Correction : parser au lieu de labelliser

```yaml
    pipeline_stages:
      - regex:
          expression: '^(?P<ip>\S+) .* "(?P<method>\S+) (?P<path>\S+).*?" (?P<status>\d{3})'
      # On garde UNIQUEMENT method et status en labels (faible cardinalité)
      - labels:
          method:
          status:
      # ip et path restent des champs : filtrables en LogQL avec | ip="..."
```

Requête équivalente, sans explosion de cardinalité :

```logql
{job="nginx"} | regexp `^(?P<ip>\S+)` | ip="203.0.113.45"
```

---

## 28. Structured metadata (Loki 2.8+)

Entre les labels (indexés, faible cardinalité) et le texte brut, il existe
un juste milieu : la **structured metadata**. Ce sont des paires clé=valeur
attachées au log, **non indexées**, mais interrogeables en LogQL.

Cas d'usage typique : `request_id`, `trace_id`, `user_id` — cardinalité
trop forte pour des labels, mais besoin de filtrer dessus ponctuellement.

```yaml
    pipeline_stages:
      - json:
          expressions:
            request_id: request_id
            trace_id: trace_id
      - structured_metadata:
          request_id:
          trace_id:
```

Requête :

```logql
{job="api"} | request_id="a3f9c1d2"
```

> ⚠️ La structured metadata n'est **pas indexée** : le filtre s'applique
> après sélection des flux, comme `|=`. À utiliser avec un sélecteur de
> flux déjà sélectif. Elle augmente la taille des chunks (compter ~+10-20 %).

---

## 29. LogQL : anatomie d'une requête

LogQL a deux types de requêtes, comme PromQL :

**1. Requête de logs** (retourne des lignes) :

```logql
{job="nginx"} |= "error" | json | status >= 500
```

Pipeline de gauche à droite :
1. `{job="nginx"}` — sélecteur de flux (index, rapide) ;
2. `|= "error"` — filtre texte : garde les lignes contenant "error" ;
3. `| json` — parse le JSON, extrait les champs ;
4. `| status >= 500` — filtre sur le champ parsé.

**2. Requête métrique** (retourne un nombre par instant) :

```logql
sum(rate({job="nginx"} |= "error" [5m]))
```

Elle enveloppe une requête de logs dans une **fonction d'agrégation**
sur une fenêtre `[5m]`. C'est ce type qui sert pour les dashboards et
les alertes.

---

## 30. Les sélecteurs de flux

Le sélecteur `{...}` choisit les flux via les labels indexés. Opérateurs :

| Opérateur | Sens | Exemple |
|---|---|---|
| `=` | égalité exacte | `{job="nginx"}` |
| `!=` | différent | `{job!="debug"}` |
| `=~` | regex (RE2) | `{host=~"web0[1-3]"}` |
| `!~` | regex négative | `{env!~"dev\|lab"}` |

Exemples :

```logql
# Tous les logs nginx de web01
{job="nginx", host="web01"}

# Tous les serveurs web sauf web03
{job="nginx", host=~"web0[12]"}

# Tout sauf la préproduction
{env!="preprod"}

# Plusieurs jobs : regex ou alternance
{job=~"nginx|apache"}
```

> 💡 **Performance** : un sélecteur avec au moins un `=` exact est
> idéal. Les regex `=~` sur les labels restent indexées mais coûtent
> plus cher. Évitez `{job=~".+"}` (tous les flux !).

---

## 31. Opérateurs de filtrage : `|=`, `!=`, `|~`, `!~`

Après le sélecteur, les **filtres de ligne** travaillent sur le texte brut :

| Opérateur | Sens | Exemple |
|---|---|---|
| `\|=` | contient | `{job="nginx"} \|= "error"` |
| `!=` | ne contient pas | `{job="nginx"} != "healthcheck"` |
| `\|~` | matche la regex | `{job="nginx"} \|~ "5\d\d"` |
| `!~` | ne matche pas | `{job="nginx"} !~ "timeout"` |

Chaînage (évaluation de gauche à droite) :

```logql
# Erreurs 500 sur /api, hors healthchecks
{job="nginx"} |= "/api" |~ " 5[0-9][0-9] " != "healthcheck"
```

**Ordre = performance** : placez d'abord le filtre le plus sélectif.
`|= "error"` (chaîne littérale, rapide) avant `|~ "5\d\d"` (regex,
plus coûteuse).

> ⚠️ Ces filtres **ne sont pas indexés** : Loki décompresse les chunks
> des flux sélectionnés puis filtre. D'où l'importance d'un sélecteur
> de flux resserré (section 39).

---

## 32. Filtrer sur les labels extraits par les parsers

Les parsers (`| json`, `| logfmt`, `| regexp`, `| pattern`) extraient des
champs utilisables comme des labels temporaires :

```logql
# Logs JSON : erreurs avec latence > 1s
{job="api"} | json | level="error" | latency_ms > 1000

# logfmt
{job="monapp"} | logfmt | level="error" | retries > 2

# Regex à la volée (si rien n'est parsé à l'ingestion)
{job="nginx"} | regexp `(?P<status>\d{3})` | status="500"

# Pattern : syntaxe simplifiée (<champ>)
{job="sshd"} | pattern `<_> Failed password for <user> from <ip> port <port>`
```

Opérateurs de comparaison sur les champs :

| Opérateur | Sens |
|---|---|
| `=`, `!=` | égalité (chaîne) |
| `>`, `>=`, `<`, `<=` | comparaison numérique |
| `=~`, `!~` | regex sur le champ |

Exemple combiné (nginx) :

```logql
{job="nginx"}
  | regexp `^(?P<ip>\S+) \S+ \S+ \[[^\]]+\] "(?P<method>\S+) (?P<path>[^ ]*)[^"]*" (?P<status>\d{3})`
  | status="500"
  | path=~"/api/.*"
```

---

## 33. `rate()` : mesurer un taux d'erreur

`rate()` calcule le **nombre de lignes par seconde** sur une fenêtre :

```logql
# Logs nginx par seconde (tous)
rate({job="nginx"}[5m])

# Erreurs 500 par seconde
rate({job="nginx"} | regexp `(?P<status>\d{3})` | status="500" [5m])

# Pourcentage d'erreurs 5xx : le classique SLI
sum(rate({job="nginx"} | regexp `(?P<status>\d{3})` | status=~"5.." [5m]))
/
sum(rate({job="nginx"}[5m]))
* 100
```

Variantes :

| Fonction | Sens |
|---|---|
| `rate(v[5m])` | lignes/seconde (lisse les bords) |
| `count_over_time(v[5m])` | nombre brut de lignes sur 5 min |
| `bytes_rate(v[5m])` | octets/seconde (utile pour le volume) |
| `bytes_over_time(v[5m])` | octets totaux sur la fenêtre |

> 💡 Pour une alerte « trop d'erreurs », préférez `rate()` (lissé) à
> `count_over_time()` (bruité). Voir section 41.

---

## 34. `count_over_time()`, `sum`, `avg` et compagnie

```logql
# Nombre de lignes par serveur sur 1h
sum by (host) (count_over_time({job="syslog"}[1h]))

# Top 5 des hosts les plus bavards
topk(5, sum by (host) (count_over_time({job="syslog"}[1h])))

# Latence moyenne par endpoint (logs JSON avec latency_ms)
avg by (path) (
  avg_over_time({job="api"} | json | unwrap latency_ms [5m])
)

# p99 de latence
quantile_over_time(0.99, {job="api"} | json | unwrap latency_ms [5m])
```

`unwrap` : extrait une valeur numérique d'un champ pour les fonctions
`_over_time` (`avg_over_time`, `max_over_time`, `sum_over_time`,
`quantile_over_time`, `stdvar_over_time`, `stddev_over_time`).

```logql
# Trafic total en octets par vhost nginx sur 24h
sum by (vhost) (
  sum_over_time({job="nginx"} | regexp `(?P<bytes>\d+)$` | unwrap bytes [24h])
)
```

---

## 35. Agréger par labels : `sum by (...)`

Les agrégations PromQL s'appliquent aux vecteurs issus des fonctions :

```logql
# Taux d'erreur par host
sum by (host) (rate({job="nginx"} |~ " 5\d\d " [5m]))

# Sans le 'by' : taux global
sum(rate({job="nginx"} |~ " 5\d\d " [5m]))

# Erreurs par minute et par service applicatif
sum by (service) (count_over_time({job="apps", level="error"}[1m]))

# Ratio d'erreurs par service (pour un tableau de bord)
sum by (service) (rate({job="apps", level="error"}[5m]))
/
sum by (service) (rate({job="apps"}[5m]))
```

> ⚠️ `sum by (label_trop_cardinal)` (ex : `by (request_id)`) génère
> autant de séries que de valeurs → risque de surcharge du frontend.
> Agrégez toujours sur des labels à faible cardinalité.

---

## 36. Requêtes métriques depuis les logs

Tout dashboard ou alerte part d'une **requête métrique** : des logs
transforment en série temporelle. Recettes courantes :

```logql
# 1. Débit de logs par job (volume d'ingestion par source)
sum by (job) (rate({job=~".+"}[5m]))

# 2. Taux d'erreur applicative global
sum(rate({job="apps", level="error"}[5m]))

# 3. Erreurs SSH par minute (brute force ?)
sum(count_over_time({job="syslog"} |= "Failed password" [1m]))

# 4. Requêtes HTTP par code de statut (nginx parsé à l'ingestion)
sum by (status) (rate({job="nginx"}[5m]))

# 5. Volume de logs en Mo/heure par host (capacité)
sum by (host) (bytes_over_time({job=~".+"}[1h])) / 1024 / 1024
```

Astuce : pré-calculez les métriques critiques avec des **règles
d'enregistrement** du ruler (section 40) pour des dashboards instantanés.

---

## 37. Détecter l'absence de logs (dead man's switch)

Un serveur qui n'envoie plus de logs est souvent un serveur en panne.
LogQL détecte l'absence avec `absent_over_time` ou un `count == 0` :

```logql
# Alerte si aucun log reçu d'un host depuis 10 min
# (à évaluer avec le ruler, section 40)
absent_over_time({job="syslog", host="srv01"}[10m])
```

> ⚠️ `absent_over_time` renvoie 1 quand **aucune ligne** n'existe sur la
> fenêtre. En règle d'alerte, combinez avec un `for: 10m` pour éviter les
> faux positifs lors des redémarrages Promtail.

Alternative sans `absent_over_time` (compatible vieux Loki) :

```logql
# Nombre de hosts actifs vus dans l'heure — à comparer à l'inventaire
count(count_over_time({job="syslog"}[1h]) by (host))
```

Si ce nombre chute, un ou plusieurs agents Promtail sont tombés.

---

## 38. Sous-requêtes et fenêtres temporelles

La fenêtre `[5m]` dans `rate(...[5m])` s'appelle un **range vector**.
Règles :

- Fenêtre trop courte (< 2× l'intervalle de scrape) → résultats bruités
  ou vides.
- Fenêtre trop longue → lissage excessif, alertes en retard.
- Pour les alertes : `[5m]` à `[15m]` est le standard.

```logql
# Comparer le taux d'erreur actuel à celui d'il y a 1h (dérive)
sum(rate({job="api", level="error"}[5m]))
/
sum(rate({job="api", level="error"}[5m] offset 1h))
```

Décalages temporels (`offset`) et comparaisons semaine/semaine :

```logql
# Le trafic d'aujourd'hui vs il y a 7 jours
sum(rate({job="nginx"}[5m]))
/
sum(rate({job="nginx"}[5m] offset 7d))
```

---

## 39. Optimiser les requêtes LogQL lentes

Une requête lente vient presque toujours d'un **sélecteur trop large**.
Méthode en 4 étapes :

1. **Resserrer les labels** : ajoutez `host`, `env`, `level` au sélecteur.
   ```logql
   -- {job=~".+"} |= "error"           -- lent : tous les flux
   ++ {job="nginx", level="error"}     -- rapide : index utilisé
   ```
2. **Réduire la plage** : 1 h plutôt que 7 j pour explorer, puis élargir.
3. **Filtres littéraux d'abord** : `|= "timeout"` avant `| json`
   (moins de lignes à parser).
4. **Limiter** : ajoutez `| line_format` seulement à la fin, utilisez le
   sélecteur de limite de Grafana (ex : 1000 lignes).

Réglages serveur contre les requêtes tueuses (`limits_config`) :

```yaml
limits_config:
  max_query_length: 721h          # 30 j max
  max_query_parallelism: 32       # parallélisme par requête
  max_entries_limit_per_query: 5000
  max_series_per_query: 1000      # garde-fou cardinalité
  max_cache_freshness_per_query: 10m
```

Et côté query-frontend : `split_queries_by_interval: 30m` +
`max_outstanding_per_tenant` (section 8).

---

## 40. Alerting : les règles du ruler Loki

Le **ruler** évalue des requêtes LogQL périodiquement et envoie les
alertes à Alertmanager. Les règles vivent dans des fichiers YAML
(`rules_directory`, voir section 13).

`/var/lib/loki/rules/fake/alerts.yaml` (le répertoire porte le nom du
tenant ; `fake` quand `auth_enabled: false`) :

```yaml
groups:
  - name: nginx-errors
    interval: 1m
    rules:
      - alert: TropDErreurs500
        expr: |
          sum(rate({job="nginx"} | regexp `(?P<status>\d{3})` | status=~"5.." [5m])) > 1
        for: 5m
        labels:
          severity: critical
          service: nginx
        annotations:
          summary: "Trop d'erreurs 500 sur nginx ({{ $labels.host }})"
          description: "Plus de 1 erreur 500/s depuis 5 minutes."

      - alert: AucunLogServeur
        expr: |
          absent_over_time({job="syslog", host="srv01"}[10m])
        for: 10m
        labels:
          severity: warning
        annotations:
          summary: "srv01 n'envoie plus de logs"
```

Configuration du ruler dans `loki.yaml` :

```yaml
ruler:
  alertmanager_url: http://localhost:9093
  enable_api: true
  storage:
    type: local
    local:
      directory: /var/lib/loki/rules
  ring:
    kvstore:
      store: inmemory
  # Évaluer les règles sur les 15 dernières minutes max :
  query_offset: 2m
```

Rechargez sans redémarrer : `curl -X POST http://localhost:3100/ruler/reload`
(ou `SIGHUP`).

---

## 41. Alertmanager : alerter sur trop d'erreurs 500

Exemple complet : Loki → Alertmanager → notification.

`alertmanager.yaml` minimal :

```yaml
global:
  smtp_smarthost: 'smtp.entreprise.lan:587'
  smtp_from: 'alertes@entreprise.lan'
  smtp_auth_username: 'alertes@entreprise.lan'
  smtp_auth_password: 'MOT_DE_PASSE'   # idéalement via fichier ou variable

route:
  receiver: 'equipe-systemes'
  group_by: ['alertname', 'service']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 4h
  routes:
    - matchers:
        - severity = "critical"
      receiver: 'astreinte'
      repeat_interval: 30m

receivers:
  - name: 'equipe-systemes'
    email_configs:
      - to: 'systemes@entreprise.lan'
  - name: 'astreinte'
    email_configs:
      - to: 'astreinte@entreprise.lan'
```

Règle Loki correspondante (fichier de la section 40) :

```logql
sum(rate({job="nginx"} | regexp `(?P<status>\d{3})` | status=~"5.." [5m])) > 1
```

> 💡 **Seuils** : commencez avec des seuils **larges** (`> 1/s` pendant
> 5 min) et resserrez après 2 semaines d'observation. Une alerte qui sonne
> tout le temps finit ignorée — c'est pire que pas d'alerte.

Tester une règle sans attendre : évaluez l'expression dans Grafana
Explore sur les 7 derniers jours et vérifiez visuellement les
dépassements.

---

## 42. Alerting : bonnes pratiques

| Principe | Exemple |
|---|---|
| Alerter sur des **symptômes**, pas des causes | taux d'erreur 5xx, pas « CPU à 80 % » |
| `for:` toujours renseigné | `for: 5m` minimum (évite le flapping) |
| Labels `severity` + `service` | routage et tri dans Alertmanager |
| Annotations actionnables | runbook, dashboard Grafana en lien |
| Pas d'alerte sans destinataire | chaque alerte critique → astreinte |
| Réévaluer les seuils | revue mensuelle des alertes bruyantes |

Modèle d'annotations recommandé :

```yaml
        annotations:
          summary: "Résumé en une ligne"
          description: "Ce qui se passe, depuis quand, ampleur"
          runbook: "https://wiki.entreprise.lan/runbooks/nginx-500"
          dashboard: "https://grafana.entreprise.lan/d/nginx-logs"
```

---

## 43. Rétention : le table manager (méthode historique)

Avec l'index **BoltDB**, la rétention est gérée par le **table manager** :
les tables périodiques (`index_20260101`) sont supprimées quand elles
dépassent `retention_period`.

```yaml
# Valable UNIQUEMENT avec l'index boltdb-shipper (legacy)
storage_config:
  boltdb_shipper:
    active_index_directory: /var/lib/loki/index
    cache_location: /var/lib/loki/index-cache

table_manager:
  retention_deletes_enabled: true
  retention_period: 744h   # 31 jours
```

> ⚠️ Avec l'index **TSDB** (recommandé depuis Loki 2.8), le table manager
> ne gère **plus** la rétention : c'est le **compactor** qui s'en charge
> (section 44). Ne mélangez pas les deux.

---

## 44. Rétention : le compactor (méthode moderne)

Avec l'index TSDB, le **compactor** fait trois choses : il compacte les
fichiers d'index, **supprime les chunks et index expirés** (rétention),
et déduplique.

```yaml
compactor:
  working_directory: /var/lib/loki/compactor
  compaction_interval: 10m
  retention_enabled: true
  retention_delete_delay: 2h      # délai de grâce avant suppression réelle
  retention_delete_worker_count: 150

limits_config:
  retention_period: 744h          # 31 jours, global
```

Vérifier que la rétention tourne :

```bash
# Le compactor a-t-il tourné récemment ?
curl -s http://localhost:3100/metrics | grep -E "loki_compactor.*(last|applied)"
# Marqueurs de suppression :
curl -s http://localhost:3100/metrics | grep loki_compactor_deleted
```

> 💡 La suppression est **asynchrone** : entre l'expiration théorique et
> la libération disque, comptez `retention_delete_delay` + le temps du
> cycle de compaction. Ne paniquez pas si le disque ne se libère pas
> instantanément.

---

## 45. Rétention par tenant

Avec `auth_enabled: true`, chaque tenant peut avoir sa propre rétention
(utile : 90 jours pour la prod, 7 jours pour le lab).

```yaml
auth_enabled: true

limits_config:
  retention_period: 744h   # défaut : 31 jours

  # Surcharges par tenant (fichier séparé recommandé) :
  per_tenant_override_config: /etc/loki/overrides.yaml
```

`/etc/loki/overrides.yaml` :

```yaml
overrides:
  "tenant-prod":
    retention_period: 2160h    # 90 jours
  "tenant-lab":
    retention_period: 168h     # 7 jours
```

> ⚠️ La rétention par tenant exige le compactor avec `retention_enabled:
> true` **et** un stockage objet (ou filesystem en mono-tenant de test).
> Les chunks partagés entre tenants ne sont supprimés que quand tous les
> tenants les ont expirés.

---

## 46. Intégration Grafana : déclarer la datasource

Dans Grafana : **Connections → Data sources → Add → Loki**.

| Champ | Valeur |
|---|---|
| URL | `http://loki.local:3100` |
| Auth | selon votre setup (section 55/57) |
| `X-Scope-OrgID` header | nom du tenant si `auth_enabled: true` |
| Maximum lines | `1000` (défaut raisonnable) |

En provisioning (`/etc/grafana/provisioning/datasources/loki.yaml`) :

```yaml
apiVersion: 1
datasources:
  - name: Loki
    type: loki
    access: proxy
    url: http://loki.local:3100
    jsonData:
      maxLines: 1000
      derivedFields:
        # Cliquer sur un trace_id ouvre Tempo/Jaeger :
        - datasourceUid: tempo
          matcherRegex: "trace_id=(\\w+)"
          name: Trace
          url: "$${__value.raw}"
```

Les **derived fields** transforment un champ du log (ex : `trace_id`)
en lien cliquable vers Tempo : le pont logs ↔ traces.

---

## 47. Grafana Explore : explorer les logs

Explore (**Compass → Explore**) est l'outil d'investigation :

1. **Query type** : `Range` (graphique + logs) ou `Instant` (table).
2. **Label browser** : cliquez sur `{job="..."}` pour découvrir les
   labels et valeurs disponibles sans connaître LogQL.
3. **Filtres ad hoc** : `|=` / `|~` / parsers s'ajoutent en un clic sur
   une ligne de log (« Filter for » / « Filter out »).
4. **Live tailing** : le bouton ▶ suit les logs en temps réel.
5. **Plage** : commencez par 1 h, élargissez si besoin.

Raccourcis qui changent la vie :

| Action | Comment |
|---|---|
| Voir le contexte d'une ligne | « Show context » (± 20 lignes autour) |
| Exclure un motif bruyant | clic droit → « Filter out » (`!=`) |
| Passer en plein écran | `e` puis `f` |
| Copier la requête | menu du panneau |

> 💡 En incident, la séquence gagnante : sélecteur resserré (`{job, host,
> level}`) → plage 1 h → `|=` sur le symptôme → « Show context » sur les
> lignes intéressantes.

---

## 48. Dashboards logs : exemples de panneaux

Panneau 1 — **Taux d'erreurs 5xx nginx** (Time series) :

```logql
sum(rate({job="nginx"} | regexp `(?P<status>\S+)` | status=~"5.." [5m]))
```

Panneau 2 — **Logs bruts filtrés** (Logs panel) :

```logql
{job="nginx", level="error"} |~ " 5\d\d "
```

Panneau 3 — **Top 10 des URLs en erreur** (Table) :

```logql
topk(10, sum by (path) (count_over_time(
  {job="nginx"} | regexp `"[A-Z]+ (?P<path>\S+)" \d{3}` | status=~"5.." [1h]
)))
```

Panneau 4 — **Volume d'ingestion par job** (Time series) :

```logql
sum by (job) (bytes_rate({job=~".+"}[5m]))
```

Panneau 5 — **Carte de chaleur des erreurs par host** (Heatmap) :

```logql
sum by (host) (count_over_time({job="syslog"} |= "error" [1m]))
```

---

## 49. Variables de dashboard LogQL

Rendez les dashboards réutilisables avec des variables :

| Variable | Type | Requête |
|---|---|---|
| `$job` | Query | `label_values({job=~".+"}, job)` |
| `$host` | Query | `label_values({job="$job"}, host)` |
| `$level` | Custom | `debug,info,warn,error` |
| `$intervalle` | Interval | `1m,5m,15m,1h` |

Utilisation dans les panneaux :

```logql
sum(rate({job="$job", host=~"$host", level=~"$level"}[$intervalle]))
```

> ⚠️ `label_values()` sur un gros périmètre peut être lent : restreignez
> avec un sélecteur (`{job="$job"}`) et activez le **refresh on dashboard
> load** plutôt que « on time range change ».

---

## 50. Cas pratique 1 : centraliser le syslog de 20 serveurs

**Objectif** : tous les `/var/log/syslog` (+ journald) de 20 serveurs
Debian dans un Loki central, avec le hostname en label.

### Côté Loki central (`loki.local`)

`loki.yaml` monolithique de la section 13 + pare-feu :

```bash
# N'autoriser que le réseau des serveurs sur le port 3100
sudo ufw allow from 10.0.0.0/24 to any port 3100 proto tcp
```

### Côté chaque serveur : Promtail

`/etc/promtail/promtail.yaml` (identique partout sauf `host`) :

```yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /var/lib/promtail/positions.yaml

clients:
  - url: http://loki.local:3100/loki/api/v1/push
    batchwait: 1s
    batchsize: 1048576

scrape_configs:
  - job_name: syslog
    static_configs:
      - targets: [localhost]
        labels:
          job: syslog
          host: web01          # ← à personnaliser par serveur
          env: prod
          __path__: /var/log/syslog
  - job_name: journal
    journal:
      path: /var/log/journal
      max_age: 12h
      labels:
        job: systemd-journal
        host: web01
        env: prod
    relabel_configs:
      - source_labels: ['__journal__systemd_unit']
        target_label: unit
```

### Déploiement automatisé (Ansible, extrait)

```yaml
- name: Déployer Promtail
  hosts: all
  become: true
  vars:
    loki_url: http://loki.local:3100/loki/api/v1/push
  tasks:
    - name: Copier le binaire
      ansible.builtin.copy:
        src: files/promtail-linux-amd64
        dest: /usr/local/bin/promtail
        mode: '0755'
    - name: Configurer (host dynamique)
      ansible.builtin.template:
        src: templates/promtail.yaml.j2   # {{ inventory_hostname }} pour host
        dest: /etc/promtail/promtail.yaml
      notify: Redémarrer promtail
  handlers:
    - name: Redémarrer promtail
      ansible.builtin.systemd:
        name: promtail
        state: restarted
        enabled: true
```

### Vérification

```logql
# Les 20 hosts envoient-ils tous ?
count(count_over_time({job="syslog"}[5m]) by (host))
# → doit retourner 20
```

---

## 51. Cas pratique 2 : suivre les erreurs nginx

**Objectif** : dashboard + alerte sur les erreurs HTTP 5xx de 3 reverse
proxies nginx.

### Promtail (sur chaque proxy)

```yaml
scrape_configs:
  - job_name: nginx
    static_configs:
      - targets: [localhost]
        labels:
          job: nginx
          host: proxy01
          env: prod
          __path__: /var/log/nginx/access.log
    pipeline_stages:
      - regex:
          expression: '^(?P<ip>\S+) \S+ \S+ \[(?P<time>[^\]]+)\] "(?P<method>\S+) (?P<path>\S+) \S+" (?P<status>\d{3}) (?P<size>\d+|-) "[^"]*" "(?P<ua>[^"]*)"'
      - labels:
          method:
          status:
      - timestamp:
          source: time
          format: "02/Jan/2006:15:04:05 -0700"
```

Format nginx correspondant (`log_format` dans `nginx.conf`) :

```nginx
log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                '$status $body_bytes_sent "$http_referer" '
                '"$http_user_agent"';
access_log /var/log/nginx/access.log main;
```

### Requêtes du dashboard

```logql
# Taux de 5xx par proxy
sum by (host) (rate({job="nginx", status=~"5.."}[5m]))

# Top URLs en erreur
topk(10, sum by (path) (count_over_time({job="nginx", status=~"5.."}[1h])))

# Taux d'erreur global en %
sum(rate({job="nginx", status=~"5.."}[5m]))
/ sum(rate({job="nginx"}[5m])) * 100
```

### Alerte (ruler)

```yaml
      - alert: NginxTropDErreurs500
        expr: sum(rate({job="nginx", status=~"5.."}[5m])) > 0.5
        for: 5m
        labels:
          severity: critical
          service: nginx
        annotations:
          summary: "Plus de 0,5 erreur 500/s sur nginx"
```

---

## 52. Cas pratique 3 : détecter les attaques SSH (avec fail2ban)

**Objectif** : repérer les scans/brute-force SSH dans Loki, et faire le
lien avec **fail2ban** qui bloque déjà les attaquants.

### Ce que fail2ban fait (rappel)

fail2ban lit `/var/log/auth.log`, détecte les `Failed password` répétés
et bannit l'IP via iptables/nftables. Loki ne le remplace pas : il apporte
la **vision globale** (quels serveurs sont attaqués ? quelles IP reviennent ?).

### Promtail : parser auth.log

```yaml
scrape_configs:
  - job_name: auth
    static_configs:
      - targets: [localhost]
        labels:
          job: sshd
          host: srv01
          __path__: /var/log/auth.log
    pipeline_stages:
      - regex:
          expression: 'Failed password for (invalid user )?(?P<user>\S+) from (?P<ip>\S+) port \d+'
      - labels:
          user:
      - structured_metadata:
          ip:
```

> ⚠️ `ip` en structured metadata (pas en label !) : la cardinalité des IP
> attaquantes est trop forte pour un label.

### Requêtes de détection

```logql
# Tentatives par minute (tous serveurs)
sum(count_over_time({job="sshd"} |= "Failed password" [1m]))

# Top IP attaquantes sur 24h
topk(20, sum by (ip) (count_over_time({job="sshd"} |= "Failed password" [24h])))

# Quels serveurs sont visés ?
sum by (host) (count_over_time({job="sshd"} |= "Failed password" [1h]))

# Utilisateurs ciblés (root ?)
topk(10, sum by (user) (count_over_time({job="sshd"} |= "Failed password" [24h])))
```

### Alerte brute-force

```yaml
      - alert: AttaqueSSHBruteForce
        expr: sum(rate({job="sshd"} |= "Failed password" [5m])) by (host) > 0.5
        for: 10m
        labels:
          severity: warning
          service: ssh
        annotations:
          summary: "Possible brute-force SSH sur {{ $labels.host }}"
          description: "Vérifier fail2ban : fail2ban-client status sshd"
```

### Corrélation avec fail2ban

```bash
# IP bannies actuellement :
sudo fail2ban-client status sshd
# Croiser avec Loki : l'IP top-attaquante est-elle bannie ?
```

> 💡 Piste d'amélioration : un script qui lit le top des IP dans Loki
> (API `/loki/api/v1/query`) et les injecte dans un ipset de blocage
> préventif. À évaluer avec prudence (risque de faux positifs).

---

## 53. Cas pratique 4 : logs applicatifs au format JSON

**Objectif** : une application Java qui logue en JSON, avec niveau de log
en label et `trace_id` en structured metadata.

### Log d'exemple

```json
{"ts":"2026-09-26T10:00:01.123Z","level":"ERROR","logger":"com.app.Paiement","msg":"Paiement refusé","trace_id":"a3f9c1d2e4b5","amount":129.99,"user_id":"u-4821"}
```

### Promtail

```yaml
scrape_configs:
  - job_name: app-paiement
    static_configs:
      - targets: [localhost]
        labels:
          job: app-paiement
          host: app01
          env: prod
          __path__: /opt/paiement/logs/app.json.log
    pipeline_stages:
      - multiline:
          firstline: '^\{"ts"'
          max_wait_time: 3s
      - json:
          expressions:
            level: level
            message: msg
            trace_id: trace_id
            ts: ts
      - labels:
          level:
      - structured_metadata:
          trace_id:
      - timestamp:
          source: ts
          format: RFC3339Nano
      - output:
          source: message
```

### Requêtes

```logql
# Toutes les erreurs de l'app
{job="app-paiement", level="ERROR"}

# Suivre une transaction de bout en bout via son trace_id
{job="app-paiement"} | trace_id="a3f9c1d2e4b5"

# Taux d'erreur par minute
sum(count_over_time({job="app-paiement", level="ERROR"}[1m]))

# Erreurs contenant "Paiement refusé" sur 7 jours
{job="app-paiement", level="ERROR"} |= "Paiement refusé"
```

---

## 54. Cas pratique 5 : audit des commandes sudo

**Objectif** : tracer qui a fait `sudo` quoi, où et quand — besoin
classique d'un chef de service systèmes.

### Source : journald (où sudo logue nativement)

```yaml
scrape_configs:
  - job_name: journal
    journal:
      path: /var/log/journal
      max_age: 12h
      labels:
        job: systemd-journal
        host: srv01
    relabel_configs:
      - source_labels: ['__journal__systemd_unit']
        target_label: unit
    pipeline_stages:
      # Ne garder que sudo (filtre à l'ingestion = moins de volume)
      - match:
          selector: '{unit="sudo.service"}'
          stages:
            - regex:
                expression: '(?P<sudo_user>\S+) : TTY=\S+ ; PWD=(?P<pwd>\S+) ; USER=(?P<target_user>\S+) ; COMMAND=(?P<command>.+)'
            - labels:
                sudo_user:
                target_user:
            - structured_metadata:
                command:
```

### Requêtes d'audit

```logql
# Toutes les commandes sudo d'un utilisateur
{job="systemd-journal", unit="sudo.service", sudo_user="martin"}

# Qui a touché au fichier shadow ?
{job="systemd-journal", unit="sudo.service"} |= "/etc/shadow"

# Commandes sudo par utilisateur sur 30 jours (rapport mensuel)
sum by (sudo_user) (count_over_time({job="systemd-journal", unit="sudo.service"}[30d]))

# sudo vers root par un compte de service (suspect)
{job="systemd-journal", unit="sudo.service", target_user="root", sudo_user=~"svc-.*"}
```

> ⚠️ `command` en structured metadata : une commande complète est quasi
> unique → jamais en label. Et pensez au RGPD : les logs sudo contiennent
> des données personnelles, cadrez la rétention (section 45).

---

## 55. Sécurité : authentification et multi-tenant

Par défaut (`auth_enabled: false`), **quiconque atteint le port 3100 peut
lire tous les logs et en pousser**. En production : activez l'authentification.

```yaml
auth_enabled: true
```

Avec `auth_enabled: true`, chaque requête doit porter le header
`X-Scope-OrgID: <tenant>` :

```bash
# Push vers le tenant 'prod'
curl -H 'X-Scope-OrgID: prod' -X POST \
  http://localhost:3100/loki/api/v1/push ...

# Requête sur le tenant 'prod'
curl -H 'X-Scope-OrgID: prod' \
  'http://localhost:3100/loki/api/v1/query_range?query={job="nginx"}'
```

Promtail côté client :

```yaml
clients:
  - url: http://loki.local:3100/loki/api/v1/push
    tenant_id: prod
```

Stratégie de tenants recommandée :

| Tenant | Contenu | Rétention |
|---|---|---|
| `prod` | serveurs de production | 90 jours |
| `infra` | équipements réseau, hyperviseurs | 90 jours |
| `lab` | tests, préproduction | 7 jours |

> 💡 Loki ne gère pas lui-même les mots de passe : placez un reverse
> proxy (nginx, section 57) avec authentification basique ou OIDC devant,
> qui injecte le `X-Scope-OrgID` selon l'utilisateur.

---

## 56. Sécurité : TLS de bout en bout

Chiffrez au minimum l'ingestion et l'interface Grafana ↔ Loki.

### TLS côté serveur Loki

```yaml
server:
  http_listen_port: 3100
  http_tls_config:
    cert_file: /etc/loki/tls/loki.crt
    key_file: /etc/loki/tls/loki.key
  grpc_listen_port: 9096
  grpc_tls_config:
    cert_file: /etc/loki/tls/loki.crt
    key_file: /etc/loki/tls/loki.key
```

### Promtail vers Loki en HTTPS

```yaml
clients:
  - url: https://loki.local:3100/loki/api/v1/push
    tls_config:
      ca_file: /etc/promtail/tls/ca.crt
      cert_file: /etc/promtail/tls/client.crt   # si mTLS
      key_file: /etc/promtail/tls/client.key
      # insecure_skip_verify: false  ← ne JAMAIS mettre à true en prod
```

Générez les certificats avec votre CA interne (voir le guide Debian :
section CA interne) ou Let's Encrypt pour les noms publics.

---

## 57. Sécurité : reverse proxy nginx devant Loki

Le reverse proxy centralise TLS + authentification basique + injection
du tenant.

```nginx
# /etc/nginx/sites-available/loki
upstream loki {
    server 127.0.0.1:3100;
}

server {
    listen 443 ssl;
    server_name loki.entreprise.lan;

    ssl_certificate     /etc/ssl/certs/loki.entreprise.lan.crt;
    ssl_certificate_key /etc/ssl/private/loki.entreprise.lan.key;

    # Authentification basique (fichier htpasswd)
    auth_basic "Loki - accès restreint";
    auth_basic_user_file /etc/nginx/htpasswd_loki;

    location / {
        proxy_pass http://loki;
        proxy_set_header Host $host;
        # Tenant selon l'utilisateur authentifié :
        # (ici : tout le monde → tenant 'prod' ; affinez avec des maps)
        proxy_set_header X-Scope-OrgID "prod";
        proxy_read_timeout 300s;
        proxy_send_timeout 300s;
        client_max_body_size 10M;   # les pushs peuvent être volumineux
    }
}
```

Pour un mapping utilisateur → tenant fin, utilisez `map $remote_user` :

```nginx
map $remote_user $loki_tenant {
    default      "lab";
    "admin"      "prod";
    "reseau"     "infra";
}
# puis : proxy_set_header X-Scope-OrgID $loki_tenant;
```

---

## 58. Sécurité : durcissement

Checklist de durcissement d'un Loki exposé au réseau interne :

- [ ] `auth_enabled: true` + tenants (section 55)
- [ ] TLS partout (section 56) ou réseau chiffré (WireGuard)
- [ ] Reverse proxy avec authentification (section 57)
- [ ] Pare-feu : 3100/9080 accessibles uniquement aux sources légitimes
      (`ufw`, nftables)
- [ ] Service systemd durci : `NoNewPrivileges`, `ProtectSystem=strict`
      (section 11)
- [ ] Pas d'interface d'admin exposée : le port 3100 sert aussi les
      endpoints `/config`, `/runtime` — filtrez via le proxy si besoin
- [ ] Mises à jour suivies (section 64) — les CVE sur les dépendances Go
      sont régulières
- [ ] Sauvegarde chiffrée des chunks si données sensibles (section 62)
- [ ] Journaliser les accès au proxy (qui a requêté quoi, quand)

---

## 59. Supervision de Loki : ses propres métriques

Loki expose ses métriques au format Prometheus sur `/metrics`
(port 3100). Ajoutez-le comme cible Prometheus :

```yaml
# prometheus.yaml
scrape_configs:
  - job_name: loki
    static_configs:
      - targets: ['loki.local:3100']
  - job_name: promtail
    static_configs:
      - targets: ['srv01:9080', 'srv02:9080']   # un par serveur
```

Métriques clés à surveiller :

| Métrique | Signification | Seuil d'alerte |
|---|---|---|
| `loki_ingester_memory_streams` | flux actifs en mémoire | croissance anormale = cardinalité |
| `loki_distributor_lines_received_total` | lignes reçues/s | chute = problème d'ingestion |
| `rate(loki_request_duration_seconds_count[5m])` | requêtes/s par route | — |
| `loki_ingester_chunk_age_seconds` | âge des chunks | — |
| `loki_boltdb_shipper_compactor_running` | compactor actif | doit être > 0 |
| `promtail_targets_active` | fichiers suivis par Promtail | chute = agent tombé |
| `promtail_file_bytes_total` | octets lus | stagnation = fichier bloqué |
| `process_resident_memory_bytes{job="loki"}` | RAM de Loki | vs limite du serveur |

---

## 60. Alertes sur la santé de Loki

Règles Prometheus (pas Loki !) pour superviser Loki lui-même :

```yaml
# /etc/prometheus/rules/loki-health.yaml
groups:
  - name: loki-sante
    interval: 1m
    rules:
      - alert: LokiIngesterMemoireElevee
        expr: process_resident_memory_bytes{job="loki"} > 8 * 1024 * 1024 * 1024
        for: 10m
        labels: {severity: warning}
        annotations:
          summary: "Loki consomme plus de 8 Go de RAM"

      - alert: LokiIngestionEnChute
        expr: rate(loki_distributor_lines_received_total[5m]) == 0
        for: 10m
        labels: {severity: critical}
        annotations:
          summary: "Aucune ligne reçue depuis 10 min — ingestion en panne ?"

      - alert: PromtailAgentTombe
        expr: up{job="promtail"} == 0
        for: 5m
        labels: {severity: warning}
        annotations:
          summary: "Promtail tombé sur {{ $labels.instance }}"

      - alert: LokiExplosionCardinalite
        expr: |
          deriv(loki_ingester_memory_streams[15m]) > 100
        for: 15m
        labels: {severity: critical}
        annotations:
          summary: "Les flux actifs explosent — vérifier les labels"
```

---

## 61. Sauvegarde : chunks sur filesystem

Avec le stockage **filesystem** (petit déploiement), la sauvegarde =
copie des répertoires de données **à froid** (Loki arrêté) ou via
snapshot filesystem (LVM/btrfs/ZFS) à chaud.

```bash
# Méthode simple et sûre : arrêt + rsync + redémarrage
sudo systemctl stop loki
sudo rsync -a --delete /var/lib/loki/ /mnt/sauvegarde/loki-$(date +%F)/
sudo systemctl start loki

# Méthode à chaud avec snapshot LVM (exemple) :
sudo lvcreate --size 10G --snapshot --name loki-snap /dev/vg0/lv-loki
sudo mount -o ro /dev/vg0/loki-snap /mnt/loki-snap
sudo rsync -a /mnt/loki-snap/ /mnt/sauvegarde/loki-$(date +%F)/
sudo umount /mnt/loki-snap
sudo lvremove -y /dev/vg0/loki-snap
```

Quoi sauvegarder impérativement :

| Répertoire | Contenu |
|---|---|
| `/var/lib/loki/chunks` | les logs eux-mêmes |
| `/var/lib/loki/tsdb-index` | l'index actif |
| `/var/lib/loki/tsdb-cache` | cache (optionnel) |
| `/var/lib/loki/wal` | chunks non flushés (critique !) |
| `/var/lib/loki/rules` | règles d'alerting |
| `/etc/loki/` | configuration |

> ⚠️ Sans le **WAL**, les logs des ~30 dernières minutes (chunks en
> mémoire) sont perdus en cas de crash. C'est la pièce la plus critique
> avec les chunks.

---

## 62. Sauvegarde : stockage objet (S3)

Avec S3/MinIO, la sauvegarde = **réplication du bucket** (versioning +
réplication inter-région ou vers un second MinIO).

Stratégie recommandée :

1. **Versioning** activé sur le bucket `loki-chunks` (protection contre
   les suppressions accidentelles du compactor).
2. **Réplication** vers un bucket secondaire (autre site / autre MinIO).
3. **Lifecycle** : transition vers stockage froid après 90 jours si le
   provider le permet (les vieux chunks sont rarement relus).
4. Sauvegarder **à part** : `/etc/loki/`, les règles, et le WAL local
   (lui n'est pas sur S3 !).

```bash
# Exemple MinIO : réplication avec mc (ponctuelle, hors versioning natif)
mc mirror --overwrite --remove \
  minio-principal/loki-chunks \
  minio-secours/loki-chunks
```

> 💡 Avec le stockage objet, Loki lui-même est **stateless** (hors WAL) :
> on peut reconstruire un Loki neuf qui relit le bucket. Testez cette
> reconstruction une fois par an (voir section 63).

---

## 63. Restauration après sinistre

### Scénario : disque du Loki filesystem mort, sauvegarde rsync disponible

```bash
# 1. Réinstaller Loki (section 10-11)
# 2. Restaurer les données AVANT le premier démarrage
sudo systemctl stop loki
sudo rsync -a /mnt/sauvegarde/loki-2026-09-25/ /var/lib/loki/
sudo chown -R loki:loki /var/lib/loki
# 3. Restaurer la config
sudo cp /mnt/sauvegarde/loki-etc/loki.yaml /etc/loki/
# 4. Démarrer et vérifier
sudo systemctl start loki
curl -s http://localhost:3100/ready
# 5. Tester une requête sur une vieille période
curl -s 'http://localhost:3100/loki/api/v1/query_range?query={job="syslog"}&start=2026-08-01T00:00:00Z&end=2026-08-02T00:00:00Z' | head -c 500
```

### Scénario : stockage objet intact, Loki à reconstruire

```bash
# 1. Nouveau serveur, même loki.yaml (mêmes buckets, mêmes préfixes d'index)
# 2. Démarrer : Loki relit l'index TSDB depuis l'objet
# 3. Le WAL est perdu → trou de ~30 min de logs récents : acceptable,
#    les agents Promtail ne renvoient PAS l'historique (positions déjà avancées)
```

> ⚠️ Point honnête : après un sinistre total **sans WAL**, les logs en
> mémoire au moment du crash sont définitivement perdus. Pour les logs
> critiques (audit, sécurité), envisagez un double push Promtail vers
> deux Loki indépendants (`clients:` avec 2 URLs).

---

## 64. Mise à jour de Loki

Procédure sans surprise (binaire systemd) :

```bash
# 1. Lire les notes de version ! (breaking changes, migrations de schéma)
#    https://github.com/grafana/loki/releases

# 2. Sauvegarde avant (section 61)
sudo systemctl stop loki
sudo rsync -a /var/lib/loki/ /mnt/sauvegarde/loki-avant-maj/

# 3. Remplacer le binaire
cd /tmp
LOKI_VERSION="3.4.1"   # exemple
curl -sSLO "https://github.com/grafana/loki/releases/download/v${LOKI_VERSION}/loki-linux-amd64.zip"
unzip -o loki-linux-amd64.zip
sudo install -m 0755 loki-linux-amd64 /usr/local/bin/loki

# 4. Vérifier la config (dépréciations)
loki -config.file=/etc/loki/loki.yaml -verify-config 2>&1 | head -30
# ou : loki --print-config-stderr 2>/dev/null | grep -i deprecat

# 5. Redémarrer et surveiller
sudo systemctl start loki
sudo journalctl -u loki -f
curl -s http://localhost:3100/ready
```

Points d'attention par version :

| Montée | Vigilance |
|---|---|
| 2.9 → 3.x | schéma v13 recommandé, TSDB par défaut, flags renommés |
| Changement de schéma | ajouter une **nouvelle entrée** `schema_config` avec `from:` futur (section 66), jamais modifier l'existante |
| Ruler | vérifier que les règles s'évaluent toujours (`/ruler` API) |

---

## 65. Mise à jour de Promtail

Promtail est **stateless** (tout est dans `positions.yaml`) : la mise à
jour est triviale, mais à grande échelle il faut de la méthode.

```bash
# Sur un serveur : même procédure que Loki
sudo systemctl stop promtail
sudo install -m 0755 promtail-linux-amd64 /usr/local/bin/promtail
sudo systemctl start promtail
sudo journalctl -u promtail --since "5 min ago" --no-pager
```

À 20+ serveurs, automatisez (Ansible, section 50) par **vagues** :

1. vague 1 : 2 serveurs pilotes → 24 h d'observation ;
2. vague 2 : 50 % du parc ;
3. vague 3 : le reste.

> ⚠️ Ne supprimez jamais `positions.yaml` pendant la mise à jour :
> Promtail relirait tous les fichiers depuis le début → **doublons
> massifs** dans Loki (et pic d'ingestion).

---

## 66. Migration : changer de schéma de stockage

Exemple : passer de `boltdb-shipper` + filesystem à `tsdb` + S3.
**On ne modifie jamais une entrée de schéma existante** : on ajoute une
nouvelle entrée avec une date `from:` future.

```yaml
schema_config:
  configs:
    # Ancien schéma : inchangé, garde l'historique lisible
    - from: 2024-01-01
      store: boltdb-shipper
      object_store: filesystem
      schema: v12
      index:
        prefix: index_
        period: 24h
    # Nouveau schéma : actif à partir du 1er octobre 2026
    - from: 2026-10-01
      store: tsdb
      object_store: s3
      schema: v13
      index:
        prefix: index_
        period: 24h

storage_config:
  boltdb_shipper:
    active_index_directory: /var/lib/loki/bolt-index
    cache_location: /var/lib/loki/bolt-cache
  tsdb_shipper:
    active_index_directory: /var/lib/loki/tsdb-index
    cache_location: /var/lib/loki/tsdb-cache
  aws:
    s3: s3://KEY:SECRET@minio.local:9000/loki-chunks
    s3forcepathstyle: true
```

Déroulement :

1. Ajouter l'entrée, redémarrer Loki **avant** la date `from`.
2. Au jour J, les nouveaux chunks partent sur S3 en TSDB.
3. L'ancien index reste requêtable jusqu'à expiration de la rétention.
4. Quand la rétention a purgé l'ancien schéma, retirer son entrée
   (et son `storage_config` associé).

---

## 67. Dépannage : les logs n'arrivent pas

Arbre de décision, dans l'ordre :

```
1. Promtail tourne-t-il ?
   └─ systemctl status promtail / journalctl -u promtail
2. Promtail lit-il les fichiers ?
   └─ curl localhost:9080/metrics | grep promtail_targets_active
   └─ permissions ? (groupes adm/systemd-journal, section 16)
3. Promtail arrive-t-il à pousser ?
   └─ journalctl -u promtail | grep -i "error sending"
   └─ métrique promtail_dropped_bytes_total > 0 ?
4. Loki reçoit-il ?
   └─ rate(loki_distributor_lines_received_total[5m])
   └─ journalctl -u loki | grep -i error
5. La requête est-elle bonne ?
   └─ tester {job="..."} seul, sans filtre, sur 1h
   └─ vérifier les labels réellement présents : /loki/api/v1/labels
```

Commandes clés :

```bash
# Labels connus de Loki (le job existe-t-il vraiment ?)
curl -s http://localhost:3100/loki/api/v1/labels

# Valeurs d'un label
curl -s http://localhost:3100/loki/api/v1/label/job/values

# Dernières erreurs Promtail
sudo journalctl -u promtail -p err --since "30 min ago" --no-pager
```

---

## 68. Dépannage : requêtes trop lentes

| Symptôme | Cause probable | Remède |
|---|---|---|
| Timeout après 60 s | sélecteur trop large | resserrer labels + plage (section 39) |
| Lent seulement sur vieilles données | chunks froids sur objet | normal ; prévoir le cache |
| Lent même sur 1 h | trop de flux (cardinalité) | revoir les labels (section 26) |
| Grafana timeout, API OK | `proxy_read_timeout` trop court | 300 s côté nginx (section 57) |
| Tout est lent d'un coup | querier saturé / disque lent | `max_query_parallelism`, IOPS |

Diagnostic express :

```bash
# Temps de réponse d'une requête simple (doit être < 5 s)
time curl -s 'http://localhost:3100/loki/api/v1/query_range?query={job="syslog"}&limit=10' -o /dev/null

# Requêtes en cours et leur durée (métriques)
curl -s http://localhost:3100/metrics | grep -E "loki_request_duration_seconds_(sum|count)"
```

> 💡 Activez le **query-frontend** avec cache (section 8/13) : la 2ᵉ
> exécution d'un dashboard est souvent 10× plus rapide que la 1ʳᵉ.

---

## 69. Dépannage : disque plein

Le disque plein **bloque l'ingestion** (l'ingester ne peut plus flusher
ni écrire le WAL). Réaction en 4 temps :

1. **Urgence** : libérer de l'espace
   ```bash
   df -h /var/lib/loki
   du -sh /var/lib/loki/*          # qui grossit ? chunks ? wal ? index ?
   # Purger les vieux chunks MANUELLEMENT est risqué : préférez baisser
   # temporairement la rétention (le compactor nettoiera)
   ```
2. **Baisser la rétention** temporairement (`retention_period: 168h`),
   redémarrer, laisser le compactor travailler.
3. **Chercher la cause** : pic d'ingestion ? (`bytes_rate` par job),
   rétention trop longue vs disque, compactor en panne (section 79) ?
4. **Prévenir** : alerte Prometheus sur `node_filesystem_avail_bytes`,
   dimensionnement (compter ~1,5× la rétention en marge).

Dimensionnement indicatif :

```
espace_disque ≈ volume_journalier × jours_rétention × 1,5 (marge + WAL + index)
```

Exemple : 50 Go/jour × 31 jours × 1,5 ≈ **2,4 To**.

---

## 70. 10 cas de dépannage concrets

### Cas 1 — Promtail envoie, Loki rejette : `entry out of order`

**Symptôme** : `journalctl -u promtail` montre des 400 `entry out of order`.
**Cause** : deux sources poussent le même flux avec des timestamps qui se
croisent (ex : deux Promtail lisent le même fichier NFS).
**Remède** : un seul agent par fichier ; ou `max_out_of_order_time`
côté Loki (défaut 0 — à n'augmenter qu'en connaissance de cause).

### Cas 2 — Doublons après redémarrage Promtail

**Symptôme** : lignes en double sur quelques minutes après chaque restart.
**Cause** : `positions.yaml` supprimé ou illisible → relecture depuis le début.
**Remède** : ne jamais effacer `positions.yaml` ; vérifier ses permissions
(`promtail:promtail`, `0644`).

### Cas 3 — `too many outstanding requests` côté client

**Symptôme** : Promtail logue `too many outstanding requests`, des logs
sont droppés.
**Cause** : Loki (ou le réseau) n'absorbe pas le débit ; file d'envoi pleine.
**Remède** : augmenter `batchsize`, vérifier le réseau, scaler Loki ;
temporairement `limits_config.ingestion_rate_mb` plus haut. Détail section 71.

### Cas 4 — Requêtes vides alors que les logs existent

**Symptôme** : `{job="nginx"}` ne retourne rien sur les dernières 24 h.
**Cause fréquente** : le `timestamp` parsé par Promtail est faux (mauvais
`format:`) → les logs sont ingérés… en 1970 ou en 2030.
**Remède** : vérifier avec une requête sans filtre sur une large plage,
corriger le `format:` du stage `timestamp`.

### Cas 5 — Grafana : « Data source error »

**Symptôme** : Explore affiche une erreur de datasource.
**Causes** : URL fausse, Loki down, header `X-Scope-OrgID` manquant
(si `auth_enabled: true`), timeout du proxy.
**Remède** : « Save & Test » de la datasource, `curl` direct sur Loki,
vérifier le header dans la config de la datasource.

### Cas 6 — L'ingester est OOM-killed

**Symptôme** : `dmesg | grep -i oom` montre loki tué, trous dans les logs.
**Cause** : explosion du nombre de flux (cardinalité) ou `chunk_target_size`
trop gros × trop de flux.
**Remède** : trouver le label coupable (section 26), fixer
`max_streams_per_user`, augmenter la RAM ou réduire les labels.

### Cas 7 — Le ruler n'envoie aucune alerte

**Symptôme** : la règle existe mais rien n'arrive dans Alertmanager.
**Checklist** : `alertmanager_url` correct ? `enable_api: true` ?
règles dans `/var/lib/loki/rules/<tenant>/` ? `curl
http://localhost:3100/ruler/rule_groups` les liste-t-il ? L'expression
retourne-t-elle quelque chose dans Explore ?

### Cas 8 — Rétention ne supprime rien

**Symptôme** : le disque grossit malgré `retention_period: 744h`.
**Causes** : `compactor.retention_enabled: false` (oubli classique !),
compactor non démarré (`-target` partiel), ou `retention_delete_delay`
pas encore écoulé. Détail section 79.

### Cas 9 — Logs en retard de plusieurs minutes

**Symptôme** : les logs arrivent dans Grafana avec 2-5 min de retard.
**Causes** : `batchwait` trop haut, horloge désynchronisée (NTP !),
ingester saturé.
**Remède** : `batchwait: 1s`, vérifier `chrony`, dimensionner.

### Cas 10 — Montée de version cassée (schéma)

**Symptôme** : après upgrade, erreurs `schema mismatch` ou requêtes vides
sur l'historique.
**Cause** : entrée `schema_config` modifiée au lieu d'ajoutée.
**Remède** : restaurer l'ancienne entrée + ajouter la nouvelle avec
`from:` futur (section 66). Ne jamais réécrire l'historique des schémas.

---

## 71. Erreur 1 : `too many outstanding requests`

**Message typique (côté Promtail)** :
```
error sending batch: too many outstanding requests
```

**Ce que ça veut dire** : le client Promtail a plus de requêtes en vol
que Loki n'en accepte (`max_outstanding_per_tenant`, défaut 2048), ou
Loki répond trop lentement et la file sature.

**Causes** :
- pic d'ingestion brutal (redémarrage d'app qui vide son buffer) ;
- Loki sous-dimensionné ou disque lent (flush bloqué) ;
- réseau saturé entre Promtail et Loki.

**Remèdes** :
1. Côté Promtail, augmenter la taille des batchs (moins de requêtes) :
   ```yaml
   clients:
     - url: http://loki.local:3100/loki/api/v1/push
       batchsize: 2097152   # 2 Mo au lieu d'1 Mo
       batchwait: 2s
   ```
2. Côté Loki, vérifier que l'ingester flushe bien (disque, `flush_op_timeout`).
3. Si récurrent : scaler (plus de RAM/disque, ou mode `write` dédié).

---

## 72. Erreur 2 : `entry out of order`

**Message** : `rpc error: entry out of order` (HTTP 400 au push).

**Ce que ça veut dire** : pour un flux donné, Loki a reçu une ligne avec
un timestamp **antérieur** à la dernière ligne déjà ingérée. Loki exige
l'ordre chronologique par flux (les chunks sont append-only).

**Causes classiques** :
- deux Promtail lisent le **même fichier** (ex : fichier partagé en NFS,
  conteneur lu par 2 agents) ;
- horloge qui recule (VM restaurée depuis un snapshot, NTP qui corrige
  un gros décalage) ;
- `positions.yaml` corrompu → relecture d'anciennes lignes.

**Remèdes** :
```yaml
limits_config:
  # Tolère un léger désordre (défaut : 0 = aucun)
  max_out_of_order_time: 30s
```
> ⚠️ N'augmentez que si la cause est comprise (ex : logs multi-threadés
> avec timestamps proches). Le vrai fix est presque toujours : **une
> seule source par fichier**.

---

## 73. Erreur 3 : `per-stream rate limit exceeded`

**Message** : `per-stream rate limit exceeded (limit: 3MB/sec, burst: 15MB/sec)`.

**Ce que ça veut dire** : **un seul flux** dépasse le débit autorisé.
C'est le garde-fou anti-« un flux qui écrase tout » (souvent le symptôme
d'un label à trop forte cardinalité… ou d'une application qui logue en
boucle).

**Diagnostic** :
```logql
# Quel flux consomme le plus ? (top débit par job/host)
topk(10, sum by (job, host) (bytes_rate({job=~".+"}[5m])))
```

**Remèdes** :
1. Trouver la cause : bug applicatif (log en boucle) ? label trop fin ?
2. Ajuster si légitime (gros job batch qui logue beaucoup) :
   ```yaml
   limits_config:
     per_stream_rate_limit: 10MB
     per_stream_rate_limit_burst: 40MB
   ```
3. Si c'est un label à forte cardinalité : le supprimer (section 26),
   pas augmenter la limite.

---

## 74. Erreur 4 : `label value too long` / `max label value length`

**Message** : `label value too long` ou `max label value length exceeded`.

**Ce que ça veut dire** : une valeur de label dépasse `max_label_value_length`
(défaut 2048 caractères). Typique quand on promeut un **champ libre**
(message d'erreur, URL, stack trace) en label via le stage `labels:`.

**Remède** : ne pas mettre ce champ en label. Le parser en champ
(`| json`, `| regexp`) ou en structured metadata suffit :

```yaml
    pipeline_stages:
      - json:
          expressions:
            error_msg: msg
      # ❌ INTERDIT :
      # - labels:
      #     error_msg:
      # ✅ autorisé :
      - structured_metadata:
          error_msg:
```

Si la limite bloque un label légitime (hostname très long…), augmentez-la
avec parcimonie :
```yaml
limits_config:
  max_label_value_length: 4096
```

---

## 75. Erreur 5 : `schema mismatch` / erreurs de schéma

**Message** : `schema mismatch: schema config for ... not found` ou requêtes
vides sur certaines périodes après un changement de config.

**Ce que ça veut dire** : Loki ne trouve pas de schéma couvrant la période
demandée. Cause quasi unique : une entrée `schema_config` a été **modifiée**
ou **supprimée** au lieu d'ajouter une nouvelle entrée.

**Règle d'or** (répétée section 66) :
> Les entrées `schema_config` sont **immuables et cumulatives**.
> On ajoute, on ne modifie jamais, on ne supprime qu'après expiration
> complète de la rétention.

**Remède** : restaurer l'entrée d'origine depuis la sauvegarde de
`/etc/loki/loki.yaml`, redémarrer, vérifier :
```bash
curl -s http://localhost:3100/loki/api/v1/status/buildinfo
# + tester une requête sur l'ancienne période
```

---

## 76. Erreur 6 : HTTP 429 `too many requests`

**Message** : le push Promtail reçoit `429 Too Many Requests`.

**Ce que ça veut dire** : le **rate limiting global** (`ingestion_rate_mb`)
est dépassé. Contrairement à l'erreur 3 (par flux), ici c'est le **tenant
entier** qui pousse trop vite.

**Diagnostic** :
```logql
# Débit d'ingestion actuel vs limite
sum(rate(loki_distributor_bytes_received_total[5m])) / 1024 / 1024
# → comparer à ingestion_rate_mb (défaut 16 Mo/s en monolithique)
```

**Remèdes** :
1. Vérifier qu'il ne s'agit pas d'un pic temporaire (redémarrage massif).
2. Augmenter si le dimensionnement le permet :
   ```yaml
   limits_config:
     ingestion_rate_mb: 32
     ingestion_burst_size_mb: 64
   ```
3. Si le 429 est **constant** : c'est un problème de capacité, pas de
   limite — scaler le stockage et les ingesters.

---

## 77. Erreur 7 : explosion de cardinalité

**Symptômes** : RAM de l'ingester en croissance continue, `loki_ingester_memory_streams`
qui ne redescend jamais, OOM-kill, requêtes qui timeout.

**Diagnostic** :
```bash
# Nombre de flux actifs
curl -s http://localhost:3100/metrics | grep loki_ingester_memory_streams
# Séries par requête rejetées ?
curl -s http://localhost:3100/metrics | grep loki_discarded_samples_total
```

**Plan d'action** :
1. **Stopper l'hémorragie** : baisser temporairement `max_streams_per_user`
   (les nouveaux flux sont rejetés, l'existant continue).
2. **Identifier** : quel job a explosé ?
   ```logql
   sum by (job) (count_over_time({job=~".+"}[15m]))
   ```
   puis inspecter les labels de ce job (`/loki/api/v1/label/.../values`).
3. **Corriger** : retirer le label à forte cardinalité côté Promtail
   (section 27), redéployer.
4. **Prévenir** : `max_label_names_per_series`, revue des `pipeline_stages`
   à chaque nouveau job.

---

## 78. Erreur 8 : `query timeout`

**Message** : `query timeout` (ou timeout côté Grafana après 60-120 s).

**Ce que ça veut dire** : le querier n'a pas fini dans le temps imparti
(`query_timeout`, défaut 5 min côté Loki ; souvent moins côté proxy/Grafana).

**Checklist** :
1. La requête est-elle raisonnable ? (plage, sélecteur — section 39)
2. `query_timeout` et `max_query_length` cohérents ?
   ```yaml
   limits_config:
     query_timeout: 5m
     max_query_length: 721h
   ```
3. Le query-frontend découpe-t-il ? (`split_queries_by_interval: 30m`)
4. Les queriers ont-ils assez de CPU/RAM ? (requêtes = CPU + décompression)
5. Côté nginx : `proxy_read_timeout 300s;` (section 57)

> 💡 Une requête qui timeout **toujours** même optimisée signale un
> problème de dimensionnement ou de cardinalité, pas un problème de timeout.

---

## 79. Erreur 9 : le compactor ne tourne pas

**Symptômes** : la rétention ne supprime rien (section 70, cas 8),
l'index TSDB grossit indéfiniment, ou les requêtes sur de vieilles
données restent lentes (pas de compaction).

**Vérifications** :
```bash
# Le compactor est-il actif ?
curl -s http://localhost:3100/metrics | grep -E "loki_boltdb_shipper_compactor_running|loki_compactor"
# Dernière compaction réussie ?
curl -s http://localhost:3100/metrics | grep loki_compactor_last_successful_run
```

**Causes fréquentes** :
1. `retention_enabled: true` oublié dans le bloc `compactor:` → la
   rétention ne s'applique jamais (compaction seule OK).
2. En mode microservices : le déploiement `backend` (ou le `-target`
   incluant le compactor) n'est pas lancé.
3. `working_directory` non accessible en écriture.
4. Conflit de leadership en cluster (ring KV) : un seul compactor actif
   à la fois, c'est normal — mais il en faut **un**.

---

## 80. Erreur 10 : Promtail ne lit pas les fichiers

**Symptômes** : `promtail_targets_active == 0`, aucun log du job,
aucune erreur réseau.

**Checklist** (dans l'ordre) :
1. **Le glob matche-t-il ?**
   ```bash
   sudo -u promtail ls -l /var/log/nginx/*.log
   # → si "permission denied" ou "no such file", c'est ici que ça coince
   ```
2. **Permissions** : l'utilisateur `promtail` est-il dans les bons
   groupes ? (`adm`, `systemd-journal` — section 16)
3. **systemd durci** : `ProtectSystem=strict` sans `ReadOnlyPaths`
   → Promtail ne voit rien (section 16).
4. **positions.yaml** : corrompu ? (JSON invalide après crash disque)
   → renommer le fichier et redémarrer (relecture depuis le début :
   doublons temporaires, à n'utiliser qu'en dernier recours).
5. **Debug** : passer le log en debug temporairement :
   ```yaml
   server:
     log_level: debug
   ```
   puis `journalctl -u promtail -f | grep -i "tail\|target"`.

---

## 81. Checklist de mise en production

### Avant le go-live

- [ ] `loki.yaml` versionné (Git) et sauvegardé
- [ ] Stockage objet (S3/MinIO) configuré, ou disque dimensionné (section 69)
- [ ] WAL de l'ingester activé (section 7)
- [ ] Rétention configurée **et** compactor vérifié (sections 44, 79)
- [ ] `auth_enabled: true` + tenants + reverse proxy TLS (sections 55-57)
- [ ] Labels relus par un pair : pas de cardinalité dangereuse (sections 25-27)
- [ ] `max_streams_per_user` et rate limits fixés (sections 6, 26)
- [ ] Query-frontend + cache activés (section 8)
- [ ] Règles d'alerte Loki déployées et **testées** (sections 40-41)
- [ ] Supervision de Loki dans Prometheus + alertes santé (sections 59-60)
- [ ] Sauvegarde testée **avec restauration** (sections 61-63)
- [ ] Runbook : « Loki down » / « disque plein » / « ingestion arrêtée »
- [ ] Promtail déployé sur tous les serveurs (Ansible), `positions.yaml` persisté

### Les 30 premiers jours

- [ ] Revoir les seuils d'alerte (bruit vs signal)
- [ ] Vérifier la croissance disque réelle vs dimensionnement
- [ ] Auditer les labels : nouveaux jobs = nouvelle revue cardinalité
- [ ] Tester une restauration complète sur un environnement isolé
- [ ] Documenter les dashboards et requêtes utiles dans le wiki d'équipe

---

## 82. Pense-bête de poche LogQL

### Sélecteurs

```logql
{job="nginx"}                    # égalité
{job="nginx", host="web01"}      # ET implicite
{job=~"nginx|apache"}            # regex
{env!="prod"}                    # négation
{host=~"web0[12]"}               # regex partielle
```

### Filtres de ligne

```logql
|= "error"        # contient
!= "healthcheck"  # ne contient pas
|~ "5\d\d"        # regex
!~ "timeout"      # regex négative
```

### Parsers

```logql
| json                       # parse JSON
| logfmt                     # parse logfmt
| regexp `(?P<s>\d{3})`       # regex nommée
| pattern `<_> Failed <w>`   # pattern simplifié
| line_format `{{.msg}}`     # reformate la ligne affichée
| label_format host=`{{.h}}` # (re)nomme un label
```

### Filtres sur champs parsés

```logql
| json | level="error" | latency_ms > 1000
| logfmt | status=~"5.." | path!="/health"
```

### Fonctions métriques

```logql
rate({job="nginx"}[5m])                          # lignes/s
count_over_time({job="nginx"}[1h])               # lignes sur 1h
bytes_rate({job="nginx"}[5m])                    # octets/s
sum by (host) (rate({job="nginx"}[5m]))           # par host
topk(5, sum by (host) (count_over_time({job="nginx"}[1h])))
avg_over_time({j} | json | unwrap latency_ms [5m])
quantile_over_time(0.99, {j} | json | unwrap latency_ms [5m])
absent_over_time({job="syslog", host="srv01"}[10m])
```

### Recettes express

```logql
# Taux d'erreur 5xx nginx
sum(rate({job="nginx"} |~ " 5\d\d " [5m]))

# Brute-force SSH par minute
sum(count_over_time({job="sshd"} |= "Failed password" [1m]))

# Top IP attaquantes (structured metadata 'ip')
topk(20, sum by (ip) (count_over_time({job="sshd"} |= "Failed password" [24h])))

# Serveurs silencieux (pas de logs depuis 10 min)
# → absent_over_time + ruler (section 37)
```

---

## 83. Glossaire

| Terme | Définition |
|---|---|
| **Chunk** | bloc de logs compressé d'un seul flux ; unité de stockage |
| **Distributeur** | composant qui reçoit les pushs et route vers les ingesters |
| **Flux (stream)** | ensemble de lignes partageant exactement les mêmes labels |
| **Ingester** | composant qui écrit les chunks en mémoire puis les flushe |
| **Label** | paire clé=valeur indexée décrivant l'origine d'un log |
| **LogQL** | langage de requête de Loki (inspiré de PromQL) |
| **Cardinalité** | nombre de valeurs distinctes d'un label ; à garder faible |
| **Compactor** | compacte l'index TSDB et applique la rétention |
| **Ruler** | évalue les règles d'alerte/enregistrement sur les logs |
| **Querier** | exécute les requêtes sur ingesters + stockage |
| **Query-frontend** | cache, découpe et parallélise les requêtes |
| **Promtail** | agent qui lit les fichiers de log et les pousse vers Loki |
| **Pipeline stages** | étapes de transformation des logs dans Promtail |
| **Structured metadata** | paires clé=valeur non indexées, filtrables en LogQL |
| **TSDB** | format d'index moderne de Loki (remplace BoltDB) |
| **WAL** | journal write-ahead de l'ingester (survit aux redémarrages) |
| **Tenant** | espace de noms isolé quand `auth_enabled: true` |
| **Rétention** | durée de conservation des logs avant suppression |
| **rate()** | fonction LogQL : lignes par seconde sur une fenêtre |
| **unwrap** | extrait une valeur numérique pour les agrégations |
| **Derived fields** | champs cliquables dans Grafana (ex : trace_id → Tempo) |

---

## 84. Quiz : 10 questions + réponses

### Questions

1. Quelle est la différence fondamentale d'indexation entre Loki et
   Elasticsearch ?
2. Qu'est-ce qu'un flux (stream) dans Loki ?
3. Pourquoi ne faut-il jamais mettre un `request_id` en label ?
4. Que fait le compactor ?
5. Que signifie `rate({job="nginx"}[5m])` ?
6. Quelle est la différence entre `|=` et `|~` ?
7. À quoi sert le fichier `positions.yaml` de Promtail ?
8. Pourquoi faut-il activer le WAL de l'ingester en production ?
9. Que se passe-t-il si on modifie une entrée `schema_config` existante ?
10. Comment détecter qu'un serveur n'envoie plus de logs ?

### Réponses

1. Loki n'indexe que les **labels** (léger) et compresse le texte brut
   dans des chunks ; Elasticsearch construit un **index plein texte**
   (index inversé) de chaque mot — plus puissant en recherche, beaucoup
   plus coûteux.
2. L'ensemble des lignes de log partageant **exactement le même jeu de
   labels**. Le nombre de flux actifs détermine la mémoire des ingesters.
3. Parce que chaque valeur unique crée un **flux distinct en mémoire** :
   un million de requêtes = un million de flux → explosion de la
   cardinalité et OOM de l'ingester. On utilise un champ parsé ou la
   structured metadata à la place.
4. Il compacte les fichiers d'index TSDB, **applique la rétention**
   (suppression des chunks/index expirés) et déduplique.
5. Le **nombre de lignes de log par seconde** du job nginx, calculé sur
   une fenêtre glissante de 5 minutes.
6. `|=` filtre sur une **chaîne littérale** (rapide) ; `|~` filtre sur
   une **expression régulière** (plus coûteux). On place `|=` en premier.
7. Il mémorise **l'offset de lecture de chaque fichier** suivi. Le
   supprimer provoque une relecture complète → doublons massifs.
8. Pour ne pas perdre les chunks en mémoire (jusqu'à ~30 min de logs)
   en cas de crash/redémarrage de l'ingester.
9. Risque de `schema mismatch` : l'historique devient illisible ou les
   requêtes renvoient vide. Il faut **ajouter** une nouvelle entrée avec
   un `from:` futur, jamais modifier l'existante.
10. Avec `absent_over_time({job="syslog", host="srvX"}[10m])` dans une
    règle du ruler, ou en comparant
    `count(count_over_time({job="syslog"}[1h]) by (host))` à l'inventaire.

---

## 85. Pour aller plus loin

### Documentation officielle

- Docs Loki : https://grafana.com/docs/loki/
- Référence LogQL : https://grafana.com/docs/loki/latest/query/
- Exemples de configuration : https://github.com/grafana/loki/tree/main/cmd/loki

### À explorer ensuite

| Sujet | Pourquoi |
|---|---|
| **Mode scalable** (`-target=read/write/backend`) | l'étape entre monolithique et microservices |
| **Recording rules** du ruler | pré-calculer les métriques critiques (dashboards instantanés) |
| **Loki + Tempo** (traces) | derived fields `trace_id` : du log à la trace en un clic |
| **LogQL metric queries avancées** | `quantile_over_time`, histogrammes de latence |
| **Alloy** (successeur de Promtail) | l'agent unifié Grafana (logs, métriques, traces) |
| **S3 + lifecycle** | transition des vieux chunks vers stockage froid |
| **Tests de charge** | simuler le pic d'ingestion avant la prod |

### Livres et formations

- *Logging in Action* (Manning) — les fondamentaux de la centralisation
- La documentation Prometheus (les concepts LogQL en sont dérivés)
- Les dashboards communautaires Grafana pour Loki (ID à importer depuis
  grafana.com/dashboards, recherche « Loki »)

### Prochaines étapes pour votre infra

1. Maquette monolithique + MinIO en lab (1 semaine).
2. 3 serveurs pilotes en Promtail (2 semaines d'observation).
3. Seuils d'alerte calibrés, runbooks écrits.
4. Généralisation au parc + dashboards par service.
5. Revue trimestrielle : cardinalité, croissance disque, rétention.

---

*Guide rédigé pour Zelef — centralisation de logs d'infrastructure.*
*Versions : Loki 3.x, Promtail 3.x, Grafana 10/11, Debian 12 / Ubuntu 22.04+.*
*Les valeurs numériques sont des ordres de grandeur : validez sur votre infra.*
