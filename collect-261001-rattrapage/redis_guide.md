# Guide Redis — De l'installation à la production

> **Public :** sysadmins, chefs de service systèmes, DevOps.
> **Version couverte :** Redis 7.x (7.0 → 7.4). Les commandes marquées ⚠️ existent depuis une version précise, indiquée à chaque fois.
> **Angle :** pratique et production. Chaque section donne des commandes `redis-cli` testables, des tableaux de référence et des checklists opérationnelles.
> **Avertissement :** les mots de passe, adresses IP et noms d'hôtes de ce guide sont fictifs. Adaptez-les à votre infrastructure.

---

## 1. Qu'est-ce que Redis ?

Redis (**RE**mote **DI**ctionary **S**erver) est un **magasin de données en mémoire**, structuré en **clé-valeur**, qui persiste optionnellement sur disque. Il est écrit en C, open source (licence RSALv2/SSPLv1 depuis la 7.4 — point juridique à vérifier avec votre DSI avant déploiement en 7.4+ ; la 7.2 reste la dernière sous BSD).

Pourquoi il est partout en production :

| Caractéristique | Détail |
|---|---|
| Vitesse | ~100 000+ op/s sur une machine modeste, latence sub-milliseconde |
| Structures riches | Strings, Hashes, Lists, Sets, Sorted Sets, Streams, Bitmaps, HyperLogLog, Geo, JSON (module) |
| Persistance | RDB (snapshot) et/ou AOF (journal), combinables |
| Haute dispo | Réplication, Sentinel (failover auto), Cluster (sharding) |
| Atomicité | Commandes atomiques, transactions MULTI/EXEC, scripts Lua |
| Écosystème | Client dans tous les langages, exporters Prometheus, intégration K8s |

Cas d'usage typiques en entreprise : **cache applicatif**, **sessions**, **files d'attente**, **compteurs temps réel**, **rate limiting**, **leaderboards**, **pub/sub** pour événements, **verrous distribués**.

---

## 2. Redis dans un système d'information : où le placer ?

```
                    ┌──────────────┐
                    │  Application │
                    │  (PHP/Python │
                    │   /Java/Go)  │
                    └──────┬───────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
       ┌──────────┐ ┌───────────┐ ┌──────────┐
       │  Redis   │ │ PostgreSQL│ │  Stockage│
       │ (cache,  │ │ / MySQL   │ │  fichiers│
       │ sessions,│ │ (données  │ │          │
       │ files…)  │ │  durables)│ │          │
       └──────────┘ └───────────┘ └──────────┘
```

Règle d'or : **Redis n'est pas votre seule copie des données critiques** (sauf usage assumé type cache volatile). La base relationnelle reste la source de vérité ; Redis accélère et découple.

Topologies courantes :

| Topologie | Usage | Complexité |
|---|---|---|
| Instance unique | Dev, petits caches non critiques | Faible |
| Maître + réplica(s) | Lecture répartie, backup à chaud | Moyenne |
| Sentinel (3+ nœuds) | Failover automatique | Moyenne |
| Cluster (6+ nœuds) | Sharding, > RAM d'une machine | Élevée |

---

## 3. Concepts fondamentaux

### 3.1 Clé-valeur, mais pas que

Chaque donnée est une **clé** (chaîne binaire, conventionnellement `objet:id:champ`, ex. `user:1042:name`) associée à une **valeur typée**. Contrairement à Memcached (que des strings), Redis comprend la structure de la valeur et propose des opérations dessus (incrémenter un compteur, pousser dans une liste, etc.).

### 3.2 En mémoire, avec persistance optionnelle

- Les lectures/écritures se font en **RAM** → vitesse maximale.
- La persistance (RDB/AOF, sections 28–30) permet de survivre à un redémarrage. **Sans persistance, un reboot = perte totale.** C'est le piège n°1 des débutants.

### 3.3 Single-threaded (presque)

Le traitement des commandes est **mono-thread** : chaque commande s'exécute atomiquement, sans verrou. Conséquences pratiques :

- ✅ Pas de race condition sur une commande simple (`INCR` est atomique).
- ⚠️ Une commande lente (ex. `KEYS *` sur des millions de clés) **bloque tout le serveur**. D'où l'interdiction de `KEYS` en prod (utiliser `SCAN`, section 91).
- Depuis Redis 6, les **E/S réseau sont multi-threadées** (`io-threads`), mais l'exécution reste mono-thread.

### 3.4 Bases de données numérotées

Par défaut 16 bases (`0` à `15`), sélection via `SELECT 2`. **En pratique :** préférez **une instance par usage** plutôt que plusieurs bases — le Cluster ne supporte que la base 0, et `FLUSHDB`/`FLUSHALL` sont sources d'incidents.

```bash
redis-cli -n 2 SET ma_cle "valeur"   # -n = numéro de base
```

---

## 4. Les structures de données en un coup d'œil

| Type | Contenu | Cas d'usage typique | Commandes clés |
|---|---|---|---|
| String | Texte / nombre / binaire (≤ 512 Mo) | Cache, compteurs, sessions, verrous | SET, GET, INCR, SETEX |
| Hash | Champ → valeur (objet) | Fiche utilisateur, produit | HSET, HGET, HGETALL |
| List | Liste ordonnée d'insertion | File d'attente, historique | LPUSH, RPOP, LRANGE |
| Set | Ensemble unique non ordonné | Tags, abonnés, votes | SADD, SMEMBERS, SINTER |
| Sorted Set | Ensemble + score (trié) | Classements, planning | ZADD, ZRANGE, ZRANK |
| Stream | Journal append-only | Event sourcing, files robustes | XADD, XREAD, XACK |
| Bitmap | Bits (⚠️ 7.x via SETBIT) | Présence/absence, stats | SETBIT, GETBIT, BITCOUNT |
| HyperLogLog | Compteur approximatif unique | Visiteurs uniques | PFADD, PFCOUNT |
| Geo | Coordonnées géographiques | Points d'intérêt proches | GEOADD, GEORADIUS (⚠️ 6.2+: GEOSEARCH) |

> Les sections 5 à 20 détaillent chaque type avec des exemples `redis-cli` complets.

---

## 5. Strings : concepts

Le type le plus simple et le plus utilisé : une clé → une valeur (chaîne, entier, ou binaire). Limite : **512 Mo** par valeur.

Idées reçues à corriger :

- Un entier stocké en string (`"1042"`) peut être **incrémenté atomiquement** avec `INCR` — Redis convertit à la volée.
- Les strings servent aussi à stocker du **binaire** (images miniatures, blobs sérialisés), mais préférez un stockage objet (S3/NFS) au-delà de quelques Mo.
- `SET` avec options `NX`/`XX`/`EX`/`PX` remplace avantageusement `SETNX`+`EXPIRE` (atomique, voir section 46).

---

## 6. Strings : commandes essentielles

```bash
# --- Bases ---
redis-cli SET user:1042:name "Awa Diallo"
redis-cli GET user:1042:name
# "Awa Diallo"

redis-cli MSET user:1042:ville "Dakar" user:1042:role "admin"
redis-cli MGET user:1042:ville user:1042:role
# 1) "Dakar"
# 2) "admin"

# --- Expiration à la création (atomique) ---
redis-cli SET session:abc123 "donnees..." EX 3600   # expire dans 1 h
redis-cli SET session:abc123 "donnees..." PX 60000  # expire dans 60 s (ms)

# --- Ne créer que si absent (verrous, voir section 46) ---
redis-cli SET verrou:job42 "worker-1" NX PX 30000
# (nil) si la clé existe déjà, "OK" sinon

# --- Compteurs atomiques ---
redis-cli SET compteur:visites 0
redis-cli INCR compteur:visites        # → 1
redis-cli INCRBY compteur:visites 10   # → 11
redis-cli DECR compteur:visites        # → 10
redis-cli INCRBYFLOAT prix:total 19.99  # flottants

# --- Manipulation de chaînes ---
redis-cli APPEND log:2026-09-26 "ligne1\n"   # concatène, retourne la taille
redis-cli STRLEN user:1042:name              # longueur
redis-cli GETRANGE user:1042:name 0 2         # "Awa" (substring)
redis-cli SETRANGE user:1042:name 4 "DIALLO"  # écrase à partir de l'offset

# --- Get-and-set atomique ---
redis-cli GETSET compteur:visites 0   # retourne l'ancienne valeur, met 0
redis-cli GETDEL session:abc123       # ⚠️ 6.2+ : lit puis supprime
redis-cli GETEX session:abc123 EX 3600  # ⚠️ 6.2+ : lit et prolonge le TTL
```

Tableau récapitulatif des options de `SET` :

| Option | Effet |
|---|---|
| `EX secondes` | TTL en secondes |
| `PX millisecondes` | TTL en millisecondes |
| `EXAT timestamp` | Expire à un timestamp Unix (⚠️ 7.0+) |
| `PXAT timestamp-ms` | Expire à un timestamp en ms (⚠️ 7.0+) |
| `NX` | Écrit seulement si la clé **n'existe pas** |
| `XX` | Écrit seulement si la clé **existe déjà** |
| `KEEPTTL` | ⚠️ 6.0+ : conserve le TTL existant |
| `GET` | ⚠️ 6.2+ : retourne l'ancienne valeur (comme GETSET) |

---

## 7. Hashes : concepts

Un hash = un **objet** (ensemble de champs/valeurs) sous une seule clé. Idéal pour les fiches : `user:1042` → `{name, email, role, ...}`.

Avantages vs N strings :

- Une seule clé à expirer/gérer (le TTL s'applique **au hash entier**, pas par champ — ⚠️ 7.4+ introduit l'expiration par champ, voir section 8).
- `HGETALL` récupère l'objet d'un coup ; `HMGET` ne prend que les champs utiles (économie réseau).
- Mémoire optimisée : avec peu de champs, Redis utilise un encodage compact (`listpack`).

Convention de nommage : `objet:id` → `user:1042`, `produit:sku-7788`, `vm:proxmox-12`.

---

## 8. Hashes : commandes essentielles

```bash
# --- Écriture ---
redis-cli HSET user:1042 name "Awa Diallo" email "awa@exemple.sn" role "admin"
# → 3 (nombre de champs créés)

redis-cli HSETNX user:1042 role "user"   # n'écrit que si le champ n'existe pas → 0 ici

# --- Lecture ---
redis-cli HGET user:1042 name
# "Awa Diallo"

redis-cli HMGET user:1042 name email role
# 1) "Awa Diallo"
# 2) "awa@exemple.sn"
# 3) "admin"

redis-cli HGETALL user:1042
# 1) "name"  2) "Awa Diallo"  3) "email" ...

redis-cli HKEYS user:1042    # liste des champs
redis-cli HVALS user:1042    # liste des valeurs
redis-cli HLEN user:1042     # nombre de champs → 3
redis-cli HEXISTS user:1042 email  # → 1

# --- Compteurs dans un hash ---
redis-cli HINCRBY stats:page:accueil vues 1
redis-cli HINCRBYFLOAT stats:panier total 49.90

# --- Suppression ---
redis-cli HDEL user:1042 role     # supprime le champ
redis-cli DEL user:1042           # supprime tout le hash

# --- Itération sûre (gros hashes) ---
redis-cli HSCAN user:1042 0
# Comme SCAN : curseur, à boucler jusqu'à curseur 0 (voir section 91)

# --- Expiration par champ (⚠️ Redis 7.4+) ---
redis-cli HEXPIRE user:1042 3600 FIELDS 1 email   # le champ email expire dans 1 h
redis-cli HTTL user:1042 FIELDS 1 email           # TTL restant du champ
```

> **Piège classique :** `HGETALL` sur un hash de 100 000 champs bloque le serveur le temps du transfert. En prod, préférez `HMGET`/`HSCAN`.

---

## 9. Lists : concepts

Liste ordonnée de strings, optimisée pour les **insertions/suppressions aux deux extrémités** (implémentation : quicklist). C'est la structure historique des **files d'attente**.

- `LPUSH` = empile à gauche (tête), `RPUSH` = à droite (queue).
- `LPOP`/`RPOP` = dépile. Avec `BLPOP`/`BRPOP`, le client **attend** (bloquant) qu'un élément arrive — parfait pour un worker.
- `LLEN`, `LRANGE ma_liste 0 -1` (tout), `LINDEX`, `LTRIM` (garder les N derniers : historique borné).

Modèle file FIFO : producteurs `RPUSH`, consommateurs `BLPOP` (ou `BRPOPLPUSH` pour fiabiliser, voir section 48).

---

## 10. Lists : commandes essentielles

```bash
# --- File d'attente FIFO ---
redis-cli RPUSH jobs:emails "mail:1" "mail:2" "mail:3"
redis-cli LLEN jobs:emails          # → 3
redis-cli LPOP jobs:emails          # "mail:1" (le plus ancien)
redis-cli RPOP jobs:emails          # "mail:3" (le plus récent)

# --- Consommation bloquante (worker) ---
redis-cli BLPOP jobs:emails 30
# attend jusqu'à 30 s un élément ; retourne ["jobs:emails","mail:2"] ou nil après timeout
# Variante multi-files : BLPOP jobs:urgent jobs:emails 10  (priorité à jobs:urgent)

# --- Lecture sans dépiler ---
redis-cli LRANGE jobs:emails 0 -1     # tout
redis-cli LRANGE jobs:emails 0 9      # les 10 premiers (pagination)
redis-cli LINDEX jobs:emails 0        # premier élément
redis-cli LPOS jobs:emails "mail:2"   # ⚠️ 6.0.6+ : position d'un élément

# --- Pile (LIFO) ---
redis-cli LPUSH pile "a" "b" "c"
redis-cli LPOP pile   # "c"

# --- Historique borné (les 100 derniers logs) ---
redis-cli RPUSH logs:app "event..." 
redis-cli LTRIM logs:app -100 -1      # ne garde que les 100 derniers
# ⚠️ LTRIM sur une liste en cours d'écriture : à combiner dans un pipeline/Lua

# --- Transfert atomique entre listes (file fiable, voir section 48) ---
redis-cli BRPOPLPUSH jobs:emails jobs:encours 30  # ⚠️ 6.2+ : préférer LMOVE
redis-cli LMOVE jobs:emails jobs:encours LEFT RIGHT  # ⚠️ 6.2+

# --- Insertion ciblée ---
redis-cli LINSERT jobs:emails BEFORE "mail:2" "mail:urgent"
redis-cli LSET jobs:emails 0 "mail:1-modifie"   # écrase par index
redis-cli LREM jobs:emails 1 "mail:2"          # supprime 1 occurrence
```

---

## 11. Sets : concepts

Ensemble **non ordonné** de membres **uniques**. Les doublons sont ignorés silencieusement. Opérations ensemblistes natives : **intersection**, **union**, **différence** — calculées côté serveur, très rapides.

Cas d'usage :

- Tags d'articles : `SADD article:55:tags php redis cache`
- Abonnés, votants (unicité garantie), IPs bannies
- « Utilisateurs en ligne » avec expiration globale
- Recommandations : `SINTER user:1:achats user:2:achats` → goûts communs

Complexité : O(1) pour SADD/SREM/SISMEMBER, O(N) pour les opérations multi-sets.

---

## 12. Sets : commandes essentielles

```bash
# --- Bases ---
redis-cli SADD tags:article:55 php redis cache redis   # → 3 (doublon ignoré)
redis-cli SMEMBERS tags:article:55
redis-cli SISMEMBER tags:article:55 php    # → 1
redis-cli SMISMEMBER tags:article:55 php java  # ⚠️ 6.2+ : 1) 1  2) 0
redis-cli SCARD tags:article:55             # → 3
redis-cli SREM tags:article:55 php         # → 1

# --- Tirage aléatoire ---
redis-cli SRANDMEMBER tags:article:55        # 1 membre au hasard
redis-cli SRANDMEMBER tags:article:55 2      # 2 membres (peut répéter si > card... non : sans répétition si positif)
redis-cli SPOP tags:article:55              # retire ET retourne un membre au hasard

# --- Opérations ensemblistes ---
redis-cli SADD user:1:achats livre souris clavier
redis-cli SADD user:2:achats livre ecran clavier
redis-cli SINTER user:1:achats user:2:achats   # → livre, clavier
redis-cli SUNION  user:1:achats user:2:achats  # → les 5
redis-cli SDIFF   user:1:achats user:2:achats  # → souris (chez 1 mais pas 2)

# --- Stocker le résultat (évite de ramener des Mo au client) ---
redis-cli SINTERSTORE reco:1:2 user:1:achats user:2:achats
redis-cli SUNIONSTORE tous:achats user:1:achats user:2:achats
redis-cli SDIFFSTORE diff:1:2 user:1:achats user:2:achats

# --- Déplacer un membre entre sets ---
redis-cli SMOVE user:1:achats user:2:achats souris

# --- Itération ---
redis-cli SSCAN tags:article:55 0
```

> **Astuce prod :** pour « N membres au hasard sans les retirer » sur un gros set, `SRANDMEMBER key -N` (négatif = avec répétitions possibles) est plus rapide que `SMEMBERS` + tirage côté client.

---

## 13. Sorted Sets : concepts

Comme un Set, mais chaque membre a un **score** (flottant). Redis maintient l'ordre **trié par score** (puis lexicographique à score égal). Implémentation : skiplist + dict → O(log N) pour ZADD/ZRANGE.

Cas d'usage rois :

- **Classements / leaderboards** (score = points)
- **Plannings / files à priorité** (score = timestamp → `ZRANGEBYSCORE` pour « les tâches dues »)
- **Rate limiting** à fenêtre glissante (score = timestamp, voir section 50)
- **Time-series** simples

Le membre doit être unique ; pour le même membre, `ZADD` **met à jour** le score (pratique pour les classements évolutifs).

---

## 14. Sorted Sets : commandes essentielles

```bash
# --- Classement ---
redis-cli ZADD leaderboard 1500 "awa" 2300 "moussa" 1800 "fatou"
redis-cli ZRANGE leaderboard 0 -1 WITHSCORES
# 1) "awa" 2) "1500" 3) "fatou" 4) "1800" 5) "moussa" 6) "2300"

# ⚠️ 6.2+ : ZRANGE remplace ZREVRANGE (option REV)
redis-cli ZRANGE leaderboard 0 2 REV WITHSCORES   # top 3 décroissant

redis-cli ZRANK leaderboard "fatou"       # rang croissant → 1
redis-cli ZREVRANK leaderboard "fatou"   # rang décroissant → 1
redis-cli ZSCORE leaderboard "moussa"    # "2300"
redis-cli ZCARD leaderboard              # → 3

# --- Mise à jour / incrément ---
redis-cli ZINCRBY leaderboard 100 "awa"  # awa passe à 1600
redis-cli ZADD leaderboard XX 2000 "awa" # XX : ne met à jour que si existe
redis-cli ZADD leaderboard NX 100 "ibrahim"  # NX : n'ajoute que si absent

# --- Requêtes par score (planning / fenêtre glissante) ---
redis-cli ZADD taches:due 1769557200 "job:1" 1769557300 "job:2"
redis-cli ZRANGEBYSCORE taches:due 0 1769557250        # tâches dues
# ⚠️ 6.2+ : préférer ZRANGE ... BYSCORE :
redis-cli ZRANGE taches:due 0 1769557250 BYSCORE

# --- Requêtes par membre (ordre lexicographique, ⚠️ scores égaux) ---
redis-cli ZADD idx 0 "aaa" 0 "aab" 0 "abc"
redis-cli ZRANGE idx "[aab" "[abc" BYLEX   # → aab, abc

# --- Suppressions ---
redis-cli ZREM leaderboard "awa"
redis-cli ZREMRANGEBYRANK leaderboard 0 9      # supprime les 10 plus bas
redis-cli ZREMRANGEBYSCORE taches:due 0 1769557250  # purge les tâches dues traitées
redis-cli ZPOPMIN taches:due 1   # ⚠️ 5.0+ : dépile le plus petit score (file à priorité !)
redis-cli ZPOPMAX leaderboard 3  # top 3 dépilés

# --- Comptage dans un intervalle ---
redis-cli ZCOUNT leaderboard 1000 2000
```

---

## 15. Streams : concepts

Les Streams (⚠️ Redis 5.0+) sont des **journaux append-only** : chaque entrée a un **ID** (`timestamp-séquence`, ex. `1769557200000-0`, ou `*` pour auto) et des champs clé-valeur. C'est la structure la plus robuste pour les **files de messages** et l'**event sourcing**.

Concepts clés :

- **Consumer groups** : plusieurs consommateurs se partagent un stream ; chaque message est distribué à **un seul** consommateur du groupe.
- **Pending entries** : messages lus mais non acquittés (`XACK`). `XPENDING`/`XAUTOCLAIM` permettent de **récupérer** les messages d'un worker mort — fiabilité impossible avec les simples lists.
- `XREAD BLOCK` : lecture bloquante comme `BLPOP`, mais multi-streams.
- `MAXLEN ~ N` : borne approximative de la taille (le `~` évite un trim exact coûteux).

> Pour une file simple sans besoin de rejeu ni de groupes : les **lists** suffisent (section 48). Pour du robuste : **streams** (section 49).

---


## 16. Streams : commandes essentielles

```bash
# --- Production de messages ---
redis-cli XADD events:commandes * user 1042 action "panier_valide" montant 149.90
# → "1769557200000-0" (ID auto)

redis-cli XADD events:commandes MAXLEN ~ 10000 * user 1043 action "panier_valide"
# borne la taille à ~10 000 entrées (trim approximatif, peu coûteux)

# --- Lecture simple ---
redis-cli XLEN events:commandes
redis-cli XRANGE events:commandes - +            # tout (borne - / +)
redis-cli XRANGE events:commandes - + COUNT 10   # les 10 premiers
redis-cli XREVRANGE events:commandes + - COUNT 5  # les 5 derniers
redis-cli XREAD COUNT 10 STREAMS events:commandes 0   # depuis le début
redis-cli XREAD BLOCK 5000 STREAMS events:commandes $  # bloque 5 s, $ = nouveaux seulement

# --- Consumer groups (le vrai mode "file robuste") ---
redis-cli XGROUP CREATE events:commandes grp:facturation 0 MKSTREAM
# 0 = depuis le début ; $ = seulement les nouveaux ; MKSTREAM crée le stream s'il n'existe pas

redis-cli XREADGROUP GROUP grp:facturation worker-1 COUNT 5 STREAMS events:commandes ">"
# ">" = messages jamais distribués. Retourne les entrées à traiter.

# --- Acquittement ---
redis-cli XACK events:commandes grp:facturation 1769557200000-0
# Sans XACK, le message reste "pending" (non traité) — voir récupération ci-dessous.

# --- Surveillance des pending ---
redis-cli XPENDING events:commandes grp:facturation
# résumé : nb pending, plus petit/plus grand ID, par consommateur

redis-cli XPENDING events:commandes grp:facturation - + 10 worker-1
# détail des 10 pending du worker-1 (idle, retry count...)

# --- Récupérer les messages d'un worker mort (⚠️ 7.0+ : XAUTOCLAIM) ---
redis-cli XAUTOCLAIM events:commandes grp:facturation worker-2 60000 0 COUNT 10
# réclame les messages idle depuis > 60 s, à partir du début

# --- Infos ---
redis-cli XINFO STREAM events:commandes
redis-cli XINFO GROUPS events:commandes
redis-cli XINFO CONSUMERS events:commandes grp:facturation

# --- Suppression / trim ---
redis-cli XDEL events:commandes 1769557200000-0   # supprime une entrée (ne libère pas toujours la mémoire de suite)
redis-cli XTRIM events:commandes MAXLEN ~ 5000    # trim manuel
```

Cycle de vie robuste d'un worker : `XREADGROUP` → traitement → `XACK`. Un superviseur périodique fait `XAUTOCLAIM` sur les idle > N secondes. Voir le cas pratique section 86.

---

## 17. Bitmaps : introduction

Un bitmap manipule des **bits individuels** dans une string. Idéal pour des **statistiques de présence** : « l'utilisateur 1042 s'est-il connecté le 2026-09-26 ? » → 1 bit par utilisateur et par jour.

```bash
# --- Marquer / lire ---
redis-cli SETBIT presence:2026-09-26 1042 1   # user 1042 présent
redis-cli SETBIT presence:2026-09-26 2048 1
redis-cli GETBIT presence:2026-09-26 1042     # → 1
redis-cli GETBIT presence:2026-09-26 9999     # → 0 (jamais défini)

# --- Comptage ---
redis-cli BITCOUNT presence:2026-09-26        # nb de bits à 1 = nb de présents
redis-cli BITCOUNT presence:2026-09-26 0 0    # sur le premier octet seulement

# --- Opérations bit à bit (rétention : présents J ET J-1) ---
redis-cli BITOP AND retention:j_et_j1 presence:2026-09-26 presence:2026-09-25
redis-cli BITCOUNT retention:j_et_j1

# --- Trouver le premier bit à 1 ---
redis-cli BITPOS presence:2026-09-26 1        # position du premier présent
```

Ordre de grandeur mémoire : **1 million d'utilisateurs = ~125 Ko par jour**. Imbattable pour des cohortes/DAU. Limite : l'offset max est 2^32-1 ; au-delà, découpez en plusieurs clés.

---

## 18. HyperLogLog : introduction

`HyperLogLog` estime le **nombre d'éléments distincts** avec une erreur standard de **0,81 %**, pour **12 Ko** de mémoire — quel que soit le volume (milliards d'éléments).

```bash
redis-cli PFADD visiteurs:2026-09-26 "user:1042" "user:2048" "user:1042"
# → 1 (le HLL a été modifié ; le doublon est ignoré)

redis-cli PFCOUNT visiteurs:2026-09-26          # estimation des uniques
redis-cli PFCOUNT visiteurs:2026-09-26 visiteurs:2026-09-25  # union sur plusieurs jours
redis-cli PFMERGE visiteurs:semaine visiteurs:2026-09-2*     # fusion persistante
```

Cas d'usage : **visiteurs uniques**, IPs distinctes, recherches uniques. ⚠️ C'est une **estimation** : n'utilisez pas pour de la facturation ou des quotas stricts. Pour un comptage exact, utilisez un Set (mémoire proportionnelle au volume).

---

## 19. Geo : index géospatiaux (introduction)

Redis indexe des points (longitude, latitude) et répond à « quoi dans un rayon de X km ? ».

```bash
# --- Ajouter des points ---
redis-cli GEOADD agences:sn -17.4441 14.7167 "agence:dakar" -16.9269 14.6937 "agence:pikine"
# ordre : LONGITUDE LATITUDE membre

# --- Distance ---
redis-cli GEODIST agences:sn "agence:dakar" "agence:pikine" km
# → "17.1234" (km ; unités : m, km, ft, mi)

# --- Recherche autour d'un point (⚠️ 6.2+ : GEOSEARCH, GEORADIUS déprécié) ---
redis-cli GEOSEARCH agences:sn FROMLONLAT -17.4441 14.7167 BYRADIUS 20 km WITHDIST
# → agences dans un rayon de 20 km, avec distances

redis-cli GEOSEARCH agences:sn FROMMEMBER "agence:dakar" BYBOX 30 30 km COUNT 5

# --- Infos ---
redis-cli GEOPOS agences:sn "agence:dakar"    # coordonnées stockées
redis-cli GEOHASH agences:sn "agence:dakar"   # geohash (interopérabilité)
```

Cas d'usage : agences les plus proches, livreurs dans un rayon, géofencing simple. Pour des polygones complexes ou des millions de points avec requêtes spatiales avancées, préférez PostGIS.

---

## 20. Modules : RedisJSON, RediSearch, RedisTimeSeries (introduction)

Depuis Redis 7, les modules les plus utiles sont regroupés dans **Redis Stack** (même binaire). Sur Debian/Ubuntu, le paquet `redis-stack-server` les inclut.

| Module | Apporte | Exemple |
|---|---|---|
| RedisJSON | Stocker/manipuler du JSON natif (type `JSON`) | `JSON.SET doc:1 $ '{"nom":"Awa"}'` |
| RediSearch | Indexation plein texte + requêtes | `FT.CREATE idx ON JSON ...`, `FT.SEARCH` |
| RedisTimeSeries | Séries temporelles (downsampling) | `TS.ADD sonde:1 * 23.5` |
| RedisBloom | Filtres probabilistes | `BF.ADD` |

```bash
# Exemple RedisJSON (si module chargé)
redis-cli JSON.SET produit:42 $ '{"nom":"Onduleur 3kVA","prix":450000,"stock":12}'
redis-cli JSON.GET produit:42 $.prix
redis-cli JSON.NUMINCRBY produit:42 $.stock -1
```

> **Note production :** chaque module augmente la surface mémoire/CPU et la complexité. N'activez que ce dont vous avez besoin (`loadmodule` sélectif dans redis.conf). Le Cluster impose des contraintes sur les clés multi-champs (hash tags, section 62).

---

## 21. Installation sur Debian/Ubuntu (méthode recommandée)

N'utilisez **pas** le paquet `redis-server` des dépôts Debian/Ubuntu standards : il est souvent **plusieurs versions en retard** (ex. Redis 6 sur d'anciennes LTS). Utilisez le **dépôt officiel Redis**.

```bash
# --- 1. Prérequis ---
sudo apt update
sudo apt install -y curl gnupg lsb-release

# --- 2. Clé GPG et dépôt officiel ---
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" \
  | sudo tee /etc/apt/sources.list.d/redis.list

# --- 3. Installation ---
sudo apt update
sudo apt install -y redis-server

# --- 4. Vérification ---
redis-server --version
# Redis server v=7.4.0 sha=... malloc=jemalloc-5.3.0 bits=64
redis-cli --version

# --- 5. Service systemd ---
sudo systemctl enable --now redis-server
sudo systemctl status redis-server --no-pager
redis-cli ping
# PONG
```

Le paquet officiel fournit :

- `/etc/redis/redis.conf` — configuration
- `/var/lib/redis/dump.rdb` — snapshot par défaut
- `/var/log/redis/redis-server.log` — logs
- Service `redis-server` supervisé par systemd

> **Épingler la version** en production (évite une montée majeure surprise) :
> ```bash
> sudo apt-mark hold redis-server
> # Pour mettre à jour volontairement : sudo apt-mark unhold redis-server && sudo apt install redis-server
> ```

---

## 22. Installation depuis les sources (compilation)

Utile pour une version précise, un patch, ou une architecture exotique.

```bash
# --- Dépendances de build ---
sudo apt update
sudo apt install -y build-essential tcl pkg-config

# --- Téléchargement et compilation ---
cd /tmp
curl -fsSLO https://download.redis.io/releases/redis-7.4.0.tar.gz
tar xzf redis-7.4.0.tar.gz
cd redis-7.4.0

# Build avec jemalloc (allocateur recommandé, défaut) et TLS :
make -j"$(nproc)" BUILD_TLS=yes

# --- Tests (optionnel mais recommandé, ~quelques minutes) ---
make test
# \o/ All tests passed without errors!

# --- Installation ---
sudo make install PREFIX=/usr/local
# binaires : /usr/local/bin/redis-server, redis-cli, redis-sentinel...

# --- Vérification TLS compilé ---
redis-server --help | grep -i tls || redis-cli --tls --help | head -1
```

> Créez ensuite l'utilisateur système, les répertoires et le service systemd manuellement (reprenez les fichiers du paquet `.deb` comme modèle : `/lib/systemd/system/redis-server.service`).

---

## 23. Installation via Docker (dev / tests)

```bash
# --- Instance éphémère pour tester ---
docker run -d --name redis-dev -p 6379:6379 redis:7.4-alpine redis-server --appendonly yes

# --- Avec persistance et config perso ---
mkdir -p /srv/redis/data
cat > /srv/redis/redis.conf <<'EOF'
bind 0.0.0.0
protected-mode yes
port 6379
requirepass CHANGEZ_MOI
appendonly yes
maxmemory 512mb
maxmemory-policy allkeys-lru
EOF

docker run -d --name redis-prod \
  -p 6379:6379 \
  -v /srv/redis/data:/data \
  -v /srv/redis/redis.conf:/usr/local/etc/redis/redis.conf \
  --restart unless-stopped \
  redis:7.4-alpine /usr/local/etc/redis/redis.conf

redis-cli -a CHANGEZ_MOI ping
```

> ⚠️ En production « vraie », préférez le paquet système ou une installation dédiée : Docker ajoute une couche réseau et complique le tuning (THP, overcommit, voir section 26). Pour du dev et des tests d'intégration, c'est parfait.

---

## 24. Premier démarrage et vérification

Checklist post-installation (à faire **avant** d'ouvrir au réseau) :

```bash
# 1. Le service tourne ?
sudo systemctl is-active redis-server   # → active
ss -ltnp | grep 6379                     # écoute ?

# 2. Répond-il ?
redis-cli ping   # → PONG

# 3. Quelle version / config ?
redis-cli INFO server | grep -E "redis_version|process_id|uptime_in_days"
redis-cli CONFIG GET maxmemory
redis-cli CONFIG GET appendonly

# 4. Persistance active ?
ls -lh /var/lib/redis/   # dump.rdb présent après un BGSAVE

# 5. Test d'écriture/lecture + expiration ---
redis-cli SET test:demarrage "ok" EX 60
redis-cli GET test:demarrage
redis-cli TTL test:demarrage

# 6. Nettoyage du test ---
redis-cli DEL test:demarrage
```

Si `redis-cli ping` répond `PONG` mais que vous prévoyez d'exposer Redis au réseau : **ne passez pas à la suite sans les sections 35–40 (sécurité)**.

---

## 25. redis.conf : organisation du fichier

Le fichier `/etc/redis/redis.conf` fait ~1 400 lignes dont 90 % de commentaires. Structure :

```
INCLUDES        → inclure d'autres fichiers (ex. /etc/redis/conf.d/*.conf)
MODULES         → loadmodule ...
NETWORK         → bind, port, protected-mode, timeout...
GENERAL         → daemonize, supervised, logfile, databases...
SNAPSHOTTING    → save, rdbcompression...
REPLICATION     → replicaof, ...
SECURITY        → requirepass, aclfile...
CLIENTS         → maxclients
MEMORY MGMT     → maxmemory, maxmemory-policy, ...
APPEND ONLY     → appendonly, appendfsync...
...
```

**Bonne pratique :** ne modifiez pas le fichier d'origine à la main en vrac. Créez un fichier d'override :

```bash
sudo mkdir -p /etc/redis/conf.d
sudo tee /etc/redis/conf.d/99-production.conf <<'EOF'
# Overrides production — Zelef, 2026-09-26
maxmemory 4gb
maxmemory-policy allkeys-lru
appendonly yes
EOF
echo "include /etc/redis/conf.d/99-production.conf" | sudo tee -a /etc/redis/redis.conf
sudo systemctl restart redis-server
```

Et pour un réglage à chaud (sans restart), `CONFIG SET` (persisté via `CONFIG REWRITE`) :

```bash
redis-cli CONFIG SET maxmemory 4gb
redis-cli CONFIG SET maxmemory-policy allkeys-lru
redis-cli CONFIG REWRITE   # réécrit redis.conf avec les valeurs courantes
```

> ⚠️ `CONFIG REWRITE` réécrit **tout** le fichier : il perd vos commentaires d'origine. Sauvegardez `redis.conf` avant (ex. `cp redis.conf redis.conf.avant-rewrite`).

---

## 26. redis.conf : réseau et système (bind, port, protected-mode)

```ini
# --- Écoute ---
bind 127.0.0.1 ::1              # défaut : localhost uniquement. En prod : IP(s) privées
# bind 10.0.5.21 127.0.0.1     # exemple : interface privée + loopback
port 6379
protected-mode yes              # refuse les connexions distantes sans AUTH ni bind restreint

# --- File d'attente TCP ---
tcp-backlog 511                 # à augmenter (ex. 2048) si "too many connections" sous charge
timeout 0                       # 0 = jamais de déconnexion idle (mettre 300 en prod exposée)
tcp-keepalive 300               # sondes keepalive : détecte les clients morts

# --- Threads E/S (⚠️ 6.0+) ---
io-threads 4                    # 4-8 sur machines multi-cœurs sous forte charge réseau
io-threads-do-reads yes         # ⚠️ 6.0+ : aussi les lectures en threads
```

Réglages **système Linux** indispensables (sans eux, Redis loggue des WARNING au démarrage) :

```bash
# 1. overcommit mémoire (sinon fork() pour BGSAVE peut échouer)
echo "vm.overcommit_memory = 1" | sudo tee /etc/sysctl.d/99-redis.conf

# 2. backlog SOMAXCONN cohérent avec tcp-backlog
echo "net.core.somaxconn = 2048" | sudo tee -a /etc/sysctl.d/99-redis.conf
sudo sysctl --system

# 3. Désactiver les Transparent Huge Pages (pics de latence !)
echo never | sudo tee /sys/kernel/mm/transparent_hugepage/enabled
# persistant via systemd :
sudo tee /etc/systemd/system/disable-thp.service <<'EOF'
[Unit]
Description=Disable Transparent Huge Pages
[Service]
Type=oneshot
ExecStart=/bin/sh -c 'echo never > /sys/kernel/mm/transparent_hugepage/enabled'
[Install]
WantedBy=multi-user.target
EOF
sudo systemctl enable --now disable-thp.service

# 4. Vérification au démarrage (aucun WARNING attendu)
sudo grep -i warning /var/log/redis/redis-server.log | head
```

---

## 27. redis.conf : mémoire (maxmemory et politiques d'éviction)

**Sans `maxmemory`, Redis grossit jusqu'à l'OOM killer.** C'est LE réglage prod n°1.

```ini
maxmemory 4gb                    # ex. 70-80 % de la RAM si Redis est seul sur la machine
maxmemory-policy noeviction      # défaut : renvoie des erreurs à l'écriture quand plein
```

Politiques d'éviction (`maxmemory-policy`) :

| Politique | Comportement | Quand l'utiliser |
|---|---|---|
| `noeviction` | Erreur OOM sur écriture | Données critiques, persistance forte |
| `allkeys-lru` | Évince les clés les moins récemment utilisées | **Cache général (choix par défaut conseillé)** |
| `allkeys-lfu` | Évince les moins fréquemment utilisées (⚠️ 4.0+) | Cache avec fréquences contrastées |
| `volatile-lru` | LRU parmi les clés **avec TTL** | Mix cache + données persistantes |
| `volatile-lfu` | LFU parmi les clés avec TTL | Idem, version LFU |
| `volatile-random` | Aléatoire parmi clés avec TTL | Rarement |
| `allkeys-random` | Aléatoire parmi toutes | Tests uniquement |
| `volatile-ttl` | Les TTL les plus courts d'abord | Files à expiration |

```bash
# Réglage à chaud :
redis-cli CONFIG SET maxmemory 4gb
redis-cli CONFIG SET maxmemory-policy allkeys-lru

# Surveiller la pression mémoire :
redis-cli INFO memory | grep -E "used_memory_human|maxmemory_human|evicted_keys|mem_fragmentation_ratio"
```

> **Dimensionnement :** gardez une marge pour le **fork** (BGSAVE/AOF rewrite duplique transitoirement la mémoire — avec `vm.overcommit_memory=1`, le COW limite le besoin réel, mais prévoyez quand même ~1,5× la donnée en RAM libre sur les pics).
> **Réplicas :** `replica-ignore-maxmemory` (défaut `yes`) — un réplica n'évince pas, il suit le maître.

---

## 28. redis.conf : persistance RDB (snapshots)

Le RDB écrit un **snapshot binaire** complet à intervalles (`dump.rdb`). Compact, rapide à charger, idéal pour les **backups**.

```ini
# --- Déclencheurs : save <secondes> <modifications> ---
save 3600 1        # 1 modif  en 1 h
save 300 100       # 100 modifs en 5 min
save 60 10000      # 10 000 modifs en 1 min
# save ""          # désactive totalement le RDB

dbfilename dump.rdb
dir /var/lib/redis
rdbcompression yes       # compresse (LZF) — laissez yes
rdbchecksum yes          # checksum CRC64 — laissez yes
rdb-del-sync-files no    # ⚠️ 7.0+
```

Commandes associées :

```bash
redis-cli BGSAVE    # snapshot en arrière-plan (fork) → "Background saving started"
redis-cli SAVE      # snapshot BLOQUANT — ne jamais utiliser en prod !
redis-cli LASTSAVE  # timestamp Unix du dernier snapshot réussi
ls -lh /var/lib/redis/dump.rdb
```

Avantages : fichier unique, petit, chargement rapide → parfait pour **backup/restore** et **redémarrages rapides**.
Inconvénients : **perte des écritures** depuis le dernier snapshot (jusqu'à plusieurs minutes). Pour une perte ~0, combinez avec l'AOF (section 30).

---

## 29. redis.conf : persistance AOF (journal)

L'AOF enregistre **chaque écriture** dans un journal (`appendonly.aof`, en pratique un manifeste + fichiers depuis Redis 7 : voir `appenddirname`). Rejeu au redémarrage = données à jour.

```ini
appendonly yes
appendfilename "appendonly.aof"   # ⚠️ 7.0+ : devient un préfixe (multi-part AOF)
appenddirname "appendonlydir"     # ⚠️ 7.0+ : répertoire des segments

# --- Durabilité vs performance ---
appendfsync always    # fsync à chaque écriture : sûr, LENT (~divise le débit par 10)
appendfsync everysec  # fsync 1×/s : compromis standard — perte max ~1 s  ✅ RECOMMANDÉ
appendfsync no        # laisse l'OS décider : rapide, perte jusqu'à ~30 s

# --- Réécriture (compaction) ---
auto-aof-rewrite-percentage 100   # réécrit quand l'AOF a doublé (+100 %)
auto-aof-rewrite-min-size 64mb     # ... et dépasse 64 Mo
aof-rewrite-incremental-fsync yes  # fsync par morceaux pendant le rewrite (évite les pics)

# --- Tolérance à la corruption de queue ---
aof-load-truncated yes   # tronque un AOF à queue corrompue au lieu de refuser de démarrer
```

Commandes :

```bash
redis-cli BGREWRITEAOF   # compaction manuelle en arrière-plan
redis-cli INFO persistence | grep -E "aof_|rdb_"
```

> ⚠️ **7.0+ : Multi-Part AOF.** L'AOF n'est plus un fichier unique mais un répertoire (`appendonlydir/`) avec fichiers base + incrémentaux. Adaptez vos scripts de backup (sauvegardez le **répertoire entier**, pas un fichier).

---

## 30. RDB vs AOF : que choisir ?

| Critère | RDB seul | AOF seul (`everysec`) | RDB + AOF (recommandé) |
|---|---|---|---|
| Perte max de données | Minutes (selon `save`) | ~1 seconde | ~1 seconde |
| Vitesse de redémarrage | ⚡ Très rapide | 🐢 Lent (rejeu) | Rapide (charge le RDB, rejoue l'incrémental) |
| Taille sur disque | Petite | Grande (avant rewrite) | Moyenne |
| Charge CPU/E-S | Faible (snapshots espacés) | Continue modérée | Modérée |
| Backup simple | ✅ Un fichier à copier | ⚠️ Répertoire à copier | ✅ RDB pour backup, AOF pour durabilité |

**Recommandation production : activez les deux.**

```ini
# /etc/redis/conf.d/99-production.conf
save 3600 1
save 300 100
save 60 10000
appendonly yes
appendfsync everysec
```

Cas particuliers :

- **Cache pur reconstituable** (sessions recréables, cache devant SQL) : RDB seul, voire **aucune persistance** + `replicaof` pour la HA. Assumez la perte.
- **File de tâches critique** : AOF `everysec` minimum, `always` si chaque message compte (avec le coût perf).
- **Gros dataset, redémarrages fréquents** : RDB indispensable pour des restarts rapides.

---


## 31. Expiration : EXPIRE, TTL, PEXPIRE

Toute clé peut avoir une **durée de vie**. À expiration, Redis la supprime automatiquement.

```bash
redis-cli SET session:xyz "donnees"
redis-cli EXPIRE session:xyz 1800        # expire dans 30 min → 1
redis-cli TTL session:xyz               # → 1799 (secondes restantes)
# TTL retourne : -2 si la clé n'existe pas, -1 si pas d'expiration

redis-cli PEXPIRE session:xyz 90000     # en millisecondes
redis-cli PTTL session:xyz              # → 89999

redis-cli EXPIREAT session:xyz 1769560800    # timestamp Unix absolu
redis-cli PEXPIREAT session:xyz 1769560800000

redis-cli PERSIST session:xyz           # retire l'expiration → 1 (clé devient persistante)

# --- Variantes conditionnelles (⚠️ 7.0+) ---
redis-cli EXPIRE session:xyz 1800 NX    # ne pose le TTL que s'il n'y en a pas
redis-cli EXPIRE session:xyz 1800 XX    # ... que s'il y en a déjà un
redis-cli EXPIRE session:xyz 1800 GT    # ... que si le nouveau TTL est plus grand
redis-cli EXPIRE session:xyz 60 LT      # ... que s'il est plus petit
```

> Depuis Redis 7, `SET` accepte aussi `EXAT`/`PXAT` (timestamps absolus). Et `GETEX`/`GETDEL` combinent lecture + gestion du TTL (section 6).

---

## 32. Expiration : stratégies interne (passive + active)

Redis combine deux mécanismes :

1. **Expiration passive :** à l'accès d'une clé expirée, elle est supprimée et traitée comme absente. Coût nul si la clé n'est jamais relue.
2. **Expiration active :** 10×/seconde (par défaut), Redis échantillonne 20 clés avec TTL par base ; si > 25 % sont expirées, il recommence immédiatement (jusqu'à 25 % du temps CPU). Avec `hz` augmenté, le cycle est plus fréquent.

```ini
hz 10   # défaut : fréquence des tâches de fond (expiration active, rehash...)
# hz 50 à 100 sur serveurs chargés en expirations — au prix d'un peu de CPU
```

```bash
redis-cli INFO stats | grep expired_keys     # clés expirées (passif+actif)
redis-cli INFO stats | grep evicted_keys     # clés évincées par maxmemory (différent !)
```

> **Distinction importante :** `expired_keys` = TTL atteint ; `evicted_keys` = `maxmemory` atteinte. Les deux font baisser le hit rate, mais les causes et remèdes diffèrent.

---

## 33. Expiration : pièges et bonnes pratiques

| Piège | Explication | Remède |
|---|---|---|
| TTL sur clé écrasée | `SET` **sans** option efface le TTL existant | Utilisez `KEEPTTL` (⚠️ 6.0+) ou reposez le TTL |
| `GETSET`/`SETRANGE` | conservent le TTL (ce sont des écritures partielles) | OK, mais à savoir |
| Horloges désynchronisées | `EXPIREAT` avec timestamp absolu + NTP en vrac | chrony partout, préférez les durées relatives |
| Avalanche d'expirations | 1 M de clés avec le même TTL → pic de CPU/suppressions | **Jitter** : TTL aléatoire ±10 % |
| Clés « immortelles » accidentelles | Oubli du TTL sur cache → mémoire qui gonfle | `volatile-lru` + TTL systématique sur le cache |
| TTL trop court | Hit rate effondré, base SQL saturée | Mesurez le hit rate avant de baisser (section 69) |

Pattern du jitter en application (pseudo-code) :

```python
import random
ttl_base = 3600
ttl = ttl_base + random.randint(-300, 300)   # ±5 min
redis.set("cache:produit:42", valeur, ex=ttl)
```

---

## 34. Persistance et expiration : cas limite

- Un **réplica** n'expire pas lui-même : c'est le **maître** qui propage les `DEL` d'expiration. Un réplica isolé (ex. `REPLICAOF NO ONE` après failover) peut donc resservir des clés expirées jusqu'à sa propre expiration active.
- Au **chargement RDB**, les clés déjà expirées sont ignorées (maître) ou conservées-mais-inaccessibles (réplica, expirées au premier accès).
- Dans l'**AOF**, l'expiration est journalisée comme un `DEL` explicite au moment où elle se produit.

---

## 35. Sécurité : AUTH avec requirepass

Le minimum vital dès que Redis écoute autre chose que localhost.

```ini
# redis.conf
requirepass "UnMotDePasseLongEtAleatoireIci"
```

```bash
# Côté client :
redis-cli -a "UnMotDePasseLongEtAleatoireIci" ping
# ou (mieux : n'apparaît pas dans l'historique avec -a... en fait si ; préférez) :
redis-cli
127.0.0.1:6379> AUTH "UnMotDePasseLongEtAleatoireIci"
# OK

# Vérifier qui est authentifié / combien de commandes :
redis-cli INFO clients
```

Limites de `requirepass` :

- Mot de passe **unique** pour tout le monde, transmis en clair (sans TLS).
- Aucune granularité : un client authentifié peut tout faire, y compris `FLUSHALL`.

> **En production moderne : préférez les ACLs (section 36).** `requirepass` reste utile en dev ou comme filet minimal.

Générez des secrets robustes, jamais dans ce guide ni en clair dans un ticket :

```bash
openssl rand -hex 32   # 64 caractères hexadécimaux
```

---

## 36. Sécurité : ACLs (Redis 6+, la bonne pratique)

Les **ACL** (Access Control Lists) donnent des **utilisateurs** avec : mot de passe, commandes autorisées, clés accessibles, canaux pub/sub.

```bash
# --- Création à chaud ---
redis-cli ACL SETUSER appli_cache on ">MotDePasseAppli" \
  +get +set +del +expire +ttl \
  -@all \
  ~cache:* ~session:* \
  &events:*

# Lecture de la règle : utilisateur actif, commandes +get+set+del+expire+ttl,
# clés limitées aux motifs cache:* et session:*, canaux events:*

# --- Vérifications ---
redis-cli ACL LIST
redis-cli ACL WHOAMI
redis-cli ACL GETUSER appli_cache

# --- Test d'interdiction ---
redis-cli --user appli_cache --pass "MotDePasseAppli" FLUSHALL
# (error) NOPERM this user has no permissions to run the 'flushall' command

# --- Utilisateur admin restreint au réseau ---
redis-cli ACL SETUSER admin on ">MotDePasseAdmin" allcommands allkeys

# --- Persister les ACL (sinon perdues au restart !) ---
redis-cli ACL SAVE        # nécessite : aclfile /etc/redis/users.acl dans redis.conf
```

Fichier `/etc/redis/users.acl` (généré par `ACL SAVE`, éditable à la main) :

```ini
user default off                          # désactive l'utilisateur anonyme par défaut !
user admin on >MotDePasseAdmin allcommands allkeys
user appli_cache on >MotDePasseAppli +get +set +del +expire +ttl -@all ~cache:* ~session:*
```

```ini
# redis.conf
aclfile /etc/redis/users.acl
# (ne pas combiner requirepass + aclfile : requirepass définit juste le mdp de "default")
```

Catégories de commandes utiles : `@connection`, `@string`, `@hash`, `@list`, `@set`, `@sortedset`, `@stream`, `@keyspace`, `@admin`, `@dangerous`.

**Recette prod :** `default off` + un user par application avec le minimum de commandes et des motifs de clés stricts. Chaque fuite de credential n'expose alors qu'un périmètre.

---

## 37. Sécurité : bind et protected-mode

```ini
bind 127.0.0.1 ::1 10.0.5.21   # UNIQUEMENT les interfaces nécessaires
protected-mode yes               # garde-fou : refuse le distant si pas d'AUTH/bind restreint
port 6379
```

Matrice de décision :

| Exposition | Configuration |
|---|---|
| Localhost uniquement (dev, agent local) | `bind 127.0.0.1 ::1`, protected-mode yes |
| Réseau privé (VLAN applicatif) | `bind 10.x.x.x`, AUTH/ACL obligatoire, firewall |
| Internet | **INTERDIT** sans TLS + ACL + firewall. Préférez un VPN/bastion. |

Vérifiez l'exposition réelle (pas seulement la config) :

```bash
ss -ltnp | grep 6379
# 127.0.0.1:6379 ✅  vs  0.0.0.0:6379 ⚠️ (toutes interfaces !)

# Depuis un poste externe, testez que c'est fermé :
nmap -p 6379 redis-prod.exemple.sn
sudo iptables -L -n | grep 6379   # ou règles nftables / security groups
```

> Des milliers d'instances Redis exposées sans mot de passe sont régulièrement compromises (ransomware `FLUSHALL` + demande de rançon dans une clé). **Ne soyez pas l'un d'eux.**

---

## 38. Sécurité : TLS (chiffrement en transit)

Redis 6+ supporte le TLS natif (compiler avec `BUILD_TLS=yes`, paquet officiel Debian : inclus).

```bash
# --- 1. Générer une CA et des certificats (exemple autosigné, prod = PKI d'entreprise) ---
mkdir -p /etc/redis/tls && cd /etc/redis/tls
openssl genrsa -out ca.key 4096
openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 -out ca.crt -subj "/CN=Redis CA"
openssl genrsa -out redis.key 2048
openssl req -new -key redis.key -out redis.csr -subj "/CN=redis-prod.exemple.sn"
openssl x509 -req -in redis.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
  -out redis.crt -days 825 -sha256

# --- 2. redis.conf ---
```

```ini
# /etc/redis/conf.d/99-tls.conf
port 0                      # désactive le port non chiffré
tls-port 6379
tls-cert-file /etc/redis/tls/redis.crt
tls-key-file /etc/redis/tls/redis.key
tls-ca-cert-file /etc/redis/tls/ca.crt
tls-replication yes         # chiffre aussi la réplication
tls-cluster yes             # ... et le bus cluster
tls-auth-clients no         # yes = exige un certificat client (mTLS)
```

```bash
# --- 3. Client ---
redis-cli --tls --cacert /etc/redis/tls/ca.crt -p 6379 ping

# --- 4. Permissions ---
sudo chown redis:redis /etc/redis/tls/redis.key
sudo chmod 600 /etc/redis/tls/redis.key
```

> Coût : +quelques % de CPU, latence légèrement accrue. Sur un VLAN privé de confiance, beaucoup d'équipes s'en passent ; dès que le trafic traverse un réseau mutualisé ou à cheval sur sites, **TLS obligatoire**.

---

## 39. Sécurité : rename-command (désarmer les commandes dangereuses)

Même avec AUTH, une erreur applicative (`FLUSHDB` dans un mauvais environnement) peut être catastrophique. Renommez ou désactivez les commandes dangereuses :

```ini
# Désactiver (nom vide = commande inexistante) :
rename-command FLUSHALL ""
rename-command FLUSHDB ""
rename-command DEBUG ""
rename-command CONFIG "CONFIG_9f8a7b6c5d"   # renommer plutôt que couper : garde l'admin possible

# Autres candidates : KEYS, SHUTDOWN, MIGRATE, MODULE, ACL
```

```bash
redis-cli FLUSHALL
# (error) unknown command `flushall`, with args beginning with:
```

> ⚠️ Avec Sentinel/Cluster, `CONFIG` renommée peut casser certains outils d'orchestration : testez. Alternative moderne : les **ACL** (`-@dangerous`) par utilisateur, plus fines que le rename global.

---

## 40. Sécurité : checklist de durcissement

Checklist à cocher avant toute mise en production :

- [ ] `bind` restreint aux interfaces strictement nécessaires
- [ ] `protected-mode yes`
- [ ] AUTH via **ACL** (`default off`, un user par appli, moindre privilège)
- [ ] Secrets forts (64+ caractères), stockés en coffre (Vault/ansible-vault), **jamais** en clair dans git
- [ ] `rename-command` ou ACL `-@dangerous` pour FLUSHALL/FLUSHDB/DEBUG/CONFIG/KEYS
- [ ] Firewall (iptables/nftables/security group) : 6379/6380/16379 ouverts **uniquement** vers les clients légitimes
- [ ] TLS si le réseau n'est pas de confiance
- [ ] Fichiers : `redis.conf` (640, root:redis), `dump.rdb`/`appendonlydir` (600, redis:redis), clés TLS (600)
- [ ] Pas d'exposition Internet (vérifié par scan externe)
- [ ] `supervised systemd`, `daemonize no` (paquet Debian : déjà OK)
- [ ] Logs : `logfile /var/log/redis/redis-server.log`, `loglevel notice`
- [ ] Test d'intrusion interne : tentative de connexion sans AUTH depuis un poste non autorisé → doit échouer

```bash
# Audit express d'une instance :
redis-cli INFO server | grep -E "redis_version|tcp_port"
redis-cli CONFIG GET "requirepass"   # ne doit pas être vide (ou aclfile en place)
redis-cli CONFIG GET "bind"
redis-cli ACL LIST 2>/dev/null | head -5
ss -ltnp | grep -E "6379|6380"
```

---

## 41. Transactions : MULTI / EXEC / WATCH

`MULTI`/`EXEC` exécute une **file de commandes de façon atomique et isolée** (pas d'entrelacement avec d'autres clients). Ce n'est **pas** un rollback : si une commande échoue à l'exécution, les autres sont quand même appliquées.

```bash
redis-cli MULTI
redis-cli INCR compteur:visites
redis-cli SET dernier:visiteur "awa"
redis-cli EXPIRE dernier:visiteur 3600
redis-cli EXEC
# 1) (integer) 1241
# 2) OK
# 3) (integer) 1

# --- Annulation ---
redis-cli MULTI
redis-cli SET a 1
redis-cli DISCARD     # annule la file

# --- Optimistic locking avec WATCH ---
redis-cli WATCH solde:compte:42
# ... lecture du solde côté appli ...
redis-cli MULTI
redis-cli DECRBY solde:compte:42 100
redis-cli EXEC
# → nil si solde:compte:42 a été modifié entre-temps (la transaction est annulée)
# → l'appli doit réessayer (boucle retry)
redis-cli UNWATCH
```

Quand préférer Lua (section 42) : dès que la logique contient des **conditions** (« décrémente seulement si > 0 ») — MULTI/EXEC ne fait pas de branchements.

---

## 42. Lua scripting : EVAL / EVALSHA

Les scripts Lua s'exécutent **côté serveur, atomiquement** : idéal pour les opérations « lire-décider-écrire » sans aller-retour réseau ni race.

```bash
# --- Exemple : décrémente un stock seulement si > 0 ---
redis-cli EVAL "
local stock = tonumber(redis.call('GET', KEYS[1]) or '0')
if stock > 0 then
  return redis.call('DECR', KEYS[1])
else
  return -1
end
" 1 stock:produit:42
# → nouveau stock, ou -1 si rupture

# --- Bonnes pratiques : charger une fois, appeler par SHA ---
redis-cli SCRIPT LOAD "return redis.call('GET', KEYS[1])"
# → "a42059b...sha1..."  (à stocker côté appli)
redis-cli EVALSHA a42059b... 1 ma_cle
# NOSCRIPT si le script a été flushé → fallback sur EVAL, ou SCRIPT EXISTS

# --- Gestion ---
redis-cli SCRIPT FLUSH          # vide le cache de scripts (⚠️ EVALSHA suivants → NOSCRIPT)
redis-cli SCRIPT EXISTS sha1 sha2
```

Règles d'or du Lua Redis :

- **Déterministe** : pas d'accès réseau, pas de temps aléatoire non seedé, pas d'itération d'ordre aléatoire sur les tables.
- **Rapide** : le script bloque le serveur pendant son exécution (mono-thread). Pas de boucle infinie !
- `KEYS[]` = clés touchées (obligatoire pour le Cluster : toutes les clés d'un script doivent être dans le **même hash slot**, utilisez les hash tags `{...}`).
- `ARGV[]` = arguments non-clés.
- En réplication, c'est le **script** qui est répliqué (effets déterministes garantis), pas ses écritures une par une.

---

## 43. Lua : cas pratiques commentés

```bash
# --- 1. Rate limiter à fenêtre fixe atomique (voir aussi section 50) ---
redis-cli EVAL "
local c = redis.call('INCR', KEYS[1])
if c == 1 then redis.call('EXPIRE', KEYS[1], ARGV[1]) end
return c
" 1 ratelimit:api:client:7 60
# → nombre de requêtes dans la fenêtre ; l'appli compare à la limite

# --- 2. Verrou avec renouvellement (prolonge seulement si on est le propriétaire) ---
redis-cli EVAL "
if redis.call('GET', KEYS[1]) == ARGV[1] then
  return redis.call('PEXPIRE', KEYS[1], ARGV[2])
else
  return 0
end
" 1 verrou:job42 worker-1 30000

# --- 3. Libération sûre d'un verrou (ne supprime que si propriétaire) ---
redis-cli EVAL "
if redis.call('GET', KEYS[1]) == ARGV[1] then
  return redis.call('DEL', KEYS[1])
else
  return 0
end
" 1 verrou:job42 worker-1

# --- 4. File à priorité avec scores + limite ---
redis-cli EVAL "
local items = redis.call('ZRANGE', KEYS[1], 0, tonumber(ARGV[1]) - 1)
if #items > 0 then redis.call('ZREM', KEYS[1], unpack(items)) end
return items
" 1 file:prioritaire 5
```

> Les scripts 2 et 3 sont le cœur d'un **verrou distribué correct** (section 46). Copiez-les tels quels dans vos libs plutôt que de réinventer le GET+DEL non atomique.

---

## 44. Pub/Sub : concepts et commandes

Messagerie **fire-and-forget** : les publishers émettent sur des canaux, les subscribers reçoivent en temps réel. **Aucune persistance** : un message publié sans abonné est perdu.

```bash
# Terminal 1 — abonné :
redis-cli SUBSCRIBE alertes:infra
# 1) "subscribe"  2) "alertes:infra"  3) (integer) 1
# ... attend les messages ...

# Terminal 2 — publisher :
redis-cli PUBLISH alertes:infra "onduleur: bypass actif"
# → 1 (nombre d'abonnés ayant reçu)

# --- Motifs (pattern subscribe) ---
# Terminal 1 :
redis-cli PSUBSCRIBE "alertes:*"
# Terminal 2 :
redis-cli PUBLISH alertes:reseau "lien down"   # reçu par le PSUBSCRIBE

# --- Sharded Pub/Sub (⚠️ 7.0+, pour le Cluster) ---
redis-cli SSUBSCRIBE commandes:shard1   # le message reste sur le shard de la clé
redis-cli SPUBLISH commandes:shard1 "hello"
```

Limites à connaître :

- Pas de rejeu, pas d'ACK, pas de file d'attente → **ne pas utiliser pour des événements critiques** (préférez les Streams, section 16).
- Un subscriber lent fait gonfler les **output buffers** du serveur → risque OOM (réglez `client-output-buffer-limit pubsub 32mb 8mb 60`).
- `UNSUBSCRIBE` / `PUNSUBSCRIBE` / `PUBSUB CHANNELS` / `PUBSUB NUMSUB canal` pour l'introspection.

Cas d'usage légitimes : invalidation de cache entre instances applicatives, notifications temps réel non critiques, bus d'événements éphémères.

---

## 45. Keyspace notifications : réagir aux événements

Redis peut **publier** des événements internes (expiration, éviction, écritures...) sur des canaux `__keyspace@<db>__:<clé>` et `__keyevent@<db>__:<op>`.

```ini
# redis.conf — quoi notifier (chaîne de flags) :
notify-keyspace-events Ex
# K = keyspace, E = keyevent, g = génériques, $ = strings, l = lists, s = sets,
# h = hashes, z = sorted sets, x = expirations, e = évictions, A = alias de "g$lshzx"
```

```bash
# Exemple : être notifié des expirations (utile : "session expirée → nettoyage")
redis-cli CONFIG SET notify-keyspace-events Ex

# Terminal 1 :
redis-cli PSUBSCRIBE "__keyevent@0__:expired"

# Terminal 2 :
redis-cli SET session:temp "x" EX 5
# ... 5 s plus tard, terminal 1 reçoit : 1) "pmessage" 2) "__keyevent@0__:expired" 3) "__keyevent@0__:expired" 4) "session:temp"

# Savoir QUI a expiré quoi par clé :
# Terminal 1 :
redis-cli SUBSCRIBE "__keyspace@0__:session:temp"
```

> ⚠️ Les notifications d'expiration ne sont **pas garanties** en cas de failover : un événement peut être manqué. Ne basez pas une logique métier critique dessus sans filet (ex. un job de réconciliation périodique).

---


## 46. Verrous distribués : le pattern SET NX PX

Besoin : « un seul worker à la fois exécute le job 42 ». La recette correcte tient en 3 commandes :

```bash
# --- 1. Acquisition (atomique grâce à NX + PX combinés) ---
# token unique par détenteur (uuid) — indispensable pour ne libérer que SON verrou
TOKEN="worker-1:$(uuidgen)"
redis-cli SET verrou:job42 "$TOKEN" NX PX 30000
# → OK  : verrou acquis pour 30 s
# → nil : déjà pris, réessayer plus tard (avec backoff + jitter)

# --- 2. Renouvellement (si le traitement dépasse le TTL) ---
redis-cli EVAL "
if redis.call('GET', KEYS[1]) == ARGV[1] then
  return redis.call('PEXPIRE', KEYS[1], ARGV[2])
else return 0 end
" 1 verrou:job42 "$TOKEN" 30000

# --- 3. Libération (atomique : seulement si on est propriétaire) ---
redis-cli EVAL "
if redis.call('GET', KEYS[1]) == ARGV[1] then
  return redis.call('DEL', KEYS[1])
else return 0 end
" 1 verrou:job42 "$TOKEN"
```

Règles impératives :

- **Token unique** par détenteur (jamais une valeur fixe comme `"1"`).
- **TTL toujours** (`PX`) : sans lui, un worker qui crash laisse un verrou éternel.
- Libération/renouvellement **en Lua** avec vérification du token (le `GET`+`DEL` en deux temps est une race condition classique).
- Le TTL doit couvrir la durée du traitement **ou** être renouvelé périodiquement (< TTL/3).
- Ce pattern est valable sur **une instance unique** (ou un maître avec réplication synchrone). Pour du multi-maître, voir Redlock (section 47).

> ⚠️ Avec réplication **asynchrone** : si le maître tombe avant d'avoir répliqué le `SET NX`, un nouveau maître peut accorder le verrou à un autre worker → **double détention possible**. Si c'est inacceptable, utilisez `WAIT` (réplication semi-synchrone) ou Redlock.

---

## 47. Redlock : concepts et avertissements

**Redlock** (algorithme proposé par l'auteur de Redis) étend le verrou à **N instances indépendantes** (ex. 5) : le verrou est acquis s'il est obtenu sur une **majorité** (quorum) en moins que le TTL.

Principe (simplifié) :

1. Générer un token unique, noter l'heure de début.
2. `SET NX PX` séquentiellement sur les N instances.
3. Si quorum atteint **et** temps écoulé < TTL → verrou acquis (TTL effectif = TTL initial − temps écoulé).
4. Sinon, libérer partout (script Lua) et réessayer.

**Avertissements honnêtes (débat Martin Kleppmann vs Salvatore) :**

- Redlock suppose des **horloges à peu près synchronisées** et des **pauses GC/processus bornées** — hypothèses fragiles.
- Il ne protège pas contre un processus en pause longue qui « se réveille » après expiration du verrou (fencing tokens requis pour une vraie sûreté).
- **Recommandation pratique :** pour 95 % des besoins (leader election, jobs exclusifs), le **verrou mono-instance** (section 46) + `WAIT 1 1000` suffit. N'utilisez Redlock que si vous avez déjà N instances indépendantes **et** que vous comprenez ses limites. Les bibliothèques (`redlock-py`, etc.) implémentent l'algorithme — ne le codez pas à la main.

---

## 48. Files d'attente avec les Lists

Le pattern historique, simple et rapide. Producteurs → `RPUSH`, workers → `BLPOP`.

```bash
# --- Producteur ---
redis-cli RPUSH jobs:impression "job: impression doc 7788, imprimante kyocera-3"

# --- Worker fiable : BRPOPLPUSH / LMOVE (⚠️ 6.2+) ---
# Le job est déplacé atomiquement vers une liste "en cours" :
redis-cli LMOVE jobs:impression jobs:impression:encours RIGHT LEFT
# → "job: ..." (le worker le traite...)
# Succès :
redis-cli LREM jobs:impression:encours 1 "job: ..."
# Échec / worker mort : un superviseur remet les jobs de :encours vers :impression
# après un délai (scan périodique).

# --- Priorités : plusieurs files ---
redis-cli BLPOP jobs:urgent jobs:impression 30   # jobs:urgent d'abord
```

Limites des lists pour les files :

- Pas d'ACK natif (le pattern `:encours` est artisanal).
- Pas de rejeu, pas de multi-consommateurs avec distribution garantie.
- Un message = une string (sérialisez votre payload en JSON).

**Quand passer aux Streams (section 49) :** besoin d'ACK, de rejeu, de groupes de consommateurs, ou de visibilité sur les pending.

---

## 49. Files d'attente avec les Streams (consumer groups)

Le pattern robuste : distribution à un seul worker par groupe + ACK + récupération des pannes.

```bash
# --- Setup (une fois) ---
redis-cli XGROUP CREATE jobs:emails grp:workers 0 MKSTREAM

# --- Worker (boucle) ---
# 1. Récupère jusqu'à 10 jobs jamais distribués, bloque 5 s :
redis-cli XREADGROUP GROUP grp:workers worker-1 COUNT 10 BLOCK 5000 STREAMS jobs:emails ">"
# 2. Traite chaque entrée...
# 3. Acquitte :
redis-cli XACK jobs:emails grp:workers 1769557200000-0 1769557200001-0

# --- Superviseur : récupère les jobs bloqués (worker mort) ---
# Toutes les 60 s : réclame les pending idle depuis > 120 s
redis-cli XAUTOCLAIM jobs:emails grp:workers superviseur 120000 0 COUNT 50
# puis XACK après retraitement

# --- Producteur ---
redis-cli XADD jobs:emails MAXLEN ~ 50000 * type "email" to "awa@exemple.sn" template "facture"
```

Tableau de décision file :

| Besoin | Lists | Streams |
|---|---|---|
| Simple, fire-and-forget | ✅ | — |
| ACK / au-moins-une-fois | artisanal (`:encours`) | ✅ natif |
| Rejeu / audit | ❌ | ✅ (`XREAD` depuis un ID) |
| Multi-workers sans doublon | ⚠️ (BLPOP distribue, sans garantie) | ✅ consumer groups |
| Latence minimale | ✅ légèrement plus rapide | très bonne aussi |

---

## 50. Rate limiting : 3 algorithmes

### 50.1 Fenêtre fixe (simple, ~suffisant)

```bash
# Autorise 100 requêtes / 60 s par clé API :
redis-cli EVAL "
local c = redis.call('INCR', KEYS[1])
if c == 1 then redis.call('EXPIRE', KEYS[1], ARGV[1]) end
return c
" 1 ratelimit:apikey:7 60
# L'appli autorise si c <= 100. Inconvénient : pic de 200 req à cheval sur 2 fenêtres.
```

### 50.2 Fenêtre glissante avec Sorted Set (précis)

```bash
# Autorise 100 requêtes sur les 60 dernières secondes :
redis-cli EVAL "
local now = tonumber(ARGV[1])
local window = tonumber(ARGV[2])
local limit = tonumber(ARGV[3])
redis.call('ZREMRANGEBYSCORE', KEYS[1], 0, now - window)
local c = redis.call('ZCARD', KEYS[1])
if c < limit then
  redis.call('ZADD', KEYS[1], now, now .. ':' .. ARGV[4])
  redis.call('EXPIRE', KEYS[1], window)
  return 1
else
  return 0
end
" 1 ratelimit:glissant:7 1769557200000 60000 100 $(uuidgen)
# → 1 autorisé, 0 refusé. Précis mais 1 clé + N entrées par client.
```

### 50.3 Token bucket (lisse les pics)

```bash
# Seau de 100 tokens, refill 10/s — implémentation Lua classique :
redis-cli EVAL "
local key = KEYS[1]
local capacity = tonumber(ARGV[1])
local refill_per_sec = tonumber(ARGV[2])
local now = tonumber(ARGV[3])
local data = redis.call('HMGET', key, 'tokens', 'ts')
local tokens = tonumber(data[1]) or capacity
local ts = tonumber(data[2]) or now
tokens = math.min(capacity, tokens + (now - ts) / 1000 * refill_per_sec)
if tokens >= 1 then
  tokens = tokens - 1
  redis.call('HSET', key, 'tokens', tokens, 'ts', now)
  redis.call('EXPIRE', key, 3600)
  return 1
else
  redis.call('HSET', key, 'tokens', tokens, 'ts', now)
  return 0
end
" 1 bucket:apikey:7 100 10 1769557200000
```

> Réponse HTTP standard en cas de refus : `429 Too Many Requests` + headers `Retry-After`, `X-RateLimit-Limit`, `X-RateLimit-Remaining`.

---

## 51. Sessions utilisateur

Stocker les sessions en Redis : invalidation instantanée, partage entre N serveurs web, expiration automatique.

```bash
# --- Création (à la connexion) ---
# Token de session opaque, jamais devinable :
TOKEN=$(openssl rand -hex 32)
redis-cli HSET session:$TOKEN user_id 1042 role "admin" ip "10.0.5.44" created 1769557200
redis-cli EXPIRE session:$TOKEN 1800     # 30 min d'inactivité

# --- Lecture + prolongation (sliding expiration) à chaque requête ---
redis-cli HGETALL session:$TOKEN
redis-cli EXPIRE session:$TOKEN 1800

# --- Déconnexion / révocation ---
redis-cli DEL session:$TOKEN             # immédiat sur tous les frontaux

# --- Révocation globale d'un utilisateur (ex. compte compromis) ---
# Maintenir un index : SADD sessions:user:1042 $TOKEN (à la création)
redis-cli SMEMBERS sessions:user:1042
# puis DEL de chaque session + DEL de l'index (script Lua pour l'atomicité)
```

Bonnes pratiques :

- Cookie `HttpOnly; Secure; SameSite=Lax`, valeur = token opaque (jamais l'ID utilisateur).
- **Sliding expiration** : `EXPIRE` à chaque requête authentifiée.
- Données minimales dans la session (ID + rôle) ; le reste en base.
- En cas de compromission : rotation du token + révocation via l'index.

---

## 52. Patterns de cache : cache-aside (lazy loading)

Le pattern le plus répandu : l'application **lit d'abord Redis**, et ne va en base qu'en cas de miss.

```python
# Pseudo-code Python (redis-py)
def get_produit(sku):
    key = f"cache:produit:{sku}"
    data = redis.get(key)
    if data:
        return json.loads(data)          # HIT
    # MISS :
    row = db.query("SELECT * FROM produits WHERE sku = %s", sku)
    if row is None:
        return None
    payload = json.dumps(row)
    ttl = 3600 + random.randint(-300, 300)   # jitter anti-avalanche
    redis.set(key, payload, ex=ttl)
    return row
```

```bash
# Équivalent manuel en redis-cli :
redis-cli GET cache:produit:sku-7788        # MISS → nil
# ... requête SQL ...
redis-cli SET cache:produit:sku-7788 '{"nom":"Onduleur 3kVA","prix":450000}' EX 3600
redis-cli GET cache:produit:sku-7788        # HIT
```

Avantages : simple, ne cache que ce qui est demandé (pas de remplissage inutile).
Inconvénients : premier appel lent (miss), **données potentiellement périmées** entre deux expirations → à combiner avec l'invalidation (section 54).

---

## 53. Patterns de cache : write-through / write-behind

**Write-through :** l'écriture va **d'abord** (ou en même temps) dans le cache **et** en base, de façon synchrone.

```python
def update_prix(sku, nouveau_prix):
    db.execute("UPDATE produits SET prix=%s WHERE sku=%s", nouveau_prix, sku)
    redis.set(f"cache:produit:{sku}", json.dumps({"prix": nouveau_prix}), ex=3600)
    # ou redis.delete(...) pour forcer un rechargement (variante write-invalidate)
```

| Pattern | Cohérence | Latence d'écriture | Quand |
|---|---|---|---|
| Cache-aside | Éventuelle (TTL) | Faible | Lecture >> écriture (catalogue, fiches) |
| Write-through | Forte | Plus élevée (2 écritures) | Données critiques lues souvent |
| Write-behind | Faible (async) | Très faible | Compteurs, stats (risque de perte) |
| Refresh-ahead | Bonne | Faible (refresh en tâche de fond) | Clés chaudes à SLA strict |

> En pratique : **cache-aside + invalidation explicite à l'écriture** couvre 80 % des besoins (section 54).

---

## 54. Invalidation de cache : stratégies

Le cache périmé est la source n°1 de bugs « incompréhensibles ». Stratégies, de la plus simple à la plus robuste :

```bash
# --- 1. TTL (expiration passive) ---
redis-cli SET cache:produit:42 "..." EX 600   # périmé au pire 10 min

# --- 2. Invalidation explicite à l'écriture ---
# Quand le produit 42 change : 
redis-cli DEL cache:produit:42

# --- 3. Invalidation par motif (⚠️ SCAN, jamais KEYS) ---
# Tous les caches liés à la catégorie 7 :
redis-cli --scan --pattern "cache:categorie:7:*" | xargs -L 100 redis-cli DEL
# Mieux : maintenir un index (Set) des clés à invalider par entité :
redis-cli SADD idx:produit:42 "cache:produit:42" "cache:recherche:page:3"
# ... à l'invalidation : SMEMBERS puis DEL (script Lua)

# --- 4. Versioning des clés (zéro DEL) ---
# Clé = cache:produit:42:v3 ; à chaque modification, on incrémente la version :
redis-cli INCR version:produit:42   # → 4, les lecteurs utilisent v4
# Les anciennes versions expirent via leur TTL. Aucune suppression, aucun SCAN.

# --- 5. Pub/Sub d'invalidation entre N frontaux (chaque frontal a son cache local) ---
redis-cli PUBLISH inval:produit "42"   # chaque frontal DEL son cache local
```

Règle d'or : **définissez la tolérance au stale par donnée** (prix d'un produit : 0 ; nombre de vues : 10 min OK) et choisissez la stratégie en conséquence.

---

## 55. Cas pratique : compteur temps réel

Besoin : afficher en temps réel le nombre de dossiers traités par l'équipe, remis à zéro chaque jour.

```bash
# --- Incrément à chaque dossier clôturé (atomique, sans race) ---
redis-cli INCR dossiers:2026-09-26:clotures
# Le premier INCR du jour crée la clé à 1 ; on pose l'expiration une fois :
redis-cli EVAL "
local c = redis.call('INCR', KEYS[1])
if c == 1 then redis.call('EXPIRE', KEYS[1], 86400 * 2) end
return c
" 1 dossiers:2026-09-26:clotures

# --- Lecture pour le dashboard ---
redis-cli GET dossiers:2026-09-26:clotures

# --- Compteurs multiples en un hash (par technicien) ---
redis-cli HINCRBY dossiers:2026-09-26:par-tech awa 1
redis-cli HGETALL dossiers:2026-09-26:par-tech
redis-cli EXPIRE dossiers:2026-09-26:par-tech 172800

# --- Top du jour ---
# (si besoin de classement : Sorted Set)
redis-cli ZINCRBY top:tech:2026-09-26 1 awa
redis-cli ZRANGE top:tech:2026-09-26 0 4 REV WITHSCORES
```

> Pour des compteurs « à la seconde près » multi-sites : un `INCR` par site + agrégation périodique, ou un stream d'événements + agrégat.

---

## 56. Cas pratique : leaderboard / classements

```bash
# --- Enregistrer un score (mise à jour si meilleur... ou pas) ---
# Score du joueur = son max :
redis-cli EVAL "
local cur = tonumber(redis.call('ZSCORE', KEYS[1], ARGV[1]) or '-1')
local new = tonumber(ARGV[2])
if new > cur then return redis.call('ZADD', KEYS[1], new, ARGV[1]) else return 0 end
" 1 leaderboard:quiz awa 850

# --- Top 10 ---
redis-cli ZRANGE leaderboard:quiz 0 9 REV WITHSCORES

# --- Rang d'un joueur + voisins (pagination autour de lui) ---
redis-cli ZREVRANK leaderboard:quiz awa     # → 3 (0-based)
# voisins : ZRANGE leaderboard:quiz <rank-2> <rank+2> REV WITHSCORES

# --- Classement hebdo avec expiration ---
redis-cli ZADD leaderboard:2026-W39 1200 awa 950 moussa
redis-cli EXPIRE leaderboard:2026-W39 604800   # 7 jours

# --- Pourcentage / percentile (côté appli) ---
# rank = ZREVRANK, total = ZCARD → percentile = (total - rank) / total * 100
```

---

## 57. Réplication : maître / réplica

Un **réplica** copie en continu un **maître** (asynchrone par défaut). Usages : lectures réparties, backup à chaud, bascule manuelle.

```bash
# --- Sur le futur réplica (ou dans son redis.conf : replicaof 10.0.5.21 6379) ---
redis-cli REPLICAOF 10.0.5.21 6379
redis-cli INFO replication
# role:slave
# master_host:10.0.5.21
# master_link_status:up

# --- Sur le maître : vérifier ---
redis-cli INFO replication | grep -E "role|connected_slaves"

# --- Promouvoir un réplica en maître (bascule manuelle) ---
redis-cli REPLICAOF NO ONE
# (puis reconfigurer l'appli / le DNS / le VIP vers lui)

# --- Réplication en chaîne (réplica de réplica, pour répartir la charge réseau) ---
# Sur replica-2 : REPLICAOF <ip-replica-1> 6379
```

Points critiques :

- **Asynchrone par défaut** : un `SET` acquitté peut ne pas encore être sur le réplica si le maître meurt dans la foulée. `WAIT 1 1000` (attend l'ACK d'1 réplica, timeout 1 s) donne une **durabilité semi-synchrone** pour les écritures critiques.
- Un réplica est **en lecture seule** par défaut (`replica-read-only yes`) — laissez ainsi.
- `repl-diskless-sync yes` (défaut) : le maître envoie le RDB via socket sans toucher le disque — préférable sauf réseau très lent.
- Protège le maître : `maxclients`, et `client-output-buffer-limit replica 256mb 64mb 60` (un réplica lent ne doit pas OOM le maître).

---

## 58. Réplication : configuration pas à pas (2 nœuds)

```ini
# --- /etc/redis/conf.d/99-replication.conf sur le RÉPLICA ---
replicaof 10.0.5.21 6379
replica-read-only yes
replica-serve-stale-data yes   # sert les données (périmées) si le lien est coupé
```

```bash
# 1. Sur le maître : créer l'utilisateur de réplication (ACL)
redis-cli ACL SETUSER replicateur on ">SecretReplication" +psync +replconf +ping ~*

# 2. Sur le réplica : configurer l'authentification de réplication
#    /etc/redis/conf.d/99-replication.conf :
#    masterauth "SecretReplication"      # ⚠️ déprécié au profit de :
#    masteruser "replicateur"
#    (les deux lignes : user + password)

# 3. Redémarrer le réplica, vérifier la synchro :
redis-cli -h 10.0.5.22 INFO replication | grep master_link_status  # → up
redis-cli -h 10.0.5.21 INFO replication | grep connected_slaves   # → 1

# 4. Test : écrire sur le maître, lire sur le réplica
redis-cli -h 10.0.5.21 SET test:repli "ok"
redis-cli -h 10.0.5.22 GET test:repli   # → "ok"
redis-cli -h 10.0.5.22 DEL test:repli:rien
redis-cli -h 10.0.5.22 SET ecriture "x"
# (error) READONLY You can't write against a read only replica.

# 5. Nettoyer le test
redis-cli -h 10.0.5.21 DEL test:repli
```

> **Topologie prod minimale avec HA automatique :** 1 maître + 2 réplicas + 3 Sentinels (section 59). Sans Sentinel, la bascule est **manuelle**.

---

## 59. Sentinel : architecture et failover automatique

**Sentinel** surveille des maîtres Redis, détecte les pannes et **élit automatiquement** un réplica comme nouveau maître. C'est un processus séparé (`redis-sentinel`, même binaire avec un autre mode).

```
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │ Sentinel │  │ Sentinel │  │ Sentinel │
        │   (1)    │  │   (2)    │  │   (3)    │
        └────┬─────┴──┴────┬─────┴──┴────┬─────┘
             │             │             │
        ┌────▼─────────────▼─────────────▼────┐
        │        Maître Redis (6379)          │
        └────┬───────────────────────┬───────┘
             │                       │
      ┌──────▼──────┐         ┌──────▼──────┐
      │ Réplica (1) │         │ Réplica (2) │
      └─────────────┘         └─────────────┘
```

Concepts :

- **Quorum** : nombre de Sentinels devant être d'accord pour déclarer un maître `ODOWN` (objectively down) et lancer le failover. Quorum = **majorité** (2 sur 3, 3 sur 5).
- Les Sentinels **se découvrent** entre eux via le maître (pas besoin de tous les lister partout).
- Les clients **doivent** utiliser une lib « Sentinel-aware » (elle demande aux Sentinels « qui est le maître ? ») — un client pointé sur une IP fixe ne profitera pas du failover.
- `down-after-milliseconds` : délai avant de déclarer un nœud `SDOWN` (subjectively down). Prod : 5 000–10 000 ms (trop bas = failovers intempestifs).
- `failover-timeout` : temps max d'un failover (défaut 3 min).

---

## 60. Sentinel : configuration pas à pas

```bash
# --- 1. Fichier /etc/redis/sentinel.conf (identique sur les 3 sentinels, sauf port/myid auto) ---
sudo tee /etc/redis/sentinel.conf <<'EOF'
port 26379
dir /var/lib/redis
daemonize no
supervised systemd
logfile /var/log/redis/redis-sentinel.log

# Surveille le maître "mymaster" à 10.0.5.21:6379, quorum = 2
sentinel monitor mymaster 10.0.5.21 6379 2

# Authentification vers les nœuds Redis (si ACL/requirepass)
sentinel auth-pass mymaster SecretRedisProd
# sentinel auth-user mymaster sentinel_user   # ⚠️ 6.0+ avec ACL

# Délais
sentinel down-after-milliseconds mymaster 8000
sentinel failover-timeout mymaster 180000
sentinel parallel-syncs mymaster 1
EOF

# --- 2. Service systemd (paquet Debian : redis-sentinel.service existe si installé) ---
# Sinon, copiez /lib/systemd/system/redis-sentinel.service depuis le paquet.

# --- 3. Démarrer les 3 sentinels, vérifier ---
sudo systemctl enable --now redis-sentinel
redis-cli -p 26379 SENTINEL masters
redis-cli -p 26379 SENTINEL sentinels mymaster   # → les 3 sentinels se voient
redis-cli -p 26379 SENTINEL replicas mymaster

# --- 4. Tester le failover (EN MAINTENANCE, jamais un vendredi 18h) ---
# Stopper le maître :
sudo systemctl stop redis-server   # sur 10.0.5.21
# Observer (quelques secondes) :
redis-cli -p 26379 SENTINEL get-master-addr-by-name mymaster
# → nouvelle IP du maître élu, ex. 10.0.5.22
# Sur l'ancien maître, au redémarrage, il rejoint comme réplica automatiquement.
```

**Côté application** (exemple Python) :

```python
from redis.sentinel import Sentinel
sentinel = Sentinel([("10.0.5.31", 26379), ("10.0.5.32", 26379), ("10.0.5.33", 26379)],
                    socket_timeout=0.5)
master = sentinel.master_for("mymaster", socket_timeout=0.5)  # écritures
replica = sentinel.slave_for("mymaster", socket_timeout=0.5)  # lectures
master.set("k", "v")
```

> **Piège :** Sentinel gère le failover, **pas** le sharding ni la répartition de charge. Pour > RAM d'une machine : Cluster (sections 62–64).

---


## 61. Sentinel : quorum, split-brain et garde-fous

| Sentinels | Quorum conseillé | Pannes tolérées |
|---|---|---|
| 3 | 2 | 1 |
| 5 | 3 | 2 |

Règles :

- **Toujours un nombre impair** de Sentinels, répartis sur **au moins 2–3 zones/racks** (sinon une panne réseau = pas de quorum = pas de failover).
- `sentinel monitor` : le quorum y est déclaré ; il doit être la **majorité**.
- En cas de **partition réseau** (split-brain) : le côté minoritaire ne fait pas de failover (pas de quorum) ; le côté majoritaire élit un nouveau maître. L'ancien maître isolé passe en lecture seule s'il voit qu'il n'a plus de quorum ? Non — Redis seul ne le fait pas ; c'est `min-replicas-to-write` qui protège :

```ini
# Sur chaque nœud Redis : refuse les écritures si moins de N réplicas joignables
min-replicas-to-write 1
min-replicas-max-lag 10   # ... avec un lag <= 10 s
```

Avec `min-replicas-to-write 1`, un maître isolé de ses réplicas **refuse les écritures** → pas de divergence silencieuse. **Recommandé en prod.**

Notifications Sentinel utiles (à brancher sur votre supervision) :

```bash
redis-cli -p 26379 PSUBSCRIBE "+*"
# Événements : +sdown, -sdown, +odown, +try-failover, +failover-end, +switch-master, +reboot...
```

---

## 62. Cluster : concepts (sharding, hash slots)

Le **Cluster** répartit les données sur N maîtres (**sharding**) : chaque clé est assignée à l'un des **16 384 hash slots** via `slot = CRC16(clé) mod 16384`.

```
Clé "user:1042" → CRC16 → slot 7421 → nœud maître B
```

- Chaque maître a 0..N réplicas (failover **interne** au cluster, sans Sentinel).
- Minimum prod : **3 maîtres + 3 réplicas** (6 nœuds). En dessous de 3 maîtres, pas de quorum.
- Les clients « cluster-aware » maintiennent la table des slots et suivent les **redirections** (`-MOVED`, `-ASK`).
- **Hash tags** : `{...}` force le même slot → `user:{1042}:name` et `user:{1042}:email` sont co-localisées. Indispensable pour les opérations multi-clés (MGET, transactions, Lua).

Contraintes à connaître avant d'adopter :

- Une seule base (`SELECT` interdit, seule la base 0).
- Les opérations multi-clés doivent cibler le **même slot** (sinon `CROSSSLOT` error).
- `KEYS`/`SCAN` par nœud (pas de vue globale native).
- Complexité opérationnelle nettement supérieure à Sentinel.

---

## 63. Cluster : mise en place (6 nœuds)

```ini
# --- /etc/redis/conf.d/99-cluster.conf sur CHAQUE nœud ---
port 6379
cluster-enabled yes
cluster-config-file /var/lib/redis/nodes-6379.conf
cluster-node-timeout 5000
appendonly yes
```

```bash
# 1. Démarrer les 6 instances (10.0.5.41 à 10.0.5.46)

# 2. Créer le cluster (3 maîtres, 1 réplica chacun) :
redis-cli --cluster create \
  10.0.5.41:6379 10.0.5.42:6379 10.0.5.43:6379 \
  10.0.5.44:6379 10.0.5.45:6379 10.0.5.46:6379 \
  --cluster-replicas 1 --cluster-yes
# --cluster-yes : ne pas demander confirmation (scriptable)

# 3. Vérifier :
redis-cli --cluster info 10.0.5.41:6379
# cluster_state:ok, cluster_known_nodes:6, cluster_slots_assigned:16384

redis-cli --cluster check 10.0.5.41:6379   # audit : slots, réplicas, clés orphelines

# 4. Tester avec un client cluster-aware :
redis-cli -c -h 10.0.5.41 SET user:{1042}:name "Awa"   # -c = suit les redirections
redis-cli -c -h 10.0.5.41 GET user:{1042}:name

# 5. Resharding (ajouter un nœud / rééquilibrer) :
redis-cli --cluster reshard 10.0.5.41:6379   # interactif
# ou : --cluster rebalance 10.0.5.41:6379 --cluster-use-empty-masters
```

> Le port **16379** (bus cluster, = port+10000) doit être ouvert entre les nœuds, en plus du 6379. Oubli classique des firewalls.

---

## 64. Cluster : clients, redirections et pièges

```bash
# Sans -c, une clé d'un autre slot renvoie une redirection :
redis-cli -h 10.0.5.41 GET user:1042
# (error) MOVED 7421 10.0.5.42:6379
# Avec -c, redis-cli suit automatiquement :
redis-cli -c -h 10.0.5.41 GET user:1042
# -> Redirected to slot [7421] located at 10.0.5.42:6379
# "Awa"
```

Pièges :

| Piège | Symptôme | Remède |
|---|---|---|
| Client non cluster-aware | `MOVED` en erreur | Lib cluster (redis-py `RedisCluster`, Lettuce, ioredis) |
| Multi-clés cross-slot | `CROSSSLOT Keys in request don't hash to the same slot` | Hash tags `{...}` |
| Transactions Lua multi-clés | `CROSSSLOT` | Toutes les clés avec le même tag |
| `SELECT 1` | `ERR SELECT is not allowed in cluster mode` | Une instance par usage |
| Slot non couvert (`cluster_state:fail`) | Écritures refusées | Au moins 1 maître par slot joignable ; `cluster-require-full-coverage no` pour dégrader gracieusement |

```bash
# Voir la répartition des slots :
redis-cli --cluster info 10.0.5.41:6379 | grep slots
redis-cli -c -h 10.0.5.41 CLUSTER SLOTS | head -20
redis-cli -c -h 10.0.5.41 CLUSTER NODES
```

---

## 65. Sauvegarde : stratégie RDB (BGSAVE)

Le RDB est votre **format de backup** : un fichier unique, copiable à chaud.

```bash
# --- Backup manuel ---
redis-cli BGSAVE
# Vérifier la fin :
redis-cli LASTSAVE
tail -5 /var/log/redis/redis-server.log | grep -i "background saving"

# --- Script de backup quotidien (à mettre en cron) ---
sudo tee /usr/local/bin/redis-backup.sh <<'EOF'
#!/bin/bash
set -euo pipefail
SRC="/var/lib/redis/dump.rdb"
DST="/srv/backups/redis/dump-$(date +%F).rdb"
mkdir -p /srv/backups/redis
redis-cli BGSAVE
# attend la fin du BGSAVE (max 120 s) :
for i in $(seq 1 120); do
  [ "$(redis-cli --raw LASTSAVE)" != "$START" ] 2>/dev/null && break
  sleep 1
done
cp -a "$SRC" "$DST"
gzip -9 "$DST"
find /srv/backups/redis -name "dump-*.rdb.gz" -mtime +30 -delete
echo "Backup OK : $DST.gz"
EOF
sudo chmod +x /usr/local/bin/redis-backup.sh
```

> ⚠️ Ne copiez **jamais** `dump.rdb` pendant un `BGSAVE` sans vérifier `LASTSAVE` : vous risqueriez un fichier tronqué. La boucle ci-dessus attend la fin.

---

## 66. Sauvegarde : AOF et réécriture

Avec le **Multi-Part AOF** (7.0+), sauvegardez le **répertoire entier** :

```bash
APPENDDIR="/var/lib/redis/appendonlydir"
ls -lh "$APPENDDIR"
# appendonly.aof.1.base.rdb  appendonly.aof.1.incr.aof  appendonly.aof.manifest

# Backup cohérent : déclencher un BGREWRITEAOF puis copier le répertoire
redis-cli BGREWRITEAOF
# attendre la fin :
redis-cli INFO persistence | grep aof_rewrite_in_progress  # → 0
tar -czf /srv/backups/redis/aof-$(date +%F).tar.gz -C /var/lib/redis appendonlydir
```

Vérifier l'intégrité d'un backup AOF/RDB avant de s'en servir :

```bash
# RDB :
redis-check-rdb /srv/backups/redis/dump-2026-09-26.rdb
# AOF (ancien format mono-fichier) :
redis-check-aof --fix appendonly.aof   # --fix tronque la queue corrompue (destructif !)
```

> **Règle 3-2-1** : 3 copies, 2 supports différents, 1 hors site. Pour Redis : RDB local + copie sur stockage réseau + copie hors site (rsync/rclone vers un bucket).

---

## 67. Restauration : procédure pas à pas

```bash
# === RESTAURATION RDB (instance arrêtée) ===
sudo systemctl stop redis-server
sudo cp /var/lib/redis/dump.rdb /var/lib/redis/dump.rdb.avant-restauration
sudo cp /srv/backups/redis/dump-2026-09-25.rdb /var/lib/redis/dump.rdb
sudo chown redis:redis /var/lib/redis/dump.rdb
sudo systemctl start redis-server
redis-cli DBSIZE        # nombre de clés restaurées
redis-cli INFO keyspace

# === RESTAURATION AOF (7.0+, répertoire) ===
sudo systemctl stop redis-server
sudo mv /var/lib/redis/appendonlydir /var/lib/redis/appendonlydir.avant
sudo tar -xzf /srv/backups/redis/aof-2026-09-25.tar.gz -C /var/lib/redis
sudo chown -R redis:redis /var/lib/redis/appendonlydir
sudo systemctl start redis-server
# Surveillez le log : le rejeu peut prendre du temps sur un gros AOF
tail -f /var/log/redis/redis-server.log | grep -i "aof\|ready"

# === Restauration sur une AUTRE machine (migration) ===
# 1. rsync du dump.rdb, 2. mêmes étapes, 3. vérifiez les ACL/bind avant d'ouvrir
```

**Testez vos restaurations** (au moins trimestriellement) sur une machine isolée : un backup jamais testé n'est pas un backup. Mesurez le **RTO** (temps de restauration) réel.

---

## 68. Monitoring : INFO, les sections à connaître

`INFO` est la commande de supervision n°1. Sections utiles :

```bash
redis-cli INFO server       # version, uptime, process_id
redis-cli INFO clients      # connected_clients, blocked_clients
redis-cli INFO memory        # used_memory_human, mem_fragmentation_ratio
redis-cli INFO persistence   # rdb_last_bgsave_status, aof_last_rewrite_time_sec
redis-cli INFO stats         # instantaneous_ops_per_sec, hit/miss, expired/evicted
redis-cli INFO replication   # role, connected_slaves, master_link_status
redis-cli INFO cpu           # used_cpu_sys/user
redis-cli INFO keyspace      # clés par base : db0:keys=...,expires=...
redis-cli INFO commandstats  # appels par commande (⚠️ 7.0+ : INFO commandstats)
redis-cli INFO errorstats    # erreurs par type (⚠️ 7.0+)
redis-cli INFO latencystats  # percentiles de latence (⚠️ 7.0+, voir section 71)
```

Exemple d'indicateurs clés en une commande :

```bash
redis-cli INFO stats | grep -E "instantaneous_ops|keyspace_hits|keyspace_misses|expired_keys|evicted_keys|total_commands_processed"
# instantaneous_ops_per_sec:1240
# keyspace_hits:98213
# keyspace_misses:1204
# → hit rate = 98213 / (98213+1204) ≈ 98,8 % ✅
```

---

## 69. Monitoring : métriques et seuils d'alerte

Tableau des alertes à configurer (Prometheus/Grafana, Zabbix, ou autre) :

| Métrique | Source | Seuil warning | Seuil critique | Signification |
|---|---|---|---|---|
| `up` / ping | `redis-cli ping` | — | down | Instance injoignable |
| `connected_clients` | INFO clients | > 80 % maxclients | = maxclients | Saturation connexions |
| `blocked_clients` | INFO clients | > 10 | > 50 | Workers bloqués (BLPOP/XREAD) anormalement nombreux |
| `used_memory_rss` / RAM | INFO memory + OS | > 80 % RAM | > 90 % | Risque OOM |
| `mem_fragmentation_ratio` | INFO memory | > 1,5 ou < 1 | > 2 | Fragmentation (restart si > 2 durable) |
| `evicted_keys` (dérivée) | INFO stats | > 0/min | > 1000/min | maxmemory trop bas ou fuite |
| `keyspace hit rate` | hits/(hits+misses) | < 90 % | < 70 % | Cache inefficace |
| `rdb_last_bgsave_status` | INFO persistence | — | err | Échec snapshot |
| `aof_last_bgrewrite_status` | INFO persistence | — | err | Échec rewrite AOF |
| `master_link_status` | INFO replication | — | down | Réplica décroché |
| `instantaneous_ops_per_sec` | INFO stats | chute brutale | 0 avec trafic | Blocage / saturation |
| `slowlog` récent | SLOWLOG GET | latence p99 | commandes > 100 ms | Commandes lentes (KEYS, HGETALL...) |
| Certificat TLS | fichier | expire < 30 j | expire < 7 j | Renouvellement |

Exemple d'exporter : **redis_exporter** (Oliver006) :

```bash
# Binaire ou Docker, avec AUTH :
docker run -d --name redis_exporter -p 9121:9121 \
  -e REDIS_ADDR="redis://10.0.5.21:6379" \
  -e REDIS_PASSWORD="SecretRedisProd" \
  oliver006/redis_exporter:latest
# → métriques Prometheus sur :9121/metrics
curl -s localhost:9121/metrics | grep -E "^redis_up|^redis_memory_used_bytes"
```

---

## 70. Monitoring : SLOWLOG (chasse aux commandes lentes)

Le slowlog enregistre les commandes dépassant un seuil (microsecondes). En prod, c'est votre **détecteur de `KEYS *`** et de `HGETALL` géants.

```bash
# --- Configuration (10 ms par défaut ; en prod : 1-5 ms) ---
redis-cli CONFIG SET slowlog-log-slower-than 2000   # 2000 µs = 2 ms
redis-cli CONFIG SET slowlog-max-len 256            # nb d'entrées conservées

# --- Consultation ---
redis-cli SLOWLOG GET 10
# 1) 1) (integer) 12        → id
#    2) (integer) 1769557200 → timestamp
#    3) (integer) 45213      → durée en µs (45 ms !)
#    4) 1) "KEYS" 2) "*"     → la commande coupable

redis-cli SLOWLOG LEN
redis-cli SLOWLOG RESET   # vider (après analyse)

# --- Rendre persistant ---
# /etc/redis/conf.d/99-production.conf :
# slowlog-log-slower-than 2000
# slowlog-max-len 256
```

> Un slowlog plein de `KEYS`, `HGETALL`, `SMEMBERS` sur de grosses clés = **code applicatif à corriger** (SCAN, HMGET, SSCAN...). Le slowlog ne ment pas.

---

## 71. Monitoring : latence (LATENCY DOCTOR)

Redis 7 mesure la latence par commande (percentiles) sans surcoût notable :

```bash
redis-cli CONFIG SET latency-tracking yes
redis-cli CONFIG SET latency-tracking-info-percentiles "50 99 99.9"

# Attendre un peu de trafic, puis :
redis-cli INFO latencystats
# latency_percentiles_usec_get:p50=8.031,p99=15.103,p99.9=22.015
# latency_percentiles_usec_hgetall:p50=120.5,p99=4500.0,...

# Diagnostic automatique :
redis-cli LATENCY DOCTOR
# Dave, I have found 2 latency spikes... (analyse les 160 derniers événements)

redis-cli LATENCY HISTORY command    # 160 derniers échantillons de la classe "command"
redis-cli LATENCY LATEST
redis-cli LATENCY RESET
```

Causes classiques de pics de latence :

| Cause | Indice | Remède |
|---|---|---|
| Commande lourde (KEYS, SORT...) | slowlog corrélé | Réécrire en SCAN / Lua borné |
| Fork BGSAVE/AOF | pics réguliers | `vm.overcommit_memory=1`, THP off |
| Swap | latence aléatoire | `vm.swappiness=1`, assez de RAM |
| AOF `always` | latence d'écriture | passer en `everysec` |
| Réseau | latence client >> serveur | `LATENCY HISTORY` vs mesure client |

---

## 72. Supervision : exemple d'alerting complet (script)

Script de health-check à brancher sur votre supervision (Nagios/Zabbix : code retour + message) :

```bash
#!/bin/bash
# /usr/local/bin/redis-health.sh — code 0 OK, 1 WARNING, 2 CRITICAL
HOST="${1:-127.0.0.1}"; PORT="${2:-6379}"
AUTH_ARGS=(-a "$REDIS_PASS")   # export REDIS_PASS depuis le coffre

ping_out=$(redis-cli -h "$HOST" -p "$PORT" "${AUTH_ARGS[@]}" ping 2>&1)
[ "$ping_out" != "PONG" ] && { echo "CRITICAL: ping=$ping_out"; exit 2; }

mem=$(redis-cli -h "$HOST" -p "$PORT" "${AUTH_ARGS[@]}" --raw INFO memory | grep used_memory_rss | cut -d: -f2)
maxmem=$(redis-cli -h "$HOST" -p "$PORT" "${AUTH_ARGS[@]}" --raw CONFIG GET maxmemory | tail -1)
if [ "$maxmem" != "0" ] && [ "$mem" -gt $(( maxmem * 90 / 100 )) ]; then
  echo "WARNING: mémoire à plus de 90% de maxmemory"; exit 1
fi

bgsave=$(redis-cli -h "$HOST" -p "$PORT" "${AUTH_ARGS[@]}" --raw INFO persistence | grep rdb_last_bgsave_status | cut -d: -f2 | tr -d '\r')
[ "$bgsave" != "ok" ] && { echo "CRITICAL: dernier BGSAVE en échec"; exit 2; }

role=$(redis-cli -h "$HOST" -p "$PORT" "${AUTH_ARGS[@]}" --raw INFO replication | grep '^role' | cut -d: -f2 | tr -d '\r')
if [ "$role" = "slave" ]; then
  link=$(redis-cli -h "$HOST" -p "$PORT" "${AUTH_ARGS[@]}" --raw INFO replication | grep master_link_status | cut -d: -f2 | tr -d '\r')
  [ "$link" != "up" ] && { echo "CRITICAL: lien réplication down"; exit 2; }
fi

echo "OK: redis $HOST:$PORT (role=$role)"
exit 0
```

---

## 73. Gestion mémoire : fragmentation et jemalloc

```bash
redis-cli INFO memory | grep -E "used_memory_human|used_memory_rss_human|mem_fragmentation_ratio|mem_allocator"
# used_memory_human:3.20G        → ce que Redis alloue (logique)
# used_memory_rss_human:4.10G    → ce que l'OS voit (résident)
# mem_fragmentation_ratio:1.28   → rss / used
# mem_allocator:jemalloc-5.3.0
```

Lecture du ratio :

| Ratio | Interprétation | Action |
|---|---|---|
| ~1,0–1,3 | Sain | Rien |
| > 1,5 | Fragmentation notable | Surveiller ; `MEMORY PURGE` (défrag jemalloc partielle) |
| > 2,0 durable | Fragmentation sévère | Planifier un **restart** (failover propre) |
| < 1,0 | Swap probable ! | Urgent : RAM/swap (le RSS < alloué = pages swappées) |

```bash
redis-cli MEMORY PURGE    # demande à l'allocateur de rendre la mémoire (effet variable)
redis-cli MEMORY STATS    # détail par classe d'allocation
redis-cli MEMORY DOCTOR   # diagnostic texte (⚠️ 4.0+)
```

> La défragmentation active (`activedefrag yes`, ⚠️ 4.0+) peut aider sur des workloads avec beaucoup d'allocations/libérations, au prix d'un peu de CPU. À tester avant d'activer en prod.

---

## 74. Gestion mémoire : traquer les big keys

Quelques grosses clés expliquent souvent 80 % de la mémoire. Les trouver **sans bloquer** :

```bash
# --- redis-cli --bigkeys : échantillonne (non bloquant, mais lent sur gros datasets) ---
redis-cli --bigkeys
# Sampled 1000000 keys in the keyspace!
# biggest string found 'cache:rapport:mensuel' has 42,123,456 bytes

# --- MEMORY USAGE : taille précise d'une clé ---
redis-cli MEMORY USAGE cache:produit:42
# (integer) 1240

# --- SCAN + MEMORY USAGE en script (top 20) ---
redis-cli --scan --pattern "*" | head -100000 | \
  while read -r k; do printf "%s %s\n" "$(redis-cli --raw MEMORY USAGE "$k")" "$k"; done | \
  sort -rn | head -20

# --- redis-cli --memkeys (⚠️ 7.x, outils récents) : top mémoire via échantillonnage ---
```

Actions sur big keys trouvées :

- String > 10 Mo : externaliser (S3/NFS) ou compresser côté appli.
- Hash/List/Set énorme : **sharder** la clé (`logs:2026-09-26`, `file:1`, `file:2`...).
- Vérifier l'encodage : `redis-cli DEBUG OBJECT ma_cle` (ou `OBJECT ENCODING`) — `raw` vs `listpack`/`intset` change tout sur les petites collections.

---

## 75. Benchmarks : mesurer avant d'optimiser

```bash
# --- redis-benchmark (fourni avec Redis) ---
# 100 000 requêtes, 50 clients parallèles, que des SET :
redis-benchmark -h 10.0.5.21 -a "$REDIS_PASS" -n 100000 -c 50 -t set,get

# Pipeline (regroupe les requêtes : le vrai gain réseau) :
redis-benchmark -h 10.0.5.21 -a "$REDIS_PASS" -n 100000 -c 50 -t set,get -P 16

# Benchmark réaliste : mélange + tailles de payload :
redis-benchmark -h 10.0.5.21 -a "$REDIS_PASS" -n 200000 -c 100 -d 256 \
  --threads 4 -t set,get,hset,hget,lpush,lpop

# --- Mesurer la latence intrinsèque ---
redis-cli --latency -h 10.0.5.21
# min: 0, max: 12, avg: 0.18 (ms) — Ctrl+C pour arrêter
redis-cli --latency-history -i 1 -h 10.0.5.21   # historique par seconde
```

Règles d'interprétation :

- Benchmarkez **depuis un client sur le même réseau** que la prod (pas depuis votre laptop via VPN).
- Le goulot est presque toujours le **réseau** (allers-retours), pas Redis → utilisez le **pipelining** côté appli.
- Comparez avant/après chaque changement (io-threads, maxmemory-policy, AOF...) avec la **même** commande.

---


## 76. Dépannage : Redis ne démarre pas

```bash
# 1. Lire le log EN PREMIER (90 % des diagnostics sont là) :
sudo tail -50 /var/log/redis/redis-server.log
sudo journalctl -u redis-server --no-pager | tail -30

# 2. Tester la config :
redis-server /etc/redis/redis.conf --test-memory 2
# ou : redis-check-rdb /var/lib/redis/dump.rdb  (si soupçon de RDB corrompu)
```

| Message / symptôme | Cause probable | Remède |
|---|---|---|
| `Can't open the append-only file: Permission denied` | Droits sur `/var/lib/redis` | `chown -R redis:redis /var/lib/redis` |
| `Fatal error loading the DB` | RDB/AOF corrompu | Restaurer le backup (section 67) ; AOF : `redis-check-aof --fix` |
| `Address already in use` | Port 6379 occupé (2e instance ?) | `ss -ltnp \| grep 6379`, tuer le doublon |
| `Can't open the log file` | `/var/log/redis` inexistant ou droits | Créer + chown redis |
| `Unknown directive` | Faute de frappe dans redis.conf | Corriger la ligne indiquée |
| `OOM command not allowed` | Démarre mais refuse tout (vieux RDB + maxmemory) | Augmenter maxmemory temporairement via CONFIG |
| Démarre puis s'arrête (systemd) | `supervised` mal réglé / `daemonize yes` + systemd | Paquet Debian : `supervised systemd`, `daemonize no` |

---

## 77. Dépannage : OOM / évictions massives

Symptômes : erreurs `OOM command not allowed when used memory > 'maxmemory'`, `evicted_keys` qui explose, hit rate en chute.

```bash
# --- Diagnostic ---
redis-cli INFO memory | grep -E "used_memory_human|maxmemory_human|mem_fragmentation"
redis-cli INFO stats | grep -E "evicted_keys|expired_keys"
redis-cli CONFIG GET maxmemory-policy
redis-cli --bigkeys | tail -20        # qui consomme ?

# --- Actions immédiates ---
# 1. Augmenter temporairement (si la RAM le permet) :
redis-cli CONFIG SET maxmemory 6gb
# 2. Purger un cache reconstituable :
redis-cli --scan --pattern "cache:tmp:*" | xargs -L 500 redis-cli DEL
# 3. Vérifier qu'un process ne remplit pas (fuite applicative) :
redis-cli INFO keyspace   # db0:keys=... en croissance continue ?
# 4. Surveiller en continu :
watch -n 2 'redis-cli INFO memory | grep used_memory_human'
```

> Si `used_memory` < `maxmemory` mais erreurs OOM quand même : c'est la **fragmentation** (section 73) ou un **fork** qui a échoué (overcommit, section 26). Lisez le log.

---

## 78. Dépannage : réplication cassée

```bash
# --- Sur le réplica ---
redis-cli INFO replication | grep -E "master_link_status|master_last_io_seconds_ago|slave_repl_offset"
# master_link_status:down  → le lien est coupé

# --- Sur le maître ---
redis-cli INFO replication | grep -E "connected_slaves|slave0"

# --- Log du réplica : indices ---
sudo grep -iE "replication|psync|sync" /var/log/redis/redis-server.log | tail -20
```

| Symptôme | Cause | Remède |
|---|---|---|
| `master_link_status:down` | Réseau / firewall / maître down | ping, `ss`, vérifier le maître |
| Resync complète en boucle | `repl-backlog-size` trop petit vs débit d'écriture | Augmenter `repl-backlog-size` (ex. 64mb) |
| `NOAUTH Authentication required` | Mot de passe réplication faux/absent | `masteruser`/`masterauth` sur le réplica |
| `READONLY` côté appli après failover | L'appli écrit encore sur l'ancien maître | Basculer l'appli (DNS/VIP/Sentinel-aware) |
| Lag qui grandit | Réplica saturé (CPU/réseau) | `INFO stats` sur le réplica, dimensionner |

```ini
# Prévention :
repl-backlog-size 64mb
replica-serve-stale-data yes
```

---

## 79. Dépannage : latence élevée

```bash
# 1. La latence vient-elle du serveur ou du réseau ?
redis-cli --latency-history -i 1        # latence vue du client
redis-cli LATENCY DOCTOR                # diagnostic serveur
redis-cli INFO latencystats             # percentiles par commande

# 2. Chercher le coupable :
redis-cli SLOWLOG GET 20

# 3. Vérifier le système :
uptime                                  # load average
free -h                                 # swap utilisé ?
dmesg | tail -20                        # OOM killer ?
cat /sys/kernel/mm/transparent_hugepage/enabled   # doit être [never]
```

Checklist express : slowlog → THP → swap → fork (BGSAVE en cours ? `INFO persistence`) → `appendfsync always` → saturation réseau → `blocked_clients` anormal.

---

## 80. Dépannage : AOF corrompu au démarrage

```
# Log typique :
# Bad file format reading the append only file: make a backup of your AOF file, then use ./redis-check-aof --fix <filename>
```

```bash
# 1. SAUVEGARDER avant toute chose :
sudo cp -a /var/lib/redis/appendonlydir /srv/backups/redis/appendonlydir.corrompu-$(date +%F)

# 2. Tenter la réparation (tronque la queue corrompue — perte des dernières écritures) :
# Ancien format mono-fichier :
redis-check-aof --fix /var/lib/redis/appendonly.aof
# Format 7.0+ multi-part : identifier le segment incrémental fautif dans appendonlydir/,
# le réparer ou le retirer du manifeste (documentez ce que vous faites !)

# 3. Alternative sûre : repartir du dernier RDB sain + accepter la perte AOF
# (voir procédure de restauration section 67)

# 4. Prévention : onduleur (coupures = 1re cause), appendfsync everysec minimum,
#    aof-load-truncated yes (déjà conseillé section 29)
```

---

## 81. Dépannage : Cluster en état `fail`

```bash
redis-cli --cluster check 10.0.5.41:6379
# [ERR] Not all 16384 slots are covered by nodes.

redis-cli -c -h 10.0.5.41 CLUSTER INFO | grep cluster_state
# cluster_state:fail
```

| Cause | Remède |
|---|---|
| Un maître down sans réplica | Redémarrer le nœud ; sinon `CLUSTER FORGET` + reshard |
| Slots non assignés après ajout de nœud | `redis-cli --cluster reshard` / `rebalance` |
| `cluster-require-full-coverage yes` (défaut) bloque tout | Temporairement `no` pour dégrader gracieusement (à vos risques) |
| Bus cluster (16379) filtré | Ouvrir le port entre nœuds |

```bash
# Forcer l'oubli d'un nœud mort (sur chaque nœud restant) :
redis-cli -c -h 10.0.5.41 CLUSTER FORGET <node-id-du-mort>
# Réassigner ses slots au préalable via reshard si des données doivent survivre.
```

---

## 82. Dépannage : saturation des connexions clients

Symptômes : `ERR max number of clients reached`, `connected_clients` = `maxclients`.

```bash
redis-cli INFO clients | grep -E "connected_clients|maxclients|blocked_clients"
redis-cli CLIENT LIST | head -5        # qui est connecté ? (addr, cmd, idle...)
redis-cli CLIENT LIST | awk '{print $2}' | sort | uniq -c | sort -rn | head
# → top des IPs par nombre de connexions (fuite de connexions applicative ?)

# Tuer les connexions idle abusives d'une IP :
redis-cli CLIENT KILL ADDR 10.0.5.99:51234
# ou par pattern :
redis-cli CLIENT KILL TYPE normal SKIPME yes
```

Prévention :

```ini
maxclients 10000
timeout 300              # déconnecte les idle > 5 min
```

Et côté applicatif : **pool de connexions** (pas une connexion par requête), avec taille bornée et timeout.

---

## 83. Les 10 erreurs classiques (et comment les éviter)

| # | Erreur | Conséquence | Prévention |
|---|---|---|---|
| 1 | `KEYS *` en production | **Blocage total** du serveur (mono-thread) | `SCAN` + `--scan` ; `rename-command KEYS ""` |
| 2 | Aucune persistance configurée | Perte totale au reboot | RDB + AOF (section 30) |
| 3 | Pas de `maxmemory` | OOM killer tue Redis (ou la machine) | `maxmemory` + politique d'éviction (section 27) |
| 4 | Exposé sur Internet sans AUTH | Compromission, `FLUSHALL` + rançon | bind + ACL + firewall (sections 35–40) |
| 5 | `FLUSHALL` sur la mauvaise instance | Suppression de **toutes** les bases | ACL `-@dangerous`, `rename-command`, confirmation en prod |
| 6 | TTL identiques en masse | Avalanche d'expirations, pic CPU | Jitter ±10 % (section 33) |
| 7 | `SAVE` au lieu de `BGSAVE` | Blocage de plusieurs secondes/minutes | Toujours `BGSAVE` |
| 8 | Verrou sans TTL ni token | Verrou éternel / libération du verrou d'autrui | Pattern section 46 (NX+PX+token+Lua) |
| 9 | Client non Sentinel/Cluster-aware | Pas de failover, `MOVED` en erreur | Libs adaptées (section 60/64) |
| 10 | Backup jamais testé | Restauration impossible le jour J | Test trimestriel chronométré (section 67) |

Bonus — erreurs de config fréquentes :

- `bind 0.0.0.0` « pour que ça marche » → exposition totale.
- `appendfsync always` « pour la sécurité » → débit divisé par ~10 sans raison.
- 16 bases `SELECT` utilisées comme namespaces → cauchemar en Cluster ; préférez des préfixes de clés ou des instances.

---

## 84. Cas pratique 1 : cache devant PostgreSQL (catalogue produits)

Contexte : 50 000 produits, 2 000 req/s en lecture, 10 écritures/s. Objectif : < 5 ms p99, tolérance stale 5 min.

```bash
# --- Convention de clés ---
# cache:produit:<sku>          → JSON du produit, TTL 300 s + jitter
# cache:categorie:<id>:liste   → liste des SKU, TTL 600 s
# version:produit:<sku>        → version pour invalidation (section 54)

# --- Lecture (cache-aside, pseudo-code déjà vu section 52) ---
# 1. GET cache:produit:sku-7788
# 2. MISS → SELECT ... FROM produits WHERE sku='sku-7788'
# 3. SET ... EX <300±30>

# --- Écriture (invalidation explicite) ---
# UPDATE produits SET prix=... WHERE sku='sku-7788'
redis-cli DEL cache:produit:sku-7788
redis-cli DEL cache:categorie:12:liste
# Variante versioning (zéro SCAN) :
redis-cli INCR version:produit:sku-7788

# --- Warm-up après déploiement (pré-remplir le top 1000) ---
# Script appli : SELECT sku FROM produits ORDER BY vues DESC LIMIT 1000
# puis pipeline de SET (1 pipeline de 1000 SET ≈ quelques ms)

# --- Monitoring du cache ---
# hit rate :
#   redis-cli INFO stats | grep keyspace_hits / keyspace_misses
# Si hit rate < 95 % : TTL trop court ? clés trop granulaires ? avalanche ?
```

Dimensionnement : 50 000 produits × ~2 Ko = ~100 Mo → `maxmemory 512mb`, `allkeys-lru`, RDB seul (reconstituable).

---

## 85. Cas pratique 2 : compteur temps réel (votes / supervision)

Contexte : sondage en direct, 10 000 votes/min, affichage temps réel + top 5.

```bash
# --- Vote (1 par utilisateur : Set pour l'unicité + compteur) ---
redis-cli EVAL "
if redis.call('SADD', KEYS[2], ARGV[1]) == 1 then
  redis.call('INCR', KEYS[1])
  return 1
else
  return 0
end
" 2 sondage:42:votes sondage:42:votants user:1042
# → 1 vote compté, 0 déjà voté

# --- Lecture temps réel ---
redis-cli GET sondage:42:votes

# --- Sondages multiples : un hash global ---
redis-cli HINCRBY sondages:votes 42 1
redis-cli HGETALL sondages:votes

# --- Fenêtre glissante : votes des 60 dernières secondes (ZSET, section 50.2) ---
# ZADD sondage:42:fenetre <timestamp_ms> <uuid>  puis ZCOUNT sur [now-60000, now]

# --- Remise à zéro / archivage ---
redis-cli RENAME sondage:42:votes archive:sondage:42:votes   # atomique, sans copie
redis-cli EXPIRE archive:sondage:42:votes 2592000           # archive 30 jours
```

---

## 86. Cas pratique 3 : file de tâches avec workers (streams)

Contexte : génération de rapports PDF, 4 workers, exigence « aucun rapport perdu ».

```bash
# ===== PRODUCTEUR (appli web) =====
redis-cli XADD jobs:rapports MAXLEN ~ 20000 * type "pdf" dossier "D-7788" demandeur "awa"

# ===== WORKER (boucle, pseudo-shell) =====
# 1. Attend un job :
#    XREADGROUP GROUP grp:pdf worker-3 COUNT 1 BLOCK 10000 STREAMS jobs:rapports ">"
# 2. Génère le PDF...
# 3. Succès → XACK jobs:rapports grp:pdf <id>
#    Échec transient → ne pas ACK (restera pending, récupéré par le superviseur)

# ===== SUPERVISEUR (cron toutes les 2 min) =====
# Réclame les jobs idle depuis > 5 min (worker crashé en plein traitement) :
redis-cli XAUTOCLAIM jobs:rapports grp:pdf superviseur 300000 0 COUNT 20
# → les réinjecter dans le traitement ou alerter après 3 tentatives
# (compter les tentatives : XPENDING donne le retry count par entrée)

# ===== OBSERVABILITÉ =====
redis-cli XLEN jobs:rapports                          # taille de la file
redis-cli XPENDING jobs:rapports grp:pdf              # jobs en cours non acquittés
redis-cli XINFO CONSUMERS jobs:rapports grp:pdf       # activité par worker
```

Alertes : `XLEN` > 5 000 (workers sous-dimensionnés), pending > 100 depuis > 10 min (workers en panne).

---

## 87. Cas pratique 4 : rate limiter d'API (100 req/min)

Contexte : API publique, quota 100 req/min par clé API, réponse 429 au-delà.

```bash
# ===== Lua déployé une fois (fenêtre fixe, cf. section 50.1) =====
# Clé : ratelimit:<apikey> — valeur : compteur — TTL : 60 s

# ===== Test manuel =====
for i in $(seq 1 105); do
  c=$(redis-cli --raw EVAL "
local c = redis.call('INCR', KEYS[1])
if c == 1 then redis.call('EXPIRE', KEYS[1], ARGV[1]) end
return c" 1 ratelimit:cle-demo 60)
  if [ "$c" -gt 100 ]; then echo "requête $i → 429 (compteur=$c)"; break; fi
done

# ===== Côté appli (pseudo-code) =====
# c = lua_ratelimit("ratelimit:" + api_key, 60)
# headers: X-RateLimit-Limit: 100, X-RateLimit-Remaining: max(0, 100 - c)
# if c > 100: return 429 + Retry-After: <TTL restant>

# ===== Variante stricte (fenêtre glissante, section 50.2) =====
# À utiliser si les pics "à cheval sur deux fenêtres" sont inacceptables
# (ex. facturation au nombre d'appels).
```

> Pour un rate limiting **global** (tous clients confondus, ex. 10 000 req/s vers un backend) : même pattern avec une clé unique. Pour du **per-IP** : `ratelimit:ip:<ip>`.

---

## 88. Cas pratique 5 : sessions + panier e-commerce

```bash
# ===== SESSION (cf. section 51) =====
TOKEN=$(openssl rand -hex 32)
redis-cli HSET session:$TOKEN user_id 1042 role client
redis-cli EXPIRE session:$TOKEN 1800

# ===== PANIER (hash, 1 champ par ligne) =====
# panier:<user_id> → { sku → quantité }
redis-cli HSET panier:1042 sku-7788 2 sku-9911 1
redis-cli HINCRBY panier:1042 sku-7788 1      # +1 article
redis-cli HINCRBY panier:1042 sku-9911 -1     # -1 article
redis-cli HDEL panier:1042 sku-9911          # retire la ligne (quantité 0 → HDEL)
redis-cli HGETALL panier:1042
redis-cli EXPIRE panier:1042 2592000         # panier conservé 30 jours

# ===== Validation commande : transaction "panier → commande" =====
# 1. Lire le panier (HGETALL)
# 2. INSERT en base SQL (source de vérité)
# 3. Si OK : DEL panier:1042  (en Lua avec vérification, ou MULTI)
redis-cli MULTI
redis-cli DEL panier:1042
redis-cli SADD commandes:user:1042 cmd-2026-0007788
redis-cli EXEC

# ===== Abandon de panier (marketing) : keyspace notifications (section 45) =====
# notify-keyspace-events Ex ; PSUBSCRIBE __keyevent@0__:expired
# → à l'expiration d'une clé panier:*, déclencher l'email de relance
# (avec le filet anti-manqué : job quotidien qui scanne aussi)
```

---

## 89. Mise à jour de Redis : procédure sans coupure

Principe : **réplica d'abord**, bascule, puis l'ancien maître. Jamais de `apt upgrade` aveugle sur le maître en pleine journée.

```bash
# === 1. Préparation ===
# - Lire les release notes (breaking changes, ex. 6→7 : fonctions, ACL par défaut...)
# - Snapshot de sauvegarde vérifié (section 65)
# - Fenêtre de maintenance annoncée

# === 2. Mettre à jour les RÉPLICAS un par un ===
sudo apt-mark unhold redis-server
sudo apt update && sudo apt install -y redis-server=7:7.4.0-1rl1~bookworm1  # version précise
redis-server --version
sudo systemctl restart redis-server
redis-cli INFO replication | grep master_link_status   # → up
redis-cli INFO server | grep redis_version

# === 3. Bascule (avec Sentinel : failover contrôlé ; sans : manuel) ===
# Avec Sentinel :
redis-cli -p 26379 SENTINEL FAILOVER mymaster
# Sans Sentinel : REPLICAOF NO ONE sur le réplica à jour, repointer l'appli

# === 4. Mettre à jour l'ancien maître (devenu réplica) ===
# même procédure apt qu'à l'étape 2

# === 5. Vérifications post-upgrade ===
redis-cli INFO server | grep redis_version     # partout pareil
redis-cli INFO replication                     # topologie saine
# rejouer les tests applicatifs critiques (connexion, lecture/écriture, TTL)
```

> **Compatibilité ascendante des fichiers** : un RDB/AOF écrit par une 7.x ne se relit pas toujours sur une 6.x → **pas de downgrade** sans backup préalable de l'ancienne version. Testez le rollback en labo avant.

---

## 90. Migration : changer de machine / de topologie

```bash
# === MÉTHODE 1 : réplication (recommandée, quasi sans coupure) ===
# 1. Nouvelle machine : installer Redis, même version, même config (sauf replicaof)
# 2. En faire un réplica de l'ancien maître :
redis-cli -h <nouvelle> REPLICAOF <ancien-maitre> 6379
# 3. Attendre master_link_status:up + offset à jour (INFO replication)
# 4. Bascule : REPLICAOF NO ONE sur la nouvelle, repointer appli/DNS
# 5. Éteindre l'ancienne après une période d'observation

# === MÉTHODE 2 : RDB (coupure courte) ===
# 1. BGSAVE sur l'ancien, rsync du dump.rdb
# 2. Stopper les écritures applicatives, dernier BGSAVE, rsync final
# 3. Démarrer le nouveau avec le dump.rdb, repointer l'appli

# === MÉTHODE 3 : MIGRATE (clé par clé, en ligne) ---
redis-cli -h <ancien> MIGRATE <nouvelle> 6379 "" 0 5000 KEYS cache:produit:42
# "" = pas d'AUTH sur la cible (ou mettre le mot de passe) ; 0 = base ; 5000 = timeout ms
# ⚠️ MIGRATE est bloquant par clé : à éviter sur des millions de clés chaudes.

# === Vérification post-migration ===
# Comparer les nombres de clés :
redis-cli -h <ancien> DBSIZE
redis-cli -h <nouveau> DBSIZE
# Échantillonner : pour 100 clés au hasard, comparer les valeurs (script).
```

---

## 91. redis-cli : les options à connaître

```bash
# --- Connexion ---
redis-cli -h 10.0.5.21 -p 6379 -a "$REDIS_PASS"       # hôte/port/auth
redis-cli -n 2                                         # base n°2
redis-cli -u redis://appli_cache:Secret@10.0.5.21:6379/0  # URI
redis-cli -s /var/run/redis/redis-server.sock          # socket Unix (plus rapide en local)
redis-cli --tls --cacert /etc/redis/tls/ca.crt         # TLS
redis-cli --user appli_cache --pass "$PASS"            # ACL (⚠️ 6.0+)
redis-cli --intrinsic-latency 100                       # mesure la latence intrinsèque (100 s)

# --- Sortie exploitable en script ---
redis-cli --raw GET ma_cle            # sans guillemets ni numéros
redis-cli --csv HGETALL user:1042     # format CSV

# --- Scan sans bloquer (JAMAIS keys *) ---
redis-cli --scan --pattern "cache:*"                 # itère toutes les clés
redis-cli --scan --pattern "session:*" | wc -l       # compte
redis-cli --scan --pattern "tmp:*" | xargs -L 1000 redis-cli DEL   # purge par lot

# --- Piping / mass insert ---
cat data.txt | redis-cli --pipe          # protocole brut, ultra-rapide
# data.txt au format : *3\r\n$3\r\nSET\r\n$3\r\nkey\r\n$5\r\nvalue\r\n

# --- Monitoring temps réel ---
redis-cli MONITOR          # ⚠️ TOUTES les commandes en temps réel (divise le débit par ~2 !)
# À n'utiliser que quelques secondes en debug, jamais en continu.

redis-cli --stat           # stats temps réel (ops/s, mémoire, clients)
redis-cli --latency        # latence min/max/avg en continu
redis-cli --bigkeys        # échantillonne les plus grosses clés
redis-cli --hotkeys        # ⚠️ outils récents : clés les plus accédées

# --- Cluster ---
redis-cli -c -h 10.0.5.41              # mode cluster (suit les redirections)
redis-cli --cluster check 10.0.5.41:6379
redis-cli --cluster info 10.0.5.41:6379

# --- Divers ---
redis-cli --rdb /tmp/dump-copy.rdb     # télécharge un RDB depuis le serveur distant
redis-cli -x SET ma_cle < fichier.bin  # -x : lit la valeur depuis stdin
```

---

## 92. redis.conf : modèle production complet (annoté)

```ini
# ============================================================
# redis.conf — modèle production Redis 7.x
# Machine : 8 Go RAM dédiée — adaptez maxmemory et les chemins
# ============================================================

# --- RÉSEAU ---
bind 10.0.5.21 127.0.0.1
protected-mode yes
port 6379
tcp-backlog 2048
timeout 300
tcp-keepalive 300

# --- GÉNÉRAL ---
supervised systemd
daemonize no
loglevel notice
logfile /var/log/redis/redis-server.log
databases 16
# include /etc/redis/conf.d/*.conf   # (à ajouter si vous utilisez des overrides)

# --- SNAPSHOTS RDB ---
save 3600 1
save 300 100
save 60 10000
dbfilename dump.rdb
dir /var/lib/redis
rdbcompression yes
rdbchecksum yes

# --- RÉPLICATION ---
# replicaof 10.0.5.20 6379        # décommenter sur les réplicas
replica-read-only yes
repl-diskless-sync yes
repl-backlog-size 64mb
min-replicas-to-write 1
min-replicas-max-lag 10

# --- SÉCURITÉ ---
# requirepass "..."               # ou aclfile ci-dessous (recommandé)
aclfile /etc/redis/users.acl
rename-command FLUSHALL ""
rename-command FLUSHDB ""
rename-command DEBUG ""

# --- LIMITES ---
maxclients 10000

# --- MÉMOIRE ---
maxmemory 6gb
maxmemory-policy allkeys-lru
maxmemory-eviction-tenacity 10    # ⚠️ 7.0+ : agressivité d'éviction (10 = défaut)
maxmemory-samples 5

# --- AOF ---
appendonly yes
appendfsync everysec
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
aof-rewrite-incremental-fsync yes
aof-load-truncated yes

# --- SLOWLOG / LATENCE ---
slowlog-log-slower-than 2000
slowlog-max-len 256
latency-tracking yes

# --- DIVERS PROD ---
hz 10
activerehashing yes
# io-threads 4                   # à activer si saturation réseau mesurée
# io-threads-do-reads yes
notify-keyspace-events ""
# tls-port 6379 / port 0         # si TLS (voir section 38)
```

---

## 93. Checklist de mise en production

À imprimer et cocher pour chaque nouvelle instance :

**Installation**
- [ ] Paquet du dépôt officiel, version épinglée (`apt-mark hold`)
- [ ] `vm.overcommit_memory=1`, `somaxconn`, THP `never` (section 26)
- [ ] Service systemd `enabled`, redémarrage testé

**Configuration**
- [ ] `bind` restreint, `protected-mode yes`
- [ ] `maxmemory` + politique d'éviction adaptée
- [ ] Persistance : RDB + AOF `everysec` (ou choix assumé, section 30)
- [ ] `timeout`, `tcp-keepalive`, `maxclients` réglés
- [ ] `slowlog-log-slower-than` ≤ 5000 µs

**Sécurité**
- [ ] ACL : `default off`, users applicatifs moindre-privilège (section 36)
- [ ] Secrets en coffre, jamais dans git
- [ ] `rename-command` FLUSHALL/FLUSHDB/DEBUG
- [ ] Firewall : 6379 (et 16379 si cluster, 26379 si sentinel) filtrés
- [ ] TLS si réseau non fiable
- [ ] Scan externe : port non visible d'Internet

**Haute disponibilité**
- [ ] Réplication : 1 maître + 2 réplicas, `min-replicas-to-write 1`
- [ ] Sentinel × 3 (quorum 2) **ou** Cluster × 6 — testé par failover réel
- [ ] Clients Sentinel/Cluster-aware

**Sauvegarde**
- [ ] Script de backup RDB quotidien + rétention 30 j
- [ ] Copie hors site (règle 3-2-1)
- [ ] **Restauration testée** et chronométrée (RTO connu)

**Supervision**
- [ ] `redis_exporter` + dashboard Grafana (ou équivalent)
- [ ] Alertes : down, mémoire, évictions, hit rate, bgsave, lien réplication, latence
- [ ] Health-check scripté (section 72)

**Documentation**
- [ ] Topologie et mots de passe (coffre) documentés
- [ ] Procédure de failover et de restauration écrites
- [ ] Astreinte informée des seuils d'alerte

---

## 94. Pense-bête de poche

```
CONNEXION
  redis-cli -h HOST -p 6379 -a PASS        connexion
  redis-cli ping                           PONG ?
  redis-cli -c -h HOST                     mode cluster

STRINGS
  SET k v [EX s|PX ms] [NX|XX]             écriture (+TTL, conditionnelle)
  GET k / MGET k1 k2 / GETDEL k            lectures
  INCR k / INCRBY k n / DECR k             compteurs atomiques
  APPEND k v / STRLEN k                    concat / longueur

HASHES
  HSET k f v [f v...] / HGET k f / HMGET k f1 f2
  HGETALL k / HDEL k f / HEXISTS k f / HLEN k
  HINCRBY k f n

LISTS (files)
  RPUSH k v / LPUSH k v / LLEN k
  LPOP k / RPOP k / BLPOP k timeout        (BLPOP = bloquant)
  LRANGE k 0 -1 / LTRIM k -100 -1          (historique borné)
  LMOVE src dst LEFT RIGHT                (transfert atomique)

SETS
  SADD k m / SREM k m / SMEMBERS k / SISMEMBER k m
  SINTER / SUNION / SDIFF k1 k2            (opérations ensemblistes)
  SINTERSTORE dst k1 k2                    (stocke le résultat)

SORTED SETS
  ZADD k score m / ZSCORE k m / ZRANK k m / ZREVRANK k m
  ZRANGE k 0 -1 [REV] [WITHSCORES]         (⚠️ 6.2+ : REV remplace ZREVRANGE)
  ZRANGE k min max BYSCORE / BYLEX
  ZINCRBY k n m / ZPOPMIN k / ZPOPMAX k
  ZREMRANGEBYSCORE k min max

STREAMS
  XADD k MAXLEN ~ N * f v ...              produire
  XREAD BLOCK ms STREAMS k $               lire (bloquant)
  XGROUP CREATE k grp 0 MKSTREAM           créer un groupe
  XREADGROUP GROUP grp c COUNT n STREAMS k ">"
  XACK k grp id [id...]                    acquitter
  XAUTOCLAIM k grp c ms 0 COUNT n          récupérer les pannes
  XLEN k / XPENDING k grp

CLÉS / TTL
  DEL k / UNLINK k (non bloquant) / EXISTS k
  EXPIRE k s / PEXPIRE k ms / TTL k / PTTL k / PERSIST k
  SCAN 0 MATCH "pat:*" COUNT 1000         (JAMAIS keys *)
  RENAME k nk / TYPE k / DUMP k / RESTORE k ttl v

TRANSACTIONS / LUA
  MULTI ... EXEC / DISCARD / WATCH k
  EVAL "script" nbcles k1 [k2...] arg1 [arg2...]
  EVALSHA sha nbcles k1... / SCRIPT LOAD / SCRIPT FLUSH

PUB/SUB
  SUBSCRIBE c / PSUBSCRIBE "pat:*" / PUBLISH c msg
  SSUBSCRIBE / SPUBLISH                    (sharded, ⚠️ 7.0+)

ADMIN
  INFO [section] / CONFIG GET|SET param / CONFIG REWRITE
  BGSAVE / BGREWRITEAOF / LASTSAVE / DBSIZE / FLUSHDB (⚠️)
  SLOWLOG GET n / LATENCY DOCTOR / MEMORY DOCTOR
  CLIENT LIST / CLIENT KILL / MONITOR (⚠️ perf)
  ACL LIST / ACL SETUSER ... / ACL SAVE
  REPLICAOF host port / REPLICAOF NO ONE

SENTINEL (port 26379)
  SENTINEL masters / SENTINEL sentinels mymaster
  SENTINEL get-master-addr-by-name mymaster
  SENTINEL FAILOVER mymaster

CLUSTER
  redis-cli --cluster create h1:6379 ... --cluster-replicas 1
  redis-cli --cluster check h1:6379 / --cluster info h1:6379
  CLUSTER NODES / CLUSTER SLOTS / CLUSTER INFO
```

---

## 95. Glossaire

| Terme | Définition |
|---|---|
| ACL | Access Control List : utilisateurs, commandes et clés autorisés (Redis 6+) |
| AOF | Append Only File : journal des écritures, rejoué au redémarrage |
| Big key | Clé anormalement grosse, source de latence/mémoire |
| BLPOP/BRPOP | Variantes **bloquantes** de LPOP/RPOP (attendent un élément) |
| Cache-aside | Pattern : l'appli lit Redis, va en base si miss |
| Cluster | Mode sharding : 16 384 slots répartis sur N maîtres |
| Consumer group | Groupe de consommateurs se partageant un stream |
| CROSSSLOT | Erreur : opération multi-clés sur des slots différents |
| Éviction | Suppression d'une clé par `maxmemory-policy` (≠ expiration) |
| Expiration | Suppression automatique à fin de TTL (passive + active) |
| Failover | Bascule automatique maître → réplica (Sentinel/Cluster) |
| Hash slot | 1 des 16 384 compartiments du cluster (`CRC16(clé) mod 16384`) |
| Hash tag | `{...}` dans une clé pour forcer le même slot |
| Hit rate | `hits / (hits + misses)` : efficacité du cache |
| HyperLogLog | Compteur approximatif d'éléments distincts (12 Ko, erreur 0,81 %) |
| IO threads | Threads d'E/S réseau (Redis 6+, l'exécution reste mono-thread) |
| Jitter | Variation aléatoire des TTL pour éviter les avalanches d'expiration |
| Latence p99 | 99 % des requêtes sont plus rapides que cette valeur |
| LFU / LRU | Least Frequently/Recently Used : politiques d'éviction |
| Lua (script) | Script exécuté côté serveur, atomiquement |
| Maître / réplica | Nœud qui accepte les écritures / copie en lecture seule |
| maxmemory | Plafond mémoire ; au-delà : évictions ou erreurs OOM |
| MOVED / ASK | Redirections cluster vers le bon nœud |
| MULTI/EXEC | Transaction : exécution atomique d'une file de commandes |
| OOM | Out Of Memory : mémoire épuisée |
| Pipeline | Envoi groupé de commandes (1 aller-retour réseau) |
| Protected mode | Garde-fou refusant les connexions distantes non authentifiées |
| Pub/Sub | Messagerie publish/subscribe, sans persistance |
| Quorum | Majorité de Sentinels requise pour un failover |
| RDB | Snapshot binaire ponctuel (`dump.rdb`) |
| Redlock | Algorithme de verrou distribué sur N instances |
| Réplication | Copie asynchrone maître → réplica(s) |
| RESP | Protocole texte/binaire client-serveur de Redis |
| Sentinel | Supervision + failover automatique (port 26379) |
| Sharding | Partitionnement des données sur plusieurs nœuds |
| Single-threaded | Exécution des commandes sur un seul thread (atomicité) |
| Slowlog | Journal des commandes lentes (> seuil en µs) |
| Sorted Set | Ensemble trié par score (skiplist) |
| Split-brain | Partition réseau créant deux « maîtres » divergents |
| Stream | Journal append-only avec consumer groups (Redis 5+) |
| THP | Transparent Huge Pages : à désactiver (pics de latence) |
| TTL | Time To Live : durée de vie d'une clé |
| WAIT | Attente de la réplication sur N réplicas (semi-synchrone) |
| Write-through | Écriture synchrone cache + base |
| XACK / XPENDING | Acquittement / messages non acquittés d'un consumer group |

---

## 96. Quiz : 10 questions + réponses

**Q1. Quelle commande liste les clés sans bloquer le serveur en production ?**
> `SCAN` (avec `MATCH` et `COUNT`). `KEYS *` bloque le thread unique d'exécution.

**Q2. Que se passe-t-il si Redis redémarre sans persistance configurée ?**
> Perte totale des données (tout était en RAM). D'où RDB et/ou AOF en production.

**Q3. `INCR` sur une clé inexistante : que retourne-t-il ?**
> `1` : la clé est créée à 0 puis incrémentée, atomiquement.

**Q4. Quelle est la différence entre `volatile-lru` et `allkeys-lru` ?**
> `volatile-lru` n'évince que les clés **avec TTL** ; `allkeys-lru` peut évincer n'importe quelle clé.

**Q5. Pourquoi `SET verrou NX PX 30000` est-il préférable à `SETNX` + `EXPIRE` ?**
> Atomicité : avec `SETNX`+`EXPIRE` en deux temps, un crash entre les deux laisse un verrou sans TTL (éternel).

**Q6. Un subscriber pub/sub reçoit-il les messages publiés avant sa souscription ?**
> Non. Le pub/sub est fire-and-forget, sans persistance ni rejeu. Pour du durable : les Streams.

**Q7. Combien de hash slots compte un cluster Redis, et comment une clé y est-elle assignée ?**
> 16 384 slots ; `slot = CRC16(clé) mod 16384`. Les hash tags `{...}` forcent la co-localisation.

**Q8. Quel est le quorum recommandé pour 3 Sentinels, et pourquoi un nombre impair ?**
> Quorum = 2 (majorité). Un nombre impair évite les égalités lors d'une partition réseau (split-brain).

**Q9. `appendfsync everysec` : quelle perte de données maximale en cas de crash ?**
> Environ 1 seconde d'écritures (le fsync a lieu une fois par seconde).

**Q10. Votre `INFO stats` montre `evicted_keys` qui augmente vite et un hit rate en chute. Diagnostic et action ?**
> `maxmemory` atteint : Redis évince. Actions : vérifier les big keys (`--bigkeys`), augmenter `maxmemory` si la RAM le permet, ajuster la politique d'éviction / les TTL, ou ajouter de la capacité (réplica/cluster).

---

## 97. Pour aller plus loin

**Documentation officielle**
- https://redis.io/docs/ — documentation complète (commandes, concepts, tuning)
- https://redis.io/docs/latest/operate/ — guide d'exploitation (persistance, réplication, cluster)

**Livres**
- *Redis in Action* (Josiah Carlson) — patterns applicatifs
- *Redis Essentials* — prise en main rapide

**Outils**
- `redis_exporter` (Prometheus) + dashboard Grafana « Redis » (ID 763)
- `redis-stat` — dashboard temps réel léger
- `redli` / `iredis` — clients CLI alternatifs (autocomplétion, coloration)

**Sujets d'approfondissement**
- Redis Functions (⚠️ 7.0+) : alternative persistante aux scripts Lua éphémères
- Redis Query Engine (ex-RediSearch) : recherche plein texte et secondaire
- Active-Active (CRDT, Redis Enterprise) : multi-maîtres multi-sites
- Chiffrement au repos : AOF/RDB sur volume chiffré (LUKS)
- Durcissement : CIS Benchmark for Redis (si applicable à votre audit)

**Prochaines étapes conseillées pour votre SI**
1. Monter un labo : 1 maître + 2 réplicas + 3 Sentinels (VM ou conteneurs).
2. Y faire passer un cas réel (cache catalogue ou sessions).
3. Simuler pannes (kill -9 du maître, partition réseau) et chronométrer le failover.
4. Écrire la procédure d'exploitation (backup, restauration, upgrade) **avant** la prod.

---

*Fin du guide — bon courage, et que vos hit rates soient élevés.* 🚀
