# PostgreSQL en production — Guide complet pour sysadmin

> **Public :** Zelef, chef de service systèmes & énergies, sysadmin.
> **Angle :** pratique, production, Debian/Ubuntu, versions 15/16/17.
> **Avertissement :** les valeurs de paramètres sont des points de départ à valider
> par vos tests de charge et vos fiches techniques. Aucun mot de passe réel
> n'apparaît dans ce guide : utilisez le coffre à secrets de votre entreprise
> (Ansible Vault, KeePass, gestionnaire de mots de passe d'équipe).

---

## Sommaire

| Partie | Sections | Thème |
|---|---|---|
| A | 1–12 | Prise en main : versions, architecture, installation |
| B | 13–22 | Connexion sécurisée : psql, rôles, pg_hba.conf, SSL |
| C | 23–35 | SQL opérationnel : DDL, DML, types, contraintes |
| D | 36–47 | Performance : index, EXPLAIN, VACUUM, transactions |
| E | 48–58 | Exploitation : sauvegardes, PITR, réplication, haute dispo |
| F | 59–68 | Tuning, monitoring, alertes |
| G | 69–75 | Mise à jour, dépannage, erreurs classiques |
| H | 76–85 | Cas pratiques, pense-bête, glossaire, quiz, ressources |

---

# Partie A — Prise en main

## 1. Pourquoi PostgreSQL en 2026

PostgreSQL (souvent abrégé **Postgres** ou **PG**) est le SGBDR open source
de référence en production : licence permissive (PostgreSQL Licence), ACID
strict, extensible (extensions comme PostGIS, pg_stat_statements, pg_trgm),
et disponible nativement sur Debian/Ubuntu.

**Points forts pour un sysadmin :**

- Réplication native (streaming), sauvegardes à chaud (PITR).
- Sécurité fine : rôles, `pg_hba.conf`, SCRAM-SHA-256, SSL/TLS, RLS.
- Supervision riche : vues `pg_stat_*`, `EXPLAIN ANALYZE`.
- Comportement prévisible sous charge : MVCC, autovacuum.

**Points de vigilance :**

- Le tuning par défaut de Debian est conservateur (adapté à une petite VM).
- Le WAL (journal) peut remplir un disque si l'archivage est mal configuré.
- Les mises à jour majeures (15 → 16 → 17) exigent `pg_upgrade` ou
  dump/restore — jamais un simple `apt upgrade`.

---

## 2. Versions 15, 16, 17 : ce qui change et cycle de vie

| Version | Sortie | Fin de support (EOL) | Nouveautés marquantes |
|---|---|---|---|
| 15 | oct. 2022 | nov. 2027 | `MERGE`, amélioration tri/parallélisme, WAL LZ4/Zstd |
| 16 | sept. 2023 | nov. 2028 | Parallélisme `FULL OUTER JOIN`, `pg_stat_io`, statistiques par défaut à 500 |
| 17 | sept. 2024 | nov. 2029 | Vacuum parallèle, `MERGE` avec `RETURNING`, amélioration `EXPLAIN` |

**Règle d'or :** choisissez la version la plus récente supportée par votre
distribution et vos applications, et planifiez la montée de version majeure
**avant** la fin de support (voir section 69, `pg_upgrade`).

> ⚠️ **Avertissement de version :** certaines syntaxes de ce guide (ex. `MERGE`,
> vues `pg_stat_io`) n'existent qu'à partir des versions indiquées. Vérifiez
> toujours avec `SELECT version();` sur votre serveur.

---

## 3. Architecture : processus et mémoire

Un cluster PostgreSQL, c'est **un postmaster** (processus parent) + des
processus fils, plus des zones de mémoire partagée.

### 3.1. Processus principaux

| Processus | Rôle |
|---|---|
| `postgres` (postmaster) | Écoute les connexions, lance les fils |
| `postgres: checkpointer` | Écrit les pages sales sur disque (checkpoints) |
| `postgres: background writer` | Écrit les pages sales en continu, en douceur |
| `postgres: walwriter` | Écrit le WAL sur disque |
| `postgres: autovacuum launcher` | Déclenche les workers autovacuum |
| `postgres: autovacuum worker` | VACUUM/ANALYZE automatiques |
| `postgres: logical replication launcher` | Réplication logique |
| `postgres: <user> <db>` | Un processus **par connexion client** (modèle fork) |

```bash
# Voir les processus d'un cluster en cours
ps aux | grep -E "postgres" | grep -v grep
```

**Conséquence pratique :** chaque connexion = un processus (~quelques Mo).
Au-delà de ~200–300 connexions, utilisez un **pooler** (PgBouncer)
plutôt que d'augmenter `max_connections` indéfiniment.

### 3.2. Mémoire

| Zone | Paramètre | Rôle |
|---|---|---|
| Mémoire partagée | `shared_buffers` | Cache de pages de données (le plus important) |
| Cache OS | `effective_cache_size` | Indication au planificateur (pas une allocation) |
| Mémoire par tri | `work_mem` | Tris, hachages **par opération** et par connexion |
| Mémoire maintenance | `maintenance_work_mem` | VACUUM, CREATE INDEX, ALTER TABLE |
| WAL buffers | `wal_buffers` | Tampon d'écriture du WAL |

```
┌─────────────────────────────────────────────┐
│              Mémoire partagée                │
│  shared_buffers │ wal_buffers │ locks, etc.  │
├─────────────────────────────────────────────┤
│  Processus par connexion (work_mem chacun)   │
├─────────────────────────────────────────────┤
│  Checkpointer / BG writer / WAL writer       │
└─────────────────────────────────────────────┘
```

---

## 4. MVCC : le contrôle de concurrence multi-version

PostgreSQL n'utilise **pas** de verrous en lecture : chaque transaction voit
un **instantané** (snapshot) des données au moment où elle démarre.

**Principes :**

- `INSERT`/`UPDATE`/`DELETE` créent de **nouvelles versions** de lignes
  (tuples) ; les anciennes restent jusqu'au `VACUUM`.
- Un `UPDATE` = `DELETE` logique + `INSERT` : la table grossit si on ne
  vacuum pas (bloat).
- Les lecteurs ne bloquent jamais les écrivains, et inversement.

```sql
-- Chaque ligne porte des numéros de transaction (xmin/xmax), invisibles
-- mais consultables pour le diagnostic :
SELECT xmin, xmax, * FROM capteurs LIMIT 3;
```

**Implications sysadmin :**

1. L'**autovacuum** n'est pas optionnel : sans lui, les tables gonflent
   et les performances s'effondrent (voir section 44).
2. Les **longues transactions** (idle in transaction) empêchent le
   nettoyage des vieilles versions → bloat + risque de wraparound du XID.
3. Le **wraparound** (dépassement du compteur de transactions sur 32 bits)
   est l'incident le plus grave : surveillez l'âge des XID (section 63).

---

## 5. Installation sur Debian/Ubuntu : dépôt officiel PostgreSQL

Les dépôts Debian/Ubuntu contiennent souvent une version en retard.
Pour les versions 15/16/17 récentes, utilisez le **dépôt officiel PGDG**.

```bash
# 1. Prérequis
sudo apt update
sudo apt install -y curl ca-certificates gnupg lsb-release

# 2. Clé du dépôt PGDG
sudo install -d /usr/share/postgresql-common/pgdg
sudo curl -o /usr/share/postgresql-common/pgdg/apt.postgresql.org.asc \
  --fail https://www.postgresql.org/media/keys/ACCC4CF8.asc

# 3. Ajouter le dépôt
sudo sh -c 'echo "deb [signed-by=/usr/share/postgresql-common/pgdg/apt.postgresql.org.asc] \
  https://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" \
  > /etc/apt/sources.list.d/pgdg.list'

# 4. Installer PostgreSQL 17 (remplacez 17 par 15 ou 16 si besoin)
sudo apt update
sudo apt install -y postgresql-17 postgresql-client-17 postgresql-contrib-17
```

**Vérification :**

```bash
# Version installée et clusters présents
pg_lsclusters
# Exemple de sortie :
# Ver Cluster Port Status Owner    Data directory              Log file
# 17  main    5432 online postgres /var/lib/postgresql/17/main /var/log/postgresql/postgresql-17-main.log

# Le service systemd
sudo systemctl status postgresql@17-main
```

> 💡 Sur Debian, l'outil `pg_wrapper` (`pg_lsclusters`, `pg_ctlcluster`,
> `pg_dropcluster`…) gère plusieurs versions/clusters côte à côte.
> C'est spécifique à Debian/Ubuntu : sur RHEL, on utilise directement
> `initdb` et `systemctl`.

---

## 6. Arborescence Debian et premier démarrage

| Chemin | Contenu |
|---|---|
| `/etc/postgresql/17/main/` | `postgresql.conf`, `pg_hba.conf`, `pg_ident.conf` |
| `/var/lib/postgresql/17/main/` | Données (PGDATA) |
| `/var/log/postgresql/` | Journaux |
| `/var/run/postgresql/` | Socket Unix |

```bash
# Se connecter en local via le socket, en tant que postgres
sudo -u postgres psql

# Dans psql :
postgres=# SELECT version();
postgres=# \q
```

**Checklist post-installation immédiate :**

- [ ] `pg_lsclusters` → cluster `online`
- [ ] `sudo -u postgres psql -c "SELECT 1;"` → OK
- [ ] Définir un mot de passe pour le superuser `postgres`
      (ou mieux : créer des rôles dédiés, section 15)
- [ ] Adapter `listen_addresses` si accès réseau nécessaire (section 7)
- [ ] Configurer `pg_hba.conf` avant d'ouvrir le réseau (section 16)
- [ ] Activer et tester la sauvegarde (section 48) **dès le jour 1**

```sql
-- Changer le mot de passe du superuser postgres (à faire une fois)
ALTER USER postgres PASSWORD 'METTRE_UN_VRAI_MOT_DE_PASSE_DU_COFFRE';
```

---

## 7. postgresql.conf : les paramètres essentiels

Le fichier `/etc/postgresql/17/main/postgresql.conf` pilote le serveur.
Sur Debian, les valeurs Debian sont en tête ; vos réglages vont en fin
de fichier (ou dans `conf.d/`).

```bash
# Recharger la conf sans couper les connexions (SIGHUP)
sudo -u postgres psql -c "SELECT pg_reload_conf();"
# ou
sudo systemctl reload postgresql@17-main

# Savoir si un paramètre demande un redémarrage
sudo -u postgres psql -c \
  "SELECT name, context FROM pg_settings WHERE name IN
   ('shared_buffers','max_connections','port','listen_addresses');"
-- context = 'postmaster'  -> redémarrage obligatoire
-- context = 'sighup'      -> reload suffit
```

### 7.1. Connexions et réseau

```ini
# --- Connexions ---
listen_addresses = 'localhost'      # '*' pour écouter toutes les interfaces
port = 5432
max_connections = 100               # défaut ; 200-300 max sans pooler

# --- Sécurité réseau de base ---
ssl = on                            # TLS (voir section 18)
```

> ⚠️ Ne passez `listen_addresses = '*'` qu'**après** avoir durci
> `pg_hba.conf` (section 16) et activé SSL (section 18).

### 7.2. Emplacements et journaux

```ini
# --- Journalisation (indispensable en prod) ---
logging_collector = on
log_directory = 'log'               # relatif à PGDATA, ou absolu
log_filename = 'postgresql-%Y-%m-%d.log'
log_rotation_age = 1d
log_rotation_size = 100MB
log_min_duration_statement = 1000   # loggue les requêtes > 1 s (ms)
log_checkpoints = on
log_connections = on
log_disconnections = on
log_lock_waits = on                 # loggue les attentes de verrous
log_autovacuum_min_duration = 0     # 0 = tout logguer (bruyant) ; 1000 en prod
```

### 7.3. Checkpoints et WAL (première approche)

```ini
# --- WAL / checkpoints ---
wal_level = replica                 # minimal pour réplication/PITR (défaut 15+)
max_wal_size = 4GB
min_wal_size = 1GB
checkpoint_timeout = 15min
checkpoint_completion_target = 0.9
```

Le détail du tuning (mémoire, checkpoints, autovacuum) est en
partie F (sections 59–62).

---

## 8. Créer et gérer les clusters Debian (pg_wrapper)

```bash
# Lister
pg_lsclusters

# Créer un second cluster (ex. version 17, port 5433, locale fr)
sudo pg_createcluster 17 test --port=5433 --locale=fr_FR.UTF-8

# Démarrer / arrêter / redémarrer / recharger
sudo pg_ctlcluster 17 main start
sudo pg_ctlcluster 17 main stop
sudo pg_ctlcluster 17 main restart
sudo pg_ctlcluster 17 main reload

# Supprimer un cluster (destructif ! demande confirmation)
sudo pg_dropcluster 17 test

# Voir la conf effective d'un paramètre
sudo -u postgres psql -c "SHOW shared_buffers;"
sudo -u postgres psql -c "SHOW config_file;"
```

**Bonnes pratiques :**

- Un cluster = une version majeure + un jeu de paramètres + un port.
- En production : **un seul cluster** par serveur (sauf besoin explicite).
- Avant `pg_dropcluster` : sauvegarde + vérification (section 48).

---

## 9. Arrêt et démarrage : les modes

| Mode | Commande | Effet |
|---|---|---|
| smart | `pg_ctlcluster … stop -m smart` | Attend la fin des sessions (défaut Debian) |
| fast | `… stop -m fast` | Coupe les connexions, rollback, arrêt propre |
| immediate | `… stop -m immediate` | Arrêt brutal → recovery au redémarrage |

```bash
# Arrêt propre rapide (recommandé pour maintenance planifiée)
sudo pg_ctlcluster 17 main stop -m fast

# Redémarrage après changement de paramètre postmaster
sudo pg_ctlcluster 17 main restart -m fast
```

> ⚠️ Le mode `immediate` force une relecture du WAL au redémarrage
> (crash recovery). C'est sans perte de données validées, mais le
> redémarrage est plus long sur une grosse base.

---

## 10. Prérequis système : disque, filesystem, locale

**Disque :**

- Séparez si possible : données (`/var/lib/postgresql`), WAL (`pg_wal`
  sur disque dédié via tablespace ou lien), sauvegardes (autre machine).
- Filesystem : **ext4** ou **XFS** (évitez les FS exotiques ou le NFS
  pour les données — le NFS est une source classique de corruption).
- Réservez 15–20 % d'espace libre pour le WAL, les tris et l'autovacuum.

**Locale et encodage :**

```bash
# Vérifier les locales disponibles AVANT de créer le cluster
locale -a | grep -i utf

# Créer le cluster en UTF-8 explicite (recommandé)
sudo pg_createcluster 17 main --locale=fr_FR.UTF-8 \
  --start-conf=auto
```

> ⚠️ L'encodage et la locale d'un cluster sont fixés à `initdb`.
> On ne les change pas après coup : il faut dump/restore.

**Checklist prérequis :**

- [ ] Disque données + disque WAL/sauvegardes séparés (ou planifiés)
- [ ] Locale `*.UTF-8` disponible
- [ ] NTP/chrony actif (horodatage WAL, certificats SSL)
- [ ] Firewall : n'ouvrir 5432 que vers les hôtes applicatifs

---

## 11. Installation : cas d'une version précise et dépôts figés (prod)

En production, figez la version mineure pour des déploiements reproductibles :

```bash
# Voir les versions disponibles dans le dépôt PGDG
apt-cache policy postgresql-17

# Installer une version mineure précise
sudo apt install -y postgresql-17=17.4-1.pgdg22.04+1

# Figer le paquet (hold) pour éviter une montée surprise
sudo apt-mark hold postgresql-17 postgresql-client-17

# Lever le hold quand vous planifiez la mise à jour mineure
sudo apt-mark unhold postgresql-17
```

**Mises à jour mineures (ex. 17.4 → 17.5) :** simple `apt upgrade` +
redémarrage du cluster. Elles ne changent pas le format des données.

**Mises à jour majeures (17 → 18) :** voir section 69 (`pg_upgrade`).

---

## 12. Vérifier une installation : la batterie de tests "jour 1"

```bash
#!/bin/bash
# health_jour1.sh — à lancer après chaque installation
set -e
echo "== Cluster ==";      pg_lsclusters
echo "== Version ==";      sudo -u postgres psql -tAc "SELECT version();"
echo "== Ecoute ==";       ss -ltnp | grep 5432 || echo "socket local uniquement"
echo "== Ecriture ==";     sudo -u postgres psql -tAc \
  "CREATE TABLE IF NOT EXISTS _t(id serial primary key); \
   INSERT INTO _t DEFAULT VALUES RETURNING id; DROP TABLE _t;"
echo "== Conf ==";         sudo -u postgres psql -tAc \
  "SHOW config_file; SHOW hba_file; SHOW data_directory;"
echo "OK"
```

Si chaque étape passe, la base est prête pour la configuration de
production (partie B).

---

# Partie B — Connexion sécurisée : psql, rôles, authentification, SSL

## 13. psql : le client en ligne de commande

`psql` est l'outil quotidien du sysadmin : requêtes, administration,
scripts.

```bash
# Connexion locale via socket (pair = OS user postgres)
sudo -u postgres psql

# Connexion TCP avec mot de passe (variable d'env, pas d'option -W en script)
PGPASSWORD='...' psql -h 10.0.0.5 -U app_lecteur -d inventaire

# Connexion avec fichier .pgpass (recommandé pour les scripts)
# ~/.pgpass : host:port:db:user:password, chmod 600
psql -h 10.0.0.5 -U app_lecteur -d inventaire -c "SELECT now();"

# Exécuter un fichier SQL
psql -h srv-bdd -U deploy -d inventaire -f migration_042.sql

# Mode non interactif pour scripts (erreurs -> exit 1)
psql -v ON_ERROR_STOP=1 -f migration.sql
```

**Options utiles :**

| Option | Effet |
|---|---|
| `-c "SQL"` | Exécute une commande puis quitte |
| `-f fichier` | Exécute un script |
| `-v ON_ERROR_STOP=1` | Stoppe au premier échec |
| `-X` | Ignore `~/.psqlrc` (scripts reproductibles) |
| `-q -t -A` | Sortie silencieuse/terse, pratique pour `$(...)` |
| `-x` | Affichage étendu (colonnes en lignes) |

---

## 14. Méta-commandes psql : l'essentiel

Les méta-commandes commencent par `\` et ne vont **pas** au serveur :
c'est `psql` qui les interprète.

| Commande | Affiche |
|---|---|
| `\l` / `\l+` | Bases de données |
| `\du` / `\du+` | Rôles et attributs |
| `\dn` | Schémas |
| `\dt` / `\dt+` | Tables (+ tailles) |
| `\di` | Index |
| `\dv` | Vues |
| `\df` | Fonctions |
| `\d nom` | Description d'une table (colonnes, index, contraintes) |
| `\d+ nom` | + tailles et descriptions |
| `\sf nom_fonction` | Code source d'une fonction |
| `\x` | Bascule affichage étendu |
| `\timing` | Affiche le temps d'exécution |
| `\pset null '[NULL]'` | Représentation des NULL |
| `\copy` | Import/export CSV côté **client** |
| `\i fichier.sql` | Exécute un fichier |
| `\o fichier` | Redirige la sortie |
| `\e` | Édite la dernière requête dans `$EDITOR` |
| `\set` / `\unset` | Variables psql |
| `\q` | Quitter |

```sql
-- Exemples en session
\dt+                  -- tables avec tailles
\d+ capteurs          -- détail d'une table
\x on                 -- lecture d'une ligne large
SELECT * FROM capteurs WHERE id = 42;
\timing on
SELECT count(*) FROM mesures;
```

> 💡 `\copy` (client) ≠ `COPY` (serveur) : `\copy` lit/écrit les fichiers
> **de votre poste**, `COPY` ceux **du serveur** (réservé au superuser).

---

## 15. Rôles, utilisateurs, groupes : le modèle PostgreSQL

PostgreSQL ne distingue pas utilisateurs et groupes : tout est un
**rôle** (`ROLE`). Un rôle avec `LOGIN` peut se connecter.

```sql
-- Créer un rôle applicatif (sans superuser !)
CREATE ROLE app_lecteur WITH LOGIN PASSWORD '...' ;
CREATE ROLE app_ecrivain WITH LOGIN PASSWORD '...';

-- Rôle groupe puis membres (bonne pratique)
CREATE ROLE equipe_reseau;                       -- NOLOGIN par défaut
GRANT equipe_reseau TO alice, bob;                -- membres
ALTER ROLE alice INHERIT;                         -- défaut : hérite des droits

-- Attributs importants
ALTER ROLE app_ecrivain WITH CONNECTION LIMIT 50; -- limite de connexions
ALTER ROLE batch SET statement_timeout = '5min';  -- paramètre par rôle

-- Voir les rôles
\du
SELECT rolname, rolsuper, rolcanlogin, rolconnlimit
FROM pg_roles ORDER BY rolname;
```

**Principes de production :**

1. **Jamais** l'application en superuser (`postgres`).
2. Un rôle par application/fonction : `app_lecteur`, `app_ecrivain`,
   `batch_nuit`, `supervision`.
3. Les humains passent par des rôles nominatifs membres de rôles groupes.
4. `CONNECTION LIMIT` pour éviter qu'un batch ne sature `max_connections`.

---

## 16. pg_hba.conf : qui se connecte, comment, d'où

`pg_hba.conf` (Host-Based Authentication) décide, **dans l'ordre des
lignes**, quelle méthode d'authentification s'applique.

```
# TYPE  DATABASE  USER       ADDRESS        METHOD
local   all       postgres                 peer
local   all       all                      peer
host    all       all        127.0.0.1/32   scram-sha-256
host    all       all        ::1/128        scram-sha-256
host    inventaire app_lecteur 10.0.1.0/24  scram-sha-256
host    all       all        0.0.0.0/0      reject
```

**Lecture :** première ligne qui correspond (type + base + user + adresse)
→ la méthode s'applique, les suivantes sont ignorées.

### 16.1. Méthodes d'authentification

| Méthode | Usage | Sécurité |
|---|---|---|
| `trust` | ⚠️ Aucune vérif. Dev/test local uniquement | Nulle |
| `peer` | Identité OS via socket Unix | Bonne (local) |
| `scram-sha-256` | Mot de passe chiffré (défaut moderne) | Bonne |
| `md5` | Obsolète, à migrer vers SCRAM | Faible |
| `cert` | Certificat client TLS | Très bonne |
| `ldap` / `radius` | Annuaire externe | Selon infra |
| `reject` | Refuse explicitement | — |

```bash
# Après modification : recharger (pas besoin de redémarrer)
sudo pg_ctlcluster 17 main reload
# ou : SELECT pg_reload_conf();

# Tester une règle depuis un hôte distant
psql "host=10.0.0.5 dbname=inventaire user=app_lecteur sslmode=require"
```

**Checklist pg_hba.conf :**

- [ ] Pas de `trust` sur `host` (jamais en prod)
- [ ] `scram-sha-256` partout où un mot de passe est utilisé
- [ ] Règles du plus spécifique au plus général, `reject` final explicite
- [ ] `peer` mappe l'OS user `postgres` au rôle `postgres` (pratique + sûr)

---

## 17. Migrer les mots de passe vers SCRAM-SHA-256

```sql
-- 1. Vérifier la méthode de stockage actuelle
SELECT rolname,
       CASE WHEN rolpassword LIKE 'SCRAM-SHA-256%' THEN 'scram'
            WHEN rolpassword LIKE 'md5%' THEN 'md5'
            ELSE 'autre/absent' END AS methode
FROM pg_authid WHERE rolcanlogin;

-- 2. Forcer SCRAM pour les nouveaux mots de passe
-- postgresql.conf :
-- password_encryption = 'scram-sha-256'   (défaut depuis la v14)

-- 3. Régénérer les mots de passe md5 (chaque rôle doit le refaire)
ALTER ROLE app_lecteur PASSWORD 'nouveau-mot-de-passe-du-coffre';

-- 4. Recharger et tester les connexions applicatives
```

> ⚠️ Changer `password_encryption` ne convertit **pas** les hash existants :
> il faut ré-émettre chaque mot de passe.

---

## 18. SSL/TLS : chiffrer les connexions

Sur Debian, le cluster génère un certificat auto-signé (`snakeoil`).
En production, utilisez un certificat de votre CA interne.

```ini
# postgresql.conf
ssl = on
ssl_cert_file = '/etc/postgresql/17/main/server.crt'
ssl_key_file  = '/etc/postgresql/17/main/server.key'  # chmod 600, owner postgres
ssl_ca_file   = '/etc/postgresql/17/main/ca.crt'      # si auth par certificat
```

```bash
# Déployer un certificat interne (exemple)
sudo cp srv-bdd.crt /etc/postgresql/17/main/server.crt
sudo cp srv-bdd.key /etc/postgresql/17/main/server.key
sudo chown postgres:postgres /etc/postgresql/17/main/server.*
sudo chmod 600 /etc/postgresql/17/main/server.key
sudo pg_ctlcluster 17 main restart   # ssl = postmaster -> restart

# Vérifier côté client
psql "host=srv-bdd dbname=inventaire user=app_lecteur sslmode=verify-full \
      sslrootcert=/etc/ssl/certs/ca-interne.crt" -c "SHOW ssl;"
```

**Modes `sslmode` côté client :**

| Mode | Chiffrement | Vérifie le certificat |
|---|---|---|
| `disable` | Non | Non |
| `require` | Oui | Non (vulnérable au MITM) |
| `verify-ca` | Oui | Signé par la CA |
| `verify-full` | Oui | CA + nom d'hôte ✅ |

**En production : `verify-full`** pour les applis, `require` minimum.

---

## 19. Droits fins : GRANT / REVOKE en détail

```sql
-- Droits sur une base (connexion)
GRANT CONNECT ON DATABASE inventaire TO app_lecteur;

-- Droits sur un schéma (USAGE pour voir les objets)
GRANT USAGE ON SCHEMA public TO app_lecteur;

-- Droits sur tables existantes
GRANT SELECT ON ALL TABLES IN SCHEMA public TO app_lecteur;
GRANT SELECT, INSERT, UPDATE ON capteurs TO app_ecrivain;

-- Droits sur les FUTURES tables (piège classique !)
ALTER DEFAULT PRIVILEGES IN SCHEMA public
  GRANT SELECT ON TABLES TO app_lecteur;
-- À exécuter pour chaque rôle créateur de tables :
ALTER DEFAULT PRIVILEGES FOR ROLE deploy IN SCHEMA public
  GRANT SELECT, INSERT, UPDATE, DELETE ON TABLES TO app_ecrivain;

-- Séquences (indispensable avec les serial/identity)
GRANT USAGE, SELECT ON ALL SEQUENCES IN SCHEMA public TO app_ecrivain;
ALTER DEFAULT PRIVILEGES IN SCHEMA public
  GRANT USAGE, SELECT ON SEQUENCES TO app_ecrivain;

-- Révoquer
REVOKE DELETE ON capteurs FROM app_ecrivain;

-- Voir les droits effectifs
\z capteurs        -- ou \dp
SELECT grantee, privilege_type FROM information_schema.role_table_grants
WHERE table_name = 'capteurs';
```

> ⚠️ **Piège n°1 des permissions :** `GRANT ... ON ALL TABLES` ne couvre
> que les tables **existantes**. Sans `ALTER DEFAULT PRIVILEGES`, la
> prochaine table créée sera inaccessible à l'appli.

---

## 20. Schémas : organiser et cloisonner

```sql
CREATE SCHEMA supervision;              -- schéma dédié
CREATE TABLE supervision.alertes (...);

-- Isoler une appli dans son schéma
CREATE SCHEMA appli_energie;
GRANT USAGE ON SCHEMA appli_energie TO app_ecrivain;
ALTER ROLE app_ecrivain SET search_path = appli_energie, public;

-- search_path : ordre de résolution des noms non qualifiés
SHOW search_path;
```

**Bonnes pratiques :**

- Un schéma par application/domaine (`energie`, `reseau`, `supervision`).
- Ne laissez pas les applis écrire dans `public` sans contrôle.
- Depuis PG 15, `public` n'est plus inscriptible par `PUBLIC` par défaut
  (durcissement bienvenu : vérifiez vos migrations).

---

## 21. Row-Level Security (RLS) : introduction

La RLS filtre les lignes visibles **selon le rôle connecté**, au niveau
du serveur : impossible à contourner depuis l'appli.

```sql
-- Activer la RLS sur une table multi-sites
ALTER TABLE mesures ENABLE ROW LEVEL SECURITY;

-- Politique : chaque technicien ne voit que son site
CREATE POLICY site_isolation ON mesures
  FOR ALL
  USING (site_id = current_setting('app.site_id')::int);

-- Le propriétaire contourne par défaut ; forcer aussi pour lui :
ALTER TABLE mesures FORCE ROW LEVEL SECURITY;

-- Côté appli : positionner la variable à chaque connexion
SET app.site_id = '3';
SELECT * FROM mesures;   -- ne retourne que le site 3
```

**Cas d'usage :** SaaS multi-tenant, cloisonnement par site/agence,
données sensibles par profil. **Coût :** une condition ajoutée à chaque
requête — indexez la colonne de filtrage (`site_id`).

---

## 22. Checklist sécurité "mise en production"

- [ ] `listen_addresses` restreint au nécessaire
- [ ] `pg_hba.conf` : `scram-sha-256`, pas de `trust` en `host`
- [ ] SSL `on`, `sslmode=verify-full` côté applis critiques
- [ ] Rôles applicatifs non-superuser, `CONNECTION LIMIT` posée
- [ ] `ALTER DEFAULT PRIVILEGES` en place pour les futurs objets
- [ ] Mots de passe dans le coffre, jamais dans les scripts en clair
- [ ] `log_connections`, `log_disconnections`, `log_min_duration_statement`
- [ ] Compte `supervision` en lecture seule (`pg_monitor`, voir section 60)
- [ ] Sauvegarde testée (section 48) **avant** l'ouverture aux applis
- [ ] Firewall : 5432 ouvert uniquement vers les hôtes applicatifs

---

# Partie C — SQL opérationnel : DDL, types, DML

## 23. Créer une base et s'y connecter

```sql
-- Créer une base (depuis postgres ou template)
CREATE DATABASE inventaire
  OWNER app_admin
  ENCODING 'UTF8'
  LC_COLLATE 'fr_FR.UTF-8'
  LC_CTYPE 'fr_FR.UTF-8'
  TEMPLATE template0;   -- template0 = vierge, sans dépendance locale

-- Lister, puis s'y connecter
\l
\c inventaire
```

```bash
# Équivalents shell
createdb -O app_admin inventaire
dropdb inventaire   # destructif, demande confirmation si données
```

> ⚠️ `CREATE DATABASE` copie un template : impossible si des connexions
> sont actives sur la base source. `TEMPLATE template0` évite les
> surprises de locale.

---

## 24. Types de données : le tableau de référence

| Famille | Types | Quand l'utiliser |
|---|---|---|
| Entiers | `smallint`, `integer`, `bigint` | Compteurs, clés ; `bigint` pour les gros volumes |
| Séries | `GENERATED ... AS IDENTITY` | Clés primaires auto-incrémentées (moderne, remplace `serial`) |
| Numériques exacts | `numeric(p,s)`, `money` | Monnaie, énergie (kWh facturés) : **jamais** `float` pour l'argent |
| Flottants | `real`, `double precision` | Mesures physiques, calculs scientifiques |
| Texte | `text`, `varchar(n)`, `char(n)` | `text` par défaut ; `varchar(n)` = contrainte de longueur |
| Booléen | `boolean` | `TRUE/FALSE/NULL` |
| Dates/heures | `date`, `time`, `timestamp`, `timestamptz` | **Toujours `timestamptz`** en prod (fuseau géré) |
| Durées | `interval` | `INTERVAL '90 minutes'` |
| Réseau | `inet`, `cidr`, `macaddr` | Adresses IP : requêtes `>>` (contenu dans) natives |
| JSON | `jsonb` | Configs, payloads : indexable (GIN), **préférer à `json`** |
| UUID | `uuid` | Clés externes, identifiants publics (`gen_random_uuid()`) |
| Tableaux | `text[]`, `int[]` | Listes simples ; pas de jointure possible |
| Géométrie | `point`, extensions PostGIS | SIG (extension `postgis`) |
| Binaire | `bytea` | Petits blobs ; gros fichiers → stockage objet externe |

```sql
-- Exemples de création avec bons types
CREATE TABLE capteurs (
  id          bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  code        text NOT NULL UNIQUE,
  ip          inet,
  site_id     integer NOT NULL REFERENCES sites(id),
  config      jsonb NOT NULL DEFAULT '{}',
  installe_le timestamptz NOT NULL DEFAULT now()
);
```

**Règles pratiques :**

- `timestamptz` partout : stocke en UTC, affiche selon `TimeZone` session.
- `numeric` pour la facturation, `double precision` pour les mesures.
- `inet` > `varchar` pour les IP (validation + opérateurs réseau).

---

## 25. DDL : CREATE / ALTER / DROP sans se blesser

```sql
-- CREATE TABLE IF NOT EXISTS : idempotent pour les migrations
CREATE TABLE IF NOT EXISTS sites (
  id   integer GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  nom  text NOT NULL,
  code text NOT NULL UNIQUE
);

-- ALTER : ajouter une colonne avec défaut (rapide depuis PG 11,
-- le défaut est stocké en méta, pas réécrit)
ALTER TABLE capteurs ADD COLUMN IF NOT EXISTS actif boolean NOT NULL DEFAULT true;

-- Renommer (attention aux applis !)
ALTER TABLE capteurs RENAME TO sondes;
ALTER TABLE sondes RENAME COLUMN code TO reference;

-- Changer un type (USING si conversion non implicite)
ALTER TABLE mesures ALTER COLUMN valeur TYPE numeric USING valeur::numeric;

-- DROP avec garde-fou
DROP TABLE IF EXISTS _tmp_import;                 -- simple
DROP TABLE mesures CASCADE;                       -- + objets dépendants (dangereux)
```

**Migrations en production :**

1. Toujours `IF NOT EXISTS` / `IF EXISTS` dans les scripts rejouables.
2. `ALTER TABLE ... ADD COLUMN` prend un verrou `ACCESS EXCLUSIVE`
   bref ; sur une table énorme, préférez les heures creuses.
3. Testez chaque migration sur une copie (restauration, section 51).

---

## 26. Contraintes : la qualité des données côté serveur

| Contrainte | Rôle | Exemple |
|---|---|---|
| `PRIMARY KEY` | Unicité + NOT NULL (index B-tree implicite) | `id bigint PRIMARY KEY` |
| `FOREIGN KEY` | Intégrité référentielle | `REFERENCES sites(id)` |
| `UNIQUE` | Unicité (NULL multiples autorisés) | `UNIQUE(code)` |
| `NOT NULL` | Valeur obligatoire | `nom text NOT NULL` |
| `CHECK` | Règle métier | `CHECK (puissance_kva > 0)` |
| `EXCLUDE` | Non-chevauchement (plages) | Pas de réservations qui se chevauchent |
| `DEFAULT` | Valeur par défaut | `DEFAULT now()` |

```sql
CREATE TABLE onduleurs (
  id            bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  reference     text NOT NULL UNIQUE,
  puissance_kva numeric(8,2) NOT NULL CHECK (puissance_kva > 0),
  site_id       integer NOT NULL REFERENCES sites(id) ON DELETE RESTRICT,
  mise_en_service date NOT NULL DEFAULT CURRENT_DATE,
  CONSTRAINT puissance_coherente CHECK (puissance_kva <= 2000)
);

-- Ajouter une contrainte sans bloquer (PG 12+) : NOT VALID puis VALIDATE
ALTER TABLE mesures ADD CONSTRAINT mesure_positive
  CHECK (valeur >= 0) NOT VALID;          -- rapide, ne scanne pas
ALTER TABLE mesures VALIDATE CONSTRAINT mesure_positive;  -- scan en SHARE UPDATE EXCLUSIVE
```

> 💡 `NOT VALID` + `VALIDATE` : la méthode propre pour ajouter un `CHECK`
> ou une `FOREIGN KEY` sur une table volumineuse **sans** verrou exclusif
> long.

---

## 27. Clés étrangères : comportements ON DELETE / ON UPDATE

```sql
CREATE TABLE interventions (
  id           bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  onduleur_id  bigint NOT NULL REFERENCES onduleurs(id)
                 ON DELETE RESTRICT      -- refuse si des interventions existent
                 ON UPDATE CASCADE,
  technicien_id bigint REFERENCES techniciens(id)
                 ON DELETE SET NULL      -- garde l'historique, annule le lien
);
```

| Action | Effet à la suppression du parent |
|---|---|
| `RESTRICT` / `NO ACTION` | Refuse (défaut prudent) |
| `CASCADE` | Supprime les enfants (dangereux si mal compris) |
| `SET NULL` | Met la colonne à NULL |
| `SET DEFAULT` | Met la valeur par défaut |

**En production :** `RESTRICT` par défaut sur les données métier critiques ;
`CASCADE` uniquement sur des tables techniques liées (lignes de détail).

---

## 28. INSERT / UPDATE / DELETE : DML de base

```sql
-- INSERT simple et multiple
INSERT INTO sites (nom, code) VALUES ('Siège', 'SIEGE');
INSERT INTO sites (nom, code) VALUES
  ('Atelier', 'ATEL'), ('Dépôt', 'DEPOT')
RETURNING id;                                   -- récupère les id générés

-- UPSERT : insère ou met à jour en cas de conflit (idempotent !)
INSERT INTO capteurs (code, site_id)
VALUES ('SONDE-01', 1)
ON CONFLICT (code) DO UPDATE SET site_id = EXCLUDED.site_id;

-- UPDATE avec jointure
UPDATE mesures m
SET valide = false
FROM capteurs c
WHERE m.capteur_id = c.id AND c.actif = false;

-- DELETE avec RETURNING (audit avant suppression réelle)
DELETE FROM _tmp_import WHERE importe_le < now() - INTERVAL '30 days'
RETURNING *;

-- TRUNCATE : vide vite (sans MVCC ligne à ligne), réinitialise les séquences
TRUNCATE TABLE _tmp_import RESTART IDENTITY;
```

> ⚠️ `UPDATE`/`DELETE` sans `WHERE` : commencez toujours par le `SELECT`
> équivalent, ou travaillez en transaction (section 45).

---

## 29. SELECT : jointures et anti-jointures

```sql
-- Jointure interne
SELECT c.code, s.nom AS site
FROM capteurs c
JOIN sites s ON s.id = c.site_id;

-- LEFT JOIN : garder les capteurs même sans mesures
SELECT c.code, count(m.id) AS nb_mesures
FROM capteurs c
LEFT JOIN mesures m ON m.capteur_id = c.id
GROUP BY c.code;

-- Anti-jointure : capteurs SANS mesures (NOT EXISTS > NOT IN avec NULL)
SELECT c.code
FROM capteurs c
WHERE NOT EXISTS (
  SELECT 1 FROM mesures m WHERE m.capteur_id = c.id
);

-- LATERAL : top-N par groupe
SELECT s.nom, t.*
FROM sites s
CROSS JOIN LATERAL (
  SELECT * FROM mesures m
  WHERE m.site_id = s.id
  ORDER BY m.horodatage DESC LIMIT 5
) t;
```

---

## 30. CTE (WITH) : requêtes lisibles et maintenables

```sql
-- CTE simple : dernière mesure par capteur
WITH dernieres AS (
  SELECT capteur_id, max(horodatage) AS h
  FROM mesures GROUP BY capteur_id
)
SELECT c.code, m.valeur, m.horodatage
FROM dernieres d
JOIN capteurs c ON c.id = d.capteur_id
JOIN mesures m ON m.capteur_id = d.capteur_id AND m.horodatage = d.h;

-- CTE récursive : arborescence (ex. sites parent/enfant)
WITH RECURSIVE arborescence AS (
  SELECT id, nom, parent_id, 1 AS niveau
  FROM sites WHERE parent_id IS NULL
  UNION ALL
  SELECT s.id, s.nom, s.parent_id, a.niveau + 1
  FROM sites s JOIN arborescence a ON s.parent_id = a.id
)
SELECT * FROM arborescence ORDER BY niveau, nom;

-- CTE chaînées pour un rapport
WITH par_site AS (
  SELECT site_id, count(*) AS nb FROM capteurs GROUP BY site_id
),
alertes AS (
  SELECT site_id, count(*) AS nb FROM supervision.alertes
  WHERE resolue = false GROUP BY site_id
)
SELECT s.nom, coalesce(p.nb,0) AS capteurs, coalesce(a.nb,0) AS alertes
FROM sites s
LEFT JOIN par_site p ON p.site_id = s.id
LEFT JOIN alertes a ON a.site_id = s.id;
```

> 💡 Depuis PG 12, les CTE non récursives sont **inlinées** par le
> planificateur quand c'est rentable : plus de barrière d'optimisation
> systématique. Ajoutez `MATERIALIZED` pour forcer l'évaluation unique.

---

## 31. Fonctions de fenêtrage : le top-N, les rangs, les écarts

```sql
-- Dernière mesure par capteur (alternative propre au DISTINCT ON)
SELECT DISTINCT ON (capteur_id) capteur_id, valeur, horodatage
FROM mesures ORDER BY capteur_id, horodatage DESC;

-- Rang des onduleurs par puissance et par site
SELECT reference, site_id, puissance_kva,
       rank() OVER (PARTITION BY site_id ORDER BY puissance_kva DESC) AS rang_site
FROM onduleurs;

-- Écart entre deux mesures successives (dérive d'un capteur)
SELECT capteur_id, horodatage, valeur,
       valeur - lag(valeur) OVER (PARTITION BY capteur_id ORDER BY horodatage)
         AS delta
FROM mesures;

-- Moyenne glissante sur 1 heure (utile : lissage de courbes)
SELECT horodatage, valeur,
       avg(valeur) OVER (
         ORDER BY horodatage
         RANGE BETWEEN INTERVAL '1 hour' PRECEDING AND CURRENT ROW
       ) AS moyenne_1h
FROM mesures WHERE capteur_id = 42;
```

---

## 32. JSONB : requêtes et index

```sql
-- Extraire
SELECT config->>'seuil_alerte' AS seuil,          -- texte
       (config->>'seuil_alerte')::numeric AS seuil_num,
       config #> '{reseau,ip}' AS ip             -- chemin imbriqué
FROM capteurs;

-- Filtrer
SELECT * FROM capteurs WHERE config @> '{"actif": true}';      -- contient
SELECT * FROM capteurs WHERE config ? 'seuil_alerte';          -- clé existe
SELECT * FROM capteurs WHERE config->>'site' = 'SIEGE';

-- Modifier (sans réécrire tout le document à la main)
UPDATE capteurs
SET config = jsonb_set(config, '{seuil_alerte}', '75')
WHERE code = 'SONDE-01';

-- Index GIN pour les recherches dans le JSON
CREATE INDEX idx_capteurs_config ON capteurs USING gin (config);
```

---

## 33. Types réseau inet/cidr : requêtes IP natives

```sql
CREATE TABLE equipements (
  id  bigint GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
  nom text NOT NULL,
  ip  inet NOT NULL
);
CREATE INDEX idx_equipements_ip ON equipements USING gist (ip inet_ops);

-- Tous les équipements du /24
SELECT * FROM equipements WHERE ip << inet '10.0.1.0/24';

-- Le réseau contenant une IP
SELECT * FROM equipements WHERE inet '10.0.1.0/24' >> ip;

-- Trier par adresse IP (ordre numérique, pas lexical !)
SELECT nom, ip FROM equipements ORDER BY ip;
```

---

## 34. Dates et heures : timestamptz sans piège

```sql
-- Toujours timestamptz : stocké en UTC
SHOW timezone;                 -- ex. 'Europe/Paris'
SET timezone = 'UTC';

SELECT now(),                    -- timestamptz : instant absolu
       CURRENT_DATE,             -- date selon TimeZone
       now() AT TIME ZONE 'Europe/Paris' AS heure_paris;

-- Plages et intervalles
SELECT * FROM interventions
WHERE debut >= now() - INTERVAL '7 days';

-- Tronquer à l'heure / au jour (agrégations de supervision)
SELECT date_trunc('hour', horodatage) AS heure,
       avg(valeur) AS moyenne
FROM mesures
GROUP BY 1 ORDER BY 1;

-- Générer une série temporelle (boucher les trous d'un graphe)
SELECT g.h AS heure, count(m.id) AS nb_mesures
FROM generate_series(
       date_trunc('day', now() - INTERVAL '1 day'),
       date_trunc('day', now()),
       INTERVAL '1 hour'
     ) g(h)
LEFT JOIN mesures m
  ON date_trunc('hour', m.horodatage) = g.h
GROUP BY 1 ORDER BY 1;
```

---

## 35. Import/export CSV : COPY et \copy

```bash
# Export côté serveur (superuser, fichier SUR le serveur)
sudo -u postgres psql -d inventaire -c \
  "COPY (SELECT * FROM mesures WHERE horodatage > now() - INTERVAL '1 day')
   TO '/var/lib/postgresql/export/mesures.csv' WITH (FORMAT csv, HEADER);"

# Import/export côté client (droits du user local, recommandé)
psql -h srv-bdd -U app_ecrivain -d inventaire \
  -c "\copy capteurs(code, site_id) FROM 'capteurs.csv' WITH (FORMAT csv, HEADER)"

# Gros import : désactiver temporairement les index/triggers est rarement
# utile ; préférez COPY (déjà très rapide) + maintenance_work_mem élevée
```

**Options `WITH` utiles :** `FORMAT csv`, `HEADER`, `DELIMITER ';'`,
`QUOTE '"'`, `ENCODING 'UTF8'`, `NULL '\N'`.

---

# Partie D — Performance : index, EXPLAIN, VACUUM, transactions

## 36. Index B-tree : le défaut qui couvre 90 % des cas

```sql
-- Index simple sur une colonne de filtrage/jointure
CREATE INDEX idx_mesures_capteur ON mesures (capteur_id);

-- Index composite : ordre = ordre des filtres (égalité d'abord)
CREATE INDEX idx_mesures_capteur_date ON mesures (capteur_id, horodatage DESC);

-- Index unique (contrainte + accès)
CREATE UNIQUE INDEX uq_capteurs_code ON capteurs (code);

-- Création sans bloquer les écritures (prod !)
CREATE INDEX CONCURRENTLY idx_mesures_site ON mesures (site_id);
```

**Règles :**

- Indexez les **clés étrangères** (sinon chaque DELETE/UPDATE du parent
  fait un seq scan de l'enfant).
- `CREATE INDEX CONCURRENTLY` en production : pas de verrou exclusif,
  mais 2× plus long et non utilisable dans un bloc transactionnel.
- Un index inutile coûte : écritures ralenties + espace + maintenance.

```sql
-- Trouver les index jamais utilisés (candidats à la suppression)
SELECT schemaname, relname, indexrelname, idx_scan
FROM pg_stat_user_indexes
WHERE idx_scan = 0 AND indexrelname NOT LIKE '%pkey%'
ORDER BY pg_relation_size(indexrelid) DESC;
```

---

## 37. Index partiels, d'expression et couvrants

```sql
-- Index PARTIEL : seulement les lignes qui nous intéressent (petit, rapide)
CREATE INDEX idx_alertes_actives ON supervision.alertes (site_id)
  WHERE resolue = false;

-- Index D'EXPRESSION : recherche insensible à la casse, fonctions
CREATE INDEX idx_capteurs_code_lower ON capteurs (lower(code));

-- Index COUVRANT (INCLUDE) : index-only scan, sans toucher la table
CREATE INDEX idx_mesures_couvrant
  ON mesures (capteur_id, horodatage DESC)
  INCLUDE (valeur);

-- Vérifier qu'une requête utilise l'index partiel : le WHERE doit
-- impliquer la même condition (resolue = false)
EXPLAIN SELECT * FROM supervision.alertes
WHERE resolue = false AND site_id = 3;
```

---

## 38. GIN, GiST, BRIN : les index spécialisés

| Type | Usage | Exemple |
|---|---|---|
| B-tree | Égalités, plages, tri (défaut) | `WHERE id = 42`, `ORDER BY date` |
| GIN | JSONB, tableaux, recherche plein texte | `config @> '{...}'`, `to_tsvector` |
| GiST | Géométrie, plages, `inet` | PostGIS, `ip << '10.0.0.0/8'` |
| BRIN | **Grosses tables** ordonnées physiquement (séries temporelles) | `horodatage` sur des milliards de lignes |
| Hash | Égalités simples (rarement mieux que B-tree) | Cas spécifiques |

```sql
-- GIN trigramme : recherche floue (LIKE '%...%') rapide
CREATE EXTENSION IF NOT EXISTS pg_trgm;
CREATE INDEX idx_sites_nom_trgm ON sites USING gin (nom gin_trgm_ops);
SELECT * FROM sites WHERE nom ILIKE '%atel%';

-- BRIN : minuscule, parfait pour les mesures horodatées insérées en ordre
CREATE INDEX idx_mesures_horodatage_brin ON mesures USING brin (horodatage);

-- Plein texte : tsvector + GIN
ALTER TABLE interventions ADD COLUMN doc_tsv tsvector
  GENERATED ALWAYS AS (to_tsvector('french', description)) STORED;
CREATE INDEX idx_interventions_fts ON interventions USING gin (doc_tsv);
SELECT * FROM interventions
WHERE doc_tsv @@ plainto_tsquery('french', 'onduleur batterie');
```

> ⚠️ `pg_trgm` et le FTS : pensez à la **configuration de langue**
> (`'french'`) pour la pertinence en français.

---

## 39. EXPLAIN : lire un plan d'exécution

```sql
EXPLAIN
SELECT * FROM mesures WHERE capteur_id = 42 ORDER BY horodatage DESC LIMIT 100;
```

```
Limit  (cost=8.45..8.47 rows=100 ...)
  ->  Sort  (cost=8.45..8.70 ...)
        Sort Key: horodatage DESC
        ->  Index Scan using idx_mesures_capteur_date on mesures
              Index Cond: (capteur_id = 42)
```

**Vocabulaire du plan :**

| Nœud | Signification |
|---|---|
| `Seq Scan` | Balayage complet : OK sur petite table, suspect sur grosse |
| `Index Scan` | Parcours d'index + accès table |
| `Index Only Scan` | Tout dans l'index (idéal, cf. `INCLUDE`) |
| `Bitmap Heap Scan` | Combine plusieurs index, puis accès table triés |
| `Nested Loop` | Boucle imbriquée : bien si la table interne est petite/indexée |
| `Hash Join` / `Merge Join` | Jointures sur gros volumes |
| `Sort` | Tri explicite : coûteux sans index |

**`cost=`** : unité arbitraire (coût d'une page disque = 1).
**`rows=`** : estimation — si elle est fausse d'un facteur 10+,
lancez `ANALYZE` (statistiques périmées, section 43).

---

## 40. EXPLAIN ANALYZE : le plan réel, avec temps

```sql
EXPLAIN (ANALYZE, BUFFERS, TIMING OFF)
SELECT s.nom, avg(m.valeur)
FROM mesures m JOIN capteurs c ON c.id = m.capteur_id
JOIN sites s ON s.id = c.site_id
WHERE m.horodatage > now() - INTERVAL '1 day'
GROUP BY s.nom;
```

**Options :**

| Option | Apport |
|---|---|
| `ANALYZE` | Exécute vraiment la requête (⚠️ `INSERT/UPDATE/DELETE` modifient !) |
| `BUFFERS` | Pages lues : `shared hit` (cache) vs `read` (disque) |
| `TIMING OFF` | Moins de surcharge de mesure sur requêtes rapides |
| `VERBOSE` | Détail des colonnes/filtres |

**Lecture :**

- `actual time=... rows=... loops=...` : temps réel par boucle.
- Écart `rows` estimé vs réel → `ANALYZE` ou statistiques ciblées.
- `Buffers: shared hit=... read=...` : beaucoup de `read` = disque
  sollicité → `shared_buffers` / index / `effective_cache_size`.
- Un `Seq Scan` avec `Rows Removed by Filter: 999000` crie "index manquant".

> ⚠️ `EXPLAIN ANALYZE` **exécute** la requête : pour un `DELETE`,
> encapsulez dans une transaction avec `ROLLBACK`.

```sql
BEGIN;
EXPLAIN (ANALYZE) DELETE FROM _tmp_import WHERE importe_le < now() - INTERVAL '1 an';
ROLLBACK;
```

---

## 41. Optimisation : la méthode en 7 étapes

1. **Mesurer** : `log_min_duration_statement = 1000` + `pg_stat_statements`
   (section 61) pour trouver les coupables, pas les suspects.
2. **EXPLAIN ANALYZE** sur la requête réelle, avec les vrais paramètres.
3. **Vérifier les estimations** : `rows` estimé vs réel → `ANALYZE`,
   voire `ALTER TABLE ... ALTER COLUMN ... SET STATISTICS 1000`.
4. **Index manquant ?** `Seq Scan` + gros `Rows Removed by Filter`.
5. **Index inutilement large ?** `Index Scan` qui lit 50 % de la table →
   filtre plus sélectif ou partitionnement.
6. **Réécrire** : `NOT IN` → `NOT EXISTS`, CTE matérialisée ou non,
   `OR` → `UNION`, `DISTINCT ON` vs `GROUP BY`.
7. **Re-mesurer** : le gain doit être visible sur `EXPLAIN ANALYZE`
   **et** sur la charge réelle (`pg_stat_statements`).

**Anti-patterns fréquents :**

```sql
-- ❌ Fonction sur colonne indexée : tue l'index
WHERE date_trunc('day', horodatage) = CURRENT_DATE
-- ✅ Plage : index utilisable
WHERE horodatage >= date_trunc('day', now())
  AND horodatage <  date_trunc('day', now()) + INTERVAL '1 day';

-- ❌ SELECT * puis tri applicatif
-- ✅ Ne sélectionner que les colonnes utiles + LIMIT + ORDER BY indexé

-- ❌ N+1 : une requête par ligne dans l'appli
-- ✅ Une seule requête avec JOIN / IN
```

---

## 42. Partitionnement : quand et comment

**Quand :** tables > ~10–50 Go, données à durée de vie (rétention),
requêtes filtrant sur la clé de partition (souvent le temps).

```sql
-- Table partitionnée par mois sur horodatage (déclaratif, PG 10+)
CREATE TABLE mesures (
  id          bigint GENERATED ALWAYS AS IDENTITY,
  capteur_id  bigint NOT NULL,
  horodatage  timestamptz NOT NULL,
  valeur      double precision NOT NULL,
  PRIMARY KEY (id, horodatage)          -- la clé de partition fait partie de la PK
) PARTITION BY RANGE (horodatage);

-- Partitions mensuelles
CREATE TABLE mesures_2026_09 PARTITION OF mesures
  FOR VALUES FROM ('2026-09-01') TO ('2026-10-01');
CREATE TABLE mesures_2026_10 PARTITION OF mesures
  FOR VALUES FROM ('2026-10-01') TO ('2026-11-01');

-- Index locaux (créés sur la table mère -> propagés)
CREATE INDEX ON mesures (capteur_id, horodatage DESC);

-- Purge d'un mois : instantané, sans VACUUM
ALTER TABLE mesures DETACH PARTITION mesures_2026_06;
DROP TABLE mesures_2026_06;
```

**Automatisation :** extension `pg_partman` pour créer les partitions
futures et purger les anciennes. **Prérequis :** la contrainte de
partition doit apparaître dans les requêtes (partition pruning).

---

## 43. ANALYZE et les statistiques du planificateur

```sql
-- Statistiques manuelles après un gros chargement
ANALYZE mesures;
ANALYZE VERBOSE mesures;

-- Augmenter l'échantillonnage sur une colonne très sélective
ALTER TABLE mesures ALTER COLUMN capteur_id SET STATISTICS 1000;
ANALYZE mesures;

-- Voir les stats d'une colonne
SELECT attname, n_distinct, most_common_vals, correlation
FROM pg_stats WHERE tablename = 'mesures' AND attname = 'capteur_id';
```

- `default_statistics_target = 100` (500 depuis PG 16) : compromis
  temps d'ANALYZE / qualité des plans.
- Après un `COPY` massif ou un changement de distribution : `ANALYZE`.
- L'autovacuum lance `ANALYZE` automatiquement (seuils, section 44).

---

## 44. VACUUM et autovacuum : le poumon de PostgreSQL

Le MVCC laisse des **versions mortes** : `VACUUM` les recycle,
`ANALYZE` met à jour les stats, et les deux libèrent de l'espace.

```sql
-- Manuel (rarement nécessaire si l'autovacuum est sain)
VACUUM (VERBOSE, ANALYZE) mesures;

-- VACUUM FULL : réécrit la table, verrou exclusif -> maintenance uniquement
VACUUM (FULL, VERBOSE) _tmp_import;
```

**Paramètres autovacuum (postgresql.conf) :**

```ini
autovacuum = on                        # ne JAMAIS couper en prod
autovacuum_max_workers = 3
autovacuum_naptime = 60s
autovacuum_vacuum_threshold = 50
autovacuum_vacuum_scale_factor = 0.1    # VACUUM si > 10 % de lignes modifiées
autovacuum_analyze_scale_factor = 0.05
autovacuum_vacuum_cost_delay = 2ms      # throttle ; 0 = agressif
```

**Réglage par table** (tables à fort churn : mesures, logs) :

```sql
ALTER TABLE mesures SET (
  autovacuum_vacuum_scale_factor = 0.02,
  autovacuum_analyze_scale_factor = 0.01
);
```

**Surveillance :**

```sql
-- Tables qui ont le plus besoin d'un VACUUM
SELECT relname, n_dead_tup, last_vacuum, last_autovacuum
FROM pg_stat_user_tables
ORDER BY n_dead_tup DESC LIMIT 10;

-- Âge des transactions (wraparound) : ALERTE si > 1 milliard
SELECT c.oid::regclass AS table,
       age(c.relfrozenxid) AS age_xid
FROM pg_class c JOIN pg_namespace n ON n.oid = c.relnamespace
WHERE n.nspname NOT IN ('pg_catalog','information_schema')
ORDER BY age_xid DESC LIMIT 10;
```

---

## 45. Transactions et niveaux d'isolation

```sql
BEGIN;
UPDATE onduleurs SET puissance_kva = 120 WHERE reference = 'UPS-042';
-- ... autres opérations ...
COMMIT;      -- ou ROLLBACK;
```

| Niveau | Lecture sale | Lecture non répétable | Fantômes |
|---|---|---|---|
| `READ COMMITTED` (défaut) | Non | Possible | Possible |
| `REPEATABLE READ` | Non | Non | Possible* |
| `SERIALIZABLE` | Non | Non | Non |

\* En pratique PG évite aussi les fantômes en `REPEATABLE READ`
via les snapshots.

```sql
-- Choisir le niveau
BEGIN ISOLATION LEVEL REPEATABLE READ;
-- ...
COMMIT;

-- Timeout anti-blocage (à mettre par rôle/appli !)
SET statement_timeout = '30s';
SET lock_timeout = '5s';
SET idle_in_transaction_session_timeout = '1min';
```

**Bonnes pratiques :**

- Transactions **courtes** : jamais d'appel réseau/fichier au milieu.
- `idle_in_transaction_session_timeout` en production : tue les
  transactions oubliées qui bloquent l'autovacuum.
- `SERIALIZABLE` : uniquement si la logique métier l'exige (erreurs
  de sérialisation à gérer par retry applicatif).

---

## 46. Verrous : diagnostiquer les blocages

```sql
-- Qui bloque qui, maintenant
SELECT blocked.pid AS pid_bloque,
       blocking.pid AS pid_bloquant,
       blocked.query AS requete_bloquee,
       blocking.query AS requete_bloquante,
       blocked.wait_event_type, blocked.wait_event
FROM pg_stat_activity blocked
JOIN pg_stat_activity blocking
  ON blocking.pid = ANY (pg_blocking_pids(blocked.pid))
WHERE blocked.wait_event_type = 'Lock';

-- Requêtes en attente de verrou depuis longtemps
SELECT pid, now() - xact_start AS duree, wait_event, query
FROM pg_stat_activity
WHERE wait_event_type = 'Lock'
ORDER BY xact_start;

-- Tueur de dernier recours (préférez comprendre d'abord !)
SELECT pg_terminate_backend(12345);  -- doux d'abord : pg_cancel_backend()
```

**Chaîne classique :** un `ALTER TABLE` attend un `ACCESS EXCLUSIVE`
derrière une longue transaction `idle in transaction` → tout s'empile.
**Prévention :** `lock_timeout`, `idle_in_transaction_session_timeout`,
migrations hors heures de pointe.

---

## 47. Checklist performance "revue mensuelle"

- [ ] Top 10 `pg_stat_statements` par temps total : EXPLAIN des pires
- [ ] Index jamais utilisés (`idx_scan = 0`) : supprimer après 1 mois
- [ ] `n_dead_tup` élevé persistant : régler l'autovacuum par table
- [ ] Âge XID max < 500 M (alerte à 1 Md)
- [ ] `log_min_duration_statement` : relire les lentes de la semaine
- [ ] Espace disque : tables + index qui grossissent anormalement
- [ ] `seq_scan` vs `idx_scan` par table (`pg_stat_user_tables`)

---

# Partie E — Exploitation : sauvegardes, PITR, réplication, haute dispo

## 48. Stratégie de sauvegarde : les 3 niveaux

| Niveau | Outil | Granularité | Restauration |
|---|---|---|---|
| Logique | `pg_dump` / `pg_dumpall` | Base ou cluster (SQL) | Flexible, lente sur gros volumes |
| Physique à chaud | `pg_basebackup` | Cluster entier (fichiers) | Rapide, base du PITR |
| Continue (PITR) | Archivage WAL + base | **À un instant T** | RPO ~ minutes |

**Règle 3-2-1 :** 3 copies, 2 supports différents, 1 hors site.
**Règle d'or :** une sauvegarde **non testée** = pas de sauvegarde.
Automatisez une restauration test mensuelle (section 78).

```bash
# Arborescence recommandée (serveur de sauvegarde dédié)
/srv/sauvegardes/postgresql/
├── logique/      # pg_dump quotidiens, rotation 14 j
├── physique/     # pg_basebackup hebdo, rotation 4 sem
└── wal/          # archives WAL continues (PITR)
```

---

## 49. pg_dump / pg_restore : la sauvegarde logique

```bash
# Dump d'une base au format custom (compressé, parallélisable à la restauration)
pg_dump -h srv-bdd -U sauvegarde -Fc -f /srv/sauvegardes/postgresql/logique/inventaire-$(date +%F).dump inventaire

# Dump parallèle (jobs = cœurs) : bien plus rapide
pg_dump -h srv-bdd -U sauvegarde -Fd -j 4 \
  -f /srv/sauvegardes/postgresql/logique/inventaire-$(date +%F)/ inventaire

# Dump des rôles et droits globaux (à part ! pg_dump ne les inclut pas)
pg_dumpall -h srv-bdd -U sauvegarde --globals-only \
  -f /srv/sauvegardes/postgresql/logique/globals-$(date +%F).sql

# Restauration
pg_restore -h srv-restore -U postgres -d inventaire --clean --if-exists \
  /srv/sauvegardes/postgresql/logique/inventaire-2026-09-26.dump

# Restauration parallèle
pg_restore -j 4 -h srv-restore -U postgres -d inventaire /chemin/dump-dir/
```

**Formats :**

| Format | Option | Avantages |
|---|---|---|
| custom | `-Fc` | Compressé, `pg_restore` sélectif (`-t table`, `-l/-L`) |
| directory | `-Fd` | Parallèle dump **et** restore |
| plain (SQL) | par défaut | Lisible, rejouable via `psql`, pas de parallélisme |
| tar | `-Ft` | Déconseillé (limité) |

**Bonnes pratiques :**

- Rôle `sauvegarde` dédié : `GRANT CONNECT`, `SELECT` sur les tables
  (ou appartenance à `pg_read_all_data`).
- Toujours dumper `--globals-only` à part (rôles, droits).
- Testez la restauration, pas seulement le dump (section 78).

---

## 50. pg_basebackup : la base physique à chaud

```bash
# Sauvegarde physique complète (prérequis : wal_level=replica, rôle REPLICATION)
pg_basebackup -h srv-bdd -U replicateur -D /srv/sauvegardes/postgresql/physique/base-$(date +%F) \
  -Ft -z -P --wal-method=stream

# Options clés
# -Ft -z   : tar + gzip
# -P       : progression
# --wal-method=stream : inclut le WAL nécessaire à la cohérence
# -R       : écrit la conf de réplication (pour cloner un standby)
```

**Prérequis serveur (primaire) :**

```ini
# postgresql.conf
wal_level = replica
max_wal_senders = 5
max_replication_slots = 5
```

```sql
-- Rôle de réplication (pas de superuser nécessaire)
CREATE ROLE replicateur WITH REPLICATION LOGIN PASSWORD '...';
```

```
# pg_hba.conf : autoriser la réplication depuis le standby/outil
host replication replicateur 10.0.2.10/32 scram-sha-256
```

---

## 51. PITR : archivage WAL et restauration à un instant T

Le **PITR** (Point-In-Time Recovery) rejoue le WAL archivé sur une base
physique : restauration à **n'importe quel instant** couvert.

### 51.1. Configurer l'archivage (primaire)

```ini
# postgresql.conf
archive_mode = on
archive_command = 'test ! -f /srv/sauvegardes/postgresql/wal/%f && cp %p /srv/sauvegardes/postgresql/wal/%f'
archive_timeout = 600   # force un segment toutes les 10 min max
```

```sql
-- Vérifier que l'archivage fonctionne
SELECT * FROM pg_stat_archiver;
-- failed_count doit rester à 0 ; last_failed_time NULL
```

> ⚠️ `archive_command` doit **échouer** si la copie échoue (sinon PG
> croit le segment archivé). Le `test ! -f` évite d'écraser.
> Surveillez l'espace : un archivage en panne fait **grossir pg_wal**
> jusqu'à remplir le disque (alerte section 63).

### 51.2. Restaurer à un instant T (procédure)

```bash
# 1. Arrêter le cluster cible (serveur de restauration isolé !)
sudo pg_ctlcluster 17 main stop

# 2. Vider PGDATA et restaurer la base physique
rm -rf /var/lib/postgresql/17/main/*
tar -xzf /srv/sauvegardes/postgresql/physique/base-2026-09-20.tar.gz \
  -C /var/lib/postgresql/17/main/

# 3. Fichier de recovery : rejouer le WAL jusqu'à l'instant voulu
cat > /var/lib/postgresql/17/main/postgresql.auto.conf <<'EOF'
restore_command = 'cp /srv/sauvegardes/postgresql/wal/%f %p'
recovery_target_time = '2026-09-26 14:30:00+02'
recovery_target_action = 'promote'
EOF
touch /var/lib/postgresql/17/main/recovery.signal
chown -R postgres:postgres /var/lib/postgresql/17/main

# 4. Démarrer : PG rejoue le WAL jusqu'à 14h30 puis promeut
sudo pg_ctlcluster 17 main start
# Suivre : tail -f /var/log/postgresql/postgresql-17-main.log
# Message attendu : "recovery stopping before ... consistent recovery state reached"

# 5. Vérifier les données, puis OUVRIR aux applis (jamais avant)
```

**Cibles de restauration :** `recovery_target_time`, `recovery_target_xid`,
`recovery_target_lsn`, `recovery_target_name` (point nommé via
`pg_create_restore_point('avant_migration')`).

> ⚠️ Restaurez **toujours** sur un serveur isolé d'abord. Une fois promu,
> le standby restauré a une nouvelle timeline : il ne peut pas redevenir
> standby de l'ancien primaire sans reconstruction.

---

## 52. Barman : l'archivage WAL industrialisé

**Barman** (pgBarman) centralise : base backups + WAL streaming +
rétention + restauration en une commande.

```bash
# Installation (serveur de sauvegarde dédié)
sudo apt install -y barman

# /etc/barman.d/srv-bdd.conf
[srv-bdd]
description = "Primaire production"
conninfo = host=srv-bdd user=barman dbname=postgres
backup_method = rsync
streaming_archiver = on
slot_name = barman
retention_policy = RECOVERY WINDOW OF 14 DAYS
wal_retention_policy = MAIN
```

```bash
# Vérifier la conf, créer le slot, lancer la première sauvegarde
barman check srv-bdd
barman receive-wal --create-slot srv-bdd
barman backup srv-bdd

# Restaurer la dernière sauvegarde + PITR à un instant T
barman recover --target-time "2026-09-26 14:30:00+02" \
  srv-bdd latest /var/lib/postgresql/17/main/

# Lister / purger selon la rétention
barman list-backup srv-bdd
barman cron   # à mettre en cron : maintenance + rétention
```

**Pourquoi Barman plutôt que des scripts maison :** slots de réplication
dédiés (pas de perte de WAL), `barman check` diagnostique, restauration
testable en une commande, politiques de rétention déclaratives.

---

## 53. Réplication streaming : primaire + standby

```bash
# 1. Sur le primaire : rôle + pg_hba (voir section 50), puis slot
sudo -u postgres psql -c \
  "SELECT pg_create_physical_replication_slot('standby1');"

# 2. Sur le futur standby : cloner depuis le primaire (cluster arrêté)
sudo pg_ctlcluster 17 main stop
sudo -u postgres rm -rf /var/lib/postgresql/17/main/*
sudo -u postgres pg_basebackup -h srv-primaire -U replicateur \
  -D /var/lib/postgresql/17/main -P -R --slot=standby1

# 3. Démarrer le standby : -R a créé standby.signal + primary_conninfo
sudo pg_ctlcluster 17 main start

# 4. Vérifier des deux côtés
# Primaire :
sudo -u postgres psql -c \
  "SELECT client_addr, state, sync_state, replay_lag FROM pg_stat_replication;"
# Standby (lecture seule) :
sudo -u postgres psql -c "SELECT pg_is_in_recovery();"  -- t -> c'est un standby
```

**Usages du standby :**

- Requêtes **lecture seule** (reporting) : `hot_standby = on` (défaut).
- Sauvegardes déportées (`pg_basebackup` depuis le standby).
- Bascule en cas de panne (section 54).

**Retard de réplication (lag) :**

```sql
-- Sur le primaire : lag par standby
SELECT client_addr, state,
       pg_size_pretty(pg_wal_lsn_diff(pg_current_wal_lsn(), replay_lsn)) AS lag
FROM pg_stat_replication;
```

> ⚠️ Un standby en retard + `max_standby_archive_delay`/`max_standby_streaming_delay`
> trop courts = requêtes annulées sur le standby ("canceling statement
> due to conflict"). Pour du reporting lourd, augmentez ces délais ou
> utilisez `hot_standby_feedback = on` (au prix d'un risque de bloat
> sur le primaire).

---

## 54. Failover manuel : promouvoir le standby

Procédure de bascule planifiée (ou d'urgence si le primaire est mort).

```bash
# --- BASCULE PLANIFIÉE (sans perte) ---
# 1. Couper les écritures applicatives (maintenance applicative)
# 2. Attendre la synchronisation complète
sudo -u postgres psql -h srv-primaire -c \
  "SELECT pg_size_pretty(pg_wal_lsn_diff(pg_current_wal_lsn(), replay_lsn))
   FROM pg_stat_replication WHERE client_addr = '10.0.2.11';"
# -> 0 bytes : synchronisé

# 3. Promouvoir le standby
sudo -u postgres pg_ctlcluster 17 main promote
# ou : SELECT pg_promote();

# 4. Vérifier
sudo -u postgres psql -c "SELECT pg_is_in_recovery();"  -- f : primaire

# 5. Repointer les applis (DNS, VIP, ou chaîne de connexion)
# 6. Reconstruire l'ancien primaire comme standby (pg_basebackup inversé)
```

```bash
# --- BASCULE D'URGENCE (primaire injoignable) ---
# 1. S'assurer que le primaire est VRAIMENT mort (pas de split-brain !)
#    STONITH / arrêt électrique si doute.
# 2. Promouvoir (perte = WAL non répliqué)
sudo -u postgres pg_ctlcluster 17 main promote
# 3. Repointer les applis, vérifier les données critiques
# 4. Quand le primaire revient : le RECONSTRUIRE comme standby
```

**Anti-split-brain :** un seul écrivain à la fois. En manuel : procédure
écrite + checklist. En automatique : Patroni (section 55) avec DCS
(etcd/Consul).

---

## 55. Patroni : tour d'horizon de la haute disponibilité auto

**Patroni** orchestre le failover automatique via un magasin distribué
(DCS : etcd le plus courant).

```
        ┌─────────┐
        │  etcd ×3 │  <- quorum : qui est le leader ?
        └────┬────┘
     ┌───────┼────────┐
┌────┴───┐ ┌─┴────┐ ┌──┴─────┐
│ PG n1  │ │PG n2 │ │ PG n3  │  <- Patroni pilote PG sur chaque nœud
│leader  │ │sync  │ │async   │
└────────┘ └──────┘ └────────┘
     ▲ HAProxy / VIP : route les écritures vers le leader
```

**Concepts clés :**

| Concept | Rôle |
|---|---|
| Leader | Le seul primaire (écritures) |
| Synchronous standby | Accusé de réception synchrone (`synchronous_standby_names`) |
| DCS (etcd) | Élection, verrou du leader, anti-split-brain |
| `patronictl` | CLI : `list`, `failover`, `switchover`, `pause` |

```bash
# Exemples patronictl
patronictl -c /etc/patroni.yml list
patronictl -c /etc/patroni.yml switchover --master pg1 --candidate pg2
```

**Quand l'adopter :** à partir du moment où le RTO manuel (15–60 min)
ne suffit plus. **Coût :** 3 nœuds etcd + 2–3 nœuds PG + expertise.
Pour un premier palier, un standby + procédure de failover manuel
**testée** (section 54) bat un Patroni mal maîtrisé.

---

## 56. Réplication logique : le cas d'usage ciblé

Contrairement au streaming (tout le cluster), la réplication logique
réplique des **tables choisies**, entre versions parfois différentes.

```sql
-- Émetteur (primaire)
CREATE PUBLICATION pub_mesures FOR TABLE mesures, capteurs;

-- Récepteur (autre cluster, voire autre version majeure)
CREATE SUBSCRIPTION sub_mesures
  CONNECTION 'host=srv-primaire dbname=inventaire user=replicateur'
  PUBLICATION pub_mesures;
```

**Usages :** migration de version majeure sans coupure longue,
consolidation multi-sites, alimentation d'un entrepôt. **Limites :**
pas de DDL répliqué, séquences non synchronisées, pas de `TRUNCATE`
(par défaut), surcharge sur l'émetteur.

---

## 57. Sauvegardes : rôles, chiffrement, rotation

```bash
# Rôle minimal pour les sauvegardes logiques
CREATE ROLE sauvegarde WITH LOGIN PASSWORD '...';
GRANT CONNECT ON DATABASE inventaire TO sauvegarde;
GRANT USAGE ON SCHEMA public TO sauvegarde;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO sauvegarde;
ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO sauvegarde;
-- Pour pg_basebackup / streaming WAL :
ALTER ROLE sauvegarde WITH REPLICATION;

# Chiffrer les dumps avant envoi hors site (gpg, clé d'équipe)
gpg --encrypt --recipient sauvegardes@entreprise.lan \
  inventaire-2026-09-26.dump

# Rotation simple : garder 14 jours de dumps logiques
find /srv/sauvegardes/postgresql/logique/ -name '*.dump' -mtime +14 -delete
```

**Checklist sauvegarde :**

- [ ] Dump logique quotidien + `--globals-only`
- [ ] Base physique hebdo + WAL archivés en continu (PITR)
- [ ] Rétention écrite et appliquée (automatique)
- [ ] Chiffrement avant sortie du site
- [ ] **Restauration testée** mensuellement (section 78)
- [ ] Supervision : échec de dump/WAL → alerte immédiate

---

## 58. Plan de reprise : RTO/RPO et qui fait quoi

| Scénario | Restauration | RTO indicatif | RPO indicatif |
|---|---|---|---|
| Table supprimée par erreur | PITR à T-5 min (serveur isolé) | 30–60 min | ~0 |
| Disque données mort | Base physique + WAL sur nouveau disque | 1–2 h | minutes |
| Primaire mort | Failover standby (manuel) | 15–30 min | secondes–minutes |
| Cluster corrompu | Barman `recover` + PITR | 1–3 h | minutes |
| Incendie salle | Sauvegardes hors site | 4–24 h | 24 h (logique) |

**Documentez :** qui décide la bascule, l'ordre des étapes, les contacts,
les chaînes de connexion par environnement. **Testez** chaque scénario
au moins une fois par an (game day).

---

# Partie F — Tuning, monitoring, alertes

## 59. Tuning mémoire : les 4 paramètres qui comptent

| Paramètre | Point de départ | Contexte | Effet |
|---|---|---|---|
| `shared_buffers` | 25 % RAM (max ~8–16 Go utiles) | `postmaster` (restart) | Cache PG |
| `effective_cache_size` | 50–75 % RAM | `sighup` | Aide le planificateur (pas d'allocation) |
| `work_mem` | 4–64 Mo selon charge | `user` (par session !) | Tris/hachages par opération |
| `maintenance_work_mem` | 256 Mo–1 Go | `user` | VACUUM, CREATE INDEX |

```ini
# Exemple : serveur dédié 32 Go RAM, ~100 connexions
shared_buffers = 8GB
effective_cache_size = 24GB
work_mem = 32MB
maintenance_work_mem = 1GB
wal_buffers = 16MB          # auto-tuning depuis PG 13 si à -1
```

**Mises en garde :**

- `work_mem` est **par opération et par connexion** : 100 connexions ×
  10 tris × 64 Mo = 64 Go potentiels → OOM. Restez conservateur,
  augmentez **par requête** si besoin (`SET LOCAL work_mem = '256MB';`).
- `shared_buffers` au-delà de ~16 Go apporte rarement un gain
  (le cache OS fait le reste) et allonge les checkpoints.
- Ne touchez qu'**un paramètre à la fois**, mesurez avant/après
  (`pg_stat_statements`, temps de requêtes représentatives).

---

## 60. Tuning écritures : checkpoints et WAL

```ini
# Checkpoints espacés et étalés : moins de pics d'I/O
checkpoint_timeout = 15min
max_wal_size = 8GB            # 4 Go par défaut ; 8-16 Go sur serveur chargé
min_wal_size = 2GB
checkpoint_completion_target = 0.9

# WAL
wal_compression = on          # LZ4/Zstd (PG 15+) : moins d'I/O d'archivage
wal_buffers = 16MB
```

**Symptômes d'un mauvais réglage :**

- `LOG: checkpoints are occurring too frequently` → augmentez
  `max_wal_size`.
- Pics d'I/O réguliers corrélés aux checkpoints → augmentez
  `checkpoint_completion_target` (0.9) et vérifiez le stockage.

---

## 61. pg_stat_statements : l'extension indispensable

```bash
# 1. Activer (postgresql.conf) puis redémarrer
# shared_preload_libraries = 'pg_stat_statements'   # postmaster !
# pg_stat_statements.track = all
```

```sql
-- 2. Créer l'extension dans chaque base à superviser
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;

-- 3. Top 10 des requêtes par temps total
SELECT query, calls, round(total_exec_time::numeric,2) AS total_ms,
       round(mean_exec_time::numeric,2) AS moyenne_ms,
       round((100*total_exec_time/sum(total_exec_time) OVER ())::numeric,1) AS pct
FROM pg_stat_statements
ORDER BY total_exec_time DESC LIMIT 10;

-- 4. Remise à zéro (après un tuning, pour mesurer le gain)
SELECT pg_stat_statements_reset();
```

**À surveiller :** `mean_exec_time` élevé + `calls` élevé (les pires),
`shared_blks_read` élevé (disque), `temp_blks_written` (tris sur disque →
`work_mem` ?). Pensez à la rétention : les stats sont en mémoire,
perdues au redémarrage (persistez via export régulier si besoin).

---

## 62. Autovacuum : réglage fin en production

Rappel section 44. Compléments production :

```ini
# Serveur avec tables à fort churn (mesures, logs)
autovacuum_max_workers = 4
autovacuum_naptime = 30s
# Ne pas mettre cost_delay à 0 globalement ; préférez le réglage par table :
```

```sql
-- Table très écrite : vacuum agressif, sans impacter les autres
ALTER TABLE mesures SET (
  autovacuum_vacuum_scale_factor = 0.01,
  autovacuum_vacuum_cost_delay = 0,
  autovacuum_analyze_scale_factor = 0.005
);

-- Table quasi statique : vacuum paresseux
ALTER TABLE sites SET (
  autovacuum_vacuum_scale_factor = 0.2,
  autovacuum_enabled = true
);

-- Vérifier l'activité autovacuum en cours
SELECT pid, query_start, query
FROM pg_stat_activity
WHERE backend_type = 'autovacuum worker';
```

---

## 63. Les vues pg_stat_* : panorama

| Vue | Contenu |
|---|---|
| `pg_stat_activity` | Sessions en cours (requêtes, attentes, états) |
| `pg_stat_database` | Stats par base (commits, rollbacks, conflits) |
| `pg_stat_user_tables` | Seq scan, index scan, `n_dead_tup`, dernier vacuum |
| `pg_stat_user_indexes` | Utilisation des index (`idx_scan`) |
| `pg_stat_statements` | Requêtes agrégées (extension) |
| `pg_stat_replication` | État des standbys (primaire) |
| `pg_stat_archiver` | Santé de l'archivage WAL |
| `pg_stat_bgwriter` | Checkpoints, écritures background |
| `pg_stat_io` (PG 16+) | I/O par backend/type d'objet |
| `pg_stat_ssl` | Sessions chiffrées ou non |
| `pg_stat_progress_vacuum` | Progression des VACUUM en cours |
| `pg_locks` | Verrous détenus/demandés |

```sql
-- Rôle lecture seule pour la supervision (principe du moindre privilège)
CREATE ROLE supervision WITH LOGIN PASSWORD '...';
GRANT pg_monitor TO supervision;   -- PG 10+ : accès aux vues de supervision
```

---

## 64. Requêtes de supervision prêtes à l'emploi

```sql
-- 1. Sessions actives et leur état
SELECT pid, usename, application_name, state,
       now() - query_start AS duree, left(query, 80)
FROM pg_stat_activity WHERE state <> 'idle' ORDER BY query_start;

-- 2. Requêtes les plus longues en ce moment
SELECT pid, now() - query_start AS duree, wait_event_type, wait_event,
       left(query, 120)
FROM pg_stat_activity
WHERE state = 'active' AND now() - query_start > INTERVAL '1 minute'
ORDER BY query_start;

-- 3. Connexions par base et par état
SELECT datname, state, count(*)
FROM pg_stat_activity GROUP BY datname, state ORDER BY datname;

-- 4. Taux de hit du cache (viser > 99 %)
SELECT datname,
       round(100.0 * blks_hit / nullif(blks_hit + blks_read, 0), 2) AS hit_ratio
FROM pg_stat_database;

-- 5. Tables les plus lues en séquentiel (candidats index)
SELECT relname, seq_scan, idx_scan,
       round(100.0 * seq_scan / nullif(seq_scan + idx_scan, 0), 1) AS pct_seq
FROM pg_stat_user_tables
WHERE seq_scan + idx_scan > 1000
ORDER BY seq_scan DESC LIMIT 10;

-- 6. Taille des bases, schémas, tables
SELECT datname, pg_size_pretty(pg_database_size(datname))
FROM pg_database ORDER BY pg_database_size(datname) DESC;
SELECT relname, pg_size_pretty(pg_total_relation_size(oid)) AS total,
       pg_size_pretty(pg_relation_size(oid)) AS table,
       pg_size_pretty(pg_indexes_size(oid)) AS index
FROM pg_class WHERE relkind = 'r' AND relnamespace = 'public'::regnamespace
ORDER BY pg_total_relation_size(oid) DESC LIMIT 10;

-- 7. Espace disque et WAL
SELECT pg_size_pretty(pg_wal_lsn_diff(pg_current_wal_lsn(), '0/0')) AS wal_genere_depuis_zero;
SHOW data_directory;
-- Shell : du -sh /var/lib/postgresql/17/main/pg_wal
```

---

## 65. Alertes : seuils recommandés

| Alerte | Seuil | Gravité |
|---|---|---|
| Espace disque PGDATA < 15 % libre | critique à 10 % | 🔴 |
| `pg_wal` grossit anormalement | > 2× `max_wal_size` | 🔴 |
| Archivage WAL en échec | `failed_count` > 0 | 🔴 |
| Âge XID max > 800 M | critique à 1 Md | 🔴 |
| Connexions > 80 % `max_connections` | | 🟠 |
| Lag réplication > 5 min (ou > seuil métier) | | 🟠 |
| Requête active > 15 min (hors batch déclaré) | | 🟠 |
| Hit ratio cache < 95 % durable | | 🟠 |
| Échec sauvegarde (dump/base/WAL) | | 🔴 |
| Certificat SSL expire < 30 j | | 🟠 |
| Autovacuum worker bloqué / `n_dead_tup` explose | | 🟠 |

```bash
# Exemple : check Nagios-like minimal (à brancher sur votre supervision)
#!/bin/bash
# check_pg_wal_archiver.sh
FAILED=$(sudo -u postgres psql -tAc "SELECT failed_count FROM pg_stat_archiver;")
[ "$FAILED" -gt 0 ] && { echo "CRITICAL: archivage WAL en echec"; exit 2; }
echo "OK: archivage WAL fonctionnel"; exit 0
```

**Intégration :** exposez ces requêtes via `check_postgres` (Nagios),
exporter Prometheus (`postgres_exporter`), ou Zabbix templates
PostgreSQL. Une alerte sans **runbook** (que faire ?) ne sert à rien :
liez chaque alerte à la section de dépannage correspondante.

---

## 66. Journaux : les exploiter vraiment

```bash
# Requêtes lentes du jour (log_min_duration_statement = 1000)
grep "duration:" /var/log/postgresql/postgresql-17-main.log | \
  sort -t: -k... # ou utilisez pgbadger :

# pgbadger : rapport HTML des logs (recommandé, hebdo)
sudo apt install -y pgbadger
pgbadger /var/log/postgresql/postgresql-17-main.log -o /tmp/rapport.html

# Centralisation : expédiez vers rsyslog / Loki / ELK
# postgresql.conf :
# log_destination = 'stderr'  (puis rsyslog imfile) ou 'csvlog' pour parsing
```

**À logger en production :** `log_checkpoints`, `log_connections`,
`log_disconnections`, `log_lock_waits`, `log_min_duration_statement`,
`log_autovacuum_min_duration = '1s'`, `log_temp_files = 0`
(fichiers temporaires = tris sur disque, à traquer).

---

## 67. Maintenance planifiée : le calendrier type

| Fréquence | Tâche |
|---|---|
| Quotidien | Dump logique + vérif. archivage WAL + espace disque |
| Hebdo | Base physique (`pg_basebackup` ou Barman) + rapport pgbadger |
| Mensuel | Revue `pg_stat_statements` + index inutilisés + test de restauration |
| Trimestriel | Revue tuning (charge réelle vs paramètres) + certificats SSL |
| Annuel | Game day PRA + montée de version mineure planifiée |

```bash
# Exemple de cron (serveur de sauvegarde)
# /etc/cron.d/postgresql-sauvegardes
0 2 * * * postgres /usr/local/bin/dump_logique.sh
0 3 * * 0 postgres /usr/local/bin/base_physique.sh
*/5 * * * * postgres /usr/local/bin/check_archivage.sh
```

---

## 68. Dimensionner le stockage : méthode

1. **Volume initial** : taille des données × 1.5 (index ≈ 30–50 % des tables).
2. **Croissance** : taux mensuel mesuré (`pg_database_size` historisé).
3. **WAL** : `max_wal_size` + marge archivage + `wal_keep_size` si slots.
4. **Marge** : 20 % libre minimum en permanence.
5. **Séparer** : données / WAL / sauvegardes / logs.

```sql
-- Historiser la taille pour la courbe de croissance (à mettre en cron)
CREATE TABLE IF NOT EXISTS supervision.tailles (
  mesure_le timestamptz NOT NULL DEFAULT now(),
  base text NOT NULL, taille bigint NOT NULL
);
INSERT INTO supervision.tailles (base, taille)
SELECT datname, pg_database_size(datname) FROM pg_database WHERE datistemplate = false;
```

---

# Partie G — Mise à jour, dépannage, erreurs classiques

## 69. Mise à jour mineure vs majeure

| Type | Exemple | Procédure | Risque |
|---|---|---|---|
| Mineure | 17.4 → 17.5 | `apt upgrade` + restart | Faible (même format de données) |
| Majeure | 17 → 18 | `pg_upgrade` ou dump/restore | Élevé si mal préparé |

```bash
# --- MINEURE ---
sudo apt update && sudo apt install -y postgresql-17
sudo pg_ctlcluster 17 main restart -m fast
sudo -u postgres psql -c "SELECT version();"

# --- MAJEURE avec pg_upgrade (méthode rapide, sans rechargement) ---
# 1. Installer la nouvelle version SANS supprimer l'ancienne
sudo apt install -y postgresql-18
# 2. Arrêter les deux clusters
sudo pg_ctlcluster 17 main stop -m fast
sudo pg_ctlcluster 18 main stop
sudo pg_dropcluster 18 main --stop   # supprime le cluster vide créé par défaut
# 3. Contrôle de compatibilité (à blanc)
sudo -u postgres /usr/lib/postgresql/18/bin/pg_upgrade \
  --old-datadir=/var/lib/postgresql/17/main \
  --new-datadir=/var/lib/postgresql/18/main \
  --old-bindir=/usr/lib/postgresql/17/bin \
  --new-bindir=/usr/lib/postgresql/18/bin \
  --old-options '-c config_file=/etc/postgresql/17/main/postgresql.conf' \
  --new-options '-c config_file=/etc/postgresql/18/main/postgresql.conf' \
  --check
# 4. Migration réelle (avec --link = instantané, ou copie = sûr mais long)
sudo -u postgres /usr/lib/postgresql/18/bin/pg_upgrade \
  ... --link
# 5. Démarrer, vérifier, puis basculer les applis
sudo pg_ctlcluster 18 main start
./analyze_new_cluster.sh   # script généré par pg_upgrade : ANALYZE
./delete_old_cluster.sh    # UNIQUEMENT après validation complète
```

**Checklist mise à jour majeure :**

- [ ] Sauvegarde complète **testée** avant
- [ ] `--check` sans erreur (extensions compatibles !)
- [ ] Extensions : `postgis`, `pg_stat_statements`… à jour pour la cible
- [ ] Fenêtre de maintenance + plan de retour arrière (l'ancien cluster
      reste intact avec `--link`… tant qu'on ne le supprime pas)
- [ ] `ANALYZE` sur le nouveau cluster (stats non migrées)
- [ ] Tests applicatifs non-régression avant bascule DNS/VIP

> ⚠️ Alternative sans `pg_upgrade` : réplication logique
> (section 56) 17 → 18 avec bascule applicative = coupure quasi nulle,
> au prix d'une préparation plus lourde.

---

## 70. Dépannage : méthode générale

1. **Qualifier** : panne totale ? lenteurs ? erreurs applicatives ?
   Depuis quand ? Qu'est-ce qui a changé (déploiement, batch, volume) ?
2. **Logs d'abord** : `/var/log/postgresql/`, `journalctl -u`.
3. **Métriques** : `pg_stat_activity`, espace disque, charge CPU/RAM.
4. **Hypothèse unique**, action unique, vérification.
5. **Documenter** : cause racine + remédiation dans le journal d'exploitation.

```bash
# Kit de survie : les 5 commandes à lancer en premier
pg_lsclusters                                          # le cluster tourne ?
df -h /var/lib/postgresql /var/log/postgresql          # disque plein ?
sudo -u postgres psql -c "SELECT count(*) FROM pg_stat_activity;"  # connexions ?
sudo -u postgres psql -c "SELECT * FROM pg_stat_archiver;"         # WAL archivé ?
tail -n 100 /var/log/postgresql/postgresql-17-main.log # erreurs récentes ?
```

---

## 71. Cas concret 1 : « FATAL: remaining connection slots are reserved »

**Symptômes :** les applis ne se connectent plus, `FATAL: too many clients`.

**Diagnostic :**

```sql
-- D'abord se connecter en superuser (slots réservés : superuser_reserved_connections)
SELECT state, count(*) FROM pg_stat_activity GROUP BY state;
-- Souvent : des centaines de 'idle' -> fuites de connexions applicatives
SELECT pid, usename, application_name, now() - state_change AS depuis,
       left(query, 60)
FROM pg_stat_activity WHERE state = 'idle' ORDER BY state_change LIMIT 10;
```

**Remédiation :**

1. Court terme : `SELECT pg_terminate_backend(pid)` sur les `idle`
   les plus anciens (après accord applicatif).
2. Moyen terme : `idle_in_transaction_session_timeout`,
   `statement_timeout` par rôle.
3. Fond : **PgBouncer** en mode transaction devant PG ; corriger le
   pool applicatif (fermeture des connexions).

---

## 72. Cas concret 2 : disque plein à cause du WAL

**Symptômes :** `PANIC: could not write to file "pg_wal/..."`, cluster arrêté.

**Diagnostic :**

```bash
du -sh /var/lib/postgresql/17/main/pg_wal
sudo -u postgres psql -c "SELECT * FROM pg_stat_archiver;"
# failed_count > 0 -> archive_command en panne (disque dest plein, droits...)
sudo -u postgres psql -c "SELECT slot_name, active, pg_size_pretty(pg_wal_lsn_diff(pg_current_wal_lsn(), restart_lsn)) AS retenu FROM pg_replication_slots;"
# slot inactif qui retient des Go de WAL -> standby mort non supprimé
```

**Remédiation :**

1. Réparer `archive_command` (espace/droits/réseau) → PG reprend l'archivage.
2. Slot mort : si le standby est abandonné,
   `SELECT pg_drop_replication_slot('standby_mort');` (⚠️ le standby
   devra être reconstruit).
3. Redémarrer, vérifier la reprise des checkpoints.

**Prévention :** alerte sur `pg_stat_archiver.failed_count` et sur la
taille de `pg_wal` (section 65).

---

## 73. Cas concret 3 : table énorme et requêtes soudain lentes

**Symptômes :** une requête habituellement rapide devient lente du jour
au lendemain, sans changement de code.

**Diagnostic :**

```sql
EXPLAIN (ANALYZE, BUFFERS) SELECT ... ;  -- la requête incriminée
-- Si rows estimé << rows réel :
SELECT last_analyze, last_autoanalyze, n_mod_since_analyze
FROM pg_stat_user_tables WHERE relname = 'mesures';
-- Si n_dead_tup énorme :
SELECT relname, n_live_tup, n_dead_tup FROM pg_stat_user_tables ORDER BY n_dead_tup DESC LIMIT 5;
```

**Causes fréquentes :**

1. Statistiques périmées après un gros chargement → `ANALYZE`.
2. Bloat : autovacuum dépassé → `VACUUM (VERBOSE, ANALYZE)` manuel puis
   réglage par table (section 62).
3. Plan qui a basculé (distribution changée) → statistiques ciblées
   `SET STATISTICS`, voire `pg_hint_plan` en dernier recours.

---

## 74. Cas concret 4 : « deadlock detected »

```sql
-- Le log indique les deux requêtes et les verrous :
-- ERROR: deadlock detected
-- DETAIL: Process 1234 waits for ShareLock on transaction 5678; blocked by process 4321.
```

**Cause :** deux transactions verrouillent les mêmes lignes dans un
ordre différent. **Remédiation :**

1. Toujours verrouiller **dans le même ordre** (ex. `ORDER BY id`
   avant `SELECT ... FOR UPDATE`).
2. Transactions courtes (section 45).
3. Côté appli : **retry** sur `deadlock_detected` (erreur 40P01) —
   c'est un incident normal et récupérable, pas un bug PG.

---

## 75. 10+ erreurs classiques et leur solution

| # | Erreur / symptôme | Cause probable | Solution |
|---|---|---|---|
| 1 | `FATAL: password authentication failed` | Mauvais mdp / pg_hba `md5` vs `scram` | Vérifier `pg_hba.conf` + `password_encryption` (sec. 16–17) |
| 2 | `FATAL: no pg_hba.conf entry for host...` | Règle manquante | Ajouter la ligne `host`, `reload` |
| 3 | `FATAL: remaining connection slots...` | Saturation connexions | Sec. 71 : pooler, timeouts |
| 4 | `ERROR: permission denied for table X` | `GRANT` manquant ou `DEFAULT PRIVILEGES` oubliés | Sec. 19 |
| 5 | `ERROR: relation "x" does not exist` | `search_path` / schéma / casse (`"MaTable"`) | Qualifier `schema.table`, vérifier `\dt` |
| 6 | `canceling statement due to conflict with recovery` | Standby en retard vs requêtes longues | `max_standby_*_delay`↑ ou `hot_standby_feedback` (sec. 53) |
| 7 | `PANIC: could not write to file pg_wal` | Disque plein / archivage HS | Sec. 72 |
| 8 | `WARNING: oldest xmin is far in the past` | Longue transaction bloque le vacuum | Tuer la transaction, `idle_in_transaction_session_timeout` |
| 9 | `ERROR: deadlock detected` | Ordre de verrouillage incohérent | Sec. 74 |
| 10 | `FATAL: the database system is in recovery mode` | Standby en lecture seule / recovery en cours | Normal sur standby ; sinon vérifier les logs |
| 11 | `ERROR: duplicate key value violates unique constraint` | UPSERT manquant / race applicative | `ON CONFLICT DO NOTHING/UPDATE` (sec. 28) |
| 12 | `could not serialize access due to concurrent update` | `SERIALIZABLE`/`REPEATABLE READ` + concurrence | Retry applicatif |
| 13 | Montée de version : `pg_upgrade --check` échoue | Extension incompatible | MAJ l'extension d'abord (sec. 69) |

---

## 76. Corruption : prévention et conduite à tenir

**Prévention :**

- Stockage fiable (pas de NFS pour PGDATA), `full_page_writes = on`
  (ne jamais couper), onduleur + arrêt propre (lien avec votre
  expertise UPS : un arrêt brutal répété fragilise tout).
- `data_checksums` activé à `initdb` (`--data-checksums`) : détecte
  les corruptions silencieuses disque.

```bash
# Vérifier si les checksums sont actifs
sudo -u postgres psql -tAc "SHOW data_checksums;"
```

**Conduite à tenir si corruption suspectée :**

1. **Stopper les écritures**, sauvegarder l'état (copie physique du
   PGDATA, même corrompu).
2. `pg_dump` ce qui est lisible ; restaurer sur un cluster neuf.
3. Restaurer depuis la sauvegarde physique + PITR **avant** la corruption
   si le dump échoue.
4. Ne jamais `VACUUM FULL` / `REINDEX` à l'aveugle sur un cluster
   corrompu : ça peut aggraver.

> ⚠️ La corruption est rare avec du matériel sain. 90 % des
> « corruptions » remontées sont en réalité du disque plein, des
> droits fichiers cassés, ou deux postmasters sur le même PGDATA.

---

# Partie H — Cas pratiques, pense-bête, glossaire, quiz

## 77. Cas pratique 1 : dimensionner PostgreSQL pour une appli

**Contexte :** application de supervision énergie : 500 capteurs,
1 mesure/capteur/minute, rétention 2 ans, 50 utilisateurs, pics
de lecture le matin.

**1. Volume :**

```
500 capteurs × 1440 mesures/jour = 720 000 lignes/jour
≈ 263 M lignes/an → ~530 M lignes / 2 ans
Ligne ≈ 40 octets + index ≈ 60 octets → ~50–60 Go pour 2 ans
```

**2. Partitionnement** (section 42) : table `mesures` partitionnée par
mois → purge par `DETACH` + requêtes filtrées par temps (pruning).

**3. Serveur :**

| Ressource | Choix | Justification |
|---|---|---|
| RAM | 32 Go | `shared_buffers` 8 Go, cache OS pour les lectures du matin |
| CPU | 8 vCPU | Pics d'agrégation ; parallélisme (`max_parallel_workers_per_gather = 4`) |
| Disque données | 500 Go SSD | 60 Go × marge de croissance 3 ans + index + marge 20 % |
| Disque WAL | 100 Go dédiés | `max_wal_size` 8 Go + archivage |
| Réseau | 10 Gb/s si réplication + sauvegardes | `pg_basebackup` sans impacter la prod |

**4. Paramètres clés :** `shared_buffers=8GB`, `effective_cache_size=24GB`,
`work_mem=32MB`, `maintenance_work_mem=1GB`, `checkpoint_completion_target=0.9`,
`autovacuum` agressif sur `mesures` (scale_factor 0.02).

**5. Exploitation :** standby streaming (reporting + bascule),
Barman (base hebdo + WAL continu, RPO ~ minutes), dump logique
quotidien, `pg_stat_statements` actif, alertes section 65.

---

## 78. Cas pratique 2 : diagnostiquer une requête lente (pas à pas)

**Ticket :** « Le dashboard énergie met 12 s à charger le matin. »

```sql
-- Étape 1 : identifier la requête (pg_stat_statements)
SELECT query, calls, round(mean_exec_time::numeric) AS moy_ms,
       round(total_exec_time::numeric) AS total_ms
FROM pg_stat_statements
ORDER BY total_exec_time DESC LIMIT 5;
-- Coupable : agrégation journalière sur mesures sans filtre de partition

-- Étape 2 : EXPLAIN ANALYZE avec les vrais paramètres
EXPLAIN (ANALYZE, BUFFERS)
SELECT date_trunc('day', horodatage) AS jour, avg(valeur)
FROM mesures
WHERE capteur_id IN (SELECT id FROM capteurs WHERE site_id = 5)
  AND horodatage >= '2026-09-01' AND horodatage < '2026-09-26'
GROUP BY 1;
-- Constat : Seq Scan sur 40 M de lignes, "Rows Removed by Filter: 39900000"
-- + Sort sur disque (temp files)

-- Étape 3 : corriger
-- a) Index manquant sur (site_id) de capteurs ? déjà là.
-- b) La requête ne profite pas du partitionnement : le filtre est sur
--    horodatage -> OK, mais l'index composite manque :
CREATE INDEX CONCURRENTLY idx_mesures_capteur_date
  ON mesures (capteur_id, horodatage DESC);

-- c) Réécrire : éviter la fonction sur la colonne partitionnée
--    (ici date_trunc est dans le SELECT, pas le WHERE : OK)

-- Étape 4 : re-mesurer
-- EXPLAIN ANALYZE -> Bitmap Heap Scan + Index, 12 s -> 0,4 s

-- Étape 5 : pérenniser
-- - ANALYZE après le CREATE INDEX (fait automatiquement par CONCURRENTLY)
-- - Ajouter la requête au dashboard de supervision (pg_stat_statements)
-- - Documenter dans le journal d'exploitation
```

**Leçon :** 80 % des « requêtes lentes » = index manquant ou
statistiques périmées. Les 20 % restants = réécriture ou
partitionnement.

---

## 79. Cas pratique 3 : restaurer à un instant T (scénario réel)

**Incident :** à 14h32, un script supprime par erreur les interventions
du mois (`DELETE FROM interventions WHERE ...` sans garde-fou).

```bash
# 1. STOP : empêcher d'autres écritures qui écraseraient le WAL utile
#    (si possible : passer l'appli en lecture seule)

# 2. Point de restauration : juste avant 14h32
#    Sur le serveur de restauration isolé :
sudo pg_ctlcluster 17 main stop
rm -rf /var/lib/postgresql/17/main/*
barman recover --target-time "2026-09-26 14:31:00+02" \
  srv-bdd latest /var/lib/postgresql/17/main/
sudo pg_ctlcluster 17 main start

# 3. Vérifier : compter les interventions du mois
sudo -u postgres psql -p 5433 -d inventaire -c \
  "SELECT count(*) FROM interventions WHERE debut >= '2026-09-01';"
# -> le chiffre attendu d'avant l'incident : OK

# 4. Réintégrer SEULEMENT les lignes manquantes (pas tout écraser !)
#    Depuis le cluster restauré (port 5433) vers la prod :
pg_dump -p 5433 -d inventaire -t interventions \
  --data-only --inserts | psql -h srv-bdd -d inventaire
# Mieux : dump dans une table _recup puis INSERT ... SELECT anti-doublons :
psql -h srv-bdd -d inventaire <<'EOF'
CREATE TEMP TABLE _recup (LIKE interventions INCLUDING ALL);
\copy _recup FROM 'interventions_recup.csv' WITH (FORMAT csv, HEADER)
INSERT INTO interventions SELECT * FROM _recup r
  WHERE NOT EXISTS (SELECT 1 FROM interventions i WHERE i.id = r.id);
EOF

# 5. Post-mortem : pourquoi le script a-t-il pu faire ça ?
#    -> droits trop larges ? pas de transaction ? pas de backup avant batch ?
#    Actions : rôle batch restreint, REVOKE DELETE direct, dry-run obligatoire
```

---

## 80. Cas pratique 4 : mise en place d'un standby de reporting

**Besoin :** soulager le primaire des requêtes BI du matin.

```bash
# 1. Primaire : slot + pg_hba (sections 50/53 déjà faits)
# 2. Cloner
sudo -u postgres pg_basebackup -h srv-primaire -U replicateur \
  -D /var/lib/postgresql/17/main -P -R --slot=standby_bi
# 3. postgresql.conf du standby : autoriser les requêtes longues
#    max_standby_streaming_delay = 30min   # requêtes BI longues
#    hot_standby_feedback = on             # évite les annulations (risque bloat primaire)
# 4. Démarrer, vérifier le lag (section 53)
# 5. Chaîne de connexion BI -> srv-standby (lecture seule)
# 6. Alerte lag > 10 min (section 65)
```

**Pièges :** le standby n'accepte **aucune** écriture (même les tables
temporaires → utilisez `temp_tablespaces` ou faites-les sur le primaire) ;
les séquences y sont « en retard » (normal : `nextval` non répliqué finement).

---

## 81. Pense-bête de poche

### Connexion

```bash
sudo -u postgres psql
psql -h srv -U user -d base -c "SELECT 1;"
PGPASSWORD='...' psql -h srv -U user -d base   # scripts (ou .pgpass)
```

### psql express

```
\l \du \dn \dt+ \di \d table \x \timing \copy \q
```

### SQL express

```sql
SELECT version();  SHOW config_file;  SHOW data_directory;
SELECT pg_reload_conf();                       -- reload conf
SELECT pg_terminate_backend(pid);              -- tuer une session
CREATE EXTENSION pg_stat_statements;
EXPLAIN (ANALYZE, BUFFERS) SELECT ...;
```

### Sauvegarde / restauration express

```bash
pg_dump -Fc -f base-$(date +%F).dump base
pg_dumpall --globals-only -f globals.sql
pg_basebackup -D /srv/bkp/base -Ft -z -P --wal-method=stream
pg_restore -j 4 -d base base-2026-09-26.dump
barman backup srv-bdd && barman list-backup srv-bdd
```

### Supervision express

```sql
SELECT * FROM pg_stat_archiver;                -- WAL OK ?
SELECT client_addr, state FROM pg_stat_replication;
SELECT pg_is_in_recovery();                    -- t = standby
SELECT datname, pg_size_pretty(pg_database_size(datname)) FROM pg_database;
```

### Fichiers

```
/etc/postgresql/17/main/postgresql.conf
/etc/postgresql/17/main/pg_hba.conf
/var/lib/postgresql/17/main/          (PGDATA)
/var/log/postgresql/
```

### Numéros à connaître

```
5432            port par défaut
25 % RAM        shared_buffers (point de départ)
50-75 % RAM     effective_cache_size
100             max_connections par défaut
```

---

## 82. Glossaire

| Terme | Définition |
|---|---|
| ACID | Atomicité, Cohérence, Isolation, Durabilité : garanties transactionnelles |
| Autovacuum | Processus automatique de nettoyage (VACUUM) et stats (ANALYZE) |
| Bloat | Gonflement d'une table/index par les versions mortes non nettoyées |
| Checkpoint | Point où toutes les pages sales sont écrites sur disque |
| Cluster | Instance PostgreSQL : un postmaster + ses bases + sa conf |
| GUC | Grand Unified Configuration : un paramètre de `postgresql.conf` |
| HOT | Heap-Only Tuple : UPDATE sans nouvelle entrée d'index (optimisation) |
| Lag | Retard d'un standby par rapport au primaire |
| LSN | Log Sequence Number : position dans le WAL |
| MVCC | Contrôle de concurrence multi-version (section 4) |
| PITR | Restauration à un instant T via WAL archivé |
| RLS | Row-Level Security : filtrage des lignes par rôle |
| RPO / RTO | Perte de données max / durée d'indisponibilité max acceptées |
| SCRAM-SHA-256 | Méthode d'authentification par mot de passe moderne |
| Slot (réplication) | Réserve le WAL pour un standby/outil (attention aux slots morts) |
| Split-brain | Deux primaires simultanés : corruption logique assurée |
| Standby | Réplica en lecture seule (streaming) |
| Timeline | Historique d'un cluster après chaque promotion (PITR) |
| TOAST | Stockage externe des grosses valeurs (text/jsonb volumineux) |
| VACUUM | Recyclage des versions mortes + gel des XID |
| WAL | Write-Ahead Log : journal des modifications, base de la réplication et du PITR |
| Wraparound | Dépassement du compteur de transactions : incident critique si l'âge XID explose |
| XID | Identifiant de transaction (surveillance : `age(relfrozenxid)`) |

---

## 83. Quiz : 10 questions pour valider

**Q1.** Pourquoi ne faut-il jamais couper `autovacuum = off` en production ?
**Q2.** Quelle différence entre `\copy` et `COPY` ?
**Q3.** Un `GRANT SELECT ON ALL TABLES IN SCHEMA public` suffit-il pour que
l'appli lise les tables créées demain ? Sinon, que manque-t-il ?
**Q4.** Votre `pg_wal` fait 10× `max_wal_size` : quelles sont les deux
causes les plus probables, et dans quel ordre vérifiez-vous ?
**Q5.** Pourquoi `CREATE INDEX CONCURRENTLY` est-il obligatoire en
production, et quelle est sa contrainte d'usage ?
**Q6.** Un standby affiche `canceling statement due to conflict with
recovery` : expliquez et donnez deux remèdes.
**Q7.** Quelle est la différence entre une mise à jour mineure (17.4→17.5)
et majeure (17→18) en termes de procédure ?
**Q8.** Pourquoi `timestamptz` plutôt que `timestamp` en production ?
**Q9.** Citez trois paramètres `postgresql.conf` qui exigent un
redémarrage (`context = 'postmaster'`).
**Q10.** Après un `DELETE` accidentel à 14h32, détaillez les grandes étapes
d'une restauration PITR sans écraser la production.

<details>
<summary><b>Réponses</b></summary>

**R1.** Sans autovacuum, les versions mortes du MVCC s'accumulent (bloat),
les statistiques se périment (mauvais plans) et l'âge des XID augmente
jusqu'au **wraparound** (arrêt forcé du cluster). C'est le poumon du moteur.
**R2.** `\copy` s'exécute côté **client** (fichiers de votre poste,
droits de votre user) ; `COPY` s'exécute côté **serveur** (fichiers du
serveur, réservé au superuser / `pg_read_server_files`).
**R3.** Non : `ON ALL TABLES` ne couvre que les tables existantes. Il faut
`ALTER DEFAULT PRIVILEGES IN SCHEMA public GRANT SELECT ON TABLES TO ...`
(et le refaire par rôle créateur si besoin).
**R4.** (1) `archive_command` en échec (`pg_stat_archiver.failed_count` > 0),
(2) slot de réplication inactif qui retient le WAL
(`pg_replication_slots`). Vérifier l'archiver d'abord, puis les slots.
**R5.** `CREATE INDEX` simple prend un verrou `SHARE`/`ACCESS EXCLUSIVE`
qui bloque les écritures ; `CONCURRENTLY` ne bloque pas. Contraintes :
2 passages (plus long), **interdit dans un bloc transactionnel**,
ne peut pas être combiné avec d'autres DDL dans la même transaction.
**R6.** Le standby applique le WAL et doit annuler une requête qui
détiendrait un snapshot trop vieux (`max_standby_streaming_delay`
dépassé). Remèdes : augmenter `max_standby_streaming_delay`, ou
`hot_standby_feedback = on` (au prix d'un possible bloat sur le primaire).
**R7.** Mineure : `apt upgrade` + restart, même format de données.
Majeure : `pg_upgrade` (ou dump/restore, ou réplication logique),
avec `--check`, sauvegarde testée, `ANALYZE` après, plan de retour.
**R8.** `timestamptz` stocke un instant absolu (UTC) et l'affiche selon le
`TimeZone` de la session : pas d'ambiguïté lors des changements
d'heure ni entre sites. `timestamp` (sans fuseau) est ambigu.
**R9.** Exemples : `shared_buffers`, `max_connections`, `port`,
`listen_addresses`, `ssl`, `shared_preload_libraries`, `wal_level`,
`max_wal_senders`, `archive_mode`.
**R10.** 1) Figer les écritures si possible. 2) Restaurer la base physique
sur un serveur **isolé**. 3) `recovery_target_time` juste avant 14h32 +
`restore_command` vers les archives WAL. 4) Démarrer, vérifier les
données. 5) Réintégrer **uniquement** les lignes manquantes
(`INSERT ... SELECT ... WHERE NOT EXISTS`). 6) Post-mortem et durcissement
des droits du batch.
</details>

---

## 84. Pour aller plus loin

**Documentation officielle :**

- https://www.postgresql.org/docs/current/ — la référence absolue,
  par version (remplacez `current` par `17`, `16`, `15`).
- Wiki PostgreSQL : https://wiki.postgresql.org/ (tuning, réplication).

**Outils cités dans ce guide :**

| Outil | Usage |
|---|---|
| PgBouncer | Pool de connexions (au-delà de ~200 connexions) |
| Barman | Sauvegarde physique + WAL + PITR industrialisés |
| Patroni | Failover automatique (avec etcd) |
| pgbadger | Rapports HTML des logs PostgreSQL |
| pg_partman | Partitionnement automatique |
| `check_postgres` / `postgres_exporter` | Supervision Nagios / Prometheus |
| PostGIS | Extension géographique (SIG) |
| pg_trgm | Recherche floue / similarité de texte |

**Livres :**

- *PostgreSQL 17 Administration Cookbook* — Packt (recettes opérationnelles).
- *The Art of PostgreSQL* — Dimitri Fontaine (SQL avancé, modélisation).

**Prochaines étapes conseillées :**

1. Monter un labo : primaire + standby + Barman sur 3 VM Debian.
2. Simuler chaque scénario de la section 58 (game day).
3. Brancher `pg_stat_statements` + exporter Prometheus sur la maquette.
4. Tester `pg_upgrade` 16→17 sur une copie avant la prochaine EOL.

---

## 85. Journal d'exploitation : modèle de fiche incident

```markdown
# Fiche incident — <date> <heure>
## Symptôme
## Détection (alerte ? utilisateur ?)
## Diagnostic (commandes passées, résultats)
## Cause racine
## Actions correctives
## Actions préventives (droits, alertes, doc à jour ?)
## RTO / RPO constatés
```

> 💡 Tenez ce journal dans votre wiki d'équipe. Dans six mois, c'est lui
> qui fera la différence entre « on a déjà vu ça » et une nouvelle
> nuit blanche.

---

*Fin du guide — bon courage, et que vos checkpoints soient espacés
et vos WAL bien archivés.*
