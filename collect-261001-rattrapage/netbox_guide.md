# NetBox — Guide complet : source de vérité IPAM / DCIM

> **Public :** chefs de service systèmes, administrateurs réseau & systèmes, responsables d'infrastructure.
> **Objectif :** installer, configurer, exploiter et maintenir NetBox comme référentiel unique
> (source de vérité) de l'infrastructure : adressage IP (IPAM), baies et équipements (DCIM),
> câblage, alimentation, circuits opérateurs, virtualisation.
> **Versions couvertes :** NetBox 4.x (4.0 → 4.2+), Debian 12/13, Ubuntu 22.04/24.04 LTS,
> PostgreSQL 15/16, Redis 7.
> **Avertissement :** les commandes et chemins correspondent à NetBox 4.x. Les versions 2.x/3.x
> diffèrent sur plusieurs points (Python 3.8+, arborescence, plugins). Vérifiez toujours la
> [documentation officielle](https://docs.netbox.dev) pour votre version exacte.

---

## Sommaire

- Concepts : source de vérité, IPAM, DCIM
- Installation pas à pas (Debian/Ubuntu : PostgreSQL + Redis + NetBox + gunicorn + nginx + systemd)
- Configuration (`configuration.py`)
- Premier lancement et organisation (tenants, régions, sites)
- DCIM : baies, types d'équipements, équipements, interfaces, câblage, alimentation (PDU/onduleurs), circuits
- IPAM : VRF, préfixes, plages IP, adresses IP, VLAN
- Virtualisation, utilisateurs, permissions, sécurité, HTTPS
- API REST (curl + Python), webhooks, scripts et rapports personnalisés
- Intégrations : Ansible (inventaire dynamique), Zabbix (export)
- Sauvegarde (pg_dump + media), restauration, montée de version, supervision
- Dépannage : cas concrets et 10+ erreurs classiques
- Bonnes pratiques (nommage, tenancy, « documenter avant de câbler »)
- Cas pratiques commentés (site complet : baie, onduleur, switchs, serveurs)
- Pense-bête de poche, glossaire, quiz, pour aller plus loin

**Convention de ce guide :**

| Marqueur | Signification |
|---|---|
| `netbox` (shell) | commande exécutée en tant qu'utilisateur `netbox` |
| `root` / `sudo` | commande avec privilèges administrateur |
| `CHANGEZ_MOI_...` | valeur d'exemple à remplacer — jamais utilisée telle quelle en production |
| ⚠️ | point de vigilance (sécurité, perte de données) |

---

## 1. NetBox, c'est quoi ? La « source de vérité »

NetBox est une application web open source (licence Apache 2.0, éditée par NetBox Labs)
qui centralise la documentation d'une infrastructure réseau et datacenter. On l'appelle
**source de vérité** (*source of truth*) : c'est l'endroit unique où l'on consigne
« ce qui existe vraiment » — chaque adresse IP, chaque équipement, chaque câble,
chaque baie.

Sans source de vérité, l'information vit dans des tableurs éparpillés, des têtes
d'administrateurs et des post-it : c'est la garantie des conflits d'IP, des baies
« pleines » qui ne le sont pas, et des interventions à l'aveugle. Avec NetBox :

- **Un seul référentiel** consultable par toute l'équipe (et par les outils, via l'API).
- **Cohérence garantie** : NetBox refuse les doublons (deux fois la même IP, deux fois
  le même nom d'équipement dans un site).
- **Traçabilité** : chaque création/modification/suppression est journalisée (qui, quand, quoi).
- **Automatisation** : Ansible, Zabbix, scripts Python lisent NetBox au lieu de fichiers statiques.

> 💡 **Pour un chef de service systèmes** : NetBox devient le socle de vos procédures
> d'exploitation. « C'est documenté dans NetBox » remplace « demande à Karim, il sait ».
> C'est aussi un formidable outil de management : l'état du parc est visible, mesurable,
> auditable.

---

## 2. Les deux grands modules : IPAM et DCIM

| Module | Sigle | Contenu | Exemples d'objets |
|---|---|---|---|
| **IPAM** | IP Address Management | Tout l'adressage logique | VRF, préfixes, plages IP, adresses IP, VLAN, ASN |
| **DCIM** | Data Center Infrastructure Management | Tout le physique | Régions, sites, baies, équipements, interfaces, câbles, alimentations, circuits |

Les deux modules sont **liés** : une adresse IP (IPAM) est assignée à une interface
d'un équipement (DCIM), elle-même installée dans une baie d'un site. Cette liaison
logique ↔ physique est la vraie force de NetBox : depuis une IP, on remonte au
serveur, à sa baie, à son site, à son PDU et à son onduleur.

Autres modules utiles au quotidien :

- **Circuits** : liaisons opérateurs/FAI (type de circuit, débit, identifiants).
- **Virtualisation** : clusters, machines virtuelles, interfaces virtuelles.
- **Tenancy** : tenants (clients, entités, départements) pour cloisonner la documentation.
- **Secrets** : ⚠️ le module natif a été retiré ; utilisez un gestionnaire dédié (Vault).

---

## 3. Architecture technique de NetBox 4.x

```
                  ┌─────────────┐
                  │   Nginx     │  :443 HTTPS (reverse proxy)
                  └──────┬──────┘
                         │ proxy HTTP local
                  ┌──────▼──────┐
                  │  Gunicorn   │  serveurs WSGI Python (netbox.service)
                  └──────┬──────┘
                         │
            ┌────────────┼────────────┐
            ▼            ▼            ▼
     ┌────────────┐ ┌────────┐ ┌────────────┐
     │ PostgreSQL │ │ Redis  │ │  Fichiers  │
     │  (données) │ │(cache,  │ │  media/    │
     │            │ │ files)   │ │(scripts,   │
     └────────────┘ └────────┘ │ rapports)  │
                               └────────────┘
```

- **PostgreSQL** : base de données relationnelle, TOUTE la donnée métier y vit.
  NetBox 4.x exige PostgreSQL 12 minimum (15/16 recommandés).
- **Redis** : cache applicatif + file d'attente des tâches (cacheops, django-rq).
- **Gunicorn** : serveur d'application Python qui exécute le code Django de NetBox.
- **Nginx** : reverse proxy en frontal (TLS, en-têtes, fichiers statiques).
- **systemd** : supervise `netbox.service` (gunicorn) et `netbox-rq.service` (travailleurs de tâches).

> ⚠️ **Sauvegarde = PostgreSQL + dossier `media/`**. Redis est un cache : il se reconstruit
> seul, inutile de le sauvegarder.

---

## 4. Prérequis matériels et dimensionnement

| Taille d'infrastructure | vCPU | RAM | Disque | Remarques |
|---|---|---|---|---|
| Lab / PME (< 500 équipements) | 2 | 4 Go | 40 Go | Une seule VM suffit |
| Moyenne (500–5 000 équipements) | 4 | 8 Go | 100 Go | PostgreSQL sur disque rapide |
| Grande (> 5 000, API intensive) | 8 | 16 Go | 200 Go+ | Séparer PostgreSQL si besoin |

- **OS** : Debian 12/13 ou Ubuntu 22.04/24.04 LTS, installation minimale, à jour.
- **Réseau** : IP fixe, nom DNS interne (ex. `netbox.lan-entreprise.fr`), accès HTTPS.
- **Python** : 3.10 minimum pour NetBox 4.x (3.12 recommandé). Ne JAMAIS utiliser le Python
  système pour autre chose que ce qu'il faut : NetBox tourne dans un **virtualenv** dédié.

Checklist avant de commencer :

- [ ] Serveur/VM provisionné, OS à jour (`apt update && apt full-upgrade`)
- [ ] Nom d'hôte (FQDN) configuré, résolution DNS directe et inverse OK
- [ ] Accès SSH par clé, compte `sudo` fonctionnel
- [ ] Flux réseau : 443 entrant (utilisateurs), 22 (admin) ; sortant : dépôts APT, PyPI si besoin
- [ ] Sauvegarde de la VM planifiée (snapshot initial avant l'installation)

---

## 5. Préparation du serveur Debian/Ubuntu

Toutes les commandes ci-dessous sont exécutées en `root` (ou via `sudo`).

```bash
# Mise à jour du système
apt update && apt full-upgrade -y

# Paquets de base
apt install -y sudo curl wget git vim htop net-tools dnsutils

# Dépendances système de NetBox 4.x (Debian 12/13, Ubuntu 22.04/24.04)
apt install -y python3 python3-pip python3-venv python3-dev \
    build-essential libxml2-dev libxslt1-dev libffi-dev libpq-dev \
    libssl-dev zlib1g-dev

# Fuseau horaire (important pour les journaux et les tâches planifiées)
timedatectl set-timezone Europe/Paris
timedatectl status
```

Créez l'utilisateur système dédié (NetBox ne tourne JAMAIS en root) :

```bash
# Groupe + utilisateur sans shell interactif inutile, avec home
adduser --system --group --home /opt/netbox netbox
# Sur Ubuntu, équivalent :
# adduser --system --disabled-password --group --home /opt/netbox netbox
```

> 💡 **Bonne pratique** : un utilisateur système par application. Les fichiers de NetBox
> appartiennent à `netbox:netbox`, jamais à `root`.

---

## 6. Installation et configuration de PostgreSQL

```bash
# Installation (Debian 12 → PostgreSQL 15, Debian 13/Ubuntu 24.04 → 16)
apt install -y postgresql postgresql-contrib

# Démarrage + activation au boot
systemctl enable --now postgresql

# Vérification
pg_lsclusters
psql --version
```

Créez la base et l'utilisateur NetBox. ⚠️ Choisissez un mot de passe robuste et
stockez-le dans votre gestionnaire de mots de passe (ne le notez jamais dans ce guide) :

```bash
sudo -u postgres psql <<'EOF'
CREATE DATABASE netbox;
CREATE USER netbox WITH PASSWORD 'CHANGEZ_MOI_PgSQL_Exemple_2026!';
ALTER DATABASE netbox OWNER TO netbox;
-- Extension utile pour certaines recherches (optionnel)
\c netbox
CREATE EXTENSION IF NOT EXISTS pg_trgm;
EOF
```

Testez la connexion avec l'utilisateur applicatif :

```bash
PGPASSWORD='CHANGEZ_MOI_PgSQL_Exemple_2026!' \
  psql -h localhost -U netbox -d netbox -c 'SELECT version();'
```

Réglages PostgreSQL recommandés pour NetBox (`/etc/postgresql/<version>/main/postgresql.conf`) :

| Paramètre | Valeur conseillée | Pourquoi |
|---|---|---|
| `max_connections` | `200` | gunicorn ouvre plusieurs connexions |
| `shared_buffers` | `25 % de la RAM` | cache principal de PostgreSQL |
| `work_mem` | `8 MB` | tris/hachages des requêtes |
| `listen_addresses` | `localhost` | pas d'écoute réseau inutile si tout est local |

```bash
systemctl restart postgresql
```

---

## 7. Installation et configuration de Redis

NetBox 4.x utilise Redis pour le cache applicatif et les files de tâches (django-rq).
Redis 7 est recommandé.

```bash
apt install -y redis-server

# Sécurisation de base : écoute locale uniquement
sed -i 's/^bind .*/bind 127.0.0.1 ::1/' /etc/redis/redis.conf
# (Optionnel mais recommandé) mot de passe Redis :
# sed -i 's/^# requirepass .*/requirepass CHANGEZ_MOI_Redis_Exemple_2026!/' /etc/redis/redis.conf

systemctl enable --now redis-server
redis-cli ping   # doit répondre : PONG
```

> ⚠️ Si vous activez `requirepass`, répercutez-le dans `configuration.py`
> (`REDIS['password']`), sinon NetBox ne pourra pas se connecter au cache.

Test de persistance (Redis doit survivre à un redémarrage) :

```bash
redis-cli set test_netbox ok
systemctl restart redis-server
redis-cli get test_netbox   # doit répondre : ok
redis-cli del test_netbox
```

---

## 8. Téléchargement et installation de NetBox

Travaillez désormais en tant qu'utilisateur `netbox`. Repérez la dernière version 4.x
sur la [page des releases GitHub](https://github.com/netbox-community/netbox/releases).

```bash
# Passer sous l'utilisateur netbox
sudo -iu netbox
cd /opt/netbox

# Télécharger l'archive de la version voulue (exemple : 4.2.x)
wget https://github.com/netbox-community/netbox/archive/refs/tags/v4.2.3.tar.gz \
     -O netbox.tar.gz
tar -xzf netbox.tar.gz
ln -s netbox-4.2.3 netbox
cd netbox

# Environnement virtuel Python
python3 -m venv /opt/netbox/venv
source /opt/netbox/venv/bin/activate

# Dépendances Python (peut prendre plusieurs minutes)
pip install --upgrade pip
pip install -r requirements.txt
```

Vérifiez que l'installation s'est bien passée :

```bash
python -m netbox --version 2>/dev/null || python netbox/manage.py version
# ou simplement :
/opt/netbox/venv/bin/python -c "import django; print(django.get_version())"
```

> 💡 **Astuce de mise à jour future** : le lien symbolique `/opt/netbox/netbox` pointe
> vers la version active. Pour monter de version, on télécharge la nouvelle archive,
> on bascule le lien, on rejoue les migrations. (Détails : section 66.)

---

## 9. Le fichier `configuration.py` : principes

`configuration.py` est LE fichier de configuration de NetBox. Modèle fourni :
`netbox/netbox/configuration_example.py`.

```bash
# Toujours en utilisateur netbox, dans /opt/netbox/netbox/netbox/
sudo -iu netbox
cd /opt/netbox/netbox/netbox
cp configuration_example.py configuration.py
chmod 600 configuration.py
```

Règles d'or :

1. **Permissions 600** (`netbox:netbox`) : ce fichier contient des secrets (clé Django,
   mots de passe). Personne d'autre ne doit pouvoir le lire.
2. **Versionnez un modèle**, pas le fichier réel : gardez `configuration.py` hors de git,
   ou chiffrez-le (Ansible Vault, SOPS).
3. **Après chaque modification** : `systemctl restart netbox netbox-rq`.
4. **Validez la syntaxe Python** avant de redémarrer :
   `python -m py_compile configuration.py && echo OK`.

Structure minimale d'un `configuration.py` fonctionnel (détaillée sections 10 à 13) :

```python
ALLOWED_HOSTS = ['netbox.lan-entreprise.fr', '10.0.0.10']
DATABASE = { ... }      # PostgreSQL
REDIS = { ... }         # Redis : cache + files d'attente
SECRET_KEY = '...'      # >= 50 caractères aléatoires
```

---

## 10. `configuration.py` : base de données et paramètres vitaux

```python
# --- Hôtes autorisés (anti Host header attack) ---
ALLOWED_HOSTS = [
    'netbox.lan-entreprise.fr',
    '10.10.0.15',          # IP du serveur si accès direct
]

# --- Base de données PostgreSQL ---
DATABASE = {
    'NAME': 'netbox',
    'USER': 'netbox',
    'PASSWORD': 'CHANGEZ_MOI_PgSQL_Exemple_2026!',  # identique à la section 6
    'HOST': 'localhost',
    'PORT': '',            # '' = socket/port par défaut
    'CONN_MAX_AGE': 300,   # connexions persistantes (secondes)
}

# --- Clé secrète Django : GÉNÉREZ-LA, ne l'inventez pas ---
# python3 -c "import secrets; print(secrets.token_urlsafe(50))"
SECRET_KEY = 'CHANGEZ_MOI_Cle_Aleatoire_50_Caracteres_Minimum_Exemple='

# --- Redis : cache + files de tâches ---
REDIS = {
    'tasks': {
        'HOST': 'localhost',
        'PORT': 6379,
        'DATABASE': 0,
        # 'PASSWORD': 'CHANGEZ_MOI_Redis_Exemple_2026!',  # si requirepass activé
        'SSL': False,
    },
    'caching': {
        'HOST': 'localhost',
        'PORT': 6379,
        'DATABASE': 1,
        # 'PASSWORD': 'CHANGEZ_MOI_Redis_Exemple_2026!',
        'SSL': False,
    },
}
```

> ⚠️ **Ne réutilisez JAMAIS la `SECRET_KEY` d'exemple** entre deux installations :
> elle sert à signer les sessions et les jetons. Une clé connue = sessions falsifiables.

---

## 11. `configuration.py` : e-mail, fuseau horaire et divers

```python
# --- E-mail (notifications, réinitialisation de mot de passe) ---
EMAIL = {
    'SERVER': 'smtp.lan-entreprise.fr',
    'PORT': 587,
    'USERNAME': 'netbox@lan-entreprise.fr',
    'PASSWORD': 'CHANGEZ_MOI_SMTP_Exemple_2026!',
    'USE_SSL': False,
    'USE_TLS': True,
    'TIMEOUT': 10,
    'FROM_EMAIL': 'netbox@lan-entreprise.fr',
}

# --- Localisation ---
TIME_ZONE = 'Europe/Paris'
DATE_FORMAT = 'd/m/Y'      # format français dans l'interface
SHORT_DATE_FORMAT = 'd/m/Y'
TIME_FORMAT = 'H:i'

# --- Divers utiles en production ---
ADMINS = [('Exploitation', 'exploitation@lan-entreprise.fr')]
LOGIN_REQUIRED = True          # oblige l'authentification pour TOUT (recommandé)
BANNER_TOP = '⚠ PRODUCTION — NetBox de la DSI — toute modification est tracée'
MAINTENANCE_MODE = False
```

Testez l'envoi d'e-mail après le premier démarrage :

```bash
cd /opt/netbox/netbox/netbox/
source /opt/netbox/venv/bin/activate
python ../manage.py sendtestemail exploitation@lan-entreprise.fr
```

---

## 12. Initialisation : migrations, statiques, superutilisateur

Toujours en utilisateur `netbox`, virtualenv activé, dans `/opt/netbox/netbox` :

```bash
cd /opt/netbox/netbox
source /opt/netbox/venv/bin/activate

# 1. Générer les SECRET_KEY-dependent artefacts + créer les tables
python netbox/manage.py migrate

# 2. Collecter les fichiers statiques (CSS/JS/images de l'interface)
python netbox/manage.py collectstatic --no-input

# 3. Créer le compte administrateur initial
python netbox/manage.py createsuperuser
#   Username: admin
#   Email: exploitation@lan-entreprise.fr
#   Password: <robuste, gestionnaire de mots de passe>

# 4. (Optionnel) charger des jeux de données de démonstration — UNIQUEMENT EN LAB
# python netbox/manage.py loaddata ...

# 5. Vérifier que tout est sain
python netbox/manage.py check
```

> ⚠️ `createsuperuser` n'est à lancer qu'une fois. Si vous perdez le mot de passe admin,
> régénérez-le avec `python netbox/manage.py changepassword admin`.

Ordre impératif : **migrate AVANT le premier démarrage de gunicorn**, sinon l'application
démarre sur une base vide et renvoie des erreurs 500.

---

## 13. Gunicorn : le serveur d'application

NetBox 4.x s'exécute via gunicorn (serveur WSGI). Fichier conseillé :
`/opt/netbox/gunicorn.py` (propriétaire `netbox:netbox`, 640) :

```python
# /opt/netbox/gunicorn.py
bind = '127.0.0.1:8001'      # écoute locale uniquement, nginx fait le frontal
workers = 5                  # règle : (2 x vCPU) + 1 ; ici pour 2 vCPU
worker_class = 'gthread'
threads = 3
timeout = 120                # les exports CSV/API volumineux prennent du temps
max_requests = 1000          # recycle les workers (anti-fuites mémoire)
max_requests_jitter = 100
preload_app = True
user = 'netbox'
group = 'netbox'
loglevel = 'info'
accesslog = '/var/log/netbox/gunicorn-access.log'
errorlog = '/var/log/netbox/gunicorn-error.log'
```

Créez le répertoire de logs :

```bash
mkdir -p /var/log/netbox
chown netbox:netbox /var/log/netbox
chmod 750 /var/log/netbox
```

Test manuel avant de créer le service systemd (doit afficher des logs sans erreur,
`Ctrl+C` pour arrêter) :

```bash
sudo -iu netbox
cd /opt/netbox/netbox
/opt/netbox/venv/bin/gunicorn --config /opt/netbox/gunicorn.py netbox.wsgi
```

---

## 14. Services systemd : `netbox` et `netbox-rq`

Deux services sont nécessaires : l'application web et les travailleurs de tâches
(webhooks, scripts, exports).

`/etc/systemd/system/netbox.service` :

```ini
[Unit]
Description=NetBox WSGI service (gunicorn)
After=network.target postgresql.service redis-server.service
Wants=postgresql.service redis-server.service

[Service]
Type=simple
User=netbox
Group=netbox
WorkingDirectory=/opt/netbox/netbox
ExecStart=/opt/netbox/venv/bin/gunicorn --config /opt/netbox/gunicorn.py netbox.wsgi
ExecReload=/bin/kill -s HUP $MAINPID
Restart=on-failure
RestartSec=5
# Sécurité systemd (durcissement)
NoNewPrivileges=true
PrivateTmp=true
ProtectSystem=strict
ReadWritePaths=/opt/netbox/netbox/netbox/media /var/log/netbox

[Install]
WantedBy=multi-user.target
```

`/etc/systemd/system/netbox-rq.service` :

```ini
[Unit]
Description=NetBox Request Queue worker (django-rq)
After=network.target postgresql.service redis-server.service netbox.service

[Service]
Type=simple
User=netbox
Group=netbox
WorkingDirectory=/opt/netbox/netbox
ExecStart=/opt/netbox/venv/bin/python netbox/manage.py rqworker high default low
Restart=on-failure
RestartSec=5

[Install]
WantedBy=multi-user.target
```

Activation :

```bash
systemctl daemon-reload
systemctl enable --now netbox netbox-rq
systemctl status netbox netbox-rq --no-pager
# Vérifier l'écoute locale :
ss -ltnp | grep 8001
```

> 💡 `rqworker high default low` : trois files de priorité. Les webhooks partent en
> `high`, les scripts/rapports en `default`. Si `netbox-rq` est arrêté, les webhooks
> s'accumulent dans Redis sans être envoyés — pensez-y en dépannage (section 70).

---

## 15. Nginx en reverse proxy (HTTP puis HTTPS)

`/etc/nginx/sites-available/netbox` :

```nginx
upstream netbox {
    server 127.0.0.1:8001;
}

server {
    listen 80;
    server_name netbox.lan-entreprise.fr;

    # Redirection systématique vers HTTPS (après Let's Encrypt, section 16)
    # return 301 https://$host$request_uri;

    client_max_body_size 10M;   # imports CSV, images d'équipements

    location /static/ {
        alias /opt/netbox/netbox/netbox/static/;
    }

    location / {
        proxy_pass http://netbox;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_connect_timeout 30s;
        proxy_read_timeout 120s;
    }
}
```

```bash
ln -s /etc/nginx/sites-available/netbox /etc/nginx/sites-enabled/
nginx -t && systemctl reload nginx
```

Vérifiez : `curl -I http://netbox.lan-entreprise.fr/` doit répondre `200 OK`
(ou `302` vers `/login/` si `LOGIN_REQUIRED = True`).

> ⚠️ Servez TOUJOURS les fichiers `/static/` via nginx, jamais via gunicorn :
> c'est plus rapide et ça évite de saturer les workers Python.

---

## 16. HTTPS avec Let's Encrypt (ou PKI interne)

En production, NetBox contient des données sensibles : **HTTPS obligatoire**.

**Option A — Let's Encrypt (serveur exposé sur Internet) :**

```bash
apt install -y certbot python3-certbot-nginx
certbot --nginx -d netbox.lan-entreprise.fr
# Renouvellement automatique déjà planifié par le paquet ; vérifiez :
certbot renew --dry-run
```

**Option B — PKI interne d'entreprise (cas le plus courant pour Zelef) :**

1. Générez une CSR ou demandez un certificat à votre AD CS / PKI interne pour
   `netbox.lan-entreprise.fr`.
2. Déposez le certificat + la chaîne dans `/etc/nginx/ssl/` (600, `root:root`).

```nginx
server {
    listen 443 ssl;
    server_name netbox.lan-entreprise.fr;

    ssl_certificate     /etc/nginx/ssl/netbox.lan-entreprise.fr.crt;
    ssl_certificate_key /etc/nginx/ssl/netbox.lan-entreprise.fr.key;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;
    ssl_session_cache shared:SSL:10m;

    # En-têtes de sécurité
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
    add_header X-Content-Type-Options nosniff always;
    add_header X-Frame-Options SAMEORIGIN always;

    # ... reprendre les blocs location /static/ et / de la section 15 ...
}

server {
    listen 80;
    server_name netbox.lan-entreprise.fr;
    return 301 https://$host$request_uri;
}
```

Et dans `configuration.py`, quand HTTPS est actif :

```python
SECURE_SSL_REDIRECT = False   # c'est nginx qui redirige, pas Django
SESSION_COOKIE_SECURE = True
CSRF_COOKIE_SECURE = True
```

> ⚠️ Après activation HTTPS, videz les cookies/session de votre navigateur si la
> connexion tourne en boucle : un cookie `Secure` posé en HTTP est ignoré.

---

## 17. Premier lancement : la check-list des 15 premières minutes

Connectez-vous à `https://netbox.lan-entreprise.fr` avec le superutilisateur.

- [ ] La page d'accueil s'affiche (tableau de bord, version en bas de page : 4.x)
- [ ] `Admin > Journaux` : aucune erreur critique
- [ ] Créez un **tenant** « DSI » (Organisation > Tenants) — voir section 18
- [ ] Créez une **région** « France » puis un **site** « Siège » (section 19)
- [ ] Créez un **préfixe** `10.0.0.0/8` avec le statut « Container » (section 36)
- [ ] Créez un **VLAN** de test (section 40)
- [ ] Créez un **token API** pour vous (en haut à droite > API Tokens) et testez :
  `curl -H "Authorization: Token VOTRE_TOKEN" https://netbox.lan-entreprise.fr/api/`
- [ ] Vérifiez la sauvegarde : lancez un `pg_dump` manuel (section 63) et contrôlez le fichier
- [ ] Notez l'URL, les comptes et la procédure dans votre documentation d'exploitation

> 💡 **Règle d'équipe** : personne ne travaille en `admin` au quotidien. Créez
> immédiatement un compte nominatif par administrateur (section 46).

---

## 18. Organisation : tenants et groupes de tenants

Le **tenant** (locataire) est l'entité à qui « appartient » un objet : un client, une
filiale, un département, un projet. C'est l'axe de cloisonnement n°1.

**Cas d'usage typiques :**

| Tenant | Objets rattachés |
|---|---|
| `DSI-Interne` | Baies, serveurs, VLAN et préfixes du SI interne |
| `Client-Dupont` | Équipements et IP dédiés à un client hébergé |
| `Projet-Video` | Caméras, préfixe et VLAN du projet vidéosurveillance |

Création : **Organisation > Tenants > + Ajouter**. Renseignez au minimum le nom et
le slug (généré automatiquement). Les **groupes de tenants** permettent de regrouper
(ex. groupe « Clients hébergés » contenant `Client-Dupont`, `Client-Martin`).

**Bonnes pratiques :**

- Un tenant `Infra-Mutualisee` pour le cœur (cœur de réseau, onduleurs, baies partagées).
- Nommage en `MAJUSCULES-avec-tirets`, sans accents ni espaces : `DSI`, `ATELIER`, `MAGASIN`.
- Attribuez un tenant à CHAQUE objet dès sa création (site, baie, équipement, préfixe,
  VLAN, circuit). Un objet « sans tenant » devient vite inclassable.

---

## 19. DCIM : régions, groupes de sites et sites

Hiérarchie géographique : **Région → (Groupe de sites) → Site**.

- **Région** : zone géographique large. Ex. `Europe`, `France`, `IDF`.
  Les régions peuvent être imbriquées (région parente `Europe` → enfant `France`).
- **Groupe de sites** : regroupement logique. Ex. `Agences`, `Datacenters`, `Usines`.
- **Site** : le lieu physique documenté. Ex. `SIEGE-Paris11`, `AGENCE-Lyon`, `DC-Gravelines`.

Fiche site — champs à remplir systématiquement :

| Champ | Exemple | Pourquoi c'est important |
|---|---|---|
| Nom | `SIEGE-Paris11` | identifiant unique, repris partout |
| Slug | `siege-paris11` | utilisé par l'API |
| Statut | Actif | distingue les sites fermés/en projet |
| Région / Groupe | `France` / `Bureaux` | filtres et rapports |
| Tenant | `DSI` | propriété |
| Adresse physique | `14 rue des Forges, 75011 Paris` | intervention sur site |
| Longitude/Latitude | `48.8637, 2.3792` | carte des sites |
| ASN | `64512` | si le site a son AS privé |

> 💡 Renseignez les coordonnées GPS : la vue « carte » de NetBox devient un vrai
> tableau de bord géographique de votre infrastructure.

---

## 20. DCIM : baies (racks), rôles et réservations

La **baie** (rack) est l'unité physique de base : on y installe les équipements par
unités U.

**Rôles de baie** (à créer dans DCIM > Rôles de baies) : `Baie-Reseau`, `Baie-Serveurs`,
`Baie-Brassage`, `Baie-Onduleur`. Le rôle colore la baie dans les vues et sert de filtre.

Création d'une baie : **DCIM > Baies > + Ajouter** :

| Champ | Exemple | Commentaire |
|---|---|---|
| Nom | `SIEGE-Baie-A01` | convention : `SITE-Fonction-Numéro` |
| Site | `SIEGE-Paris11` | rattachement obligatoire |
| Tenant | `DSI` | |
| Statut | Actif | |
| Rôle | `Baie-Reseau` | |
| Type | 19 pouces | |
| Largeur | 19 pouces | |
| Hauteur | `42U` | standard datacenter |
| Unités numérotées | descendant (42→1) | le U1 est en bas : standard |

**Bonnes pratiques d'exploitation :**

- Réservez les U avec la fonction **Réservations** (ex. « U20-U21 réservés pour le
  futur firewall — ticket #1234 ») : fini les installations qui se marchent dessus.
- Documentez la baie AVANT d'y visser quoi que ce soit (voir section 74, la règle d'or).
- Photographiez la baie et joignez l'image à la fiche (pièces jointes).

---

## 21. DCIM : fabricants et types d'équipements

Ne créez jamais un équipement « à la main » sans **type d'équipement** : le type décrit
le modèle physique (dimensions, interfaces, alimentations, console).

1. **Fabricant** (DCIM > Fabricants) : `Cisco`, `HPE`, `Schneider-Electric`, `Eaton`, `APC`.
2. **Type d'équipement** (DCIM > Types d'équipements) : ex. `Cisco Catalyst C9300-48P`,
   `APC Smart-UPS SRT 6000`, `Eaton 9PX 3000`.

Fiche type d'équipement — à remplir avec soin (c'est du travail mutualisé : fait une
fois, réutilisé pour chaque exemplaire) :

| Champ | Exemple |
|---|---|
| Fabricant | `Cisco` |
| Modèle | `Catalyst C9300-48P` |
| Slug | `cisco-catalyst-c9300-48p` |
| Hauteur (U) | `1` |
| Est full-depth | oui/non |
| Commentaires | `48x 1G PoE+, 4x SFP+ — alim. redondante` |

Ensuite, définissez les **composants modèles** du type (templates) : interfaces
(`GigabitEthernet1/0/1` … `1/0/48`), ports d'alimentation, console. À la création de
l'équipement réel, NetBox instancie automatiquement tous ces composants : énorme gain
de temps.

> 💡 Créez les types d'équipements pour vos onduleurs et PDU (ex.
> `Eaton 9PX 3000 RT`, `APC AP8858 PDU`) : vous pourrez câbler l'alimentation
> proprement (section 26) — le lien direct avec votre métier énergies.

---

## 22. DCIM : équipements (devices)

L'**équipement** est l'instance physique : « le switch n°3 de la baie A01 ».
**DCIM > Équipements > + Ajouter** :

| Champ | Exemple | Obligatoire ? |
|---|---|---|
| Nom | `SIEGE-SW-A01-01` | ✅ (unique par site) |
| Type d'équipement | `Cisco Catalyst C9300-48P` | ✅ |
| Rôle d'équipement | `Switch-Acces` | ✅ (à créer : `Switch-Coeur`, `Serveur`, `Onduleur`, `Firewall`…) |
| Site | `SIEGE-Paris11` | ✅ |
| Baie | `SIEGE-Baie-A01` | si monté en baie |
| Position (U) | `12` | ✅ si baie renseignée |
| Face | Avant | |
| Statut | Actif | ✅ |
| Tenant | `DSI` | recommandé |
| Numéro de série | `FOC1234ABCD` | très recommandé (SAV, garantie) |
| Asset tag (inventaire) | `INV-2024-0342` | recommandé (lien avec l'inventaire comptable) |

**Cycle de vie avec les statuts** : `En stock` → `Actif` → `En maintenance` →
`Hors service` → `Retiré`. Utilisez-les : un équipement `Hors service` reste documenté
(utile pour l'historique) sans polluer les vues d'exploitation (filtrez sur `Actif`).

---

## 23. DCIM : interfaces et bonnes pratiques de nommage

Les interfaces sont créées automatiquement depuis le type d'équipement, mais vérifiez
et complétez :

- **Nommage** : gardez le nommage constructeur (`GigabitEthernet1/0/1`, `eth0`, `eno1`).
  N'inventez pas vos propres noms : l'interface NetBox doit correspondre à ce que
  l'OS/CLI affiche, sinon le dépannage devient un cauchemar.
- **Description** : utilisez-la pour la destination : `Vers SRV-HYP-01:eth0`,
  `Uplink vers COEUR-01:Gi1/0/48`, `Imprimante Accueil`.
- **Type** : `1000BASE-T`, `10GBASE-SR`, `SFP+`… (conditionne les câbles compatibles).
- **Mode 802.1Q** : `Access` + VLAN, ou `Tagged` + VLANs (trunk), ou `Tagged All`.
- **MTU** : `1500`, ou `9000` (jumbo) si votre infra le supporte de bout en bout.
- **Activé/désactivé** : reflétez l'état réel du port (un port `shutdown` côté switch
  = désactivé dans NetBox).

**Interface management** : créez une interface dédiée (ex. `mgmt0`, `iDRAC`, `iLO`)
avec son IP : c'est elle qui portera l'adresse de supervision (lien IPAM, section 38).

---

## 24. DCIM : câblage — câbles et connexions

NetBox modélise les liaisons physiques entre interfaces/ports via des **câbles**.
Deux interfaces reliées = un câble entre elles (ou une chaîne via des panneaux de brassage).

Création : depuis la fiche d'une interface > **Connecter**, ou **DCIM > Câbles**.

| Champ | Exemple |
|---|---|
| Type | `Cat6a`, `Fibre OM4`, `DAC` |
| Longueur / unité | `3`, `mètres` |
| Couleur | `Jaune` (jarretières), `Bleu`… |
| Étiquette | `JAR-A01-014` |

**Code couleur conseillé** (à afficher dans le local technique) :

| Couleur | Usage |
|---|---|
| Bleu | brassage cuivre standard |
| Jaune | uplinks / inter-baies |
| Rouge | liens critiques / cœur |
| Vert | fibre optique |
| Noir | alimentation (ne jamais mélanger avec les jarretières data) |

> ⚠️ Câblez dans NetBox **avant ou pendant** l'intervention, jamais « quand on aura
> le temps ». Un câblage non documenté est un câblage perdu : la section 74 en fait
> une règle d'équipe.

---

## 25. DCIM : chemins de câbles (cable paths)

Quand un lien traverse plusieurs segments (switch → panneau de brassage → panneau
distant → serveur), NetBox calcule automatiquement le **chemin de câble** de bout en bout.

Chaîne type :

```
SW-A01-01:Gi1/0/1 --(jarretière)--> PB-A01:Panneau-24p:Port-01
   --(rocade)--> PB-B02:Panneau-24p:Port-01 --(jarretière)--> SRV-02:eth0
```

Intérêts concrets :

- **Dépannage** : « le port 12 du panneau B02 ne répond plus » → le chemin montre
  tous les segments à tester, dans l'ordre.
- **Changement de jarretière** : on sait exactement quoi débrancher sans couper
  le voisin.
- **Audit** : exportez les chemins pour vérifier la redondance (deux chemins
  physiquement disjoints pour un lien critique ?).

Pour que les chemins fonctionnent, modélisez les **panneaux de brassage** comme des
équipements (rôle `Panneau-Brassage`, type passif) avec leurs ports avant/arrière,
puis câblez chaque segment. C'est du travail initial, mais c'est ce qui transforme
NetBox en véritable jumeau numérique de votre câblage.

---

## 26. DCIM : alimentation — PDU, onduleurs et lien avec le métier énergies

C'est la section qui relie NetBox à votre cœur de métier. NetBox modélise la chaîne
électrique complète : **onduleur → PDU → alimentation de l'équipement**.

### 26.1. Modéliser l'onduleur et les PDU

1. Créez le type d'équipement onduleur (ex. `Eaton 9PX 3000 RT`, 2U) avec ses
   **prises de sortie** (composants « power outlets ») : `Sortie-1` … `Sortie-8`.
2. Créez l'équipement onduleur dans la baie (rôle `Onduleur`), ex. `SIEGE-UPS-A01`.
3. Créez les PDU (rôle `PDU`, type `APC AP8858` 0U ou 1U) avec leurs prises :
   ex. `SIEGE-PDU-A01-A` et `SIEGE-PDU-A01-B` (double alimentation A/B).
4. Câblez : `UPS:Sortie-1` → `PDU-A:Entrée`, `UPS:Sortie-2` → `PDU-B:Entrée`
   (connexions d'alimentation, comme des câbles data mais entre ports électriques).

### 26.2. Raccorder les équipements

Pour chaque serveur/switch à double alimentation :

```
SRV-01:PSU1 --câble C13--> PDU-A:Prise-05
SRV-01:PSU2 --câble C13--> PDU-B:Prise-05
```

Bénéfices immédiats :

- **Plan de délestage** : en cas de maintenance onduleur, la liste des équipements
  branchés sur chaque PDU est exacte — fini les « on coupe et on verra bien ».
- **Équilibrage de charge** : visualisez la répartition A/B prise par prise.
- **Capacité** : additionnez les puissances (champ « puissance max » des alimentations)
  par PDU et comparez au calibre du disjoncteur amont.

### 26.3. Champs utiles à renseigner (onglet Alimentation)

| Champ | Exemple | Usage |
|---|---|---|
| Tension d'alimentation | `230 V` | dimensionnement |
| Puissance max (W) | `800` | bilan de puissance par baie |
| Type de prise | `C13/C14`, `C19/C20` | commande des cordons |

> 💡 **Idée d'exploitation** : créez un **rapport personnalisé** (section 59) « Bilan
> de puissance par baie » qui additionne les puissances des équipements et alerte
> quand on dépasse 80 % du calibre onduleur/PDU. C'est exactement le type d'outil
> qui fait la différence entre « on subit » et « on pilote » son énergie.

---

## 27. Circuits : opérateurs et liaisons FAI

Le module **Circuits** documente vos liens opérateurs (fibre, xDSL, 4G/5G, MPLS).

1. **Fournisseur** (Circuits > Fournisseurs) : `Orange-Business`, `Free-Pro`, `OVH`.
   Renseignez le portail client, le support (téléphone, horaires) et les identifiants
   de compte — en intervention à 2h du matin, c'est précieux.
2. **Type de circuit** : `Fibre FTTO`, `FTTH`, `MPLS`, `xDSL`, `4G-Secours`.
3. **Circuit** : ex. `CT-OBS-2024-001` :
   - Débit montant/descendant (`1 Gb/s` / `1 Gb/s`),
   - Identifiant opérateur (n° de ligne, référence contrat),
   - Dates de mise en service / engagement,
   - **Terminaison A** (votre routeur : `SIEGE-RTR-01:Gi0/0`) et **terminaison Z**
     (équipement opérateur si connu).

**Bonnes pratiques :**

- Nommez les circuits avec l'année et un compteur : `CT-<Fournisseur>-<AAAA>-<NNN>`.
- Renseignez les **dates d'engagement** : NetBox ne relance pas tout seul, mais un
  rapport personnalisé « engagements arrivant à échéance » (section 59) le fait.
- Liez le circuit au **tenant** concerné et au **site**.

---

## 28. Virtualisation : clusters, VM et interfaces virtuelles

NetBox documente aussi le virtuel : **Virtualisation > Clusters**, puis **Machines virtuelles**.

1. **Type de cluster** : `Proxmox-VE`, `VMware-vSphere`, `Hyper-V`.
2. **Cluster** : ex. `PVE-SIEGE` (site = `SIEGE-Paris11`, 3 nœuds).
3. **Machine virtuelle** : ex. `VM-NETBOX-01` :
   - Cluster, statut (`Actif`), rôle (`Infra`, `Applicatif`),
   - vCPU, RAM, disque (Go),
   - Plateforme (`Debian 12`), tenant.

Les **interfaces virtuelles** (`eth0`…) portent les adresses IP comme en physique :
votre IPAM reste cohérent entre monde physique et virtuel.

> 💡 **Cohérence d'inventaire** : la VM `VM-NETBOX-01` qui héberge NetBox lui-même
> doit exister dans NetBox (cluster, ressources, IP). C'est le test ultime :
> « si NetBox ne se documente pas lui-même, personne ne le fera ».

---

## 29. Adresses MAC et découverte

NetBox stocke les adresses MAC au niveau des interfaces (physiques et virtuelles).
Renseignez-les pour :

- les équipements réseau (table ARP, DHCP statique),
- les serveurs (inventaire, Wake-on-LAN),
- les VM (cohérence hyperviseur).

Astuce d'alimentation en masse : exportez la table ARP de votre cœur de réseau,
puis utilisez l'API (section 56) ou un script pour rattacher les MAC aux interfaces.
Ne saisissez jamais des centaines de MAC à la main : **script > clic**.

---

## 30. Pièces jointes, photos et documentation liée

Chaque objet NetBox accepte des **pièces jointes** (photos, plans, PDF) :

- Photo de la baie (avant/après intervention),
- Plan du local technique,
- Fiche technique de l'onduleur (courbe d'autonomie),
- Contrat opérateur (sur la fiche circuit),
- Procédure de redémarrage (sur la fiche équipement critique).

Limitez la taille (`client_max_body_size` nginx, section 15) et **ne stockez jamais
de secrets** (mots de passe, clés privées) en pièce jointe : NetBox n'est pas un
coffre-fort. Pour les mots de passe : gestionnaire dédié (Vault, KeePass d'équipe).

> ⚠️ Les pièces jointes vivent dans `media/` : elles font partie de la sauvegarde
> (section 64). Une sauvegarde BDD sans `media/` = des fiches sans photos.

---

## 31. IPAM : les VRF (tables de routage virtuelles)

Une **VRF** (*Virtual Routing and Forwarding*) isole des plans d'adressage qui se
chevauchent : même préfixe `10.1.0.0/16` utilisable dans deux VRF différentes sans conflit.

**IPAM > VRF > + Ajouter** :

| Champ | Exemple |
|---|---|
| Nom | `VRF-PROD`, `VRF-GUEST`, `VRF-MGMT` |
| RD (Route Distinguisher) | `65000:100` |
| Tenant | `DSI` |
| Description | `VRF de production — routage MPLS` |

Cas d'usage typiques :

- `VRF-MGMT` : plan d'administration des équipements (10.255.0.0/16), jamais routé
  vers les utilisateurs.
- `VRF-GUEST` : Wi-Fi invités, cloisonné.
- `VRF-CLIENT-X` : chevauchement d'adressage entre deux clients hébergés.

> ⚠️ Un préfixe hors VRF (VRF « Global ») et le même préfixe dans une VRF sont deux
> objets distincts : c'est voulu. En revanche, deux fois le même préfixe dans la
> MÊME VRF = doublon refusé par NetBox.

---

## 32. IPAM : RIR et agrégats (blocs d'adresses)

- **RIR** (*Regional Internet Registry*) : `RIPE-NCC` (Europe), `ARIN`… À créer dans
  IPAM > RIR. Utile si vous gérez vos propres blocs publics.
- **Agrégat** : le bloc de haut niveau que vous possédez ou qui vous est alloué.
  Ex. `193.0.2.0/24` (bloc RIPE alloué), `10.0.0.0/8` (privé, RIR « RFC-1918 »).

Créez un RIR `RFC-1918` et les trois agrégats privés une fois pour toutes :

| Agrégat | Usage conseillé |
|---|---|
| `10.0.0.0/8` | interconnexions, datacenters, gros sites |
| `172.16.0.0/12` | agences, Wi-Fi, vidéosurveillance |
| `192.168.0.0/16` | petits sites, management local, lab |

Depuis un agrégat, NetBox affiche l'**utilisation** : quels sous-préfixes sont
découpés, quel espace reste libre. C'est votre tableau de bord d'adressage.

---

## 33. IPAM : préfixes — le cœur de l'adressage

Le **préfixe** est l'objet IPAM central : `10.10.20.0/24`, `2001:db8:1::/64`.
**IPAM > Préfixes > + Ajouter**.

| Champ | Exemple | Commentaire |
|---|---|---|
| Préfixe | `10.10.20.0/24` | notation CIDR stricte |
| VRF | `VRF-PROD` ou Global | |
| Site | `SIEGE-Paris11` | rattachement géographique |
| VLAN | `VLAN-20` | le VLAN qui porte ce sous-réseau |
| Statut | `Actif` | voir ci-dessous |
| Rôle | `LAN-Utilisateurs` | à créer : `Serveurs`, `Management`, `WiFi`… |
| Tenant | `DSI` | |
| Description | `LAN utilisateurs — bâtiment A, étage 2` | |

**Statuts de préfixe** (à utiliser avec rigueur) :

| Statut | Signification |
|---|---|
| `Container` | bloc parent découpé en sous-préfixes (ex. `10.10.0.0/16`) |
| `Actif` | en production |
| `Réservé` | attribué à un projet, pas encore déployé |
| `Déprécié` | en cours de retrait, ne plus l'utiliser |

> 💡 **Convention** : tout bloc `/16` ou plus grand est `Container`. On ne met JAMAIS
> d'adresses IP directement dans un `Container` : on découpe d'abord en préfixes enfants.

---

## 34. IPAM : découper et visualiser un plan d'adressage

Depuis la fiche d'un préfixe `Container`, le bouton **+ Préfixes enfants** permet de
découper : NetBox propose les sous-préfixes libres.

Exemple : `10.10.0.0/16` (Container, site Siège) découpé en :

| Préfixe | Rôle | VLAN | Usage |
|---|---|---|---|
| `10.10.10.0/24` | `Serveurs` | 10 | serveurs physiques et VM |
| `10.10.20.0/24` | `LAN-Utilisateurs` | 20 | postes, étage 2 |
| `10.10.30.0/24` | `WiFi` | 30 | Wi-Fi corporate |
| `10.10.40.0/24` | `Impression` | 40 | imprimantes/MFP |
| `10.10.254.0/24` | `Management` | 254 | iDRAC, switchs, onduleurs (cartes réseau) |

La vue du préfixe affiche une **barre d'utilisation** : % d'IP utilisées, libres,
et la liste des adresses. C'est l'outil n°1 pour répondre à « il reste des IP sur
le VLAN 20 ? » en 10 secondes.

**Plan d'adressage type pour un site moyen** (à adapter) :

- `.0` réseau, `.1` passerelle (VRRP/HSRP : `.2`/`.3` physiques, `.1` virtuelle),
- `.4`–`.19` : équipements réseau (switchs, AP),
- `.20`–`.49` : serveurs / hyperviseurs,
- `.50`–`.199` : DHCP dynamique,
- `.200`–`.249` : équipements fixes (imprimantes, caméras),
- `.250`–`.254` : management (iDRAC/iLO, cartes onduleurs, PDU).

---

## 35. IPAM : plages IP (IP ranges)

Une **plage IP** réserve un intervalle à l'intérieur d'un préfixe sans créer chaque
adresse : parfait pour les pools DHCP, les VIP, les plages d'impression.

**IPAM > Plages IP > + Ajouter** :

| Champ | Exemple |
|---|---|
| Plage | `10.10.20.50` → `10.10.20.199` |
| VRF / préfixe parent | déduit automatiquement |
| Statut | `Actif` |
| Rôle | `DHCP` |
| Description | `Pool DHCP — LAN utilisateurs étage 2` |

Cas d'usage :

- `DHCP` : pools des serveurs DHCP (à croiser avec la conf. réelle du serveur !),
- `VIP` : adresses virtuelles (VRRP, load-balancers),
- `Réservé` : plage gelée pour un projet futur.

> 💡 Une plage `DHCP` documentée + un export API vers votre serveur DHCP (section 56)
> = des pools toujours synchronisés avec la documentation. C'est l'automatisation
> qui commence.

---

## 36. IPAM : adresses IP et assignation aux interfaces

L'**adresse IP** est l'objet le plus créé au quotidien : `10.10.10.15/24`.
**IPAM > Adresses IP > + Ajouter**, ou directement depuis une interface
(**Assigner une IP**).

| Champ | Exemple |
|---|---|
| Adresse | `10.10.10.15/24` (le masque est obligatoire) |
| VRF | héritée du préfixe |
| Statut | `Actif` (`Réservé`, `DHCP`) |
| Rôle | `VIP`, `Loopback`, `Anycast`… |
| DNS | `srv-fichiers-01.lan-entreprise.fr` |
| Description | `Interface de production` |

**Assignation** : depuis la fiche IP > **Assigner** > choisir l'objet :
interface d'équipement (`SIEGE-SW-A01-01:mgmt0`), interface de VM, ou plage.
Une IP assignée apparaît sur la fiche de l'interface et inversement : la liaison
IPAM ↔ DCIM est faite.

**Règles d'hygiène :**

- Une IP = un masque correct (celui du préfixe). NetBox alerte si l'IP n'appartient
  à aucun préfixe connu (« hors préfixe » = anomalie à traiter).
- Statut `DHCP` pour les adresses dynamiques connues (réservations), `Réservé`
  pour les futures attributions.
- Renseignez le **DNS** : NetBox devient aussi votre aide-mémoire de nommage.

---

## 37. IPAM : rôles de préfixes et d'adresses

Les **rôles** qualifient l'usage. Créez votre référentiel une fois (IPAM > Rôles),
réutilisez partout :

| Rôle | Couleur suggérée | Usage |
|---|---|---|
| `Loopback` | gris | /32 des routeurs |
| `Interconnexion` | orange | /30, /31 entre équipements |
| `Serveurs` | bleu | sous-réseaux serveurs |
| `LAN-Utilisateurs` | vert | postes |
| `WiFi` | violet | SSID corporate/invités |
| `Management` | rouge | administration (OOB) |
| `Impression` | jaune | imprimantes/MFP |
| `Videosurveillance` | noir | caméras |
| `DMZ` | rouge foncé | zone exposée |
| `Secours-4G` | cyan | liens de backup |

Les couleurs apparaissent dans les barres d'utilisation : d'un coup d'œil, on voit
la nature de chaque préfixe. **Ne multipliez pas les rôles** : 10–12 rôles stables
valent mieux que 40 rôles approximatifs.

---

## 38. VLAN : groupes de VLAN et VLAN

Hiérarchie : **Groupe de VLAN → VLAN**. Le groupe porte la plage d'ID
(ex. `100–199 : Serveurs`), le VLAN est l'instance (`VLAN 110 : SRV-Production`).

**IPAM > Groupes de VLAN > + Ajouter** : nom (`VLAN-Siege`), site, plage d'ID.
**IPAM > VLAN > + Ajouter** :

| Champ | Exemple |
|---|---|
| Nom | `SRV-Production` |
| ID (VID) | `110` |
| Groupe | `VLAN-Siege` |
| Site | `SIEGE-Paris11` |
| Statut / Rôle | `Actif` / `Serveurs` |
| Tenant | `DSI` |

**Liaison VLAN ↔ préfixe** : depuis le préfixe, renseignez le VLAN (et inversement).
NetBox vérifie la cohérence : un préfixe `10.10.10.0/24` marqué VLAN 110 dont les
interfaces en mode Access portent le VLAN 110 = documentation cohérente.

> ⚠️ Les VLAN 1, 1002–1005 (Cisco) et les plages réservées : documentez-les comme
> `Réservé` pour éviter qu'un collègue ne les réutilise par mégarde.

---

## 39. Services (ports TCP/UDP par équipement/VM)

Le module **IPAM > Services** documente « quoi écoute où » : utile pour les règles
de pare-feu et le dépannage.

Exemple sur `VM-NETBOX-01` :

| Nom | Protocole | Ports | Description |
|---|---|---|---|
| `https` | TCP | `443` | interface web NetBox |
| `ssh` | TCP | `22` | administration |

Exemple sur un serveur de fichiers : `smb` TCP 445, sur un onduleur avec carte
réseau : `https` TCP 443 + `snmp` UDP 161.

> 💡 En audit de sécurité, l'export des services par site donne la cartographie
> des surfaces d'exposition sans scanner le réseau. Pensez à le maintenir à jour
> lors de chaque mise en production.

---

## 40. Recherche globale, filtres et vues personnalisées

- **Recherche globale** (barre en haut) : tapez `10.10.10.15`, `SIEGE-SW`, `FOC1234`
  (n° de série) : NetBox cherche dans tous les objets. C'est le réflexe n°1.
- **Filtres** : chaque liste (préfixes, équipements…) a un panneau de filtres
  (statut, site, tenant, rôle…). Combinez-les : « équipements Actif + site Siège +
  rôle Switch-Acces ».
- **Colonnes** : personnalisez les colonnes affichées par liste (roue crantée).
- **Signets** : enregistrez vos filtres fréquents comme signets d'équipe
  (« Baies pleines à +80 % », « IP sans DNS »).

Astuce : les filtres sont encodés dans l'URL : copiez l'URL d'une vue filtrée dans
votre documentation d'exploitation ou vos tickets — le destinataire voit exactement
la même sélection.

---

## 41. Exports CSV natifs

Chaque liste propose **Exporter > CSV** : parfait pour les inventaires ponctuels,
les audits, les tableaux de reporting.

Limites à connaître :

- L'export suit les filtres actifs : filtrez d'abord, exportez ensuite.
- Gros volumes (> 50 000 lignes) : préférez l'API paginée (section 54).
- L'export CSV n'inclut pas les pièces jointes ni les champs personnalisés complexes :
  pour un vrai reporting, utilisez les rapports personnalisés (section 59).

---

## 42. Comptes utilisateurs et groupes

**Admin > Utilisateurs** : créez UN compte nominatif par personne. Jamais de compte
partagé « admin » ou « tech » : le journal d'audit (section 45) perd tout son sens
si on ne sait pas QUI a modifié quoi.

**Groupes** (Admin > Groupes) : modélisez vos rôles d'équipe :

| Groupe | Périmètre type |
|---|---|
| `Admins-Infra` | tous droits |
| `Exploitants-Resau` | lecture/écriture sur DCIM+IPAM, pas d'admin |
| `Techniciens-Terrain` | lecture globale + modification des câbles et baies |
| `Lecture-Seule` | consultation (management, auditeurs, prestataires) |
| `Stagiaires` | lecture seule + bac à sable (un tenant dédié) |

> 💡 Le compte `admin` initial : réservez-le aux urgences (mot de passe sous scellés
> dans le coffre). Au quotidien, chacun utilise son compte nominatif, y compris vous.

---

## 43. Permissions : le modèle objet de NetBox

NetBox applique des permissions fines : **qui peut faire quoi sur quels objets**.
Admin > Permissions > + Ajouter :

1. **Actions** : voir, ajouter, modifier, supprimer (par modèle : préfixe, équipement…).
2. **Contraintes** (optionnel mais puissant) : limite l'action à un sous-ensemble,
   exprimé en JSON. Exemple : les techniciens terrain ne modifient que les objets
   du site de Lyon :

```json
{
  "site": ["AGENCE-Lyon"]
}
```

3. Assignez la permission au **groupe** (jamais directement à l'utilisateur :
   ingérable à terme).

Exemple complet : groupe `Techniciens-Terrain` :

| Permission | Contrainte |
|---|---|
| Voir : tout (ou presque) | — |
| Modifier : `dcim.cable`, `dcim.device` (champs position/statut) | `{"site": ["AGENCE-Lyon"]}` |
| Ajouter : `dcim.cable` | `{"site": ["AGENCE-Lyon"]}` |

> ⚠️ Testez TOUJOURS une permission avec un compte du groupe concerné avant de la
> déployer : une contrainte mal écrite bloque silencieusement (l'utilisateur voit
> des listes vides et ouvre un ticket « NetBox est cassé »).

---

## 44. Authentification externe : LDAP / SSO (OIDC)

En entreprise, on ne gère pas les mots de passe dans chaque appli : on délègue à
l'annuaire. NetBox 4.x supporte LDAP (via `django-auth-ldap`) et l'OIDC
(fournisseurs SSO) via configuration.

**LDAP** — ajoutez dans `configuration.py` (paquet `django-auth-ldap` à installer
dans le venv) :

```python
import ldap
from django_auth_ldap.config import LDAPSearch, GroupOfNamesType

AUTH_LDAP_SERVER_URI = "ldaps://ad.lan-entreprise.fr:636"
AUTH_LDAP_BIND_DN = "CN=svc-netbox,OU=Comptes de service,DC=lan-entreprise,DC=fr"
AUTH_LDAP_BIND_PASSWORD = "CHANGEZ_MOI_LDAP_Exemple_2026!"
AUTH_LDAP_USER_SEARCH = LDAPSearch(
    "OU=Utilisateurs,DC=lan-entreprise,DC=fr",
    ldap.SCOPE_SUBTREE, "(sAMAccountName=%(user)s)",
)
AUTH_LDAP_GROUP_SEARCH = LDAPSearch(
    "OU=Groupes,DC=lan-entreprise,DC=fr",
    ldap.SCOPE_SUBTREE, "(objectClass=group)",
)
AUTH_LDAP_GROUP_TYPE = GroupOfNamesType(name_attr="cn")
AUTH_LDAP_USER_ATTR_MAP = {
    "first_name": "givenName",
    "last_name": "sn",
    "email": "mail",
}
# Miroir des groupes AD -> groupes NetBox (pratique !)
AUTH_LDAP_MIRROR_GROUPS = True
```

Avec `AUTH_LDAP_MIRROR_GROUPS = True`, l'appartenance aux groupes AD
(`GG-NetBox-Admins`…) pilote les groupes NetBox : **l'habilitation se gère dans
l'AD**, NetBox suit automatiquement. C'est le montage recommandé.

---

## 45. Journal d'audit (changelog) et traçabilité

Chaque création, modification, suppression est enregistrée : **qui, quand, quoi,
avant/après**. Consultez :

- le journal global : en bas de la page d'accueil ou via l'objet « Journal »,
- le journal d'un objet : onglet **Journal** sur sa fiche.

Cas d'usage :

- « Qui a déplacé le serveur SRV-02 de la baie A01 vers A02 ? » → réponse en 30 s.
- « Quand l'IP 10.10.10.15 a-t-elle été assignée ? » → horodatage exact.
- Conflit d'équipe : l'historique tranche sans débat.

**Rétention** : le changelog grossit avec l'usage. Paramètre dans `configuration.py` :

```python
CHANGELOG_RETENTION = 90   # jours ; 0 = conservation infinie (défaut)
```

> ⚠️ En environnement audité (ISO 27001…), conservez le changelog longtemps et
> incluez la table `extras_objectchange` dans votre politique de sauvegarde :
> c'est elle qui porte la traçabilité (la sauvegarde BDD standard l'inclut déjà).

---

## 46. Sécurité : durcissement de l'installation

Checklist de durcissement (à appliquer dès la mise en production) :

- [ ] `configuration.py` en `600`, propriétaire `netbox:netbox`
- [ ] `LOGIN_REQUIRED = True`, `SECRET_KEY` unique et robuste
- [ ] HTTPS partout, cookies `Secure`, HSTS (section 16)
- [ ] Comptes nominatifs, groupes + permissions (sections 42–43), compte `admin`
    sous scellés
- [ ] LDAP/SSO si disponible (section 44) ; sinon politique de mots de passe forte
- [ ] PostgreSQL : écoute `localhost` uniquement, mot de passe robuste
- [ ] Redis : `bind 127.0.0.1`, `requirepass` si exposition réseau inévitable
- [ ] Pare-feu (nftables/UFW) : n'ouvrir que 443 (et 22 restreint aux admins)
- [ ] `DEBUG = False` (jamais `True` en production : fuite d'informations garantie)
- [ ] Mises à jour OS + NetBox suivies (section 66)
- [ ] Sauvegardes testées (section 65) — une sauvegarde non testée n'existe pas
- [ ] Bannière `BANNER_TOP` pour marquer l'environnement (PROD vs LAB)

```bash
# Exemple UFW minimaliste
ufw default deny incoming
ufw allow from 10.10.0.0/16 to any port 443 proto tcp
ufw allow from 10.10.254.10 to any port 22 proto tcp   # bastion uniquement
ufw enable
```

---

## 47. Champs personnalisés (custom fields)

Les **champs personnalisés** (Admin > Champs personnalisés) étendent les objets
NetBox sans plugin : texte, entier, booléen, date, URL, JSON, sélection.

Exemples utiles pour un service systèmes :

| Objet | Champ | Type | Usage |
|---|---|---|---|
| Équipement | `date_fin_garantie` | date | alertes de renouvellement |
| Équipement | `contrat_maintenance` | texte | n° de contrat |
| Équipement | `criticite` | sélection (`Critique/Majeur/Mineur`) | priorisation |
| Baie | `disjoncteur_amont` | texte | `TGBT-DJ-14 — 32A` (lien énergies !) |
| Préfixe | `responsable` | texte | référent métier |
| Circuit | `date_engagement` | date | fin d'engagement opérateur |

> 💡 Règle : un champ personnalisé doit servir à un **usage réel** (filtre, rapport,
> alerte). Un champ « au cas où » que personne ne remplit = du bruit. Revue annuelle
> des champs personnalisés conseillée.

---

## 48. Étiquettes (tags)

Les **tags** (Admin > Étiquettes) sont des marqueurs libres, transverses aux objets :
`a-migrer`, `supervise-zabbix`, `contrat-gold`, `a-verifier`.

Différences avec les champs personnalisés :

| | Tag | Champ personnalisé |
|---|---|---|
| Nature | marqueur libre, multiple | champ typé, défini à l'avance |
| Usage | tri rapide, lots temporaires | donnée structurée durable |
| Exemple | `incident-2026-09` sur 12 équipements | `date_fin_garantie` sur tous les serveurs |

Bonnes pratiques : couleurs sobres, noms en minuscules avec tirets, et surtout
**nettoyage régulier** : un tag temporaire (`a-verifier`) vieux de 2 ans n'est plus
temporaire, c'est de la dette documentaire.

---

## 49. Modèles (templates) d'export et import en masse

- **Import CSV** : chaque liste propose **Importer** : préparez un CSV avec les
  colonnes exactes (téléchargez d'abord un export pour voir le format), importez,
  corrigez les erreurs signalées ligne par ligne. Idéal pour : 200 VLAN, 48 ports
  d'un panneau, un parc d'imprimantes.
- **Export** : CSV natif (section 41) ou via l'API pour les gros volumes.

Exemple de CSV d'import de préfixes (en-têtes exacts requis) :

```csv
prefix,status,vrf,site,vlan,role,tenant,description
10.20.1.0/24,active,,AGENCE-Lyon,120,LAN-Utilisateurs,DSI,"LAN agence Lyon"
10.20.2.0/24,active,,AGENCE-Lyon,121,WiFi,DSI,"WiFi agence Lyon"
```

> ⚠️ L'import est puissant et irréversible en masse : testez TOUJOURS sur 3 lignes
> dans un tenant `Bac-A-Sable` avant d'importer 500 lignes en production.

---

## 50. Tâches planifiées et maintenance interne

NetBox s'auto-entretient partiellement ; à vous de planifier le reste :

| Tâche | Fréquence | Commande / moyen |
|---|---|---|
| Nettoyage des sessions expirées | hebdo | `python netbox/manage.py clearsessions` |
| Purge du changelog (si rétention) | hebdo | automatique via `CHANGELOG_RETENTION` |
| Vérification disque BDD | mensuel | `SELECT pg_size_pretty(pg_database_size('netbox'));` |
| Test de restauration sauvegarde | trimestriel | section 65 |
| Revue des comptes inactifs | trimestriel | Admin > Utilisateurs (filtre dernière connexion) |
| Revue des permissions | semestriel | avec le RSSI / la direction |

Automatisez avec cron (utilisateur `netbox`) :

```bash
# /etc/cron.d/netbox-maintenance
0 3 * * 0 netbox /opt/netbox/venv/bin/python /opt/netbox/netbox/netbox/manage.py clearsessions
```

---

## 51. API REST : principes et authentification par token

Tout ce qui se fait dans l'interface se fait via l'**API REST** : c'est le socle de
l'automatisation. Base URL : `https://netbox.lan-entreprise.fr/api/`.

1. Créez un token : profil (en haut à droite) > **API Tokens** > + Ajouter.
   - Donnez-lui une description (`Script inventaire Ansible — svc-ansible`).
   - **Permissions d'écriture** : décochez si le script ne fait que lire (principe
     du moindre privilège).
   - Date d'expiration : mettez-en une pour les tokens temporaires.
2. Utilisez-le dans l'en-tête HTTP : `Authorization: Token <votre_token>`.

> ⚠️ Un token = un mot de passe. Ne le commitez JAMAIS dans git en clair, ne le
> collez pas dans un ticket. Stockez-le dans un gestionnaire de secrets / variable
> d'environnement (`NETBOX_TOKEN`). En cas de doute : révoquez et régénérez.

L'API est **auto-documentée** : `https://netbox.lan-entreprise.fr/api/docs/`
(Swagger/OpenAPI) — chaque endpoint, chaque champ, avec exemples. C'est la
référence en cas de doute sur ce guide.

---

## 52. API REST : exemples `curl` (lecture)

```bash
export NETBOX_URL="https://netbox.lan-entreprise.fr"
export NETBOX_TOKEN="CHANGEZ_MOI_Token_Exemple_ABCDEF1234567890"

# Lister les sites (paginé : 50 objets par page par défaut)
curl -s -H "Authorization: Token $NETBOX_TOKEN" \
     -H "Accept: application/json" \
     "$NETBOX_URL/api/dcim/sites/" | python3 -m json.tool | head -40

# Chercher un préfixe précis
curl -s -H "Authorization: Token $NETBOX_TOKEN" \
     "$NETBOX_URL/api/ipam/prefixes/?prefix=10.10.20.0/24" | python3 -m json.tool

# Filtrer : équipements actifs du site Siège
curl -s -H "Authorization: Token $NETBOX_TOKEN" \
     "$NETBOX_URL/api/dcim/devices/?site=siege-paris11&status=active" \
     | python3 -c "import json,sys; d=json.load(sys.stdin); print(d['count'], 'équipements')"

# Suivre la pagination (exemple : récupérer TOUTES les adresses IP)
curl -s -H "Authorization: Token $NETBOX_TOKEN" \
     "$NETBOX_URL/api/ipam/ip-addresses/?limit=1000" -o /tmp/ips.json
```

Points d'API les plus utilisés :

| Endpoint | Contenu |
|---|---|
| `/api/dcim/sites/` | sites |
| `/api/dcim/racks/` | baies |
| `/api/dcim/devices/` | équipements |
| `/api/dcim/interfaces/` | interfaces |
| `/api/dcim/cables/` | câbles |
| `/api/ipam/vrfs/` | VRF |
| `/api/ipam/prefixes/` | préfixes |
| `/api/ipam/ip-addresses/` | adresses IP |
| `/api/ipam/vlans/` | VLAN |
| `/api/virtualization/virtual-machines/` | VM |
| `/api/circuits/circuits/` | circuits |
| `/api/tenancy/tenants/` | tenants |

---

## 53. API REST : écriture avec `curl` (création)

```bash
# Créer un VLAN (notez le Content-Type JSON)
curl -s -X POST -H "Authorization: Token $NETBOX_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"name": "SRV-Production", "vid": 110, "site": "siege-paris11",
          "status": "active", "description": "VLAN serveurs prod"}' \
     "$NETBOX_URL/api/ipam/vlans/" | python3 -m json.tool

# Créer une adresse IP (le masque fait partie de la valeur)
curl -s -X POST -H "Authorization: Token $NETBOX_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"address": "10.10.10.99/24", "status": "reserved",
          "description": "Réservée — futur cluster"}' \
     "$NETBOX_URL/api/ipam/ip-addresses/" | python3 -m json.tool

# Modifier (PATCH) la description d'un préfixe existant (id=12)
curl -s -X PATCH -H "Authorization: Token $NETBOX_TOKEN" \
     -H "Content-Type: application/json" \
     -d '{"description": "LAN utilisateurs — bâtiment A (maj sept 2026)"}' \
     "$NETBOX_URL/api/ipam/prefixes/12/" | python3 -m json.tool

# Supprimer (DELETE) — irréversible, à manier avec précaution
# curl -s -X DELETE -H "Authorization: Token $NETBOX_TOKEN" \
#      "$NETBOX_URL/api/ipam/ip-addresses/456/"
```

> ⚠️ L'API respecte les permissions du compte propriétaire du token : un token
> « lecture seule » renverra `403 Forbidden` en écriture. C'est une sécurité, pas un bug.

---

## 54. API REST : Python avec `pynetbox` (installation et bases)

`pynetbox` est le client Python officiel : bien plus agréable que `curl` pour scripter.

```bash
# Dans un venv dédié à vos scripts (pas celui de NetBox !)
python3 -m venv ~/venv-scripts && source ~/venv-scripts/bin/activate
pip install pynetbox
```

```python
#!/usr/bin/env python3
"""Connexion de base à NetBox et lecture d'objets."""
import os
import pynetbox

nb = pynetbox.api(
    "https://netbox.lan-entreprise.fr",
    token=os.environ["NETBOX_TOKEN"],
)
nb.http_session.verify = True   # vérification TLS (certificat interne : voir encadré)

# Lister les sites actifs
for site in nb.dcim.sites.filter(status="active"):
    print(f"{site.name:20s} {site.region.name if site.region else '-'}")

# Chercher un équipement par nom
sw = nb.dcim.devices.get(name="SIEGE-SW-A01-01")
print(sw.name, "->", sw.device_type.model, "| baie:", sw.rack.name, "| U:", sw.position)

# Toutes les IP d'un préfixe, avec leur statut
prefix = nb.ipam.prefixes.get(prefix="10.10.20.0/24")
for ip in nb.ipam.ip_addresses.filter(parent=str(prefix.prefix)):
    print(f"{ip.address}  [{ip.status.value}]  {ip.description or ''}")
```

> 💡 **TLS avec PKI interne** : si votre certificat est signé par votre CA d'entreprise,
> ajoutez-la au bundle de confiance du système (`/usr/local/share/ca-certificates/` +
> `update-ca-certificates`), ou passez `nb.http_session.verify = "/chemin/ca.pem"`.
> Ne mettez JAMAIS `verify = False` en production (attaque MITM possible).

---

## 55. API REST : créer un préfixe et une VM en Python (exemple complet)

```python
#!/usr/bin/env python3
"""Provisionner un préfixe + une VM via l'API NetBox."""
import os
import pynetbox

nb = pynetbox.api("https://netbox.lan-entreprise.fr", token=os.environ["NETBOX_TOKEN"])

# 1. Créer un préfixe enfant dans le container 10.10.0.0/16
prefix = nb.ipam.prefixes.create({
    "prefix": "10.10.60.0/24",
    "site": "siege-paris11",
    "vlan": {"vid": 60, "name": "SRV-Test"},   # le VLAN doit exister
    "status": "active",
    "role": "Serveurs",
    "tenant": "dsi",
    "description": "VLAN 60 — serveurs de test (provisionné par API)",
})
print("Préfixe créé :", prefix.prefix, "| id:", prefix.id)

# 2. Réserver la première IP utilisable pour la passerelle
gw = nb.ipam.ip_addresses.create({
    "address": "10.10.60.1/24",
    "status": "active",
    "description": "Passerelle VLAN 60",
})
print("IP passerelle :", gw.address)

# 3. Créer une VM dans le cluster Proxmox
vm = nb.virtualization.virtual_machines.create({
    "name": "VM-TEST-01",
    "cluster": "pve-siege",
    "status": "active",
    "role": "Applicatif",
    "tenant": "dsi",
    "vcpus": 2,
    "memory": 4096,
    "disk": 40,
    "platform": "debian-12",
    "description": "VM de test — créée par script",
})
print("VM créée :", vm.name)

# 4. Interface + IP pour la VM
iface = nb.virtualization.interfaces.create({
    "virtual_machine": vm.id, "name": "eth0", "enabled": True,
})
ip = nb.ipam.ip_addresses.create({
    "address": "10.10.60.10/24", "status": "active",
    "assigned_object_type": "virtualization.vminterface",
    "assigned_object_id": iface.id,
    "description": "Interface de production",
})
# Définir l'IP primaire de la VM (pratique : affichée dans les listes)
vm.primary_ip4 = ip.id
vm.save()
print("VM provisionnée avec IP :", ip.address)
```

Ce script est le patron de tous vos provisionnements : **créer dans NetBox D'ABORD,
déployer ensuite** — jamais l'inverse.

---

## 56. API REST : synchroniser un serveur DHCP (exemple d'usage avancé)

Exemple : générer la configuration des réservations DHCP (ISC DHCPd) depuis NetBox.

```python
#!/usr/bin/env python3
"""Génère les host declarations ISC-DHCP depuis les IP 'reserved' d'un préfixe."""
import os
import pynetbox

nb = pynetbox.api("https://netbox.lan-entreprise.fr", token=os.environ["NETBOX_TOKEN"])

print("# Généré depuis NetBox — ne pas éditer à la main")
for ip in nb.ipam.ip_addresses.filter(parent="10.10.20.0/24", status="reserved"):
    # On ne génère que les IP assignées à une interface avec une MAC connue
    obj = ip.assigned_object
    mac = getattr(obj, "mac_address", None)
    if not mac:
        continue
    hostname = (ip.dns_name or ip.description or f"host-{ip.id}").split(".")[0]
    print(f"host {hostname} {{")
    print(f"  hardware ethernet {mac.lower()};")
    print(f"  fixed-address {str(ip.address).split('/')[0]};")
    print("}")
```

Chaîne d'automatisation complète :

1. Le technicien réserve l'IP dans NetBox (statut `Réservé`, MAC renseignée).
2. Ce script tourne chaque nuit (cron) et régénère `/etc/dhcp/dhcpd.conf`.
3. Le service DHCP recharge la conf. **Zéro saisie en double**, zéro dérive.

> ⚠️ Versionnez le fichier généré (git) : en cas de génération corrompue, le
> `dhcpd -t` (test de syntaxe) avant rechargement est obligatoire dans le script cron.

---

## 57. Webhooks : réagir aux événements NetBox

Les **webhooks** envoient un POST HTTP à chaque création/modification/suppression
d'objet : le pont entre NetBox et vos outils (Slack/Teams, GLPI, scripts maison).

**Admin > Webhooks > + Ajouter** :

| Champ | Exemple |
|---|---|
| Nom | `notify-teams-device-change` |
| URL | `https://logic-app-entreprise.azure.com/...` |
| Méthode HTTP | `POST` |
| Content type | `application/json` |
| Secret | `CHANGEZ_MOI_Webhook_Exemple` (signature HMAC) |
| Événements | mise à jour d'équipement |

Exemple de récepteur minimal (Python/Flask) qui journalise les changements :

```python
from flask import Flask, request
import hmac, hashlib

app = Flask(__name__)
SECRET = b"CHANGEZ_MOI_Webhook_Exemple"

@app.post("/netbox-hook")
def hook():
    sig = request.headers.get("X-Hook-Signature", "")
    expected = hmac.new(SECRET, request.data, hashlib.sha512).hexdigest()
    if not hmac.compare_digest(sig, expected):
        return "signature invalide", 403
    event = request.json
    print(f"[{event['event']}] {event['model']} -> {event['data']['name']}")
    return "ok", 200
```

Cas d'usage : notifier l'équipe réseau sur Teams à chaque ajout d'équipement,
déclencher un inventaire Ansible à chaque modification d'IP, alimenter un CMDB externe.

> ⚠️ Les webhooks sont traités par `netbox-rq` (file `high`) : si le service est
> arrêté, les événements s'accumulent dans Redis. Surveillez la profondeur des files
> (section 67).

---

## 58. Scripts personnalisés (custom scripts)

Les **scripts** s'exécutent DANS NetBox (Admin > Scripts) : formulaires web qui
appellent l'API interne. Parfait pour les actions récurrentes d'exploitation.

Emplacement : `/opt/netbox/netbox/netbox/scripts/` (ou un répertoire configuré via
`SCRIPTS_ROOT`). Exemple : script « Réserver les N prochaines IP d'un préfixe » :

```python
from netbox.scripts import Script, StringVar, IntegerVar
import ipaddress

class ReserveIPs(Script):
    class Meta:
        name = "Réserver des IP"
        description = "Réserve les N prochaines IP libres d'un préfixe."

    prefix = StringVar(description="Préfixe CIDR (ex. 10.10.20.0/24)")
    count = IntegerVar(description="Nombre d'IP à réserver", default_value=5)
    description = StringVar(description="Description commune", required=False)

    def run(self, data, commit):
        net = ipaddress.ip_network(data["prefix"])
        # Adresses déjà connues de NetBox dans ce préfixe
        used = {str(ip.address.ip) for ip in
                self.get_ips_in_prefix(data["prefix"])}
        reserved = []
        for host in net.hosts():
            if str(host) in used:
                continue
            if commit:
                # création réelle via l'ORM
                from ipam.models import IPAddress
                IPAddress.objects.create(
                    address=f"{host}/{net.prefixlen}",
                    status="reserved",
                    description=data.get("description") or "Réservée par script",
                )
            reserved.append(str(host))
            if len(reserved) >= data["count"]:
                break
        self.log_success(f"{len(reserved)} IP réservées : {', '.join(reserved)}")

    def get_ips_in_prefix(self, prefix):
        from ipam.models import IPAddress
        return IPAddress.objects.filter(
            address__net_contained=str(ipaddress.ip_network(prefix)))
```

> 💡 Les scripts tournent avec les droits de l'utilisateur qui les lance et
> s'exécutent d'abord en **mode test** (`commit=False`) : on voit ce qui VA se passer
> avant de l'appliquer. Idéal pour déléguer des actions aux techniciens sans leur
> donner un accès API brut.

---

## 59. Rapports personnalisés (custom reports)

Les **rapports** vérifient la cohérence des données et produisent des tableaux de bord
d'exploitation. Emplacement : `/opt/netbox/netbox/netbox/reports/` (`REPORTS_ROOT`).

Exemple : rapport « Équipements sans adresse IP de management » (classique !) :

```python
from netbox.reports import Report

class ManagementIPReport(Report):
    description = "Liste les équipements actifs sans IP de management assignée."

    def test_device_mgmt_ip(self):
        from dcim.models import Device
        bad = []
        for dev in Device.objects.filter(status="active").exclude(
                role__name__in=["Panneau-Brassage"]):
            has_mgmt = dev.interfaces.filter(
                mgmt_only=True, ip_addresses__isnull=False).exists()
            if not has_mgmt:
                bad.append(dev)
        if bad:
            self.log_failure(
                f"{len(bad)} équipements sans IP de management : "
                + ", ".join(d.name for d in bad[:20])
            )
        else:
            self.log_success("Tous les équipements actifs ont une IP de management.")

    def test_rack_utilization(self):
        from dcim.models import Rack
        for rack in Rack.objects.filter(status="active"):
            pct = rack.get_utilization()
            if pct >= 90:
                self.log_warning(f"Baie {rack.name} remplie à {pct} % — prévoir extension.")
        self.log_success("Contrôle du remplissage des baies terminé.")
```

Idées de rapports pour un service systèmes/énergies :

- Bilan de puissance par baie vs calibre onduleur/PDU (section 26.3),
- Garanties expirant dans < 90 jours (champ `date_fin_garantie`, section 47),
- Engagements opérateurs arrivant à échéance (section 27),
- Préfixes > 80 % d'utilisation,
- Câbles sans étiquette.

---

## 60. Jobs, files d'attente et exécution différée

NetBox 4.x exécute les tâches longues via **django-rq** (files `high`, `default`, `low`),
traitées par `netbox-rq.service` (section 14) :

| File | Contenu typique |
|---|---|
| `high` | webhooks |
| `default` | scripts personnalisés, imports CSV volumineux |
| `low` | rapports planifiés, nettoyage |

Surveillance via l'interface : **Admin > Files d'attente RQ** (ou en CLI) :

```bash
# État des files dans Redis
redis-cli -n 0 llen rq:queue:high
redis-cli -n 0 llen rq:queue:default
# Voir les jobs en échec
sudo -iu netbox /opt/netbox/venv/bin/python \
  /opt/netbox/netbox/netbox/manage.py rqinfo
```

> ⚠️ Une file `default` qui grossit sans se vider = `netbox-rq` arrêté ou planté.
> Symptômes côté utilisateurs : scripts qui restent « en cours » indéfiniment,
> webhooks jamais envoyés. Redémarrez le service et surveillez les logs.

---

## 61. Intégration Ansible : inventaire dynamique depuis NetBox

Le plugin d'inventaire `netbox.netbox.nb_inventory` fait de NetBox LA source de
l'inventaire Ansible : plus de fichier `hosts.ini` à maintenir.

```bash
# Sur le contrôleur Ansible
ansible-galaxy collection install netbox.netbox
pip install pynetbox
```

`inventory/netbox.yml` :

```yaml
plugin: netbox.netbox.nb_inventory
api_endpoint: https://netbox.lan-entreprise.fr
token: "{{ lookup('env', 'NETBOX_TOKEN') }}"   # jamais en clair !
validate_certs: true

# Groupes construits depuis les champs NetBox
group_by:
  - device_roles      # ex. group_switch-acces
  - sites             # ex. group_siege-paris11
  - tenants
plumb_graph: true     # expose les relations (voisins câblés)

# Variables d'hôte depuis NetBox
compose:
  ansible_host: primary_ip4.address | default(interfaces[0].ip_addresses[0].address, '')
  ansible_network_os: >-
    {'Cisco IOS': 'ios', 'Arista EOS': 'eos'}.get(platform.name, 'ios')

# On ne veut que les équipements actifs, joignables
filters:
  status: active
  has_primary_ip: true
```

Test :

```bash
export NETBOX_TOKEN="CHANGEZ_MOI_Token_Exemple"
ansible-inventory -i inventory/netbox.yml --graph | head -30
ansible -i inventory/netbox.yml group_switch-acces -m ping
```

> 💡 **Chaîne vertueuse** : un nouveau switch est créé dans NetBox (avec son IP et
> son rôle) → il apparaît automatiquement dans l'inventaire Ansible → le playbook
> de provisionnement le configure. Le provisionnement « NetBox d'abord » (section 55)
> prend ici tout son sens.

---

## 62. Intégration Zabbix : exporter vers la supervision

Deux stratégies complémentaires :

**A. Création automatique des hôtes Zabbix depuis NetBox** (script Python + API Zabbix) :

```python
#!/usr/bin/env python3
"""Crée dans Zabbix les équipements NetBox taggés 'supervise-zabbix'."""
import os
import pynetbox
from pyzabbix import ZabbixAPI

nb = pynetbox.api("https://netbox.lan-entreprise.fr", token=os.environ["NETBOX_TOKEN"])
zx = ZabbixAPI("https://zabbix.lan-entreprise.fr")
zx.login(os.environ["ZABBIX_USER"], os.environ["ZABBIX_PASSWORD"])

for dev in nb.dcim.devices.filter(tag="supervise-zabbix", status="active"):
    if not dev.primary_ip4:
        print(f"SKIP {dev.name} : pas d'IP primaire")
        continue
    ip = str(dev.primary_ip4.address).split("/")[0]
    # mapping rôle NetBox -> modèle Zabbix
    templates = {"Switch-Acces": "Cisco IOS SNMP",
                 "Serveur": "Linux by Zabbix agent",
                 "Onduleur": "UPS SNMP"}.get(dev.device_role.name, "ICMP Ping")
    print(f"CREATE {dev.name} ({ip}) template={templates}")
    # zx.host.create(...) — à adapter à votre version de Zabbix
```

**B. Zabbix → NetBox** : remonter les alertes vers un champ personnalisé
(`alarme_en_cours`) via webhook Zabbix → API NetBox : la fiche équipement affiche
l'état de supervision en temps réel.

> ⚠️ Pour les onduleurs (votre métier !) : supervisez via SNMP les variables
> `upsBatteryStatus`, `upsEstimatedChargeRemaining`, `upsSecondsOnBattery` et
> faites pointer la fiche NetBox de l'onduleur vers son hôte Zabbix (champ
> personnalisé `zabbix_hostid`). En incident énergie, on part de NetBox (chaîne
> électrique, section 26) et on bascule sur Zabbix (métriques temps réel).

---

## 63. Plugins utiles (écosystème NetBox 4.x)

Les plugins s'installent via pip dans le venv et se déclarent dans `configuration.py`
(`PLUGINS = [...]`). Sélection éprouvée :

| Plugin | Usage | Remarque |
|---|---|---|
| `netbox-topology-views` | cartes topologiques du réseau | très parlant en réunion |
| `netbox-bgp` | sessions BGP, communities | si vous faites du BGP |
| `netbox-dns` | zones DNS gérées dans NetBox | alternative au DNS dispersé |
| `netbox-inventory` | suivi des assets (garanties, mouvements) | lien avec l'inventaire comptable |
| `netbox-qrcode` | QR codes sur les fiches (baies, équipements) | flashez la baie → sa fiche ! |
| `netbox-attachments` | pièces jointes enrichies | selon version |

Exemple d'activation :

```python
PLUGINS = [
    "netbox_topology_views",
    "netbox_qrcode",
]
```

```bash
sudo -iu netbox /opt/netbox/venv/bin/pip install netbox-topology-views netbox-qrcode
sudo -iu netbox /opt/netbox/venv/bin/python /opt/netbox/netbox/netbox/manage.py migrate
systemctl restart netbox netbox-rq
```

> ⚠️ **Règle de prudence** : chaque plugin est du code tiers exécuté avec les droits
> de NetBox. N'installez que des plugins maintenus, compatibles avec votre version
> 4.x (vérifiez la matrice de compatibilité sur leur dépôt), et testez-les en LAB
> avant la production. Un plugin abandonné peut bloquer une montée de version.

---

## 64. Sauvegarde : base PostgreSQL (`pg_dump`)

La donnée métier vit dans PostgreSQL : la sauvegarde est non négociable, quotidienne,
et **testée**.

```bash
#!/bin/bash
# /opt/netbox/backup-netbox-db.sh  (propriétaire root, 700)
set -euo pipefail
BACKUP_DIR="/var/backups/netbox"
RETENTION=30
mkdir -p "$BACKUP_DIR"
DATE=$(date +%Y%m%d-%H%M%S)

# Dump complet au format custom (restauration sélective possible)
sudo -u postgres pg_dump -Fc netbox > "$BACKUP_DIR/netbox-$DATE.dump"
# + dump SQL texte (lisible, dépannage)
sudo -u postgres pg_dump netbox > "$BACKUP_DIR/netbox-$DATE.sql"

gzip -f "$BACKUP_DIR/netbox-$DATE.sql"
find "$BACKUP_DIR" -name 'netbox-*' -mtime +$RETENTION -delete
echo "OK $DATE : $(du -h "$BACKUP_DIR/netbox-$DATE.dump" | cut -f1)"
```

```bash
chmod 700 /opt/netbox/backup-netbox-db.sh
# Cron quotidien à 2h du matin
echo "0 2 * * * root /opt/netbox/backup-netbox-db.sh >> /var/log/netbox/backup.log 2>&1" \
  > /etc/cron.d/netbox-backup
```

Vérifications automatiques à ajouter : taille du dump > 0, test `pg_restore --list`
sur le dernier dump (intégrité), copie vers un stockage **externe** (NAS, S3, autre site).

> ⚠️ Un dump sur le même disque que la base ne protège que des erreurs logiques,
> pas d'une panne disque. La règle 3-2-1 s'applique : 3 copies, 2 supports, 1 hors site.

---

## 65. Sauvegarde : fichiers `media/` et `configuration.py`

```bash
#!/bin/bash
# /opt/netbox/backup-netbox-files.sh  (700)
set -euo pipefail
BACKUP_DIR="/var/backups/netbox"
DATE=$(date +%Y%m%d-%H%M%S)

# media/ : pièces jointes, images, scripts et rapports personnalisés
tar -czf "$BACKUP_DIR/netbox-media-$DATE.tar.gz" \
    -C /opt/netbox/netbox/netbox media

# configuration sensible (chiffrée au repos si possible)
tar -czf "$BACKUP_DIR/netbox-config-$DATE.tar.gz" \
    -C /opt/netbox/netbox/netbox configuration.py \
    -C /opt/netbox gunicorn.py

find "$BACKUP_DIR" -name 'netbox-media-*' -mtime +30 -delete
find "$BACKUP_DIR" -name 'netbox-config-*' -mtime +30 -delete
echo "OK $DATE"
```

Plan de sauvegarde complet (à documenter dans votre PRA) :

| Élément | Méthode | Fréquence | Rétention |
|---|---|---|---|
| Base PostgreSQL | `pg_dump -Fc` + `.sql.gz` | quotidienne 2h | 30 jours |
| `media/` | tar.gz | quotidienne 2h05 | 30 jours |
| `configuration.py`, `gunicorn.py` | tar.gz | à chaque changement + hebdo | 90 jours |
| Copie hors site | rsync vers NAS / S3 | quotidienne 3h | selon politique |
| Snapshot VM | hyperviseur | hebdo | 4 semaines |

---

## 66. Restauration : procédure complète (à tester chaque trimestre)

> ⚠️ Testez la restauration sur une VM de LAB, jamais en production la première fois.
> Chronométrez-la : c'est votre RTO réel.

```bash
# 1. Arrêter les services (évite toute écriture pendant la restauration)
systemctl stop netbox netbox-rq

# 2. Recréer une base vierge
sudo -u postgres psql -c "DROP DATABASE netbox;"
sudo -u postgres psql -c "CREATE DATABASE netbox OWNER netbox;"

# 3. Restaurer le dump (format custom)
sudo -u postgres pg_restore -d netbox -U netbox /var/backups/netbox/netbox-20260926-020000.dump

# 4. Restaurer les fichiers media/
tar -xzf /var/backups/netbox/netbox-media-20260926-020000.tar.gz \
    -C /opt/netbox/netbox/netbox/
chown -R netbox:netbox /opt/netbox/netbox/netbox/media

# 5. Redémarrer et vérifier
systemctl start netbox netbox-rq
sleep 5
curl -sk -o /dev/null -w "%{http_code}\n" https://localhost/   # 200 ou 302 attendu
sudo -iu netbox /opt/netbox/venv/bin/python \
  /opt/netbox/netbox/netbox/manage.py check
```

Checklist post-restauration : connexion admin OK, un site/une baie/une IP vérifiés
au hasard, un appel API avec un token existant, les files RQ vides et actives.

---

## 67. Supervision de NetBox lui-même

NetBox est un outil critique : s'il est en panne, l'équipe travaille à l'aveugle.
Supervisez-le comme une application de production :

| Sonde | Méthode | Seuil d'alerte |
|---|---|---|
| HTTPS | check HTTP sur `/login/` → 200 | critique si KO |
| Certificat TLS | expiration < 21 jours | warning, < 7 j critique |
| Processus gunicorn | `systemctl is-active netbox` | critique si inactif |
| File RQ | `llen rq:queue:default` | warning si > 50 |
| Espace disque | `/var/lib/postgresql`, `/var/backups` | warning 80 %, critique 90 % |
| Fraîcheur sauvegarde | âge du dernier dump | critique si > 26 h |
| PostgreSQL | connexion + `SELECT 1` | critique si KO |

Exemple avec Zabbix (agent) — `UserParameter` :

```bash
# /etc/zabbix/zabbix_agentd.d/netbox.conf
UserParameter=netbox.https, curl -sk -o /dev/null -w "%{http_code}" https://localhost/login/
UserParameter=netbox.rq.default, redis-cli -n 0 llen rq:queue:default
UserParameter=netbox.backup.age, echo $(( $(date +%s) - $(stat -c %Y /var/backups/netbox/netbox-*.dump | sort -n | tail -1) ))
```

> 💡 Ajoutez NetBox lui-même dans NetBox (section 28) avec le tag `supervise-zabbix` :
> la boucle est bouclée — l'outil qui documente la supervision est supervisé.

---

## 68. Montée de version de NetBox (4.x → 4.y)

> ⚠️ Ne sautez jamais de version majeure sans lire les *release notes* : chaque
> version 4.x peut exiger une version Python/PostgreSQL/Redis minimale différente
> et des migrations spécifiques.

Procédure type (exemple 4.2.3 → 4.3.0) :

```bash
# 0. SAUVEGARDE COMPLÈTE + snapshot VM (obligatoire)
# 1. Télécharger la nouvelle version (utilisateur netbox)
sudo -iu netbox
cd /opt/netbox
wget https://github.com/netbox-community/netbox/archive/refs/tags/v4.3.0.tar.gz -O netbox-4.3.0.tar.gz
tar -xzf netbox-4.3.0.tar.gz

# 2. Arrêter les services
systemctl stop netbox netbox-rq

# 3. Nouveau venv ou mise à jour des dépendances ? -> suivre les release notes.
#    En général : réutiliser le venv et mettre à jour requirements :
/opt/netbox/venv/bin/pip install -r /opt/netbox/netbox-4.3.0/requirements.txt

# 4. Recopier la configuration et les personnalisations
cp /opt/netbox/netbox/netbox/configuration.py /opt/netbox/netbox-4.3.0/netbox/netbox/
# + scripts/, reports/, media/ si personnalisés :
cp -r /opt/netbox/netbox/netbox/scripts/* /opt/netbox/netbox-4.3.0/netbox/netbox/scripts/ 2>/dev/null || true
cp -r /opt/netbox/netbox/netbox/reports/* /opt/netbox/netbox-4.3.0/netbox/netbox/reports/ 2>/dev/null || true

# 5. Basculer le lien symbolique
ln -sfn /opt/netbox/netbox-4.3.0 /opt/netbox/netbox

# 6. Migrations + statiques (dans le venv)
cd /opt/netbox/netbox
/opt/netbox/venv/bin/python netbox/manage.py migrate
/opt/netbox/venv/bin/python netbox/manage.py collectstatic --no-input

# 7. Redémarrer et vérifier
systemctl start netbox netbox-rq
/opt/netbox/venv/bin/python netbox/manage.py check
curl -sk -o /dev/null -w "%{http_code}\n" https://localhost/login/
```

En cas d'échec : rebasculez le lien symbolique vers l'ancienne version, restaurez
le dump pré-upgrade, redémarrez. D'où l'importance de l'étape 0.

---

## 69. Dépannage : méthode générale

Avant de chercher l'erreur précise, appliquez toujours ce triptyque :

1. **Les services tournent-ils ?**
   `systemctl status netbox netbox-rq postgresql redis-server nginx --no-pager`
2. **Que disent les logs ?** (dans cet ordre)
   - `/var/log/netbox/gunicorn-error.log` (erreurs applicatives),
   - `journalctl -u netbox -n 50` (service systemd),
   - `/var/log/nginx/error.log` (frontal),
   - `journalctl -u postgresql -n 30` (base).
3. **Qu'est-ce qui a changé ?** (mise à jour, `configuration.py` modifié, disque plein,
   certificat expiré — 80 % des pannes viennent d'un changement récent).

```bash
# Kit de survie en une commande : état global
echo "=== SERVICES ==="; systemctl is-active netbox netbox-rq postgresql redis-server nginx
echo "=== ECOUTE ==="; ss -ltnp | grep -E '8001|443'
echo "=== DISQUE ==="; df -h / /var/lib/postgresql /var/backups | awk '{print $1,$5,$6}'
echo "=== DERNIER DUMP ==="; ls -lt /var/backups/netbox/*.dump 2>/dev/null | head -1
```

Les sections 70 à 79 détaillent les 10 erreurs classiques rencontrées en production.

---

## 70. Erreur n°1 : « Bad Request (400) » juste après l'installation

**Symptômes** : nginx répond, mais NetBox affiche « Bad Request (400) ».

**Causes possibles** (par ordre de probabilité) :

1. Le nom d'hôte utilisé n'est pas dans `ALLOWED_HOSTS` (section 10). Vous accédez
   via l'IP `10.10.0.15` mais seuls `netbox.lan-entreprise.fr` sont autorisés.
2. En-tête `Host` non transmis par nginx (`proxy_set_header Host $host;` manquant).

**Diagnostic / remède** :

```bash
# Voir quel Host arrive réellement à Django
tail -20 /var/log/netbox/gunicorn-error.log
# Corriger configuration.py :
ALLOWED_HOSTS = ['netbox.lan-entreprise.fr', '10.10.0.15']
# puis :
python -m py_compile /opt/netbox/netbox/netbox/configuration.py && \
systemctl restart netbox
```

> 💡 En LAB uniquement, `ALLOWED_HOSTS = ['*']` dépanne, mais ne laissez JAMAIS
> cela en production (vulnérabilité Host header attack).

---

## 71. Erreur n°2 : page blanche / 500 après modification de `configuration.py`

**Symptômes** : erreur 500 sur toutes les pages après un changement de config.

**Cause n°1** : erreur de syntaxe Python dans `configuration.py` (virgule manquante,
guillemet non fermé). **Cause n°2** : service non redémarré après modification.

```bash
# 1. Vérifier la syntaxe AVANT tout :
sudo -iu netbox python3 -m py_compile /opt/netbox/netbox/netbox/configuration.py \
  && echo "syntaxe OK"

# 2. Lancer le check Django (détecte les paramètres invalides) :
sudo -iu netbox /opt/netbox/venv/bin/python \
  /opt/netbox/netbox/netbox/manage.py check

# 3. Lire l'erreur exacte :
tail -30 /var/log/netbox/gunicorn-error.log

# 4. Redémarrer PROPREMENT (les deux services) :
systemctl restart netbox netbox-rq
```

> 💡 Prenez l'habitude du rituel « py_compile → check → restart » : il élimine 90 %
> des erreurs 500 liées à la configuration.

---

## 72. Erreur n°3 : « could not connect to server » — PostgreSQL injoignable

**Symptômes** : 500 au démarrage, logs gunicorn : `connection refused`, `password
authentication failed for user "netbox"`.

**Arbre de décision** :

```bash
# PostgreSQL tourne-t-il ?
systemctl status postgresql --no-pager
pg_lsclusters

# Le mot de passe de configuration.py correspond-il ?
PGPASSWORD='...' psql -h localhost -U netbox -d netbox -c 'SELECT 1;'
# -> si KO ici, le problème est côté BDD (mot de passe, droits), pas côté NetBox.

# L'utilisateur a-t-il les droits sur la base ?
sudo -u postgres psql -c '\l' | grep netbox
sudo -u postgres psql -c "ALTER DATABASE netbox OWNER TO netbox;"

# Cas classique après restauration : objets appartenant à postgres au lieu de netbox
# -> refaire le pg_restore en tant que netbox, ou corriger avec :
# sudo -u postgres psql -d netbox -c "REASSIGN OWNED BY postgres TO netbox;"
```

> ⚠️ Après un `pg_restore` fait en `root`/`postgres`, pensez à `chown`-er les objets :
> sinon NetBox lit mais ne peut pas écrire (erreurs 500 à la première modification).

---

## 73. Erreur n°4 : Redis injoignable — cache et files KO

**Symptômes** : lenteurs extrêmes, webhooks jamais envoyés, scripts bloqués « en cours »,
erreurs `Error 111 connecting to localhost:6379. Connection refused.`

```bash
# Redis tourne-t-il ?
systemctl status redis-server --no-pager
redis-cli ping   # PONG attendu

# Si requirepass activé mais pas renseigné dans configuration.py :
redis-cli -a 'CHANGEZ_MOI_Redis_Exemple_2026!' ping

# Vérifier ce que NetBox attend :
grep -A6 "'tasks'" /opt/netbox/netbox/netbox/configuration.py

# Files d'attente bloquées ?
redis-cli -n 0 llen rq:queue:default
redis-cli -n 0 llen rq:queue:high
```

**Remède** : alignez la configuration des deux côtés (mot de passe identique ou
absent des deux côtés), `systemctl restart redis-server netbox netbox-rq`.

---

## 74. Erreur n°5 : `collectstatic` oublié — interface sans CSS

**Symptômes** : après installation ou montée de version, l'interface s'affiche « nue »
(HTML brut, sans mise en forme), ou des icônes/images manquent.

**Cause** : les fichiers statiques n'ont pas été (re)générés, ou nginx ne les sert pas.

```bash
# 1. Régénérer :
sudo -iu netbox /opt/netbox/venv/bin/python \
  /opt/netbox/netbox/netbox/manage.py collectstatic --no-input

# 2. Vérifier le alias nginx :
grep -A2 'location /static/' /etc/nginx/sites-enabled/netbox
# doit pointer vers /opt/netbox/netbox/netbox/static/

# 3. Droits :
ls -ld /opt/netbox/netbox/netbox/static
# lisible par www-data (nginx) : chmod -R a+rX si besoin

nginx -t && systemctl reload nginx
```

> 💡 Après CHAQUE montée de version : `collectstatic` est obligatoire (les assets
> changent). Ajoutez-le à votre checklist de mise à jour (section 68, étape 6).

---

## 75. Erreur n°6 : conflits d'IP et doublons refusés

**Symptômes** : « Cette adresse IP existe déjà » / « Ce préfixe chevauche un préfixe
existant » lors d'une création.

**Ce n'est pas un bug, c'est la protection** : NetBox refuse les doublons dans la
même VRF. Conduite à tenir :

1. Cherchez l'objet existant (recherche globale) : il a peut-être été créé par un
   collègue avec un nom différent.
2. S'il s'agit d'une erreur de saisie (mauvais masque, mauvaise VRF), corrigez
   l'objet existant plutôt que d'en créer un second.
3. Si le chevauchement est légitime (deux usages distincts du même plan), utilisez
   des **VRF** différentes (section 31) : c'est exactement à ça qu'elles servent.
4. Pour les imports CSV massifs : dédupliquez AVANT l'import
   (`sort -u`, ou script Python), sinon l'import s'arrête à la première collision.

---

## 76. Erreur n°7 : token API rejeté (401/403)

**Symptômes** : `curl` renvoie `{"detail":"Invalid token."}` (401) ou
`{"detail":"You do not have permission..."}` (403).

| Code | Cause | Remède |
|---|---|---|
| 401 | token faux, expiré ou révoqué | régénérez-le dans le profil utilisateur |
| 401 | en-tête mal formé | format exact : `Authorization: Token <token>` (pas `Bearer`) |
| 403 | token en lecture seule + tentative d'écriture | cochez « Write enabled » ou utilisez un token dédié |
| 403 | permissions insuffisantes du compte | vérifiez les groupes/permissions (section 43) |

```bash
# Tester un token (doit renvoyer 200 + JSON) :
curl -s -o /dev/null -w "%{http_code}\n" \
  -H "Authorization: Token $NETBOX_TOKEN" \
  https://netbox.lan-entreprise.fr/api/users/me/
```

> 💡 Créez des tokens **par usage** (`svc-ansible-lecture`, `svc-zabbix`, `script-dhcp`)
> avec le périmètre minimal : en cas de fuite, on révoque UN token sans tout casser.

---

## 77. Erreur n°8 : migrations en échec lors de l'installation / upgrade

**Symptômes** : `python netbox/manage.py migrate` s'interrompt avec une traceback
(`django.db.utils...`, `duplicate key`, `relation already exists`).

Conduite à tenir :

```bash
# 1. NE RELANCEZ PAS en boucle : lisez l'erreur complète
sudo -iu netbox /opt/netbox/venv/bin/python \
  /opt/netbox/netbox/netbox/manage.py migrate 2>&1 | tail -40

# 2. Cas fréquent : migration déjà partiellement appliquée
#    -> revenir au snapshot pré-upgrade (section 68, étape 0), COMPRENDRE, puis rejouer.

# 3. Vérifier l'état des migrations :
sudo -iu netbox /opt/netbox/venv/bin/python \
  /opt/netbox/netbox/netbox/manage.py showmigrations | grep -v '\[X\]'
```

**Prévention** : upgrade toujours testé en LAB sur une copie de la BDD de production
(dump restauré). Une migration qui échoue en LAB échouera en production : c'est le
moment de lire les release notes, pas le jour J à 3h du matin.

---

## 78. Erreur n°9 : lenteurs — l'interface rame, l'API timeout

**Symptômes** : pages > 5 s, timeouts gunicorn (120 s), exports CSV qui n'aboutissent pas.

Pistes, dans l'ordre :

1. **Workers gunicorn sous-dimensionnés** : `(2 × vCPU) + 1` minimum (section 13).
   Surveillez `htop` pendant une heure de pointe.
2. **PostgreSQL** : `shared_buffers` trop bas, disque saturé (`df -h`), ou absence
   d'index après un gros import → `VACUUM ANALYZE;` (utilisateur postgres).
3. **Requêtes API sans pagination** : `?limit=1000` au lieu de tout charger ;
   côté scripts, itérez sur `nb.ipam.prefixes.all()` (pynetbox gère la pagination).
4. **Redis saturé** : `redis-cli info memory` → `used_memory` proche de `maxmemory` ?
   Augmentez la RAM ou la limite.
5. **Debug toolbar / DEBUG=True** : vérifiez `DEBUG = False` en production.

```sql
-- Taille des plus grosses tables (psql, base netbox)
SELECT relname, pg_size_pretty(pg_total_relation_size(relid))
FROM pg_stat_user_tables ORDER BY pg_total_relation_size(relid) DESC LIMIT 10;
```

---

## 79. Erreur n°10 : certificat TLS expiré ou chaîne incomplète

**Symptômes** : les navigateurs affichent « connexion non sécurisée », les scripts API
échouent avec `SSL: CERTIFICATE_VERIFY_FAILED`, l'inventaire Ansible est vide.

```bash
# Vérifier l'expiration (depuis un poste) :
echo | openssl s_client -connect netbox.lan-entreprise.fr:443 -servername \
  netbox.lan-entreprise.fr 2>/dev/null | openssl x509 -noout -dates

# Chaîne complète ? (doit lister les intermédiaires)
openssl s_client -connect netbox.lan-entreprise.fr:443 -showcerts </dev/null 2>/dev/null \
  | grep -c 'BEGIN CERTIFICATE'

# Renouvellement Let's Encrypt :
certbot renew --dry-run   # test
certbot renew              # réel (recharge nginx automatiquement)
```

**Prévention** : sonde Zabbix « expiration certificat » (section 67) avec alerte à
J-21. Pour la PKI interne : notez la date d'expiration dans un champ personnalisé
ou un ticket récurrent — les certificats internes sont la cause n°1 des pannes
« mystères » un vendredi soir.

---

## 80. Erreur n°11 : import CSV partiellement en échec

**Symptômes** : sur 500 lignes importées, 120 rejetées avec des messages d'erreur
parfois cryptiques.

Méthode :

1. **Téléchargez le modèle** : faites d'abord un export CSV d'objets existants pour
   voir les en-têtes et formats exacts (slugs, pas les noms !).
2. Importez par **petits lots** (50 lignes) : l'erreur est plus facile à localiser.
3. Erreurs fréquentes :
   - `site` inexistant → le slug doit matcher EXACTEMENT (`siege-paris11`, pas `Siège`),
   - statut invalide → valeurs anglaises en base (`active`, pas `actif`) selon version,
   - VLAN référencé par VID sans groupe → ambiguïté si plusieurs groupes.
4. Corrigez le CSV, réimportez **uniquement les lignes en échec** (pas tout le fichier,
   sinon doublons → voir erreur n°6).

> 💡 Gardez chaque CSV d'import versionné (git) avec sa date : c'est la preuve de ce
> qui a été injecté, et ça permet de rejouer un import après correction.

---

## 81. Erreur n°12 : « permission denied » sur `media/` après restauration

**Symptômes** : les pièces jointes ne s'affichent plus, l'upload échoue
(`PermissionError: [Errno 13]`), après une restauration ou une montée de version.

```bash
# Le coupable habituel : l'extraction tar en root a écrasé les propriétaires
ls -ld /opt/netbox/netbox/netbox/media
# -> doit appartenir à netbox:netbox

# Réparation :
chown -R netbox:netbox /opt/netbox/netbox/netbox/media
chmod -R u+rwX,go+rX /opt/netbox/netbox/netbox/media
# + durcissement systemd : vérifier ReadWritePaths dans netbox.service (section 14)
systemctl restart netbox netbox-rq
```

**Prévention** : extrayez toujours les archives en tant que `netbox`
(`sudo -iu netbox tar -xzf ...`), ou corrigez les propriétaires juste après.
Ajoutez ce contrôle à votre checklist de restauration (section 66).

---

---

## 82. Bonnes pratiques : convention de nommage (à faire valider en équipe)

Sans convention, NetBox devient un dépotoir en 6 mois. Faites valider ce référentiel
par l'équipe, affichez-le, et ne transigez pas.

| Objet | Format | Exemple |
|---|---|---|
| Site | `TYPE-Ville` (majuscules) | `SIEGE-Paris11`, `AGENCE-Lyon`, `DC-Gravelines` |
| Baie | `SITE-Fonction-NN` | `SIEGE-Baie-A01`, `SIEGE-Brassage-B02` |
| Équipement réseau | `SITE-Rôle-Baie-NN` | `SIEGE-SW-A01-01`, `SIEGE-RTR-01` |
| Serveur physique | `SITE-SRV-Usage-NN` | `SIEGE-SRV-HYP-01` |
| VM | `VM-Usage-NN` | `VM-NETBOX-01`, `VM-FICHIERS-02` |
| Onduleur | `SITE-UPS-Baie` | `SIEGE-UPS-A01` |
| PDU | `SITE-PDU-Baie-Voie` | `SIEGE-PDU-A01-A` |
| Circuit | `CT-Fournisseur-AAAA-NNN` | `CT-OBS-2024-001` |
| VLAN | `Usage` (lisible) + VID | `SRV-Production (110)` |
| Tenant | `MAJUSCULES-tirets` | `DSI`, `CLIENT-DUPONT` |

Règles transverses :

- **Pas d'accents, pas d'espaces, pas de caractères spéciaux** dans les noms techniques
  (les descriptions, elles, peuvent être en français courant).
- **Zéros non significatifs** pour le tri : `SW-01`, `SW-02` … `SW-12` (pas `SW-1`).
- **Immuabilité** : on ne renomme pas un équipement « parce que » — le nom est repris
  dans les scripts, la supervision, les tickets. Renommage = procédure (changer aussi
  DNS, Zabbix, Ansible).

---

## 83. Bonnes pratiques : tenancy systématique

Chaque objet NetBox peut porter un tenant : **utilisez-le toujours**. Le tenancy
répond à trois questions vitales :

1. **« À qui appartient cet équipement ? »** → refacturation, fin de contrat client.
2. **« Que dois-je éteindre si le client X part ? »** → filtre par tenant = liste exacte.
3. **« Qu'est-ce qui est mutualisé ? »** → tenant `INFRA-MUTUALISEE` pour le cœur,
   les onduleurs, les baies partagées.

Matrice de rattachement conseillée :

| Objet | Tenant |
|---|---|
| Baie partagée, onduleur, cœur de réseau | `INFRA-MUTUALISEE` |
| Serveurs du SI interne | `DSI` |
| Équipements dédiés à un client hébergé | `CLIENT-<Nom>` |
| Vidéosurveillance | `SECURITE` |
| Objets de test / bac à sable | `BAC-A-SABLE` |

> 💡 Le tenant `BAC-A-SABLE` est votre soupape : les nouveaux utilisateurs, les tests
> d'import CSV et les scripts en développement s'y déroulent. On y fait des bêtises
> sans conséquence, puis on nettoie (filtre par tenant → suppression en masse).

---

## 84. Règle d'or : « documenter AVANT de câbler »

C'est LA règle qui fait la différence entre un NetBox utile et un NetBox décoratif :

> **Aucune intervention physique (brassage, ajout/retrait d'équipement, déplacement)
> sans fiche NetBox créée ou mise à jour AVANT de toucher au matériel.**

Déclinaison opérationnelle :

1. **Préparation** : créez l'équipement, sa position en baie, ses interfaces, les
   câbles prévus (statut `Planifié` si votre workflow le prévoit).
2. **Intervention** : le technicien travaille AVEC la fiche NetBox ouverte
   (ou le QR code de la baie, plugin section 63).
3. **Clôture** : vérification (positions, étiquettes de câbles, photos), passage des
   statuts à `Actif`, clôture du ticket avec lien vers la fiche.

Pour l'imposer : intégrez-la aux **procédures d'intervention** et aux **revues d'équipe**
(« la baie B02 a été recâblée : les fiches sont à jour ? »). Un chef de service qui
montre l'exemple sur ses propres interventions fait plus que dix notes de service.

---

## 85. Hygiène des données : revues périodiques

NetBox se périme comme tout document : planifiez des revues.

| Revue | Fréquence | Action |
|---|---|---|
| Objets `Planifié`/`Réservé` vieux de > 90 j | mensuelle | activer ou supprimer |
| Équipements `En stock` depuis > 6 mois | trimestrielle | réaffecter ou sortir |
| IP `Réservées` jamais assignées | trimestrielle | libérer |
| Tags temporaires (`a-verifier`…) | mensuelle | traiter puis supprimer le tag |
| Comptes utilisateurs inactifs | trimestrielle | désactiver |
| Champs personnalisés inutilisés | annuelle | supprimer |
| Sites/baies `Déprécié` | annuelle | archiver (export) puis supprimer |

Confiez chaque revue à un **référent** (tournante dans l'équipe) : la qualité des
données est une responsabilité partagée, pas « le problème de l'admin NetBox ».

---

## 86. Cas pratique n°1 : documenter un site complet (fil rouge)

**Contexte** : nouveau site `AGENCE-Nantes` (15 personnes). À documenter : 1 baie 42U,
1 onduleur 3 kVA, 2 PDU, 2 switchs d'accès + 1 routeur, 2 serveurs, 1 NAS, câblage,
adressage `10.30.0.0/16`, VLAN.

### Étape 1 — Arborescence géographique et tenants

```
Région : France
└── Groupe de sites : Agences
    └── Site : AGENCE-Nantes (adresse, GPS, tenant DSI)
Tenant : DSI (+ INFRA-MUTUALISEE pour l'onduleur/la baie si partagés)
```

### Étape 2 — Plan d'adressage (préfixe container + enfants)

| Préfixe | VLAN | Rôle | Usage |
|---|---|---|---|
| `10.30.0.0/16` | — | (Container) | bloc du site |
| `10.30.10.0/24` | 10 | Serveurs | serveurs + NAS |
| `10.30.20.0/24` | 20 | LAN-Utilisateurs | 15 postes (DHCP `.50`–`.150`) |
| `10.30.30.0/24` | 30 | WiFi | AP + invités (VRF-GUEST pour les invités !) |
| `10.30.40.0/24` | 40 | Impression | 2 MFP |
| `10.30.254.0/24` | 254 | Management | switchs, routeur, iDRAC, carte onduleur |

Créez les VLAN 10/20/30/40/254 dans le groupe `VLAN-Nantes`, puis les préfixes
(statut `Actif`, sauf le `/16` en `Container`), puis les plages DHCP.

### Étape 3 — Baie et chaîne électrique

```
Baie : AGENCE-Baie-A01 (42U, rôle Baie-Reseau, tenant INFRA-MUTUALISEE)
U42 : (vide — ventilation haute)
U40 : AGENCE-UPS-A01 (onduleur 3 kVA, 2U → U40-U41)
U38 : AGENCE-PDU-A01-A (0U vertical — noté en commentaire si 0U)
U37 : AGENCE-PDU-A01-B
U30 : AGENCE-RTR-01 (routeur, 1U)
U28 : AGENCE-SW-A01-01 (switch 48p, 1U)
U27 : AGENCE-SW-A01-02 (switch 48p, 1U)
U20 : AGENCE-SRV-HYP-01 (serveur 2U → U20-U21)
U18 : AGENCE-SRV-HYP-02 (serveur 2U → U18-U19)
U10 : AGENCE-NAS-01 (NAS 2U → U10-U11)
U01-U05 : réserve (réservation NetBox : "extension 2027")
```

Câblage électrique (ports d'alimentation) :

```
AGENCE-UPS-A01:Sortie-1 --> AGENCE-PDU-A01-A:Entrée
AGENCE-UPS-A01:Sortie-2 --> AGENCE-PDU-A01-B:Entrée
AGENCE-SRV-HYP-01:PSU1 --> PDU-A:Prise-01   / PSU2 --> PDU-B:Prise-01
AGENCE-SRV-HYP-02:PSU1 --> PDU-A:Prise-02   / PSU2 --> PDU-B:Prise-02
AGENCE-NAS-01:PSU1     --> PDU-A:Prise-03   / PSU2 --> PDU-B:Prise-03
AGENCE-SW-A01-01:PSU1  --> PDU-A:Prise-04   (mono-alim : noter "pas de redondance")
```

> ⚠️ Le switch mono-alimenté est un **risque documenté** : ajoutez le tag
> `mono-alimentation` et un champ personnalisé `criticite=Majeur`. En cas de
> maintenance PDU-A, on sait qu'il faut planifier une coupure.

### Étape 4 — Câblage data et uplink

```
AGENCE-RTR-01:Gi0/0 --fibre OM4 10m--> AGENCE-SW-A01-01:SFP-49 (uplink, trunk)
AGENCE-SW-A01-01:SFP-50 --fibre--> AGENCE-SW-A01-02:SFP-49 (stack/uplink)
AGENCE-SW-A01-01:Gi1/0/1 --Cat6a 3m--> AGENCE-SRV-HYP-01:eth0 (VLAN 10, access)
AGENCE-SW-A01-01:Gi1/0/2 --Cat6a 3m--> AGENCE-SRV-HYP-02:eth0
Patch panel : AGENCE-PB-01 (24 ports) en U25, rocade vers les prises murales.
```

### Étape 5 — Adressage et management

- IP management : `10.30.254.11` (RTR), `.12`/`.13` (SW), `.21`/`.22` (iDRAC),
  `.30` (carte réseau onduleur — supervision SNMP !).
- Passerelles : `.1` sur chaque VLAN (VRRP si deux routeurs).
- DNS : `agence-nantes-sw-01.lan-entreprise.fr`… renseigné dans chaque fiche IP.
- Services : `ssh`/`https`/`snmp` sur les équipements réseau.

### Étape 6 — Vérifications finales (checklist)

- [ ] Chaque équipement a : site, baie+U, n° de série, IP de management, tenant
- [ ] Chaque câble a : type, longueur, étiquette
- [ ] La chaîne électrique est complète (aucun équipement « flottant » sans PDU)
- [ ] Le rapport « bilan de puissance » ne dépasse pas 80 % du calibre onduleur
- [ ] Photos de la baie jointes à la fiche
- [ ] Export CSV archivé dans la documentation du site

---

## 87. Cas pratique n°2 : refonte du plan d'adressage d'un site existant

**Contexte** : le site `SIEGE-Paris11` utilise encore du `192.168.1.0/24` « historique »
partagé entre serveurs, utilisateurs et imprimantes. Objectif : migrer vers un plan
propre sans couper la production.

1. **Inventaire de l'existant** : scan + table ARP + DHCP → importez TOUT dans NetBox
   dans le préfixe `192.168.1.0/24` (statut `Déprécié`, rôle par usage réel constaté).
   Vous avez maintenant la photo exacte du chaos.
2. **Concevez la cible** : nouveau container `10.10.0.0/16`, découpé par rôle
   (section 34). Créez les préfixes en statut `Réservé`.
3. **Plan de migration par VLAN** : un VLAN = une soirée d'intervention.
   Ex. semaine 1 : VLAN 40 (imprimantes — faible risque), semaine 2 : VLAN 10 (serveurs).
4. **Double documentation pendant la migration** : l'IP ancienne (statut `Déprécié`,
   description « migré vers 10.10.x.y le … ») reste 30 jours, puis suppression.
5. **Bascule** : changez les statuts (`Réservé` → `Actif`, ancien → `Déprécié`),
   mettez à jour DHCP/DNS via les scripts (section 56), vérifiez avec le rapport
   « IP sans DNS » et « équipements sans IP de management » (section 59).
6. **Clôture** : quand le vieux préfixe est vide, supprimez-le. Fêtez ça. 🎉

> 💡 Pendant toute la migration, le **changelog** (section 45) est votre meilleur ami :
> chaque changement est horodaté et attribué. En cas de rollback, on sait exactement
> quoi défaire.

---

## 88. Cas pratique n°3 : migrer un tableur d'inventaire vers NetBox

**Contexte** : 300 lignes Excel « inventaire réseau » (nom, IP, modèle, baie, U).
Objectif : import propre, sans doublons, avec traçabilité.

1. **Nettoyez le tableur** : une colonne = un champ NetBox ; normalisez les noms
   selon la convention (section 82) ; vérifiez l'unicité des noms et des IP
   (`=NB.SI` / suppressions de doublons).
2. **Préparez le référentiel** : fabricants, types d'équipements, rôles, site, baie,
   VLAN, préfixes — l'import des équipements ÉCHOUE si le type ou le site n'existe pas.
3. **Importez par couches**, dans l'ordre des dépendances :
   `sites → baies → types → équipements → interfaces → IP → câbles`.
4. **Testez sur 5 lignes** dans le tenant `BAC-A-SABLE`, vérifiez, supprimez, puis
   importez le lot réel par paquets de 50 (section 81).
5. **Réconciliez** : rapport « équipements sans IP » + « IP sans équipement » ;
   traitez les écarts (le tableur mentait probablement sur 5–10 % des lignes —
   c'est normal, c'est pour ça qu'on migre).
6. **Archivez le tableur** en pièce jointe d'un ticket « migration inventaire »,
   puis **supprimez-le des partages** : deux sources de vérité = zéro source de vérité.

---

## 89. Cas pratique n°4 : préparer une maintenance onduleur sans surprise

**Contexte** : remplacement des batteries de `SIEGE-UPS-A01` (bypass + arrêt).
C'est LE cas qui justifie la modélisation électrique (section 26).

1. **Liste d'impact** : depuis la fiche de l'onduleur > onglet **Alimentations connectées**,
   exportez tous les équipements alimentés (via PDU-A et PDU-B).
2. **Tri par criticité** : champ personnalisé `criticite` (section 47) → les
   `Critique` (cœur, firewalls) exigent un créneau validé par la direction.
3. **Vérifiez la redondance** : pour chaque équipement, contrôlez qu'il a bien ses
   deux PSU câblées (A ET B). Les mono-alimentés (tag `mono-alimentation`) seront
   coupés : planifiez leur arrêt propre AVANT.
4. **Ordre d'arrêt/démarrage** : documentez-le dans la description de l'intervention
   (VM d'abord, hyperviseurs ensuite, stockage en dernier à l'arrêt ; inverse au
   redémarrage — les dépendances sont visibles via les câbles et les rôles).
5. **Le jour J** : passez les équipements en statut `En maintenance` dans NetBox
   (ils disparaissent des vues « Actif », la supervision peut être mise en pause
   proprement via l'export Zabbix, section 62).
6. **Retour** : repassez en `Actif`, joignez le rapport d'intervention et la photo
   des batteries neuves à la fiche de l'onduleur, mettez à jour `date_fin_garantie`.

> 💡 Ce cas pratique, mené une fois « pour de vrai », convertit les derniers
> sceptiques : « on a su EXACTEMENT ce qui allait s'éteindre » vaut tous les discours.

---

## 90. Cas pratique n°5 : audit de sécurité express depuis NetBox

**Contexte** : le RSSI demande « qu'est-ce qui est exposé sur le site de Lyon ? ».
Réponse en 30 minutes, sans scanner réseau :

1. **Périmètre** : filtre `site=AGENCE-Lyon, statut=Actif` sur les équipements →
   export CSV.
2. **Services exposés** : onglet Services (section 39) → listez `https`, `ssh`,
   `telnet` (⚠️ si vous en trouvez, c'est un finding d'audit à lui tout seul).
3. **Management** : rapport « équipements sans IP de management » (section 59) →
   tout équipement administrable sans IP tracée = zone d'ombre.
4. **Comptes** : Admin > Utilisateurs → derniers accès, comptes inactifs depuis
   > 90 jours → à désactiver (section 85).
5. **Accès API** : tokens sans expiration, tokens `write` inutilisés → révoquer.
6. **Livrable** : un export CSV + 10 lignes de synthèse. Le RSSI adore les tableaux
   courts avec des chiffres.

---

## 91. Pense-bête de poche — commandes essentielles

### Services et logs

```bash
systemctl status netbox netbox-rq postgresql redis-server nginx --no-pager
systemctl restart netbox netbox-rq          # après modif configuration.py
journalctl -u netbox -n 50 --no-pager       # logs applicatifs
tail -50 /var/log/netbox/gunicorn-error.log # erreurs Django/gunicorn
tail -50 /var/log/nginx/error.log           # erreurs frontal
```

### Django / NetBox (utilisateur netbox, venv activé)

```bash
cd /opt/netbox/netbox
python netbox/manage.py check               # santé de la configuration
python netbox/manage.py migrate             # appliquer les migrations
python netbox/manage.py collectstatic --no-input
python netbox/manage.py createsuperuser     # (une seule fois)
python netbox/manage.py changepassword admin
python netbox/manage.py clearsessions       # ménage hebdomadaire
python netbox/manage.py rqworker high default low   # worker manuel (debug)
```

### Base de données

```bash
sudo -u postgres pg_dump -Fc netbox > /var/backups/netbox/netbox-$(date +%Y%m%d).dump
psql -h localhost -U netbox -d netbox -c \
  "SELECT pg_size_pretty(pg_database_size('netbox'));"
```

### API (rappel)

```bash
export NETBOX_TOKEN="..."
curl -s -H "Authorization: Token $NETBOX_TOKEN" \
  https://netbox.lan-entreprise.fr/api/dcim/devices/?status=active \
  | python3 -m json.tool | head -30
```

---

## 92. Pense-bête de poche — où cliquer pour quoi

| Je veux… | Aller à… |
|---|---|
| Trouver une IP / un équipement | Barre de recherche globale (en haut) |
| Voir l'utilisation d'un sous-réseau | IPAM > Préfixes > cliquer le préfixe |
| Savoir ce qu'il y a dans une baie | DCIM > Baies > cliquer la baie (vue U) |
| Tracer un câble de bout en bout | Fiche interface > onglet Câble > chemin |
| Voir qui a modifié quoi | Fiche objet > onglet Journal |
| Créer un token API | Profil (avatar) > API Tokens |
| Lancer un script | Admin > Scripts |
| Voir les rapports | Admin > Rapports |
| Importer en masse | Liste d'objets > Importer |
| Exporter en CSV | Liste d'objets > Exporter |
| Gérer les utilisateurs/permissions | Admin > Utilisateurs / Groupes / Permissions |
| Voir les files de tâches | Admin > Files d'attente RQ |
| Configurer un webhook | Admin > Webhooks |

---

## 93. Glossaire

| Terme | Définition |
|---|---|
| Agrégat | Bloc d'adresses de haut niveau (ex. `10.0.0.0/8`), rattaché à un RIR |
| API REST | Interface HTTP JSON permettant de lire/écrire dans NetBox par programme |
| ASN | Autonomous System Number — numéro de système autonome (BGP) |
| Baie (rack) | Armoire 19 pouces recevant les équipements, mesurée en U |
| Câble (cable) | Liaison physique entre deux terminaisons (interfaces, ports, alimentations) |
| Changement (changelog) | Journal d'audit : qui a modifié quoi et quand |
| Champ personnalisé | Attribut supplémentaire typé ajouté à un modèle NetBox |
| Circuit | Liaison opérateur/FAI documentée (débit, identifiants, terminaisons) |
| Container (statut) | Préfixe parent découpé en sous-préfixes, sans adresses directes |
| DCIM | Data Center Infrastructure Management — le module « physique » |
| Équipement (device) | Instance physique d'un type d'équipement, installée dans une baie/un site |
| FQDN | Fully Qualified Domain Name — nom d'hôte complet (`netbox.lan-entreprise.fr`) |
| Gunicorn | Serveur d'application Python (WSGI) exécutant NetBox |
| IPAM | IP Address Management — le module « adressage logique » |
| OOB | Out-Of-Band — réseau d'administration séparé du trafic de production |
| PDU | Power Distribution Unit — multiprise de baie alimentée par l'onduleur |
| Plage IP (IP range) | Intervalle réservé dans un préfixe (DHCP, VIP…) |
| Préfixe | Sous-réseau en notation CIDR (`10.10.20.0/24`) |
| RIR | Regional Internet Registry — registre régional d'adresses (ex. RIPE-NCC) |
| Rôle | Qualification d'usage (rôle d'équipement, de préfixe, de baie…) |
| RQ (django-rq) | Files de tâches asynchrones (webhooks, scripts, rapports) |
| Script (custom script) | Formulaire exécutable dans NetBox pour automatiser une action |
| Site | Lieu physique documenté (bureaux, agence, datacenter) |
| Source de vérité | Référentiel unique faisant foi sur l'état de l'infrastructure |
| Tag | Étiquette libre et transverse apposée sur les objets |
| Tenant | Entité propriétaire d'objets (client, département, projet) |
| Type d'équipement | Modèle physique (fabricant + modèle + composants types) |
| U | Unité de hauteur en baie (1U = 44,45 mm) |
| VID | VLAN Identifier — numéro du VLAN (1–4094) |
| VLAN | Virtual LAN — domaine de broadcast de niveau 2 |
| VRF | Virtual Routing and Forwarding — table de routage isolée |
| Webhook | Notification HTTP envoyée par NetBox à chaque événement objet |
| WSGI | Interface Python entre serveur web et application (ici : gunicorn) |

---

## 94. Quiz — 10 questions pour valider vos acquis

**Q1.** Que se passe-t-il si vous créez deux fois la même adresse IP dans la même VRF ?
**Q2.** Quelle est la différence entre un préfixe au statut `Container` et un préfixe `Actif` ?
**Q3.** Citez les 4 briques techniques indispensables pour faire tourner NetBox 4.x en production.
**Q4.** Pourquoi faut-il lancer `migrate` AVANT le premier démarrage de gunicorn ?
**Q5.** Un technicien doit modifier uniquement les câbles du site de Lyon : quel mécanisme
de NetBox utilisez-vous, et à quel niveau l'appliquez-vous ?
**Q6.** Où sont stockées les pièces jointes (photos de baies), et que faut-il sauvegarder
en plus de la base PostgreSQL ?
**Q7.** Votre inventaire Ansible doit refléter NetBox en temps réel : quelle solution
recommandée par ce guide, et quel est le principal bénéfice ?
**Q8.** Lors d'une maintenance onduleur, comment NetBox vous aide-t-il à établir la liste
exacte des équipements impactés ?
**Q9.** Un webhook n'est jamais reçu par votre application : quels sont les deux premiers
points à vérifier côté NetBox ?
**Q10.** Citez trois éléments de la check-list de durcissement (section 46).

### Réponses

**R1.** NetBox refuse le doublon : l'unicité d'une adresse IP est garantie par VRF.
C'est une protection volontaire, pas un bug (voir section 75). Si le doublon semble
légitime, c'est probablement qu'il faut deux VRF distinctes.

**R2.** `Container` = bloc parent uniquement découpé en sous-préfixes (ex. `10.10.0.0/16`),
on n'y met jamais d'adresses IP directement. `Actif` = préfixe en production, qui
contient les adresses (voir section 33).

**R3.** PostgreSQL (données), Redis (cache + files de tâches), gunicorn (serveur
d'application Python), nginx (reverse proxy HTTPS). Le tout supervisé par systemd
(`netbox.service` + `netbox-rq.service`) — voir sections 3, 13, 14.

**R4.** Parce que `migrate` crée les tables de la base. Sans elles, gunicorn démarre
sur une base vide et chaque page renvoie une erreur 500 (voir section 12).

**R5.** Une **permission** avec **contrainte** JSON `{"site": ["AGENCE-Lyon"]}` sur les
modèles `dcim.cable` (ajout/modification), assignée à son **groupe** (jamais en
direct à l'utilisateur), puis testée avec un compte du groupe (voir section 43).

**R6.** Dans le dossier `media/` (`/opt/netbox/netbox/netbox/media/`). La sauvegarde
complète = `pg_dump` de PostgreSQL **+** archive de `media/` **+** `configuration.py`
(voir sections 64–65). Sans `media/`, les fiches perdent leurs photos et documents.

**R7.** Le plugin d'inventaire dynamique `netbox.netbox.nb_inventory` : l'inventaire
Ansible est généré à la volée depuis NetBox, donc tout équipement créé dans NetBox
(avec IP et rôle) devient automatiquement configurable par Ansible — zéro fichier
`hosts.ini` à maintenir (voir section 61).

**R8.** Grâce à la modélisation de la chaîne électrique (section 26) : depuis la fiche
de l'onduleur, l'onglet des alimentations connectées liste tous les équipements
branchés via les PDU. On trie par criticité (champ personnalisé), on repère les
mono-alimentés (tag), et on planifie arrêts propres et créneaux sans surprise.

**R9.** (1) Le service `netbox-rq` tourne-t-il ? Les webhooks partent via la file
`high` — s'il est arrêté, les événements s'accumulent dans Redis sans être envoyés.
(2) La file est-elle vide ? (`redis-cli llen rq:queue:high`, Admin > Files d'attente RQ.)
Ensuite seulement : URL, secret/signature, logs du récepteur (voir sections 57, 60).

**R10.** Trois au choix parmi : `configuration.py` en 600, `LOGIN_REQUIRED = True`,
`SECRET_KEY` unique, HTTPS + cookies `Secure` + HSTS, comptes nominatifs + permissions
(plus de compte partagé), LDAP/SSO, PostgreSQL/Redis en écoute locale, pare-feu
(443 uniquement), `DEBUG = False`, sauvegardes testées, bannière d'environnement
(voir section 46).

---

## 95. Pour aller plus loin

| Ressource | Adresse / référence |
|---|---|
| Documentation officielle NetBox | `https://docs.netbox.dev` (installation, API, release notes 4.x) |
| Dépôt GitHub | `https://github.com/netbox-community/netbox` (releases, issues) |
| Démo publique | `https://demo.netbox.dev` (bac à sable pour s'entraîner) |
| Collection Ansible | `netbox.netbox` sur Ansible Galaxy |
| Client Python | `pynetbox` (PyPI) — `https://pynetbox.readthedocs.io` |
| Communauté | Slack NetBox Community, forum GitHub Discussions |

**Feuille de route suggérée pour votre équipe (6 mois) :**

- **Mois 1** : installation + durcissement + convention de nommage validée ; 1 site pilote documenté à 100 %.
- **Mois 2** : migration de l'inventaire existant (section 88) ; inventaire Ansible dynamique en production.
- **Mois 3** : modélisation électrique complète (onduleurs/PDU) + premier rapport « bilan de puissance ».
- **Mois 4** : export Zabbix automatisé ; webhooks vers l'outil de ticketing.
- **Mois 5** : scripts et rapports d'exploitation (garanties, engagements opérateurs, préfixes saturés).
- **Mois 6** : revue qualité des données, audit de sécurité express (section 90), formation des nouveaux arrivants sur ce guide.

> 💡 **Dernier conseil de chef de service** : NetBox ne vaut que par la discipline
> collective qu'on met autour. Nommez un **référent NetBox** (tournant, 6 mois),
> faites-en un point fixe de votre réunion d'équipe mensuelle (revues section 85),
> et mesurez : « % d'équipements avec IP de management », « % de baies documentées ».
> Ce qui est mesuré progresse. Bon courage — votre infrastructure mérite mieux que
> des tableurs. 🚀

---

*Guide rédigé le 2026-09-26 — NetBox 4.x, Debian 12/13, Ubuntu 22.04/24.04 LTS.
Vérifiez les release notes officielles avant toute installation ou montée de version.*
