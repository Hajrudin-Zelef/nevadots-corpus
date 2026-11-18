# Guide Docker complet — Production & Sysadmin

> **Public :** administrateurs systèmes, chefs de service systèmes/énergies, équipes
> exploitation. Angle : pratique, production, robustesse.
> **Version :** Docker Engine 25.x/26.x, Compose v2 (plugin `docker compose`).
> Tous les exemples ont été conçus pour Debian 12/13 et Ubuntu 22.04/24.04 LTS.
> *Avertissement : testez toujours les commandes sensibles (iptables, macvlan,
> pruning) sur un environnement de pré-production avant la production.*

---

## 1. Pourquoi Docker en production ?

- **Isolation** : chaque application tourne dans son propre espace (namespaces,
  cgroups), sans interférer avec ses voisines.
- **Reproductibilité** : une image fige l'application + ses dépendances. « Ça
  marchait sur ma machine » disparaît.
- **Densité** : bien moins lourd qu'une VM (pas de noyau invité).
- **Déploiement** : `docker compose up -d` et la stack est en ligne.
- **Rollback** : une image est taguée, on peut revenir à la version précédente en
  une commande.

## 2. Le vocabulaire indispensable

| Terme | Définition | Analogie |
|---|---|---|
| Image | Modèle immuable (lecture seule) qui contient l'app et ses dépendances | Le moule |
| Conteneur | Instance en cours d'exécution d'une image | Le gâteau sorti du moule |
| Volume | Stockage persistant géré par Docker | Le disque dur externe |
| Bind mount | Dossier de l'hôte monté dans le conteneur | Un dossier partagé |
| Réseau | Réseau virtuel reliant les conteneurs | Le switch virtuel |
| Registre (registry) | Entrepôt d'images (Docker Hub, registre privé) | Le magasin de moules |
| Dockerfile | Recette texte pour construire une image | La recette de cuisine |
| Compose | Fichier YAML décrivant une stack multi-conteneurs | Le plan du repas complet |

- **Image ≠ conteneur** : on peut lancer 10 conteneurs depuis la même image.
- **Conteneur = éphémère par défaut** : tout ce qui est écrit dans sa couche
  inscriptible disparaît à sa suppression, sauf volumes et bind mounts.
- **Tag** : étiquette de version d'image (`nginx:1.27`, `postgres:16`).
  `latest` = tag mouvant, à éviter en production (voir section 63).

## 3. Architecture : client, daemon, registres

```
┌─────────────┐      API REST      ┌──────────────────┐
│ docker CLI  │ ◄────────────────► │ dockerd (daemon) │
└─────────────┘   /var/run/        └────────┬─────────┘
                  docker.sock               │
                                            ▼
                                   ┌──────────────────┐
                                   │ containerd + runc│  (exécution réelle)
                                   └────────┬─────────┘
                                            │
                        ┌───────────────────┼───────────────────┐
                        ▼                   ▼                   ▼
                   conteneurs          images/volumes       réseaux
```

- Le **daemon** (`dockerd`) fait tout le travail lourd ; la CLI n'est qu'un
  client qui lui parle via le socket Unix `/var/run/docker.sock`.
- **containerd** gère le cycle de vie des conteneurs, **runc** les lance.
- Le **socket Docker = root sur l'hôte** : quiconque peut y écrire peut lancer
  un conteneur `--privileged` et prendre le contrôle total de la machine
  (voir section 55, Sécurité).

## 4. Installation sur Debian/Ubuntu — dépôt officiel (pas le snap !)

> ⚠️ **Ne pas utiliser** le paquet `docker.io` des dépôts Ubuntu (souvent en
> retard de plusieurs versions) ni le snap (problèmes de permissions, de
> volumes et de réseau). Toujours le dépôt officiel `download.docker.com`.

```bash
# 1. Nettoyer d'anciennes installations conflictuelles
sudo apt-get remove -y docker.io docker-doc docker-compose \
  docker-compose-v2 podman-docker containerd runc || true

# 2. Prérequis
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg

# 3. Clé GPG officielle Docker
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg \
  | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
# Sur Ubuntu : remplacer "debian" par "ubuntu" dans les deux URL/commandes

# 4. Ajouter le dépôt officiel
echo "deb [arch=$(dpkg --print-architecture) \
  signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" \
  | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# 5. Installer le moteur + le plugin Compose
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin

# 6. Vérifier
docker --version
docker compose version
sudo docker run --rm hello-world
```

Checklist post-installation :

- [ ] `docker run --rm hello-world` fonctionne
- [ ] Le service démarre au boot : `sudo systemctl is-enabled docker`
- [ ] Version du moteur ≥ 24 : `docker version`
- [ ] Le plugin compose répond : `docker compose version`

## 5. Gestion du groupe docker et démarrage automatique

```bash
# Permettre à votre utilisateur d'utiliser docker SANS sudo
# (attention : membre du groupe docker = équivalent root, voir section 55)
sudo usermod -aG docker "$USER"
newgrp docker   # ou reconnectez-vous

# Activer le démarrage automatique
sudo systemctl enable --now docker
sudo systemctl enable --now containerd

# Vérifier l'état
sudo systemctl status docker --no-pager
docker info
docker info --format '{{.ServerVersion}}'
```

> 🔐 En production sur serveur partagé, **préférez `sudo docker ...`** plutôt
> que d'ajouter tout le monde au groupe `docker`.

## 6. Configuration du daemon : /etc/docker/daemon.json

Fichier de configuration central du daemon. Toute modification nécessite un
rechargement : `sudo systemctl reload docker` (ou restart si le reload ne
suffit pas).

```json
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "storage-driver": "overlay2",
  "exec-opts": ["native.cgroupdriver=systemd"],
  "live-restore": true,
  "default-address-pools": [
    { "base": "172.26.0.0/16", "size": 24 }
  ],
  "dns": ["9.9.9.9", "1.1.1.1"],
  "icc": true,
  "iptables": true,
  "userland-proxy": false,
  "default-ulimits": {
    "nofile": { "Name": "nofile", "Hard": 65536, "Soft": 65536 }
  },
  "metrics-addr": "127.0.0.1:9323",
  "experimental": false
}
```

Explication des clés :

| Clé | Rôle |
|---|---|
| `log-driver` / `log-opts` | Driver de logs + **rotation** (indispensable : sans rotation, les logs remplissent le disque) |
| `storage-driver` | `overlay2` = le driver moderne et recommandé |
| `live-restore` | Les conteneurs **survivent** au redémarrage du daemon (mises à jour sans coupure) |
| `default-address-pools` | Plages IP des réseaux bridge (à adapter pour éviter les collisions avec votre LAN/VPN) |
| `dns` | DNS utilisés par les conteneurs (si vide : ceux de l'hôte) |
| `userland-proxy: false` | Désactive le proxy userspace (performances), nécessite iptables actif |
| `metrics-addr` | Expose les métriques Prometheus du daemon (voir section 58) |

```bash
# Appliquer la configuration
sudo systemctl reload docker
# Si le reload échoue (changement profond), faire :
sudo systemctl restart docker
# Vérifier que la config est prise en compte
docker info | grep -i -E "logging|storage|cgroup"
```

## 7. Premières commandes : cycle de vie d'un conteneur

```bash
# Lancer un conteneur nginx en tâche de fond
docker run -d --name mon-nginx -p 8080:80 nginx:1.27

# Voir les conteneurs en cours
docker ps
docker ps -a          # même les arrêtés
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"

# Voir les logs
docker logs mon-nginx
docker logs -f --tail 50 mon-nginx   # suivi temps réel, 50 dernières lignes

# Entrer dans le conteneur
docker exec -it mon-nginx bash
docker exec -it mon-nginx sh         # si pas de bash (images alpine)

# Arrêter / démarrer / redémarrer
docker stop mon-nginx      # SIGTERM puis SIGKILL après 10 s
docker start mon-nginx
docker restart mon-nginx
docker stop -t 30 mon-nginx  # délai de grâce de 30 s

# Supprimer (arrêté d'abord, sauf -f)
docker rm mon-nginx
docker rm -f mon-nginx      # force : stop + rm
```

## 8. Images : pull, tag, lister, nettoyer

```bash
docker pull nginx:1.27
docker pull postgres:16-alpine

docker images
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"

# Tagger une image (ex : pour un registre privé)
docker tag mon-app:1.0 registry.interne:5000/mon-app:1.0

# Inspecter une image : couches, variables d'env, point d'entrée
docker image inspect nginx:1.27 | less

# Historique des couches d'une image
docker history nginx:1.27

# Supprimer une image
docker rmi nginx:1.27
docker rmi $(docker images -q)   # TOUTES les images : dangereux !

# Nettoyage : conteneurs arrêtés, réseaux et images non utilisés
docker system prune        # demande confirmation
docker system prune -a --volumes  # radical : + images + volumes (DANGEREUX)
docker system df           # voir l'espace utilisé par Docker
```

> 💡 `docker system df` est votre ami : il montre combien pèsent images,
> conteneurs, volumes et build cache.

## 9. Publier des ports et nommer les conteneurs

```bash
# -p [IP_hôte:]port_hôte:port_conteneur
docker run -d -p 8080:80 nginx              # toutes les interfaces
docker run -d -p 127.0.0.1:8080:80 nginx    # localhost uniquement (plus sûr)
docker run -d -p 8080:80 -p 8443:443 nginx  # plusieurs ports

# --name : nom explicite (sinon nom aléatoire type "stoic_bose")
docker run -d --name api-prod -p 3000:3000 mon-api:2.1

# --hostname : nom d'hôte vu depuis l'intérieur
docker run -d --hostname web1 --name web1 nginx
```

Tableau de décision d'exposition :

| Besoin | Commande |
|---|---|
| Site public | `-p 80:80 -p 443:443` |
| Admin locale uniquement | `-p 127.0.0.1:8080:80` |
| Conteneurs entre eux (compose) | pas de `-p`, le réseau interne suffit |
| Debug temporaire | `-p 8080:80`, à retirer ensuite |

## 10. Exécuter des commandes : exec, run, attacher

```bash
# Une commande ponctuelle dans un conteneur qui tourne
docker exec mon-nginx nginx -t            # tester la conf nginx
docker exec mon-nginx cat /etc/nginx/nginx.conf

# Shell interactif
docker exec -it mon-nginx bash

# Lancer un conteneur jetable pour un test
docker run --rm -it ubuntu:24.04 bash
docker run --rm alpine:3.20 ping -c 3 8.8.8.8

# Voir les processus d'un conteneur
docker top mon-nginx

# Statistiques temps réel (CPU, RAM, réseau, I/O)
docker stats
docker stats --no-stream --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemUsage}}"
```

## 11. Inspect : tout savoir sur un conteneur

```bash
docker inspect mon-nginx | less
# Extraire des champs précis (format Go template)
docker inspect --format '{{.State.Status}}' mon-nginx
docker inspect --format '{{.NetworkSettings.IPAddress}}' mon-nginx
docker inspect --format '{{range .Mounts}}{{.Source}} -> {{.Destination}}{{end}}' mon-nginx
```

Champs les plus utiles : `.State` (statut, code de sortie, heure de démarrage),
`.Mounts` (volumes), `.NetworkSettings` (IP, ports), `.Config` (image, env,
entrypoint).

## 12. Aide-mémoire des commandes quotidiennes

| Action | Commande |
|---|---|
| Lister conteneurs actifs | `docker ps` |
| Lister tout | `docker ps -a` |
| Logs temps réel | `docker logs -f <nom>` |
| Suivre les ressources | `docker stats` |
| Redémarrer | `docker restart <nom>` |
| Mettre à jour une image | `docker pull <image>:<tag>` + `docker compose up -d` |
| Espace disque Docker | `docker system df` |
| Tout nettoyer (prudent) | `docker system prune` |
| Version / infos | `docker version`, `docker info` |

## 13. Le Dockerfile : principes

- Un `Dockerfile` est une **recette** : chaque instruction crée une **couche**
  (layer) en lecture seule empilée sur la précédente.
- Les couches sont **mises en cache** : reconstruire après un petit changement
  ne refait que les couches modifiées et suivantes → ordonnez les instructions
  de la moins changeante à la plus changeante.
- Règle d'or : **un conteneur = un processus principal** (le `CMD`).

```dockerfile
# Exemple minimal : serveur web statique
FROM nginx:1.27-alpine
COPY ./site/ /usr/share/nginx/html/
EXPOSE 80
```

```bash
# Construire l'image
docker build -t mon-site:1.0 .
docker build -t mon-site:1.0 -f Dockerfile.prod .

# Lancer
docker run -d --name site -p 8080:80 mon-site:1.0
```

## 14. Instruction FROM : choisir sa base

```dockerfile
FROM ubuntu:24.04
FROM python:3.12-slim
FROM nginx:1.27-alpine
FROM node:20-alpine
```

- Préférez les images **officielles** (Docker Hub, badge « Official Image »).
- Préférez les variantes **`-slim`** (Debian allégé) ou **`-alpine`**
  (musl libc, ~5 Mo) pour réduire la surface d'attaque et le temps de pull.
- ⚠️ Alpine + musl : certaines applis (binaires compilés glibc, wheels Python
  exotiques) peuvent poser problème → testez, sinon restez sur `-slim`.
- **Épinglez toujours un tag précis** (`python:3.12-slim`), jamais `latest`
  en production.

## 15. Instructions RUN, COPY, ADD

```dockerfile
# RUN : exécute une commande pendant le build (nouvelle couche à chaque RUN)
RUN apt-get update && apt-get install -y --no-install-recommends \
      curl ca-certificates \
    && rm -rf /var/lib/apt/lists/*
# ^ combinez update+install+nettoyage dans UN SEUL RUN,
#   sinon le cache apt reste dans une couche intermédiaire.

# COPY : copie fichiers/dossiers du contexte vers l'image (préféré)
COPY ./app/ /app/
COPY requirements.txt /app/

# ADD : comme COPY + décompression auto des .tar + téléchargement d'URL
# À ÉVITER sauf pour décompresser une archive locale :
ADD https://example.com/fichier.tar.gz /tmp/   # déconseillé : préférez curl + RUN
```

Bonnes pratiques `RUN` :

- Chaînez avec `&&` et nettoyez dans la même instruction.
- `--no-install-recommends` avec apt pour limiter la taille.
- En fin de RUN apt : `rm -rf /var/lib/apt/lists/*`.

## 16. Instructions CMD vs ENTRYPOINT

```dockerfile
# CMD : commande par défaut, REMPLAÇABLE par des arguments de `docker run`
CMD ["nginx", "-g", "daemon off;"]

# ENTRYPOINT : commande FIXE, les arguments de `docker run` s'y AJOUTENT
ENTRYPOINT ["python", "app.py"]
CMD ["--port", "8000"]
# docker run mon-image --port 9000  =>  python app.py --port 9000
```

- Forme **exec** (`["..."]`, JSON) : recommandée — le processus devient PID 1
  et reçoit correctement les signaux (SIGTERM lors du `docker stop`).
- Forme **shell** (`CMD python app.py`) : lance `/bin/sh -c`, le signal
  n'atteint pas toujours l'app → arrêt brutal après le timeout.

## 17. Instructions ENV, WORKDIR, EXPOSE, USER

```dockerfile
ENV PYTHONUNBUFFERED=1 \
    APP_ENV=production
# ^ les logs Python sortent immédiatement (pas de buffer)

WORKDIR /app
# ^ crée le dossier et s'y place ; tous les COPY/RUN/CMD suivants sont relatifs

EXPOSE 8000
# ^ documentation du port d'écoute (ne PUBLIE pas le port, c'est -p qui le fait)

# Créer un utilisateur non-root et l'utiliser (SÉCURITÉ, voir section 52)
RUN useradd -m -u 1001 appuser && chown -R appuser:appuser /app
USER appuser
```

> 🔐 **Ne jamais laisser tourner une appli en root dans le conteneur** si on
> peut l'éviter. `USER` doit apparaître avant `CMD`.

## 18. Instructions VOLUME, LABEL, HEALTHCHECK, ARG

```dockerfile
ARG APP_VERSION=1.0.0
# ^ variable de BUILD (docker build --build-arg APP_VERSION=2.0 .)
#   Non persistée dans l'image finale (contrairement à ENV).

LABEL maintainer="infra@entreprise.fr" \
      version="${APP_VERSION}" \
      description="API de production"
# ^ métadonnées : traçabilité, inventaire.

VOLUME ["/data"]
# ^ déclare un point de montage persistant (préférez le déclarer dans compose).

HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD curl -f http://localhost:8000/health || exit 1
# ^ Docker surveille la santé de l'app ; visible dans `docker ps` et exploitable
#   par l'orchestrateur (restart, load balancer).
```

Différence **ARG vs ENV** :

| | ARG | ENV |
|---|---|---|
| Disponible au build | ✅ | ✅ |
| Persisté dans l'image/conteneur | ❌ | ✅ |
| Usage typique | version, options de compilation | config runtime |

⚠️ Ne mettez **jamais de secret** dans un `ARG` ou un `ENV` du Dockerfile :
ils restent visibles dans l'historique de l'image (`docker history`).

## 19. Multi-stage builds : des images petites et propres

Principe : une étape **build** (avec compilateurs, SDK) puis une étape
**runtime** qui ne récupère que les artefacts. Résultat : image finale légère,
sans outils de compilation (= moins de vulnérabilités).

```dockerfile
# ---------- Étape 1 : build ----------
FROM golang:1.22-alpine AS builder
WORKDIR /src
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN CGO_ENABLED=0 go build -o /app/mon-api ./cmd/api

# ---------- Étape 2 : runtime minimal ----------
FROM alpine:3.20
RUN adduser -D -u 1001 appuser
COPY --from=builder /app/mon-api /usr/local/bin/mon-api
USER appuser
EXPOSE 8000
ENTRYPOINT ["mon-api"]
```

Exemple Node.js :

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production=false
COPY . .
RUN npm run build

FROM node:20-alpine
WORKDIR /app
ENV NODE_ENV=production
COPY package*.json ./
RUN npm ci --only=production && npm cache clean --force
COPY --from=builder /app/dist ./dist
USER node
CMD ["node", "dist/index.js"]
```

Exemple Python :

```dockerfile
FROM python:3.12-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

FROM python:3.12-slim
WORKDIR /app
COPY --from=builder /install /usr/local
COPY ./src ./src
RUN useradd -m -u 1001 appuser && chown -R appuser /app
USER appuser
CMD ["python", "-m", "src.main"]
```

## 20. Bonnes pratiques de taille d'image

Checklist anti-obésité :

- [ ] Base `alpine` ou `slim` quand c'est compatible
- [ ] Multi-stage pour les langages compilés / builds frontend
- [ ] Un seul `RUN` pour `apt-get update + install + rm -rf /var/lib/apt/lists/*`
- [ ] `--no-install-recommends` (apt), `--no-cache-dir` (pip), `npm cache clean`
- [ ] `.dockerignore` complet (voir section 21)
- [ ] Pas de secrets, pas de `.git`, pas de fichiers de test inutiles
- [ ] `docker images` : visez < 100 Mo pour une API simple, < 300 Mo sinon

```bash
# Analyser ce qui pèse dans une image
docker history --human --format "{{.CreatedBy}} : {{.Size}}" mon-image:1.0
# Outil visuel (à installer) : dive
# https://github.com/wagoodman/dive
```

## 21. Le .dockerignore : ne pas envoyer n'importe quoi au daemon

Le **contexte de build** (le dossier envoyé au daemon) doit être minimal :
chaque fichier inutile ralentit le build et peut fuiter dans l'image.

```gitignore
# .dockerignore
.git
.gitignore
.github/
.vscode/
.idea/
node_modules/
__pycache__/
*.pyc
.pytest_cache/
venv/
.env
.env.*
*.log
Dockerfile*
docker-compose*.yml
README.md
tests/
docs/
```

```bash
# Vérifier ce qui part dans le contexte (buildx)
docker build --progress=plain -t test . 2>&1 | grep -i "transferring context"
```

## 22. Build : cache, tags, bonnes habitudes

```bash
# Build simple avec tag versionné
docker build -t registry.interne:5000/mon-app:1.4.2 .

# Plusieurs tags d'un coup
docker build -t mon-app:1.4.2 -t mon-app:latest .

# Sans utiliser le cache (build 100 % propre, ex : release)
docker build --no-cache -t mon-app:1.4.2 .

# Passer un ARG
docker build --build-arg APP_VERSION=1.4.2 -t mon-app:1.4.2 .

# BuildKit (par défaut sur les versions récentes) : parallélise et sécurise
DOCKER_BUILDKIT=1 docker build -t mon-app:1.4.2 .

# Voir les logs détaillés d'un build qui échoue
docker build --progress=plain -t mon-app:1.4.2 . 2>&1 | tail -60
```

Convention de tags recommandée :

- `1.4.2` : version exacte (déploiement reproductible)
- `1.4` : dernière 1.4.x (mises à jour mineures auto)
- `latest` : uniquement pour le dev local, **jamais** en prod

## 23. Cas pratique : Dockerfile d'une API Python (FastAPI) production-ready

```dockerfile
# Dockerfile
FROM python:3.12-slim AS builder
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir --prefix=/install -r requirements.txt

FROM python:3.12-slim
ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1
WORKDIR /app
COPY --from=builder /install /usr/local
COPY ./app ./app
RUN useradd --create-home --uid 1001 appuser \
    && chown -R appuser:appuser /app
USER appuser
EXPOSE 8000
HEALTHCHECK --interval=30s --timeout=5s --retries=3 \
  CMD python -c "import urllib.request; urllib.request.urlopen('http://localhost:8000/health')" || exit 1
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

```txt
# requirements.txt
fastapi==0.115.0
uvicorn[standard]==0.30.0
```

```bash
docker build -t mon-api:1.0.0 .
docker run -d --name api --user 1001 -p 8000:8000 mon-api:1.0.0
curl http://localhost:8000/health
docker inspect --format '{{.State.Health.Status}}' api
```

## 24. Cas pratique : Dockerfile frontend (build statique + nginx)

```dockerfile
# Build du frontend (ex : Vite/React)
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build   # produit /app/dist

# Serveur statique léger
FROM nginx:1.27-alpine
COPY --from=builder /app/dist /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf
EXPOSE 80
```

```nginx
# nginx.conf : SPA avec fallback + cache des assets
server {
    listen 80;
    root /usr/share/nginx/html;
    location / {
        try_files $uri $uri/ /index.html;
    }
    location /assets/ {
        expires 1y;
        add_header Cache-Control "public, immutable";
    }
}
```

## 25. Docker Compose : le chef d'orchestre local

Compose décrit **toute une stack** (services, réseaux, volumes) dans un fichier
`compose.yaml` (ou `docker-compose.yml`), puis la pilote d'un bloc.

```bash
docker compose up -d        # lance la stack en tâche de fond
docker compose ps           # état des services
docker compose logs -f      # logs de tous les services
docker compose logs -f web  # logs d'un seul service
docker compose down         # arrête + supprime conteneurs et réseaux
docker compose down -v       # + supprime les VOLUMES (données ! prudent)
docker compose pull         # met à jour les images de la stack
docker compose up -d --build  # rebuild les images locales puis relance
```

> Le nom du projet (préfixe des conteneurs/réseaux/volumes) vient du dossier
> ou de `-p monprojet` / `name: monprojet` en tête du YAML.

## 26. Compose : anatomie d'un fichier

```yaml
name: demo

services:
  web:
    image: nginx:1.27-alpine
    container_name: demo-web
    ports:
      - "8080:80"
    volumes:
      - ./site:/usr/share/nginx/html:ro
    networks:
      - front
    restart: unless-stopped
    depends_on:
      api:
        condition: service_healthy

  api:
    build: ./api
    environment:
      DATABASE_URL: "postgres://app:secret@db:5432/appdb"
    networks:
      - front
      - back
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 5s
      retries: 3
      start_period: 10s
    restart: unless-stopped

  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: app
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
      POSTGRES_DB: appdb
    secrets:
      - db_password
    volumes:
      - db-data:/var/lib/postgresql/data
    networks:
      - back
    restart: unless-stopped

networks:
  front:
  back:

volumes:
  db-data:

secrets:
  db_password:
    file: ./secrets/db_password.txt
```

## 27. Compose : services, images et build

```yaml
services:
  # 1. Depuis une image du registre
  web:
    image: nginx:1.27-alpine

  # 2. Depuis un Dockerfile local
  api:
    build:
      context: ./api
      dockerfile: Dockerfile.prod
      args:
        APP_VERSION: "1.4.2"
      target: runtime        # étape du multi-stage à utiliser

  # 3. Commande et point d'entrée surchargés
  worker:
    image: mon-app:1.4.2
    command: ["python", "-m", "worker"]
    entrypoint: ["/bin/sh", "-c"]
```

```bash
# Rebuild uniquement le service api puis relance
docker compose build api
docker compose up -d api

# Forcer le rebuild sans cache
docker compose build --no-cache api
```

## 28. Compose : depends_on et ordre de démarrage

```yaml
services:
  api:
    depends_on:
      db:
        condition: service_healthy   # attend que db soit SAIN
      cache:
        condition: service_started   # attend le simple démarrage
  db:
    image: postgres:16-alpine
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U app"]
      interval: 10s
      timeout: 5s
      retries: 5
```

- `depends_on` ne garantit **que l'ordre**, pas que l'app soit prête :
  combinez avec `healthcheck` + `condition: service_healthy`.
- L'application doit aussi **réessayer** sa connexion à la base au démarrage
  (backoff), car en production l'ordre ne suffit jamais.

## 29. Compose : restart policies

```yaml
services:
  web:
    restart: unless-stopped   # redémarre toujours, sauf arrêt manuel explicite
```

| Politique | Comportement |
|---|---|
| `no` | jamais redémarré (défaut) |
| `always` | toujours redémarré, même après un `docker stop` + reboot hôte |
| `unless-stopped` | comme `always`, mais respecte un arrêt manuel (**recommandé en prod**) |
| `on-failure[:N]` | redémarre seulement si code de sortie ≠ 0 (N = max de tentatives) |

> En production : `unless-stopped` partout, sauf jobs ponctuels (`no`).

## 30. Compose : healthcheck

```yaml
services:
  api:
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      # variantes :
      # test: ["CMD-SHELL", "pg_isready -U app -d appdb"]
      # test: ["CMD", "wget", "--no-verbose", "--tries=1", "--spider", "http://localhost/"]
      interval: 30s        # entre deux contrôles
      timeout: 5s          # délai max d'un contrôle
      retries: 3           # échecs avant "unhealthy"
      start_period: 15s    # grâce au démarrage (pas compté comme échec)
      start_interval: 5s   # (optionnel) intervalle pendant start_period
```

```bash
docker inspect --format '{{.State.Health.Status}}' projet-api-1
docker compose ps   # la colonne STATUS affiche (healthy) / (unhealthy)
```

Un service `unhealthy` : ne le laisse jamais tourner en prod sans alerte —
c'est le signal d'un déploiement raté ou d'une dépendance tombée.

## 31. Compose : variables d'environnement

```yaml
services:
  api:
    environment:
      APP_ENV: production          # valeur en clair dans le YAML
      LOG_LEVEL: info
      DB_HOST: db
      # Ne JAMAIS mettre de mot de passe en clair ici en prod :
      DB_PASSWORD: ${DB_PASSWORD}  # lu depuis l'environnement du shell / .env
    env_file:
      - ./config/app.env           # fichier clé=valeur
      - ./config/override.env
```

```bash
# .env à côté du compose.yaml (chargé automatiquement par compose)
DB_PASSWORD=S3cret_tres_long
APP_ENV=production

# Priorité (du plus fort au plus faible) :
# 1. `docker compose run -e VAR=...`
# 2. environment: du YAML (${VAR} interpolée depuis le shell/.env)
# 3. env_file:
# 4. valeurs par défaut ${VAR:-defaut} dans le YAML
```

```yaml
# Valeur par défaut si la variable est absente
image: "mon-app:${APP_VERSION:-latest}"
```

> ⚠️ Le fichier `.env` contient des secrets : `chmod 600 .env` et **jamais**
> dans git (ajoutez-le au `.gitignore`).

## 32. Compose : networks

```yaml
services:
  web:
    networks:
      - front
  api:
    networks:
      front:
        aliases: ["api-interne"]   # nom DNS supplémentaire
      back:
  db:
    networks:
      - back

networks:
  front:                            # réseau applicatif
  back:                             # réseau données, isolé du web
    internal: true                  # PAS d'accès Internet pour ce réseau
```

- Chaque réseau compose = un **bridge** avec **DNS intégré** : les conteneurs
  se joignent par **nom de service** (`db`, `api`).
- `internal: true` : parfait pour le réseau base de données.
- Réseau externe pré-existant (ex : celui de Traefik, voir section 73) :

```yaml
networks:
  proxy:
    name: proxy
    external: true
```

## 33. Compose : volumes et bind mounts

```yaml
services:
  db:
    image: postgres:16-alpine
    volumes:
      # Volume nommé (géré par Docker, recommandé pour les données)
      - db-data:/var/lib/postgresql/data
      # Bind mount (dossier de l'hôte)
      - ./backups:/backups
      # Fichier unique, en lecture seule
      - ./config/postgresql.conf:/etc/postgresql/postgresql.conf:ro
      # tmpfs (RAM, non persisté, pour caches/secrets temporaires)
      - type: tmpfs
        target: /tmp
        tmpfs_size: 100m

volumes:
  db-data:
    driver: local
    # driver_opts pour monter un NFS par ex. (voir section 45)
```

Syntaxe courte vs longue :

| Besoin | Syntaxe courte |
|---|---|
| Volume nommé | `db-data:/var/lib/postgresql/data` |
| Bind mount | `./config:/etc/app:ro` |
| Lecture seule | ajouter `:ro` |

> `:ro` partout où l'appli n'a pas besoin d'écrire : principe du moindre
> privilège, aussi pour les fichiers.

## 34. Compose : ressources, logging, divers

```yaml
services:
  api:
    deploy:
      resources:
        limits:
          cpus: "1.5"
          memory: 512M
        reservations:
          memory: 256M
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"
    ulimits:
      nofile:
        soft: 65536
        hard: 65536
    sysctls:
      net.core.somaxconn: 1024
    stop_grace_period: 30s     # délai avant SIGKILL (défaut 10 s)
    init: true                 # mini-init (tini) comme PID 1 : récole les zombies
```

- `deploy.resources` fonctionne avec `docker compose` (ignoré en partie par
  l'ancien `docker-compose` v1, mais le plugin v2 l'applique).
- `init: true` : recommandé si l'app ne gère pas les processus fils.

## 35. Compose : profils et surcharges par environnement

```yaml
services:
  web:
    image: nginx:1.27-alpine
    profiles: ["prod"]        # ne démarre que si le profil est activé
  mailhog:
    image: mailhog/mailhog
    profiles: ["dev"]         # outil de dev uniquement
```

```bash
docker compose --profile prod up -d
docker compose --profile dev up -d
```

Surcharges par environnement (pattern classique) :

```bash
# compose.yaml        : base commune
# compose.prod.yaml   : surcharge production
# compose.dev.yaml    : surcharge développement
docker compose -f compose.yaml -f compose.prod.yaml up -d
docker compose -f compose.yaml -f compose.dev.yaml up -d
```

```yaml
# compose.prod.yaml : exemple de surcharge
services:
  web:
    restart: unless-stopped
    logging:
      options: { max-size: "10m", max-file: "5" }
```

## 36. Compose en pratique : commandes de tous les jours

```bash
# État, logs, top
docker compose ps
docker compose top
docker compose logs -f --tail=100 api

# (Re)lancer un seul service
docker compose up -d --build api
docker compose restart api
docker compose stop api && docker compose rm -f api

# Exécuter une commande / shell dans un service
docker compose exec api bash
docker compose exec db psql -U app -d appdb

# Lancer un conteneur one-shot (ne démarre pas les dépendances liées)
docker compose run --rm api python -m pytest

# Voir la config finale après interpolation et surcharges
docker compose config

# Mettre à jour toute la stack
docker compose pull && docker compose up -d

# Tout arrêter proprement
docker compose down
```

## 37. Registres : Docker Hub

```bash
# Connexion (stockée chiffrée dans ~/.docker/config.json)
docker login
docker login registry.interne:5000

# Pousser une image (le nom doit commencer par votre namespace)
docker tag mon-app:1.4.2 zelef/mon-app:1.4.2
docker push zelef/mon-app:1.4.2

# Se déconnecter
docker logout
```

- Docker Hub : gratuit pour les images publiques, quotas de pull pour les
  anonymes (limites périodiques) → en prod, **authentifiez** le daemon
  (`docker login`) ou utilisez un miroir/registre interne.
- Préférez les images **officielles** (`nginx`, `postgres`…) et les éditeurs
  **vérifiés** (badge). Méfiez-vous des images au nom approximatif
  (typosquatting : `nglnx` au lieu de `nginx`).

## 38. Registre privé : le lancer soi-même

Le registre officiel est lui-même un conteneur :

```yaml
# compose.yaml du registre privé
services:
  registry:
    image: registry:3
    container_name: registry
    ports:
      - "127.0.0.1:5000:5000"   # ou 5000:5000 si accès réseau voulu
    volumes:
      - registry-data:/var/lib/registry
    environment:
      REGISTRY_STORAGE_DELETE_ENABLED: "true"
    restart: unless-stopped

volumes:
  registry-data:
```

```bash
docker compose up -d
docker tag mon-app:1.4.2 localhost:5000/mon-app:1.4.2
docker push localhost:5000/mon-app:1.4.2
docker pull localhost:5000/mon-app:1.4.2
```

- En HTTPS avec certificat : montez les certs et mettez
  `REGISTRY_HTTP_TLS_CERTIFICATE` / `REGISTRY_HTTP_TLS_KEY`.
- Registre **insecure** (HTTP, labo uniquement) : à déclarer côté daemon :

```json
{ "insecure-registries": ["192.168.1.50:5000"] }
```

> En production, préférez Harbor ou Nexus (RBAC, scan de vulnérabilités,
> réplication) plutôt que le registre nu.

## 39. Réseaux Docker : vue d'ensemble

```bash
docker network ls
docker network inspect mon-reseau
```

| Driver | Usage | Isolement |
|---|---|---|
| `bridge` | défaut, conteneurs d'un même hôte | NAT vers l'hôte |
| `host` | conteneur partage la pile réseau de l'hôte | aucun (perf maximale) |
| `macvlan` | conteneur avec sa propre MAC/IP sur le LAN | apparaît comme une machine physique |
| `overlay` | multi-hôtes (Swarm) | chiffré en option |
| `none` | pas de réseau | total |
| `internal` (option) | bridge sans accès Internet | sortant bloqué |

## 40. Bridge : le réseau par défaut (et ses limites)

```bash
# Créer un réseau bridge dédié (TOUJOURS préféré au bridge par défaut "bridge")
docker network create --driver bridge --subnet 172.30.0.0/24 mon-bridge

docker run -d --name web --network mon-bridge nginx:1.27-alpine
docker run -d --name api --network mon-bridge mon-api:1.0

# Depuis api, "web" se résout en DNS :
docker exec api ping -c 2 web
docker exec api getent hosts web
```

Pourquoi éviter le bridge par défaut `bridge` :

- pas de **résolution DNS par nom** entre conteneurs (il faut des `--link`,
  obsolètes) ;
- tous les conteneurs « par défaut » partagent le même segment.

> Règle : **un réseau bridge dédié par application/stack**.

## 41. Host : quand la performance réseau prime

```bash
docker run -d --name dns --network host coredns/coredns:1.11.1
```

- Le conteneur utilise directement les interfaces de l'hôte : **zéro NAT**,
  latence minimale.
- Inconvénients : pas d'isolation des ports (conflits possibles), pas de
  mapping `-p`, surface d'attaque élargie.
- Cas d'usage : serveurs DNS, exporters réseau, applis très sensibles à la
  latence. À éviter pour une appli web standard.

## 42. Macvlan : le conteneur sur le LAN physique

Le conteneur obtient sa **propre adresse MAC et IP** sur votre réseau local :
il apparaît comme une machine physique (utile pour DHCP, supervision SNMP,
vieux protocoles qui n'aiment pas le NAT).

```bash
# Créer le réseau macvlan (adaptez l'interface et le sous-réseau !)
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 \
  lan-net

docker run -d --name srv --network lan-net --ip 192.168.1.60 nginx:1.27-alpine
```

Points d'attention :

- L'hôte **ne peut pas joindre** directement ses conteneurs macvlan (limite du
  noyau) : créez une interface macvlan sur l'hôte si besoin.
- Réservez les IP dans le DHCP du LAN pour éviter les collisions.
- Certains hyperviseurs / cartes réseau filtrent les MAC multiples (mode
  promiscuous requis sur l'interface).

## 43. Overlay : le multi-hôtes (Swarm)

- Disponible en mode **Swarm** (voir section 66) : un réseau logique réparti
  sur plusieurs machines.
- Chiffrement du trafic inter-nœuds : `--opt encrypted`.
- En mono-hôte avec compose, inutile : restez sur `bridge`.

```bash
# Exemple (manager Swarm)
docker network create -d overlay --opt encrypted mon-overlay
```

## 44. DNS intégré et communication inter-conteneurs

- Sur tout réseau **dédié** (bridge custom, overlay), Docker fournit un
  serveur DNS embarqué (`127.0.0.11` dans le conteneur).
- Chaque conteneur est joignable par son **nom** ou **nom de service**
  (compose), plus les `aliases` éventuels.
- `docker exec app getent hosts db` : vérification rapide.

```bash
# Tester la connectivité entre deux conteneurs
docker run --rm --network mon-bridge alpine:3.20 \
  sh -c "apk add -q curl && curl -s -o /dev/null -w '%{http_code}' http://web:80"
```

Personnaliser le DNS d'un conteneur :

```yaml
services:
  app:
    dns:
      - 9.9.9.9
      - 1.1.1.1
    dns_search:
      - entreprise.lan
```

## 45. Volumes : le stockage persistant géré par Docker

```bash
docker volume create db-data
docker volume ls
docker volume inspect db-data
docker run -d --name db -v db-data:/var/lib/postgresql/data postgres:16-alpine

# Où sont les données sur l'hôte ?
sudo ls /var/lib/docker/volumes/db-data/_data
```

- Les volumes survivent à la suppression du conteneur (`docker rm`) et même
  à `docker compose down` (seul `down -v` les supprime).
- Sauvegarde : voir section 61 — **c'est LE point critique** en production.

Volume sur stockage réseau (NFS) :

```yaml
volumes:
  nfs-data:
    driver: local
    driver_opts:
      type: nfs
      o: "addr=192.168.1.10,rw,nfsvers=4"
      device: ":/exports/docker"
```

## 46. Bind mounts : quand monter un dossier de l'hôte

```bash
# Dossier de config en lecture seule
docker run -d --name web \
  -v /srv/web/nginx.conf:/etc/nginx/nginx.conf:ro \
  -v /srv/web/html:/usr/share/nginx/html:ro \
  nginx:1.27-alpine
```

| | Volume nommé | Bind mount |
|---|---|---|
| Géré par | Docker | vous |
| Chemin hôte | `/var/lib/docker/volumes/...` | libre (`/srv/...`) |
| Sauvegarde | via conteneur helper (section 61) | rsync/cp direct |
| Permissions | Docker initialise avec le contenu de l'image | **UID/GID de l'hôte** (piège classique, section 70) |
| Cas d'usage | bases de données, données applicatives | configs, code en dev |

## 47. tmpfs : le stockage en RAM

```yaml
services:
  app:
    tmpfs:
      - /run
      - /tmp:size=100m,mode=1777
```

- Contenu **jamais écrit sur disque**, perdu à l'arrêt du conteneur.
- Idéal pour caches, sockets temporaires, et pour rendre un conteneur en
  lecture seule (`read_only: true`, voir section 53) tout en gardant des
  zones inscriptibles.

## 48. Partager des données entre conteneurs

```bash
# volumes_from : héritage (ancien, encore vu dans la nature)
docker run -d --name data-holder -v /data busybox true
docker run --volumes-from data-holder alpine ls /data

# Moderne : même volume nommé monté dans deux services
```

```yaml
services:
  app:
    image: mon-app:1.0
    volumes: ["shared:/data"]
  backup:
    image: alpine:3.20
    volumes: ["shared:/data:ro"]
    command: ["sh", "-c", "tar czf /backup/data.tgz -C /data ."]
    volumes:
      - shared:/data:ro
      - ./backups:/backup

volumes:
  shared:
```

## 49. Logs : drivers et lecture

```bash
docker logs --tail 100 -f mon-conteneur
docker logs --since 1h mon-conteneur
docker logs --since 2026-09-26T08:00 --until 2026-09-26T09:00 mon-conteneur
docker compose logs -f --tail=200 api db
```

Drivers de logs (`--log-driver` ou `daemon.json`) :

| Driver | Usage |
|---|---|
| `json-file` | défaut, fichiers locaux avec rotation |
| `syslog` | envoi vers syslog/rsyslog de l'hôte |
| `journald` | journal systemd (`journalctl -u docker`) |
| `gelf` | Graylog / Logstash |
| `none` | aucun log (debug extrême uniquement) |

```bash
# Logs via journald
docker run -d --log-driver journald --name app mon-app:1.0
journalctl CONTAINER_NAME=app -f
```

> ⚠️ Avec un driver distant (syslog, gelf), `docker logs` ne fonctionne plus
> en local : prévoyez votre lecteur de logs centralisé.

## 50. Rotation des logs : éviter le disque plein

**Le piège n°1 des débutants** : sans rotation, un conteneur verbeux remplit
`/var/lib/docker` jusqu'au plantage de l'hôte. La rotation se configure :

1. **Globalement** dans `daemon.json` (recommandé, voir section 6) :

```json
{
  "log-driver": "json-file",
  "log-opts": { "max-size": "10m", "max-file": "3" }
}
```

2. **Par conteneur/service** (surcharge) :

```yaml
services:
  api:
    logging:
      driver: json-file
      options:
        max-size: "10m"
        max-file: "3"
```

```bash
# Vérifier la taille des logs d'un conteneur
docker inspect --format '{{.LogPath}}' mon-conteneur
sudo du -sh /var/lib/docker/containers/*/*-json.log | sort -h | tail
```

## 51. Limites de ressources : CPU et mémoire

Sans limites, un conteneur peut affamer l'hôte. En production, **toujours**
poser des limites.

```bash
docker run -d --name api \
  --memory=512m --memory-swap=1g \
  --cpus=1.5 \
  --pids-limit=200 \
  mon-api:1.0
```

| Option | Effet |
|---|---|
| `--memory=512m` | RAM max ; dépassement → le conteneur est **tué (OOM)** |
| `--memory-swap=1g` | RAM + swap max (mettre = à `--memory` pour **désactiver le swap**) |
| `--cpus=1.5` | 1,5 cœur max |
| `--pids-limit=200` | anti fork-bomb |
| `--restart=unless-stopped` | redémarre après un OOM kill |

En compose (voir section 34) :

```yaml
deploy:
  resources:
    limits: { cpus: "1.5", memory: 512M }
    reservations: { memory: 256M }
```

```bash
# Surveiller
docker stats --no-stream
# Vérifier qu'une limite est appliquée
docker inspect --format '{{.HostConfig.Memory}}' api   # en octets
```

> 💡 Dimensionnez `memory` d'après `docker stats` en charge réelle + 20 % de
> marge, pas au doigt mouillé.

## 52. Variables d'environnement : les 4 voies (et leurs pièges)

```bash
# 1. -e en ligne de commande
docker run -d -e APP_ENV=production -e LOG_LEVEL=info mon-app:1.0

# 2. --env-file
docker run -d --env-file ./config/app.env mon-app:1.0

# 3. compose: environment: / env_file: (voir section 31)

# 4. Valeurs par défaut dans l'image (ENV du Dockerfile)
```

Règles :

- Les variables **non secrètes** (env, log level, ports) : `environment:` OK.
- Les **secrets** (mots de passe, clés API, tokens) : **jamais** en clair
  dans le YAML versionné → utilisez les **secrets Docker** ou un fichier
  `.env` en `chmod 600` hors git (section 54).
- `docker inspect` affiche les variables d'un conteneur en clair : toute
  personne avec accès Docker les voit.

## 53. Secrets Docker (Swarm) et fichiers de secrets en compose

En **Swarm** : secrets chiffrés, montés en `tmpfs` sous `/run/secrets/`.

```bash
# Créer un secret (Swarm)
echo "motdepasse_tres_long" | docker secret create db_password -
docker secret ls
```

En **compose simple** (mono-hôte), l'équivalent pragmatique :

```yaml
services:
  db:
    image: postgres:16-alpine
    environment:
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password  # _FILE : lu depuis le fichier
    secrets:
      - db_password
  api:
    image: mon-api:1.0
    secrets:
      - db_password

secrets:
  db_password:
    file: ./secrets/db_password.txt   # chmod 600, hors git !
```

- Les images officielles (`postgres`, `mysql`, `redis`…) supportent les
  variantes `*_FILE` : le secret est lu depuis le fichier, **jamais** exposé
  en variable d'environnement visible par `docker inspect`.
- Permissions : `chmod 600 secrets/`, ajoutez `secrets/` au `.gitignore`.

## 54. Gestion des secrets : stratégie complète

| Niveau | Solution | Quand |
|---|---|---|
| Labo / dev | `.env` en `chmod 600`, hors git | poste isolé |
| Mono-hôte prod | secrets compose (`file:`) + `*_FILE` | PME, un serveur |
| Multi-hôtes | Vault / Doppler / Infisical, injection au déploiement | équipe, plusieurs serveurs |
| Swarm | `docker secret` natif | cluster Swarm |

Anti-patterns à bannir :

```dockerfile
# ❌ INTERDIT : secret dans le Dockerfile (visible dans docker history)
ENV DB_PASSWORD=S3cret
ARG API_KEY=xxx
```

```yaml
# ❌ INTERDIT : secret en clair dans un YAML versionné
environment:
  DB_PASSWORD: S3cret
```

Checklist secrets :

- [ ] Aucun secret dans les Dockerfiles ni dans git (`git log -S password`)
- [ ] `secrets/` et `.env` dans le `.gitignore`, `chmod 600`
- [ ] Rotation des mots de passe documentée (qui, quand, comment)
- [ ] Les images officielles utilisent les variantes `*_FILE`

## 55. SÉCURITÉ — le socket Docker et le groupe docker

> 🔴 **Point le plus important du guide** : l'accès au daemon Docker
> (`/var/run/docker.sock` ou le groupe `docker`) équivaut à **root sur l'hôte**,
> sans mot de passe :

```bash
# N'importe quel membre du groupe docker peut faire ceci :
docker run --rm -it --privileged -v /:/host alpine:3.20 chroot /host bash
# => shell root sur l'HÔTE. Game over.
```

Règles :

- N'ajoutez au groupe `docker` que des administrateurs de confiance.
- **Ne montez jamais** `/var/run/docker.sock` dans un conteneur, sauf outil
  d'administration légitime (Portainer, watchtower) — et comprenez le risque.
- Sur un serveur multi-utilisateurs, imposez `sudo docker` + audit.
- Protégez le socket : `srw-rw---- root:docker /var/run/docker.sock`.

## 56. SÉCURITÉ — bannir --privileged et durcir le runtime

```bash
# ❌ JAMAIS en production (donne accès à tous les devices de l'hôte)
docker run --privileged ...

# ✅ À la place : accordez uniquement la capability nécessaire
docker run --cap-add=NET_ADMIN --cap-drop=ALL mon-app:1.0
docker run --cap-drop=ALL --cap-add=CHOWN --cap-add=SETUID ...
```

Checklist de durcissement d'un conteneur :

```yaml
services:
  api:
    image: mon-api:1.0.0
    user: "1001:1001"          # ne pas tourner en root (même avec USER dans le Dockerfile, ceinture + bretelles)
    read_only: true            # système de fichiers racine en lecture seule
    tmpfs: ["/tmp", "/run"]    # zones inscriptibles en RAM
    cap_drop: ["ALL"]          # retire toutes les capabilities...
    cap_add: ["NET_BIND_SERVICE"]  # ...puis n'ajoute que le nécessaire
    security_opt:
      - no-new-privileges:true # empêche l'élévation via setuid
    pids_limit: 200
```

Vérification :

```bash
docker exec api id -u          # doit afficher 1001, pas 0
docker inspect --format '{{.HostConfig.ReadonlyRootfs}}' projet-api-1
```

## 57. SÉCURITÉ — images : choisir, épingler, scanner

- **Images officielles** ou éditeurs vérifiés uniquement.
- **Épinglez le digest** pour les déploiements critiques (le tag peut être
  réécrit, le digest non) :

```bash
docker pull nginx:1.27-alpine
docker inspect --format '{{index .RepoDigests 0}}' nginx:1.27-alpine
# => nginx@sha256:abc123...
```

```yaml
services:
  web:
    image: nginx:1.27-alpine@sha256:abc123...  # immuable, garanti
```

- **Scannez** les images avant production :

```bash
# Docker Scout (intégré aux versions récentes)
docker scout quickview mon-app:1.0
docker scout cves mon-app:1.0

# Trivy (open source, recommandé en CI)
trivy image mon-app:1.0
trivy image --severity HIGH,CRITICAL mon-app:1.0
```

- Rebuildez régulièrement les images (les CVE se corrigent dans les bases
  Debian/Alpine) : au moins mensuellement, ou à chaque CVE critique.
- Signature : vérifiez la provenance (`DOCKER_CONTENT_TRUST=1` / cosign)
  pour les images sensibles.

## 58. Supervision — métriques du daemon et des conteneurs

Activer les métriques Prometheus du daemon (`daemon.json`, voir section 6) :

```json
{ "metrics-addr": "127.0.0.1:9323" }
```

```bash
curl -s 127.0.0.1:9323/metrics | grep -E "^engine_daemon_container"
```

Stack de supervision recommandée :

```yaml
services:
  prometheus:
    image: prom/prometheus:v2.53.0
    volumes: ["./prometheus.yml:/etc/prometheus/prometheus.yml:ro", "prom-data:/prometheus"]
    ports: ["127.0.0.1:9090:9090"]
  cadvisor:
    image: gcr.io/cadvisor/cadvisor:v0.49.1
    volumes:
      - /:/rootfs:ro
      - /var/run:/var/run:ro
      - /sys:/sys:ro
      - /var/lib/docker/:/var/lib/docker:ro
    ports: ["127.0.0.1:8080:8080"]
  grafana:
    image: grafana/grafana:11.1.0
    volumes: ["grafana-data:/var/lib/grafana"]
    ports: ["127.0.0.1:3000:3000"]

volumes: { prom-data:, grafana-data: }
```

Métriques clés à alerter : `container_memory_usage_bytes` proche de la limite,
`container_cpu_usage_seconds_total` en saturation, `engine_daemon_container_states`
(conteneurs `unhealthy`), espace `/var/lib/docker` > 80 %.

## 59. Supervision — healthchecks et alertes

- Chaque service critique a un `healthcheck` (sections 18, 30).
- Remontée d'alertes : un exporteur (Prometheus Alertmanager), ou plus simple :

```bash
# Cron : alerte si un conteneur est unhealthy ou restart en boucle
#!/bin/bash
UNHEALTHY=$(docker ps --filter "health=unhealthy" --format '{{.Names}}')
RESTARTING=$(docker ps --filter "status=restarting" --format '{{.Names}}')
[ -n "$UNHEALTHY$RESTARTING" ] && \
  echo "Docker alerte: unhealthy=[$UNHEALTHY] restarting=[$RESTARTING]" | \
  mail -s "[PROD] Docker alerte" astreinte@entreprise.fr
```

```bash
# Voir l'historique des healthchecks d'un conteneur
docker inspect --format '{{range .State.Health.Log}}{{.Output}}{{end}}' api | tail -5
```

## 60. Journalisation centralisée (option production)

```yaml
services:
  api:
    logging:
      driver: gelf
      options:
        gelf-address: "udp://192.168.1.20:12201"
        tag: "prod-api"
```

Alternatives : `syslog` vers rsyslog central, `journald` + forwarder,
ou un agent (Fluent Bit / Promtail) qui lit les `json-file` locaux et les
expédie vers Loki/Elasticsearch. Gardez **toujours** une copie locale avec
rotation en secours (si le collecteur distant tombe, vous gardez les logs).

## 61. SAUVEGARDE — les volumes (le point critique)

> Un conteneur se reconstruit en une commande. **Les volumes, eux, contiennent
> vos données.** Pas de sauvegarde des volumes = pas de PRA.

Méthode 1 — conteneur helper (recommandée, à chaud, sans arrêter le service) :

```bash
# Sauvegarde d'un volume nommé vers un .tar.gz daté
docker run --rm \
  -v db-data:/data:ro \
  -v /srv/backups:/backup \
  alpine:3.20 \
  tar czf /backup/db-data-$(date +%F).tar.gz -C /data .

# Sauvegarde de TOUS les volumes d'une stack compose (depuis son dossier)
docker compose run --rm -v /srv/backups:/backup db \
  sh -c 'tar czf /backup/db-$(date +%F_%H%M).tar.gz -C /var/lib/postgresql/data .'
```

Méthode 2 — dump applicatif (complément indispensable pour les SGBD) :

```bash
# Dump PostgreSQL AVANT la sauvegarde du volume (cohérence garantie)
docker compose exec db pg_dump -U app appdb | gzip > /srv/backups/appdb-$(date +%F).sql.gz

# Dump MySQL/MariaDB
docker compose exec db sh -c 'mysqldump -u root -p"$MARIADB_ROOT_PASSWORD" --all-databases' \
  | gzip > /srv/backups/mariadb-$(date +%F).sql.gz
```

Restauration :

```bash
# Restaurer un volume depuis l'archive
docker run --rm \
  -v db-data:/data \
  -v /srv/backups:/backup:ro \
  alpine:3.20 \
  sh -c 'rm -rf /data/* && tar xzf /backup/db-data-2026-09-26.tar.gz -C /data'

# Restaurer un dump SQL
gunzip -c /srv/backups/appdb-2026-09-26.sql.gz | docker compose exec -T db psql -U app appdb
```

Stratégie 3-2-1 adaptée à Docker :

- [ ] Dump applicatif + archive du volume, **quotidiens**, horodatés
- [ ] 3 copies : hôte + NAS/synology + hors site (rclone vers S3/Backblaze)
- [ ] Rétention : 7 jours glissants + 4 semaines + 3 mois (script de purge)
- [ ] **Test de restauration trimestriel** (une sauvegarde non testée n'existe pas)
- [ ] `docker compose down -v` **interdit** en prod sans double confirmation

Script de purge type :

```bash
#!/bin/bash
# purge_backups.sh : garde 7 jours
find /srv/backups -name "*.tar.gz" -mtime +7 -delete
find /srv/backups -name "*.sql.gz" -mtime +7 -delete
```

## 62. Mises à jour — les images

Procédure standard (zéro surprise) :

```bash
cd /srv/stacks/mon-app
# 1. Sauvegarde (volumes + dumps) AVANT
./backup.sh
# 2. Récupérer les nouvelles images
docker compose pull
# 3. Relancer (recrée uniquement les conteneurs dont l'image a changé)
docker compose up -d
# 4. Vérifier la santé
docker compose ps
docker compose logs --tail=50 -f
# 5. Nettoyer les vieilles images
docker image prune -a -f --filter "until=72h"
```

Rollback express :

```bash
# Revenir au tag précédent (d'où l'intérêt des tags versionnés !)
# éditer compose.yaml : image: mon-app:1.4.1 (au lieu de 1.4.2)
docker compose up -d
```

> Avec `latest`, impossible de savoir vers quoi on revient : **tags versionnés
> obligatoires** (voir section 22).

## 63. Mises à jour — automatiques ? Watchtower avec prudence

[Watchtower](https://containrrr.dev/watchtower/) met à jour les conteneurs
automatiquement quand une nouvelle image sort.

```yaml
services:
  watchtower:
    image: containrrr/watchtower:1.7.1
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock  # ⚠️ risque assumé (section 55)
    environment:
      WATCHTOWER_SCHEDULE: "0 0 4 * * *"   # tous les jours à 4h
      WATCHTOWER_ROLLING_RESTART: "true"    # un par un, pas tout d'un coup
      WATCHTOWER_INCLUDE_STOPPED: "false"
    restart: unless-stopped
```

⚠️ **Prudence en production** :

- Réservé aux services **non critiques** ou aux environnements de test.
- Une mise à jour auto peut casser un service à 4h du matin sans prévenir.
- En prod critique : mises à jour **manuelles**, en heures ouvrées, avec
  sauvegarde + procédure de rollback (section 62).
- Alternative : Watchtower en mode **notification seule**
  (`WATCHTOWER_MONITOR_ONLY=true`) → vous décidez.

## 64. Mises à jour — le moteur Docker lui-même

```bash
# 1. Sauvegarder les stacks (compose.yaml + volumes, section 61)
# 2. Mettre à jour via le dépôt officiel
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin

# 3. Vérifier
docker version
sudo systemctl status docker --no-pager

# 4. Les conteneurs redémarrent (restart policy) ; avec live-restore (section 6),
#    ils survivent même au restart du daemon
```

Bonnes pratiques :

- Lisez le changelog avant une montée de version majeure.
- Testez d'abord sur un hôte de pré-production.
- Ne laissez pas le moteur prendre 3 ans de retard (CVE sur runc/containerd).

## 65. Swarm : introduction (l'orchestrateur intégré)

Swarm transforme plusieurs hôtes Docker en **cluster** : les services sont
répartis, répliqués, et redémarrés automatiquement en cas de panne d'un nœud.

```bash
# Initialiser le cluster (sur le futur manager)
docker swarm init --advertise-addr 192.168.1.10

# Joindre un worker (commande affichée par l'init)
docker swarm join --token SWMTKN-... 192.168.1.10:2377

# Voir les nœuds
docker node ls

# Déployer une stack répliquée
docker stack deploy -c compose.yaml mon-app

# Mettre à l'échelle
docker service scale mon-app_api=3

# Voir les tâches (quel conteneur tourne sur quel nœud)
docker service ps mon-app_api
docker stack services mon-app
```

compose.yaml adapté à Swarm :

```yaml
services:
  api:
    image: registry.interne:5000/mon-app:1.4.2
    deploy:
      replicas: 3
      update_config:
        parallelism: 1
        delay: 10s
        failure_action: rollback
      restart_policy:
        condition: on-failure
```

Limites à connaître :

- Swarm est **en maintenance** chez Docker (pas de grosses nouveautés) ;
  pour du gros multi-hôtes moderne, Kubernetes domine.
- Mais pour 2–5 serveurs, Swarm reste **simple, robuste et suffisant**.
- Les `build:` locaux ne marchent pas en Swarm : poussez d'abord l'image
  dans un registre accessible par tous les nœuds.

## 66. Portainer : administrer via une interface web

[Portainer](https://www.portainer.io/) : UI web pour voir/gérer conteneurs,
images, volumes, réseaux, stacks compose.

```yaml
services:
  portainer:
    image: portainer/portainer-ce:2.21.0
    container_name: portainer
    ports:
      - "127.0.0.1:9443:9443"   # HTTPS ; exposez via reverse proxy + auth
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock  # ⚠️ voir section 55
      - portainer-data:/data
    restart: unless-stopped

volumes:
  portainer-data:
```

Règles d'usage :

- **Jamais exposé tel quel sur Internet** : derrière un reverse proxy avec
  TLS + authentification forte (et idéalement restriction IP/VPN).
- Pratique pour : visualiser l'état, déployer une stack compose via l'UI,
  donner un accès **limité** (rôles) à une équipe sans accès SSH.
- Ne remplace pas la maîtrise de la CLI pour le dépannage.

## 67. Reverse proxy : Traefik (recommandé)

Traefik découvre **automatiquement** les conteneurs via des labels et génère
les certificats Let's Encrypt tout seul.

```yaml
services:
  traefik:
    image: traefik:v3.1
    container_name: traefik
    command:
      - --providers.docker=true
      - --providers.docker.exposedbydefault=false
      - --entrypoints.web.address=:80
      - --entrypoints.websecure.address=:443
      - --certificatesresolvers.letsencrypt.acme.httpchallenge.entrypoint=web
      - --certificatesresolvers.letsencrypt.acme.email=admin@entreprise.fr
      - --certificatesresolvers.letsencrypt.acme.storage=/letsencrypt/acme.json
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro  # ⚠️ lecture seule !
      - traefik-data:/letsencrypt
    restart: unless-stopped

  web:
    image: nginx:1.27-alpine
    labels:
      - traefik.enable=true
      - traefik.http.routers.web.rule=Host(`www.entreprise.fr`)
      - traefik.http.routers.web.entrypoints=websecure
      - traefik.http.routers.web.tls.certresolver=letsencrypt
      - traefik.http.services.web.loadbalancer.server.port=80
    networks:
      - proxy

networks:
  proxy:
    name: proxy
    external: true

volumes:
  traefik-data:
```

```bash
docker network create proxy   # réseau partagé (voir section 32)
chmod 600 letsencrypt/acme.json  # si créé à la main
```

## 68. Reverse proxy : alternative nginx manuel

Si vous préférez un nginx classique devant vos conteneurs :

```nginx
# /srv/proxy/nginx.conf
upstream api { server api:8000; }

server {
    listen 80;
    server_name api.entreprise.fr;
    location / {
        proxy_pass http://api;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

```yaml
services:
  proxy:
    image: nginx:1.27-alpine
    ports: ["80:80", "443:443"]
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
      - ./certs:/etc/nginx/certs:ro
    depends_on: [api]
```

Certificats : utilisez certbot sur l'hôte ou le compagnon
`nginxproxy/acme-companion` pour automatiser Let's Encrypt.

## 69. Cas pratique : stack LAMP complète (commentée)

```yaml
name: lamp

services:
  web:
    image: php:8.3-apache
    container_name: lamp-web
    ports:
      - "127.0.0.1:8080:80"      # exposé en local ; le reverse proxy prendra le relais
    volumes:
      - ./www:/var/www/html:ro              # code source (lecture seule)
      - ./config/php.ini:/usr/local/etc/php/php.ini:ro
      - ./config/vhost.conf:/etc/apache2/sites-enabled/000-default.conf:ro
    environment:
      DB_HOST: db
      DB_NAME: appdb
      DB_USER: app
      # mot de passe via secret, jamais en clair :
    secrets: [db_password]
    depends_on:
      db: { condition: service_healthy }
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/"]
      interval: 30s
      timeout: 5s
      retries: 3
    restart: unless-stopped
    networks: [front, back]

  db:
    image: mariadb:11.4
    container_name: lamp-db
    environment:
      MARIADB_DATABASE: appdb
      MARIADB_USER: app
      MARIADB_PASSWORD_FILE: /run/secrets/db_password
      MARIADB_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
    secrets: [db_password, db_root_password]
    volumes:
      - db-data:/var/lib/mysql          # DONNÉES persistantes (à sauvegarder !)
      - ./backups:/backups
    healthcheck:
      test: ["CMD", "healthcheck.sh", "--connect", "--innodb_initialized"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped
    networks: [back]                    # pas d'accès direct depuis l'extérieur

  phpmyadmin:
    image: phpmyadmin:5.2
    container_name: lamp-pma
    environment:
      PMA_HOST: db
      PMA_USER: app
      PMA_PASSWORD_FILE: /run/secrets/db_password
    secrets: [db_password]
    ports:
      - "127.0.0.1:8081:80"            # admin locale uniquement
    depends_on:
      db: { condition: service_healthy }
    restart: unless-stopped
    networks: [back]
    profiles: ["admin"]                 # ne démarre qu'avec --profile admin

networks:
  front:
  back:
    internal: true

volumes:
  db-data:

secrets:
  db_password:      { file: ./secrets/db_password.txt }
  db_root_password: { file: ./secrets/db_root_password.txt }
```

```bash
docker compose up -d
docker compose --profile admin up -d   # avec phpMyAdmin
curl http://localhost:8080/
# Sauvegarde :
docker compose exec db sh -c 'mariadb-dump -u root -p"$MARIADB_ROOT_PASSWORD" appdb' \
  | gzip > backups/appdb-$(date +%F).sql.gz
```

## 70. Cas pratique : Nextcloud (fichier + base + cache)

```yaml
name: nextcloud

services:
  app:
    image: nextcloud:29-apache
    container_name: nc-app
    ports: ["127.0.0.1:8080:80"]
    volumes:
      - nc-html:/var/www/html          # code + config + data par défaut
      - ./data:/var/www/html/data      # données utilisateurs (bind, simple à sauvegarder)
    environment:
      MYSQL_HOST: db
      MYSQL_DATABASE: nextcloud
      MYSQL_USER: nc
      MYSQL_PASSWORD_FILE: /run/secrets/db_password
      NEXTCLOUD_ADMIN_USER_FILE: /run/secrets/admin_user
      NEXTCLOUD_ADMIN_PASSWORD_FILE: /run/secrets/admin_password
      REDIS_HOST: cache
    secrets: [db_password, admin_user, admin_password]
    depends_on:
      db: { condition: service_healthy }
      cache: { condition: service_started }
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost/status.php"]
      interval: 60s
      timeout: 10s
      retries: 3
    restart: unless-stopped

  db:
    image: mariadb:11.4
    environment:
      MYSQL_DATABASE: nextcloud
      MYSQL_USER: nc
      MYSQL_PASSWORD_FILE: /run/secrets/db_password
      MYSQL_ROOT_PASSWORD_FILE: /run/secrets/db_root_password
    secrets: [db_password, db_root_password]
    volumes: [nc-db:/var/lib/mysql]
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    healthcheck:
      test: ["CMD", "healthcheck.sh", "--connect"]
      interval: 10s
      timeout: 5s
      retries: 5
    restart: unless-stopped

  cache:
    image: redis:7-alpine
    command: ["redis-server", "--requirepass", "changeme-redis"]
    volumes: [nc-redis:/data]
    restart: unless-stopped

  cron:
    image: nextcloud:29-apache
    entrypoint: ["/cron.sh"]           # tâches de fond Nextcloud
    volumes: [nc-html:/var/www/html, ./data:/var/www/html/data]
    depends_on: [app]
    restart: unless-stopped

volumes:
  nc-html:
  nc-db:
  nc-redis:

secrets:
  db_password:      { file: ./secrets/db_password.txt }
  db_root_password: { file: ./secrets/db_root_password.txt }
  admin_user:       { file: ./secrets/admin_user.txt }
  admin_password:   { file: ./secrets/admin_password.txt }
```

Points d'attention Nextcloud :

- Mettez un **reverse proxy TLS** devant (section 67/68), Nextcloud exige
  HTTPS pour un usage sérieux.
- `trusted_domains` et `overwrite.cli.url` dans `config.php` après le premier
  démarrage.
- Sauvegarde = **les 3 volumes** (`nc-html`, `nc-db`, `./data`) + dump MariaDB.
- Montées de version : une **majeure à la fois** (29 → 30, jamais 29 → 31).

## 71. Dépannage — le conteneur crash en boucle

```bash
# 1. Voir pourquoi il meurt
docker ps -a                       # colonne STATUS : Exited (1) ...
docker logs --tail 100 mon-app     # l'erreur est souvent dans les 20 dernières lignes
docker inspect --format '{{.State.Error}}' mon-app

# 2. Cas fréquents et remèdes
```

| Symptôme dans les logs | Cause probable | Remède |
|---|---|---|
| `connection refused` vers `db` | base pas prête | `depends_on` + healthcheck + retry dans l'app |
| `permission denied` sur `/data` | UID du conteneur ≠ propriétaire du bind mount | `chown -R 1001:1001 /srv/data` ou `user:` adapté |
| `address already in use` | port hôte déjà pris | changer le port publié, `ss -tlnp` pour trouver le coupable |
| `no such file` au démarrage | mauvais `WORKDIR`/`COPY` dans le Dockerfile | vérifier les chemins, rebuild |
| `exec format error` | image pour une autre archi (ARM vs x86) | `--platform linux/amd64` ou image multi-arch |
| OOMKilled (`docker inspect` → `OOMKilled: true`) | limite mémoire trop basse | augmenter `memory`, chercher la fuite |

```bash
# Démarrer un shell malgré un CMD qui crash (pour inspecter)
docker run --rm -it --entrypoint sh mon-app:1.0
```

## 72. Dépannage — réseau : le conteneur ne joint rien

```bash
# 1. Le DNS intégré répond-il ?
docker exec app getent hosts db
docker exec app cat /etc/resolv.conf   # doit contenir 127.0.0.11

# 2. Le réseau existe-t-il, qui est dedans ?
docker network inspect mon-reseau --format '{{range .Containers}}{{.Name}} {{end}}'

# 3. Connecter/déconnecter à chaud
docker network connect mon-reseau app
docker network disconnect mon-reseau app

# 4. Tester depuis un conteneur jetable sur le même réseau
docker run --rm --network mon-reseau alpine:3.20 \
  sh -c "apk add -q curl && curl -sv http://api:8000/health" 2>&1 | tail -20

# 5. Côté hôte : iptables et IP forwarding
sudo iptables -L DOCKER-USER -n -v
sysctl net.ipv4.ip_forward   # doit valoir 1
```

Causes classiques :

- Conteneurs sur **deux réseaux différents** sans réseau commun.
- `network_mode: host` + tentative de joindre par nom de service (pas de DNS
  dans ce mode).
- Firewall de l'hôte (ufw) qui bloque le forwarding : avec ufw, réglez
  `DEFAULT_FORWARD_POLICY="ACCEPT"` dans `/etc/default/ufw`.
- Collision de sous-réseaux avec le VPN/LAN (voir `default-address-pools`,
  section 6).

## 73. Dépannage — disque plein : /var/lib/docker déborde

```bash
# Diagnostic
df -h /var/lib/docker
docker system df
sudo du -sh /var/lib/docker/* | sort -h | tail

# Les gros consommateurs de logs
sudo du -sh /var/lib/docker/containers/*/*-json.log 2>/dev/null | sort -h | tail -5

# Nettoyage progressif (du moins au plus agressif)
docker container prune -f                    # conteneurs arrêtés
docker image prune -f                        # images non utilisées (dangling)
docker image prune -a -f --filter "until=720h"  # images non utilisées depuis 30 j
docker builder prune -f                      # cache de build
docker volume prune -f                       # volumes orphelins (⚠️ vérifiez avant !)
docker system prune -a -f --volumes          # TOUT (dernier recours)
```

Prévention :

- Rotation des logs configurée (section 50) — **non négociable**.
- Cron hebdo : `docker image prune -a -f --filter "until=720h"`.
- Alerte disque à 80 % (supervision, section 58).
- Ne jamais laisser `docker system prune -a` dans un cron aveugle sur un
  hôte de prod (il peut supprimer une image encore référencée par un
  conteneur arrêté que vous comptiez relancer).

## 74. Dépannage — permissions sur les volumes (bind mounts)

Le piège : le conteneur tourne avec l'UID 1001, mais `/srv/data` appartient à
root sur l'hôte → `permission denied`.

```bash
# 1. Identifier l'UID utilisé dans le conteneur
docker exec app id
# uid=1001(appuser) gid=1001(appuser)

# 2. Aligner le propriétaire côté hôte
sudo chown -R 1001:1001 /srv/data

# 3. Ou forcer l'UID au lancement (si l'image le supporte)
docker run -d --user 1001:1001 -v /srv/data:/data mon-app:1.0
```

```yaml
# En compose
services:
  app:
    user: "1001:1001"
    volumes: ["/srv/data:/data"]
```

> Les **volumes nommés** n'ont pas ce problème : Docker les initialise avec
> les permissions du contenu de l'image. C'est une raison de plus de les
> préférer pour les données.

## 75. Dépannage — le daemon ne démarre plus

```bash
# 1. Logs du daemon
sudo journalctl -u docker --no-pager | tail -50

# 2. Cause n°1 : daemon.json invalide (JSON mal formé)
sudo python3 -m json.tool /etc/docker/daemon.json
# ou : sudo dockerd --validate  (selon version)

# 3. Cause n°2 : disque plein (voir section 73)

# 4. Cause n°3 : conflit iptables / réseau après changement de config
# Revenir à une config minimale et restart :
sudo systemctl restart docker

# 5. Vérifier le socket
ls -l /var/run/docker.sock
sudo systemctl status containerd --no-pager
```

## 76. Dépannage — méthode générale (checklist)

Face à n'importe quel problème Docker, dans l'ordre :

1. [ ] `docker ps -a` : le conteneur existe ? quel statut / code de sortie ?
2. [ ] `docker logs --tail 100 <nom>` : l'erreur est dans les logs 9 fois sur 10
3. [ ] `docker inspect <nom>` : mounts, networks, env, health, OOMKilled
4. [ ] `docker stats` : saturation CPU/RAM ?
5. [ ] `docker system df` + `df -h` : disque plein ?
6. [ ] Reproduire en minimal : `docker run --rm -it image sh`
7. [ ] `docker compose config` : le YAML interpolé est-il celui attendu ?
8. [ ] `journalctl -u docker` : le daemon lui-même se plaint-il ?

## 77. Les 12 erreurs classiques (et comment les éviter)

### Erreur 1 — `latest` en production
Symptôme : un `pull` + `up -d` change de version majeure sans prévenir, tout
casse. Remède : tags versionnés épinglés (sections 22, 57).

### Erreur 2 — Pas de rotation de logs
Symptôme : `/var/lib/docker` plein à 100 %, daemon qui ne répond plus.
Remède : `max-size`/`max-file` dans `daemon.json` (sections 6, 50).

### Erreur 3 — Données dans le conteneur au lieu d'un volume
Symptôme : `docker compose down -v` ou recréation → données perdues.
Remède : volume nommé pour **toutes** les données (sections 45, 61).

### Erreur 4 — Secrets en clair dans le YAML versionné
Symptôme : mot de passe root de la BDD dans git. Remède : secrets compose +
`*_FILE` (sections 53, 54).

### Erreur 5 — `--privileged` « pour que ça marche »
Symptôme : faille de sécurité béante. Remède : `--cap-add` ciblé, durcissement
(section 56).

### Erreur 6 — Tout en root dans les conteneurs
Symptôme : compromission d'un conteneur = root facile. Remède : `USER`
dans le Dockerfile + `user:` en compose (sections 17, 56).

### Erreur 7 — Ports publiés sur toutes les interfaces par défaut
Symptôme : `-p 8080:80` expose le service à tout le réseau alors qu'on voulait
du local. Remède : `-p 127.0.0.1:8080:80` (section 9).

### Erreur 8 — `depends_on` sans healthcheck
Symptôme : l'API démarre avant la BDD et crash en boucle. Remède :
`condition: service_healthy` + retry applicatif (section 28).

### Erreur 9 — Images jamais mises à jour
Symptôme : CVE critiques vieilles de 2 ans dans les images. Remède : rebuild
mensuel, `docker scout`/`trivy` en CI (sections 57, 62).

### Erreur 10 — Aucune limite de ressources
Symptôme : un conteneur qui fuit la RAM tue l'hôte entier (OOM killer).
Remède : `memory`/`cpus`/`pids-limit` partout (section 51).

### Erreur 11 — Le socket Docker monté n'importe où
Symptôme : un conteneur compromis = hôte compromis. Remède : ne monter le
socket que pour Portainer/Traefik/watchtower, en `:ro` quand possible
(section 55).

### Erreur 12 — Pas de sauvegarde testée des volumes
Symptôme : le jour où le disque meurt, on découvre que le « backup » ne
restaurait rien. Remède : dumps + archives + **test de restauration
trimestriel** (section 61).

## 78. Sécurité — récapitulatif du chapitre (checklist d'audit)

À passer en revue pour chaque hôte Docker de production :

- [ ] Installation depuis le dépôt officiel, moteur à jour (section 4, 64)
- [ ] Groupe `docker` réservé aux admins (section 55)
- [ ] Socket non monté dans les conteneurs applicatifs (section 55)
- [ ] Aucun `--privileged` ; capabilities minimales (section 56)
- [ ] Conteneurs non-root (`USER`/`user:`), `read_only` quand possible (section 56)
- [ ] `no-new-privileges:true` (section 56)
- [ ] Images officielles/vérifiées, tags épinglés ou digests (section 57)
- [ ] Scan de vulnérabilités (scout/trivy) avant mise en prod (section 57)
- [ ] Secrets hors git, via fichiers `*_FILE`, `chmod 600` (sections 53, 54)
- [ ] Ports publiés restreints (`127.0.0.1:` si local) (section 9)
- [ ] Réseaux dédiés par stack, `internal: true` pour les données (section 32)
- [ ] Rotation des logs activée (section 50)
- [ ] Limites de ressources sur tous les services (section 51)
- [ ] Healthchecks sur les services critiques (section 30)
- [ ] Sauvegardes des volumes + test de restauration (section 61)
- [ ] Mises à jour planifiées (images + moteur) (sections 62, 64)

## 79. Pense-bête de poche — conteneurs

```bash
docker ps -a                    # tout voir
docker logs -f --tail 50 NOM    # suivre les logs
docker exec -it NOM sh          # entrer dedans
docker stop/start/restart NOM   # cycle de vie
docker inspect NOM | less       # tout savoir
docker stats                    # ressources temps réel
docker top NOM                  # processus
docker cp NOM:/chemin ./local   # copier un fichier hors du conteneur
docker cp ./local NOM:/chemin   # ... ou vers le conteneur
docker rename ANCIEN NOUVEAU    # renommer
docker update --memory 1g NOM   # changer une limite à chaud (selon option)
```

## 80. Pense-bête de poche — images, volumes, réseaux

```bash
# Images
docker images
docker pull IMAGE:TAG
docker rmi IMAGE
docker tag SRC DST
docker history IMAGE
docker system df

# Volumes
docker volume ls
docker volume inspect NOM
docker volume prune              # ⚠️ orphelins

# Réseaux
docker network ls
docker network create --driver bridge --subnet 172.30.0.0/24 NET
docker network connect NET CONTENEUR

# Nettoyage
docker system prune              # doux
docker system prune -a --volumes # radical ⚠️
```

## 81. Pense-bête de poche — compose

```bash
docker compose up -d              # lancer
docker compose ps                 # état
docker compose logs -f SERVICE    # logs d'un service
docker compose exec SERVICE sh    # shell dans un service
docker compose run --rm SERVICE CMD  # one-shot
docker compose pull && docker compose up -d  # mettre à jour
docker compose down               # arrêter (garde les volumes)
docker compose down -v            # arrêter + SUPPRIME les volumes ⚠️
docker compose config             # config finale résolue
docker compose build SERVICE      # rebuild une image
```

## 82. Glossaire

| Terme | Définition |
|---|---|
| Bridge | Driver réseau par défaut : NAT entre conteneurs et hôte |
| BuildKit | Moteur de build moderne (parallèle, cache avancé, secrets de build) |
| cgroup | Mécanisme noyau limitant CPU/RAM par groupe de processus |
| Containerd | Daemon gérant le cycle de vie des conteneurs pour dockerd |
| Couche (layer) | Différence immuable empilée pour former une image |
| Daemon (dockerd) | Service qui construit, lance et supervise les conteneurs |
| Digest | Empreinte SHA256 immuable d'une image (`image@sha256:...`) |
| Dockerfile | Recette de construction d'une image |
| Healthcheck | Test périodique de santé d'un conteneur |
| Host (réseau) | Mode où le conteneur partage la pile réseau de l'hôte |
| Image | Modèle immuable pour créer des conteneurs |
| Macvlan | Driver donnant au conteneur une MAC/IP propre sur le LAN |
| Multi-stage | Build en plusieurs étapes pour des images finales légères |
| Namespace | Isolation noyau (PID, réseau, montage…) entre conteneurs |
| Orchestrateur | Système répartissant les conteneurs (Swarm, Kubernetes) |
| Overlay | Réseau virtuel multi-hôtes (Swarm) |
| Overlay2 | Driver de stockage moderne (union filesystem) |
| runc | Exécutable bas niveau lançant les conteneurs (OCI) |
| Registry | Entrepôt d'images (Docker Hub, Harbor, registre privé) |
| Restart policy | Comportement au crash/arrêt (`unless-stopped`…) |
| Rootless | Mode Docker sans daemon root (sécurité renforcée) |
| Secret | Donnée sensible injectée via fichier, jamais en variable claire |
| Socket Docker | `/var/run/docker.sock` : API du daemon (équivaut à root) |
| Stack | Ensemble de services déployés ensemble (compose / Swarm) |
| Tag | Étiquette de version d'image (`1.4.2`, `latest`…) |
| tmpfs | Montage en RAM, non persisté |
| Volume | Stockage persistant géré par Docker |
| Bind mount | Dossier/fichier de l'hôte monté dans un conteneur |

## 83. Quiz — 10 questions (réponses en fin de section)

1. Quelle différence entre une image et un conteneur ?
2. Pourquoi faut-il éviter le snap et le paquet `docker.io` d'Ubuntu pour
   installer Docker ?
3. Que se passe-t-il si on ne configure pas la rotation des logs ?
4. Pourquoi `depends_on` seul ne suffit-il pas à garantir qu'une API trouve
   sa base de données prête ?
5. Citez 3 raisons de préférer un volume nommé à un bind mount pour une base
   de données.
6. Que risque-t-on à monter `/var/run/docker.sock` dans un conteneur ?
7. Quelle est la différence entre `ARG` et `ENV` dans un Dockerfile ?
8. Pourquoi faut-il éviter le tag `latest` en production ?
9. Que fait `docker compose down -v` et pourquoi est-ce dangereux ?
10. Citez 4 éléments de la checklist de durcissement d'un conteneur.

**Réponses :**

1. L'image est un modèle immuable (lecture seule) ; le conteneur est une
   instance en cours d'exécution de cette image, avec une couche inscriptible
   éphémère.
2. Le paquet `docker.io` est souvent en retard de versions (CVE non corrigées)
   et le snap pose des problèmes de permissions, volumes et réseau. Le dépôt
   officiel `download.docker.com` garantit des versions à jour et supportées.
3. Les fichiers `*-json.log` grossissent indéfiniment jusqu'à remplir
   `/var/lib/docker` à 100 %, ce qui bloque le daemon et peut planter l'hôte.
4. `depends_on` ne garantit que l'**ordre de démarrage**, pas que le service
   soit prêt à accepter des connexions. Il faut `condition: service_healthy`
   + un healthcheck sur la base + une logique de retry dans l'application.
5. (a) Docker initialise le volume avec le contenu/permissions de l'image
   (pas de problème d'UID comme avec les bind mounts) ; (b) le chemin est
   géré par Docker, portable entre hôtes ; (c) cycle de vie découplé du
   conteneur et adapté aux drivers distants (NFS…).
6. Quiconque contrôle le socket peut lancer un conteneur `--privileged`
   montant `/` de l'hôte → **root sur l'hôte**. Réservé aux outils d'admin
   légitimes, en `:ro` si possible.
7. `ARG` n'existe qu'au build (non persisté dans l'image finale) ; `ENV`
   persiste dans l'image et le conteneur (visible via `docker inspect`).
   Aucun des deux ne doit contenir de secret.
8. `latest` est un tag mouvant : on ne sait pas quelle version exacte tourne,
   impossible de faire un rollback fiable ni un déploiement reproductible.
9. Arrête la stack **et supprime les volumes nommés** → perte des données
   persistantes. Dangereux en production.
10. Au choix : utilisateur non-root (`USER`/`user:`), `read_only: true`,
    `cap_drop: [ALL]` + `cap_add` minimal, `no-new-privileges:true`,
    `pids_limit`, pas de `--privileged`, ports restreints, healthcheck.

## 84. Pour aller plus loin

- **Documentation officielle** : https://docs.docker.com (référence, guides,
  bonnes pratiques de sécurité)
- **Docker Bench for Security** : script d'audit CIS Docker
  (`docker run --rm -v /var/run/docker.sock:/var/run/docker.sock:ro ...`)
- **Rootless Docker** : faire tourner le daemon sans root
  (https://docs.docker.com/engine/security/rootless/)
- **Hadolint** : linter de Dockerfiles (à intégrer en CI)
- **Trivy** : scan de vulnérabilités images + IaC (https://aquasecurity.github.io/trivy/)
- **Harbor** : registre privé d'entreprise (RBAC, scan, réplication)
- **Kubernetes** : quand Swarm ne suffit plus (k3s pour commencer léger)
- **Livre** : *Docker in Practice* / *Docker Deep Dive* (Nigel Poulton)
- **Pratique** : refaites ce guide en labo — une stack LAMP, un Traefik, un
  Nextcloud — puis cassez-les volontairement (kill -9, disque plein,
  `down -v` sur une copie) pour apprendre le dépannage sans risque.

---

*Fin du guide — bon courage, et sauvegardez vos volumes.* 🐳
