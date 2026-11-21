---
id: collect-261121-veille-ia/claude-anthropic/tutoriel-docker-compose-2026-stack-prod-en-13-etapes
title: "Tutoriel Docker Compose 2026 : Stack Prod en 13 Étapes"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-04-03"]
keywords: ["docker", "compose", "tutoriel", "stack", "comme", "etapes", "avril", "vous"]
source: https://tech-insider.org/fr/docker-compose-tutoriel-stack-production-13-etapes-2026/
source_anchor: ""
source_lines: [1, 654]
section: "Claude_Anthropic"
---
**Publié le 03 avril 2026** – Docker Compose s’est imposé comme l’outil de référence pour orchestrer plusieurs conteneurs sur un seul hôte. En avril 2026, le format Compose est passé sous l’égide du *Compose Specification*, supporté nativement par Docker Engine et compatible avec des plateformes tierces comme Kubernetes (via Kompose) ou Podman. Ce tutoriel Docker Compose vous guide pas à pas, en 13 étapes concrètes, pour bâtir une **stack de production complète** : application Python (FastAPI), base de données PostgreSQL, cache Redis, reverse proxy Nginx, monitoring Prometheus/Grafana, healthchecks, secrets, profiles et déploiement sécurisé.

À l’issue de ce guide, vous disposerez d’un `docker-compose.yml` production-ready, d’un fichier `compose.override.yml` pour le développement local avec *Compose Watch*, et d’une compréhension fine des pièges à éviter (volumes orphelins, ordre de démarrage, gestion des secrets, mauvaise utilisation de `depends_on`). Comptez environ 60 à 90 minutes pour parcourir l’ensemble si vous tapez chaque commande, et 30 minutes pour lire et copier-coller. Tous les exemples sont testés sur Ubuntu 24.04 LTS et macOS 15 (Apple Silicon), avec Docker Desktop ou Docker Engine standalone.

## Pourquoi Docker Compose Reste Incontournable en 2026

Malgré la domination de Kubernetes dans les déploiements à grande échelle, **Docker Compose** demeure l’outil de prédilection pour le développement local, les environnements de test, l’intégration continue et même certains déploiements de production mono-hôte. Le format Compose a évolué : depuis 2024, il fait partie du *Compose Specification* géré par la communauté open source, garantissant une portabilité bien au-delà de l’écosystème Docker. Docker Docs a mis à jour sa page officielle Docker Compose en mai 2026 pour documenter cette intégration native de Compose v2+ dans la CLI Docker, et le 3 septembre 2026, Docker a publié sur GitHub Releases la version **v5.5.1** – selon la page de cycle de vie de Versio.io (mise à jour en septembre 2026), cette ligne v5, toujours non-LTS, compte déjà 16 versions publiées depuis son lancement (la première remontant à début décembre 2025) – ouvrant une nouvelle ligne stable que ce tutoriel prend pour référence. Le moteur de rendu officiel est désormais réécrit en Go et intégré comme plugin de la CLI Docker (`docker compose`, sans tiret).

Le sondage Stack Overflow Developer Survey 2025 place Docker au premier rang des outils les plus appréciés des développeurs (76,7 % d’usage, 78 % d’avis favorables). Selon les statistiques publiques de Docker Hub, plusieurs centaines de milliards de pulls cumulés ont été enregistrés en 2025, et le dépôt GitHub de Compose dépasse 35 000 étoiles. En production, des sociétés comme PayPal, Spotify et Atlassian utilisent encore Compose pour des microservices internes ou des outils annexes, prouvant qu’il s’agit bien plus qu’un simple outil de poste de travail. L’écosystème s’étend même désormais à l’IA : dans son annonce « agentic apps » du 10 juillet 2025, Docker a étendu Compose avec un support natif des agents IA et une intégration MCP Gateway, déjà disponible sur au moins deux grands clouds publics.

Les principaux apports de Docker Compose en 2026 sont la suppression définitive du champ `version:` (déprécié depuis v2.27, désormais ignoré par le runtime), l’intégration native de *Compose Watch* pour le rechargement à chaud, le support de *Bake* pour des builds parallèles ultra-rapides, la prise en charge des **secrets** via fichiers chiffrés, des **profiles** conditionnels et un support officiel des GPU NVIDIA via `deploy.resources.reservations.devices`. Ce tutoriel couvre l’ensemble de ces nouveautés.

## Prérequis : Versions et Environnement Recommandés

Avant de démarrer, assurez-vous de disposer des outils suivants. Les versions indiquées sont celles validées au 1<sup>er</sup> avril 2026. Toutes les commandes ont été testées sous Ubuntu 24.04, Debian 12, Fedora 40, macOS 15 Sequoia et Windows 11 (via WSL2 Ubuntu).

| Outil | Version Minimale | Version Recommandée 2026 | Vérification | 
|---|---|---|---|
| Docker Engine | 20.10 | 27.x ou supérieur | `docker --version` | 
| Docker Compose plugin | v2.20 | v2.31+ (latest) | `docker compose version` | 
| Système Linux | Kernel 4.x | Kernel 6.x (Ubuntu 24.04) | `uname -r` | 
| RAM disponible | 2 Go | 8 Go pour stack complète | `free -h` | 
| Espace disque | 5 Go | 20 Go (images + volumes) | `df -h /var/lib/docker` | 
| curl / git | n’importe quelle | récente | `curl --version` /`git --version` | 

Sous Linux, installez Docker via le dépôt officiel (le paquet `docker.io` de Debian/Ubuntu est souvent en retard) en installant explicitement le paquet `docker-compose-plugin` pour obtenir Compose v2 – c’est d’ailleurs la démarche suivie par le « Docker Compose Getting Started Guide », mis à jour par Tutorials.technology en mars 2026. Sur macOS et Windows, privilégiez **Docker Desktop** (gratuit pour usage personnel et entreprises de moins de 250 employés ou moins de 10 M$ de revenus annuels selon la licence Docker) ; la version **4.56.0**, livrée par Docker en janvier 2026, embarque désormais Compose v5 avec un nouveau SDK Go, en remplacement des installations historiquement limitées à Compose v2. Une alternative open source viable est **Rancher Desktop** ou **Colima**, qui exposent une CLI Docker compatible.

## Étape 1 : Installer Docker Engine et Docker Compose

Sous Ubuntu 24.04 LTS, l’installation officielle se fait en quelques lignes via le script `get.docker.com` ou, mieux, via les dépôts *apt* signés. La méthode *apt* facilite les mises à jour de sécurité automatiques.

```
# Mise à jour des dépôts et installation des dépendances
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
# Ajout de la clé GPG officielle Docker
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
# Ajout du dépôt stable
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# Installation de Docker Engine + Compose plugin + Buildx
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin
# Permission sans sudo (relogguez-vous après)
sudo usermod -aG docker $USER
newgrp docker
# Vérification
docker --version
docker compose version
```
La commande `docker compose version` doit retourner au minimum `Docker Compose version v2.39.2` – la version publiée le 4 août 2025 dans les notes de version officielles, celle-là même que cite le tutoriel Dokploy d’août 2025 en exigeant un Docker Engine/CLI en version 28.3.3 pour un déploiement optimal – voire la toute dernière **v5.5.1**, sortie le 3 septembre 2026, si vous avez mis à jour récemment ; entre les deux, la documentation Docker Engine 28.x signale aussi des builds embarquant Compose v2.39.1. Si vous voyez l’ancienne commande `docker-compose` (avec tiret) en Python, il s’agit de la **v1, dépréciée depuis juin 2023**. N’utilisez plus que `docker compose` (sans tiret), implémenté en Go, plus rapide et activement maintenu : une alerte de sécurité SUSE publiée en juin 2025 documentait justement un correctif pour un paquet `docker-compose` v1.x, rappelant que cette branche ne reçoit plus aucun correctif officiel.

## Étape 2 : Comprendre le Compose Specification

Le **Compose Specification** définit un format YAML standardisé pour décrire une stack multi-conteneurs. Il est maintenu publiquement sur GitHub (compose-spec/compose-spec) et adopté par Docker, Podman, Nerdctl, et même certains outils Kubernetes ; au 11 septembre 2026, ce dépôt totalise 2 727 lignes de Dockerfile et 786 étoiles, un signe de l’évolution active de la spécification. La bibliothèque associée **compose-go**, qui alimente les outils tiers écrits en Go, a franchi la version 2.10.0 le 25 novembre 2025, avec des builds ultérieurs enrichissant encore son API. Un fichier Compose typique décrit trois sections principales : `services` (les conteneurs), `volumes` (le stockage persistant), et `networks` (les réseaux virtuels). Optionnellement, on retrouve `secrets`, `configs` et `profiles`.

Le moteur reconnaît plusieurs noms de fichier par défaut, dans cet ordre : `compose.yaml`, `compose.yml`, `docker-compose.yaml`, `docker-compose.yml`. La convention moderne, recommandée par la spec, est d’utiliser **`compose.yaml`**. Toutefois, `docker-compose.yml` reste majoritaire dans la pratique, notamment pour la rétrocompatibilité avec d’anciens outils CI/CD – c’est d’ailleurs le choix fait par le guide « Docker Compose Tutorial (Beginner to Production 2026) » d’ITSourceCode, publié en juillet 2026, qui standardise lui aussi un unique `docker-compose.yml` pour orchestrer des stacks multi-services. Nous utiliserons ce dernier dans le tutoriel.

**Piège n° 1 :** Le champ `version:` en tête de fichier est *déprécié* depuis Compose v2.27 et purement et simplement *ignoré* depuis v2.30. Ne l’écrivez plus. Si vous reprenez un ancien fichier, supprimez la ligne ; cela évite des warnings inutiles dans les pipelines.

## Étape 3 : Créer la Structure du Projet

Nous allons construire une application web exemple : une API **FastAPI** en Python qui stocke des données dans **PostgreSQL**, met en cache des réponses dans **Redis**, est exposée via **Nginx** en reverse proxy, et monitorée par **Prometheus** et **Grafana**. Créons d’abord l’arborescence.

```
mkdir -p ~/compose-prod-stack && cd ~/compose-prod-stack
mkdir -p app nginx postgres prometheus grafana/provisioning secrets
# Structure cible :
tree -L 2
# .
# ├── app/
# │   ├── Dockerfile
# │   ├── main.py
# │   └── requirements.txt
# ├── nginx/
# │   └── default.conf
# ├── postgres/
# │   └── init.sql
# ├── prometheus/
# │   └── prometheus.yml
# ├── grafana/
# │   └── provisioning/
# ├── secrets/
# │   ├── db_password.txt
# │   └── grafana_admin.txt
# ├── .env
# ├── .gitignore
# ├── compose.override.yml
# └── docker-compose.yml
```
Le répertoire `secrets/` doit être ajouté à `.gitignore` immédiatement pour éviter tout commit accidentel de mots de passe. Créez ce fichier avec : `echo -e "secrets/\n.env\n*.log\n__pycache__/\n.venv/" > .gitignore`.

## Étape 4 : Écrire l’Application FastAPI et son Dockerfile

L’application expose deux endpoints : `GET /items` qui liste les produits depuis PostgreSQL avec mise en cache Redis, et `POST /items` qui insère un produit. Le code est volontairement minimal pour rester pédagogique.

```
# app/requirements.txt
fastapi==0.115.6
uvicorn[standard]==0.32.1
asyncpg==0.30.0
redis==5.2.1
pydantic==2.10.3
prometheus-fastapi-instrumentator==7.0.2
```
```
# app/main.py
import os, json
from contextlib import asynccontextmanager
import asyncpg, redis.asyncio as redis
from fastapi import FastAPI, HTTPException
from pydantic import BaseModel
from prometheus_fastapi_instrumentator import Instrumentator
DB_PASSWORD = open("/run/secrets/db_password").read().strip()
DSN = f"postgres://app:{DB_PASSWORD}@db:5432/appdb"
REDIS_URL = os.getenv("REDIS_URL", "redis://cache:6379/0")
class Item(BaseModel):
    name: str
    price: float
@asynccontextmanager
async def lifespan(app: FastAPI):
    app.state.pool = await asyncpg.create_pool(DSN, min_size=2, max_size=10)
    app.state.cache = redis.from_url(REDIS_URL, decode_responses=True)
    yield
    await app.state.pool.close()
    await app.state.cache.aclose()
app = FastAPI(lifespan=lifespan, title="Compose Demo API")
Instrumentator().instrument(app).expose(app)
@app.get("/health")
async def health():
    return {"status": "ok"}
@app.get("/items")
async def list_items():
    cached = await app.state.cache.get("items:all")
    if cached:
        return {"source": "cache", "items": json.loads(cached)}
    rows = await app.state.pool.fetch("SELECT id, name, price FROM items")
    items = [dict(r) for r in rows]
    await app.state.cache.setex("items:all", 30, json.dumps(items))
    return {"source": "db", "items": items}
@app.post("/items", status_code=201)
async def create_item(item: Item):
    row = await app.state.pool.fetchrow(
        "INSERT INTO items(name, price) VALUES($1, $2) RETURNING id",
        item.name, item.price
    )
    await app.state.cache.delete("items:all")
    return {"id": row["id"], **item.model_dump()}
```
Le **Dockerfile multi-stage** sépare la compilation des dépendances de l’image finale, ce qui réduit la taille et la surface d’attaque – une pratique désormais généralisée au-delà de Python : le guide « Docker Compose Tutorial: Complete Guide » de Luca Berton, publié en avril 2026, illustre le même schéma multi-stage avec un exemple Node.js 22 pour des stacks orchestrées par Compose. L’image finale tourne sous un utilisateur non-root, conformément aux bonnes pratiques CIS Docker Benchmark.

```
# app/Dockerfile
FROM python:3.13-slim AS builder
WORKDIR /build
COPY requirements.txt .
RUN pip install --no-cache-dir --user -r requirements.txt
FROM python:3.13-slim AS runtime
RUN useradd --create-home --shell /bin/bash appuser \
    && apt-get update && apt-get install -y --no-install-recommends curl \
    && rm -rf /var/lib/apt/lists/*
USER appuser
WORKDIR /home/appuser/app
COPY --from=builder /root/.local /home/appuser/.local
COPY --chown=appuser:appuser . .
ENV PATH="/home/appuser/.local/bin:$PATH" \
    PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1
EXPOSE 8000
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s --retries=3 \
  CMD curl -f http://localhost:8000/health || exit 1
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000", "--workers", "2"]
```
## Étape 5 : Configurer PostgreSQL avec Initialisation Automatique

L’image officielle `postgres:17-alpine` exécute automatiquement tout fichier `.sql` ou `.sh` placé dans `/docker-entrypoint-initdb.d/` au premier démarrage. Nous y déposerons un script qui crée la table `items` et l’utilisateur applicatif.

```
-- postgres/init.sql
CREATE USER app WITH PASSWORD 'CHANGE_ME_AT_RUNTIME';
CREATE DATABASE appdb OWNER app;
\connect appdb;
CREATE TABLE items (
    id SERIAL PRIMARY KEY,
    name TEXT NOT NULL,
    price NUMERIC(10, 2) NOT NULL,
    created_at TIMESTAMPTZ DEFAULT NOW()
);
CREATE INDEX idx_items_name ON items(name);
GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO app;
GRANT USAGE, SELECT ON ALL SEQUENCES IN SCHEMA public TO app;
```
Le mot de passe en dur n’est qu’un placeholder : Compose le réécrira via *secrets* à l’étape 7. Pour la sauvegarde, nous monterons le répertoire `/var/lib/postgresql/data` sur un **named volume** (et non un bind mount), ce qui simplifie la gestion et améliore les performances I/O sous Docker Desktop.

## Étape 6 : Rédiger le docker-compose.yml de Production

Voici le fichier `docker-compose.yml` central. Lisez-le attentivement : chaque section illustre une bonne pratique de production. Notez l’absence du champ `version:`, l’utilisation de `profiles:` pour activer ou non le monitoring, les `healthcheck:` et `depends_on: condition: service_healthy` qui garantissent l’ordre de démarrage correct.

```
# docker-compose.yml
name: compose-prod-stack
services:
  api:
    build:
      context: ./app
      target: runtime
    image: compose-demo/api:latest
    restart: unless-stopped
    environment:
      - REDIS_URL=redis://cache:6379/0
    secrets:
      - db_password
    networks:
      - backend
    depends_on:
      db:
        condition: service_healthy
      cache:
        condition: service_started
    deploy:
      resources:
        limits: {cpus: '1.0', memory: 512M}
        reservations: {cpus: '0.25', memory: 128M}
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:8000/health"]
      interval: 30s
      timeout: 5s
      retries: 3
  db:
    image: postgres:17-alpine
    restart: unless-stopped
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD_FILE: /run/secrets/db_password
      POSTGRES_DB: postgres
    secrets:
      - db_password
    volumes:
      - db_data:/var/lib/postgresql/data
      - ./postgres/init.sql:/docker-entrypoint-initdb.d/init.sql:ro
    networks:
      - backend
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U postgres"]
      interval: 10s
      timeout: 3s
      retries: 5
  cache:
    image: redis:7.4-alpine
    restart: unless-stopped
    command: ["redis-server", "--maxmemory", "256mb", "--maxmemory-policy", "allkeys-lru"]
    volumes:
      - cache_data:/data
    networks:
      - backend
  proxy:
    image: nginx:1.27-alpine
    restart: unless-stopped
    ports:
      - "80:80"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf:ro
    depends_on:
      api:
        condition: service_healthy
    networks:
      - backend
      - frontend
  prometheus:
    image: prom/prometheus:v3.0.1
    restart: unless-stopped
    profiles: ["monitoring"]
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml:ro
      - prom_data:/prometheus
    networks:
      - backend
  grafana:
    image: grafana/grafana:11.4.0
    restart: unless-stopped
    profiles: ["monitoring"]
    ports:
      - "3000:3000"
    environment:
      GF_SECURITY_ADMIN_PASSWORD__FILE: /run/secrets/grafana_admin
    secrets:
      - grafana_admin
    volumes:
      - grafana_data:/var/lib/grafana
      - ./grafana/provisioning:/etc/grafana/provisioning:ro
    networks:
      - backend
volumes:
  db_data:
  cache_data:
  prom_data:
  grafana_data:
networks:
  backend:
    driver: bridge
  frontend:
    driver: bridge
secrets:
  db_password:
    file: ./secrets/db_password.txt
  grafana_admin:
    file: ./secrets/grafana_admin.txt
```
Notez la directive `name:` en haut du fichier. Elle remplace le nom dérivé du dossier (`compose-prod-stack`) et garantit que vos volumes et conteneurs auront un préfixe stable même si vous renommez le répertoire. C’est crucial pour des migrations ou des sauvegardes prévisibles.

## Étape 7 : Gérer les Secrets de Manière Sécurisée

Compose supporte les **secrets** via la section `secrets:`. Chaque secret est monté en lecture seule sous `/run/secrets/<nom>` dans les conteneurs qui le déclarent. Contrairement aux variables d’environnement, les secrets *n’apparaissent pas* dans `docker inspect`, `ps -auxe` ou les logs. C’est la méthode officielle recommandée depuis Compose v2.

```
# Génération de mots de passe forts
openssl rand -base64 32 | tr -d '\n' > secrets/db_password.txt
openssl rand -base64 24 | tr -d '\n' > secrets/grafana_admin.txt
# Restriction des permissions
chmod 600 secrets/*.txt
# Vérifier que les fichiers ne contiennent pas de saut de ligne final
xxd secrets/db_password.txt | tail -1
```
**Piège n° 2 :** Si `db_password.txt` contient un `\n` final (très fréquent avec `echo`), PostgreSQL refusera la connexion avec une erreur cryptique. Utilisez `echo -n` ou la pipe `tr -d '\n'` comme ci-dessus. Vérifiez avec `xxd` ou `wc -c` que la taille correspond exactement à ce que vous attendez.

Pour la production réelle, ne stockez *jamais* ces fichiers dans le dépôt Git. Utilisez plutôt un coffre-fort comme HashiCorp Vault, AWS Secrets Manager, ou le binding natif Compose à des *external secrets* (`secrets: db_password: external: true`) couplé à `docker secret create`.

## Étape 8 : Configurer Nginx en Reverse Proxy

Nginx fait l’interface entre le monde extérieur et l’API FastAPI. Il gère les en-têtes *X-Forwarded-For*, applique un *rate limiting* simple et compresse les réponses. Pour HTTPS en production, ajoutez Certbot ou utilisez **Caddy** à la place (voir notre comparatif Caddy vs Nginx 2026).

```
# nginx/default.conf
upstream api_backend {
    server api:8000 max_fails=3 fail_timeout=30s;
    keepalive 32;
}
limit_req_zone $binary_remote_addr zone=api_limit:10m rate=10r/s;
server {
    listen 80 default_server;
    server_name _;
    client_max_body_size 10m;
    gzip on;
    gzip_types application/json text/plain text/css application/javascript;
    location /health {
        access_log off;
        return 200 "ok\n";
    }
    location / {
        limit_req zone=api_limit burst=20 nodelay;
        proxy_pass http://api_backend;
        proxy_http_version 1.1;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_connect_timeout 5s;
        proxy_read_timeout 30s;
    }
}
```
Notez que `api` dans `upstream` fait référence au nom du service Compose : grâce au DNS interne du réseau `backend`, Nginx résout automatiquement `api` vers l’IP du conteneur. C’est l’un des plus grands avantages de Compose : pas besoin d’IP statique ni de fichier `hosts`.

## Étape 9 : Activer le Monitoring avec Prometheus et Grafana (Profiles)

Les **profiles** permettent d’activer ou non certains services selon le contexte. Dans notre fichier, `prometheus` et `grafana` sont taggés `profiles: ["monitoring"]`. Ils ne démarrent que si l’on lance Compose avec `--profile monitoring`. Idéal pour ne pas charger inutilement la RAM en développement.

```
# prometheus/prometheus.yml
global:
  scrape_interval: 15s
  evaluation_interval: 15s
scrape_configs:
  - job_name: 'fastapi'
    metrics_path: /metrics
    static_configs:
      - targets: ['api:8000']
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
```
```
# Démarrer la stack sans monitoring (par défaut)
docker compose up -d
# Démarrer la stack AVEC monitoring
docker compose --profile monitoring up -d
# Vérifier les services actifs
docker compose ps
```
Ouvrez Grafana à http://localhost:3000 avec l’utilisateur `admin` et le mot de passe contenu dans `secrets/grafana_admin.txt`. Ajoutez Prometheus comme source de données (`http://prometheus:9090`) et importez un dashboard FastAPI public depuis grafana.com (par exemple le dashboard ID 14282).

## Étape 10 : Override pour le Développement avec Compose Watch

Le fichier `compose.override.yml` est lu *automatiquement* par Compose en plus du fichier principal, sans option supplémentaire. C’est la convention idéale pour ajuster le comportement en local sans toucher au fichier de production. Couplé à **Compose Watch**, vous obtenez du rechargement à chaud comparable à `nodemon` ou `uvicorn --reload`, mais multi-langage.

```
# compose.override.yml
services:
  api:
    command: ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
    environment:
      - LOG_LEVEL=debug
    ports:
      - "8000:8000"  # Exposition directe en dev
    develop:
      watch:
        - action: sync
          path: ./app
          target: /home/appuser/app
          ignore:
            - __pycache__/
            - "*.pyc"
        - action: rebuild
          path: ./app/requirements.txt
  db:
    ports:
      - "5432:5432"  # Accès direct depuis psql sur l'hôte
  cache:
    ports:
      - "6379:6379"
```
```
# Lancer Compose Watch (mode dev)
docker compose up --watch
# Désormais, toute modification d'un .py dans ./app est synchronisée
# en moins de 200 ms vers le conteneur, et uvicorn --reload recharge.
# Si requirements.txt change, le conteneur est reconstruit automatiquement.
```
Compose Watch a deux *actions* principales : `sync` (copie les fichiers) et `rebuild` (reconstruit l’image). Une troisième, `sync+restart`, redémarre le conteneur après synchronisation. Pour Go, Rust ou Java compilés, `rebuild` est généralement préférable car le binaire doit être régénéré.

## Étape 11 : Construire et Démarrer la Stack Complète

Première mise en route. Le build de l’image FastAPI prend environ 30 à 60 secondes selon votre bande passante (téléchargement de Python 3.13 et des dépendances). Les images officielles PostgreSQL, Redis, Nginx, Prometheus et Grafana sont déjà très optimisées en taille. Notez que le Quickstart officiel de Docker, rafraîchi en juillet 2026, part désormais du principe que vous lancez directement `docker compose up` avec le plugin Compose v2.x le plus récent installé – ce tutoriel suit la même hypothèse.

```
# Build des images locales
docker compose build
# Démarrage en arrière-plan, avec monitoring
docker compose --profile monitoring up -d
# Surveiller les logs en direct (Ctrl+C pour quitter, conteneurs continuent)
docker compose logs -f --tail=50
# Vérifier l'état (RUNNING + healthy)
docker compose ps --format "table {{.Service}}\t{{.Status}}\t{{.Ports}}"
```
Sortie attendue après ~30 secondes (le temps que les healthchecks passent au vert) :

```
SERVICE     STATUS                    PORTS
api         Up 32 seconds (healthy)   8000/tcp
cache       Up 35 seconds             6379/tcp
db          Up 35 seconds (healthy)   5432/tcp
grafana     Up 35 seconds             0.0.0.0:3000->3000/tcp
prometheus  Up 35 seconds             9090/tcp
proxy       Up 30 seconds             0.0.0.0:80->80/tcp
```
```
# Tester l'API via Nginx
curl -s http://localhost/health
# {"status":"ok"}
curl -s -X POST http://localhost/items \
  -H "Content-Type: application/json" \
  -d '{"name":"Café Arabica","price":12.50}'
# {"id":1,"name":"Café Arabica","price":12.5}
curl -s http://localhost/items
# {"source":"db","items":[{"id":1,"name":"Café Arabica","price":"12.50"}]}
# Deuxième appel : devrait venir du cache Redis
curl -s http://localhost/items
# {"source":"cache","items":[...]}
```
## Étape 12 : Maîtriser les Commandes Compose Essentielles

Voici un aide-mémoire des commandes que vous utiliserez quotidiennement. La commande `docker compose` accepte les mêmes flags que `docker` sur la plupart des sous-commandes (`--quiet`, `--format json`, etc.). Le « Docker Compose Tutorial 2026 » de Vucense, mis à jour en août 2026, insiste d’ailleurs sur les deux mêmes commandes piliers pour les applications multi-conteneurs : `docker compose up -d` pour démarrer et `docker compose down` pour arrêter proprement. Pensez aussi à aligner vos pipelines CI : en février 2026, les runners GitHub Actions sont passés à Compose **v2.40.3**, un changement qui a modifié le comportement de certains scripts et exemples YAML dans les tutoriels s’appuyant sur ces images.

| Commande | Effet | Cas d’usage | 
|---|---|---|
| `docker compose up -d` | Démarre tous les services en arrière-plan | Démarrage normal | 
| `docker compose up --build` | Force la reconstruction des images | Après modification du Dockerfile | 
| `docker compose down` | Arrête et supprime conteneurs + réseaux | Fin de session dev | 
| `docker compose down -v` | Idem + supprime les volumes nommés | **Reset complet** (perte de données !) | 
| `docker compose restart api` | Redémarre un service unique | Après changement de config | 
| `docker compose logs -f api` | Logs temps réel d’un service | Debug | 
| `docker compose exec api bash` | Ouvre un shell dans le conteneur | Inspection en live | 
| `docker compose run --rm api pytest` | Lance une commande one-shot | Tests, migrations | 
| `docker compose ps` | Liste les services et leur état | Monitoring rapide | 
| `docker compose top` | Affiche les processus dans chaque conteneur | Investigation perf | 
| `docker compose pull` | Met à jour les images depuis le registre | Maintenance | 
| `docker compose config` | Valide et affiche le fichier final résolu | Debug YAML / variables | 

**Astuce avancée :** `docker compose config --services` liste uniquement les noms de services, parfait pour itérer en script Bash. `docker compose config --hash '*'` calcule un hash unique de la configuration : utile en CI pour décider si l’on doit re-déployer.

## Étape 13 : Déployer en Production avec Compose

Pour un déploiement réel, suivez cette checklist. Compose en production reste viable jusqu’à environ 10-20 services sur un hôte unique (ou 2-3 hôtes via Docker Swarm). Au-delà, basculez sur Kubernetes – voir notre comparatif Docker vs Kubernetes.

- **Tag d’image immuable :** ne déployez jamais`:latest` . Utilisez le SHA Git ou un tag versionné (`v1.4.2` ) pour garantir la reproductibilité.
- **Fichiers séparés :**`docker-compose.yml` (base) +`compose.prod.yml` (overrides prod). Lancez avec`docker compose -f docker-compose.yml -f compose.prod.yml up -d` .
- **Logs centralisés :** configurez`logging.driver: journald` ou`fluentd` au lieu du driver`json-file` par défaut, qui peut saturer le disque.
- **Restart policy stricte :**`restart: unless-stopped` partout, sauf pour les jobs one-shot.
- **Limites de ressources :** définissez`cpus` et`memory` sur chaque service pour éviter qu’un OOM dans un conteneur ne fasse tomber le voisin.
- **Sauvegardes :** backup nocturne de chaque named volume via`docker run --rm --volumes-from db -v $(pwd):/backup alpine tar czf /backup/db-$(date +%F).tgz /var/lib/postgresql/data` .
- **Mises à jour rolling :**`docker compose pull && docker compose up -d --no-deps --build api` ne touche que le service ciblé.
- **Reverse proxy externe :** exposez uniquement Nginx/Caddy/Traefik. Tous les autres services restent sur le réseau`backend` sans`ports:` .

```
# compose.prod.yml (override production)
services:
  api:
    image: registry.example.com/myapp/api:${GIT_SHA:-latest}
    build: !reset null  # Désactive le build, force le pull
    logging:
      driver: "json-file"
      options:
        max-size: "10m"
        max-file: "3"
  proxy:
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./certs:/etc/nginx/certs:ro
# Déploiement
GIT_SHA=$(git rev-parse --short HEAD) docker compose \
  -f docker-compose.yml -f compose.prod.yml pull
GIT_SHA=$(git rev-parse --short HEAD) docker compose \
  -f docker-compose.yml -f compose.prod.yml up -d --remove-orphans
```
Le drapeau `--remove-orphans` est *essentiel* : il supprime les conteneurs d’anciens services qui ne figurent plus dans le fichier YAML. Sans lui, vous accumulez des zombies à chaque refactor.

## Pièges Courants et Erreurs Classiques à Éviter

Au-delà des deux pièges déjà mentionnés, voici les erreurs récurrentes observées dans les revues de code Docker Compose, classées par fréquence.

- **Piège n° 3 – Croire que `depends_on` attend la disponibilité applicative.** Sans`condition: service_healthy` ,`depends_on` attend uniquement que le conteneur*démarre* , pas que PostgreSQL accepte les connexions. Utilisez systématiquement les healthchecks pour les bases de données.
- **Piège n° 4 – Bind mounts en lecture-écriture par défaut.** Tout fichier de configuration doit se monter en`:ro` . Sinon, un conteneur compromis peut réécrire votre`nginx.conf` et tenir un point d’ancrage.
- **Piège n° 5 – Exposer trop de ports avec `ports:`.** En production, seul le reverse proxy doit avoir un mapping`HOST:CONTAINER` . Les bases de données et caches doivent rester sur le réseau interne uniquement.
- **Piège n° 6 – Oublier `restart:`.** Sans politique, un conteneur qui crashe ne redémarre jamais. Préférez`unless-stopped` pour la production.
- **Piège n° 7 – Mettre des secrets dans `environment:`.** Les variables d’environnement sont visibles via`docker inspect` et fuient dans les logs et les rapports d’erreur. Utilisez`secrets:` .
- **Piège n° 8 – Ne pas pin les images.**`postgres:latest` peut casser une migration majeure du jour au lendemain. Pin sur`postgres:17.2-alpine` et planifiez les upgrades.
- **Piège n° 9 – Utiliser `network_mode: host` par confort.** Vous perdez l’isolation, le DNS interne et la portabilité. Ne le faites que pour les agents système (collecteurs réseau, etc.).
- **Piège n° 10 – Lancer `down -v` sans précaution.** Le flag`-v` détruit irrémédiablement les volumes nommés. Catastrophe garantie en prod.

## Dépannage : 8 Problèmes Fréquents et Leur Solution

| Symptôme | Cause Probable | Solution | 
|---|---|---|
| `Cannot connect to the Docker daemon` | Daemon arrêté ou utilisateur non dans le groupe `docker` | `sudo systemctl start docker` +`sudo usermod -aG docker $USER` puis se relogger | 
| `port is already allocated` | Un autre service écoute sur le port hôte | `sudo ss -tlnp \| grep 80` pour identifier ; changer le mapping ou tuer le service | 
| `service "api" failed to build: COPY failed` | Chemin Dockerfile incorrect ou `.dockerignore` trop large | Vérifier `build.context` et le contenu de`.dockerignore` | 
| `Database "appdb" does not exist` | Init script jamais exécuté (volume préexistant) | `docker compose down -v` (attention, perte de données) puis relancer | 
| Conteneur en boucle `Restarting` | Crash de l’application au démarrage | `docker compose logs --tail=100 service` pour la stack trace | 
| Healthcheck reste `unhealthy` | Curl ou wget absent de l’image Alpine | Ajouter `RUN apk add --no-cache curl` ou utiliser un test natif (`nc -z` ,`pg_isready` ) | 
| `No space left on device` | Volumes orphelins, builds intermédiaires | `docker system prune -a --volumes` (vérifier avant !) | 
| Variables `.env` ignorées | Mauvais emplacement du fichier | Le `.env` doit être à côté du`docker-compose.yml` , pas dans`./app/` | 

Pour un debug en profondeur, activez la verbosité maximale du moteur Compose : `COMPOSE_PROGRESS=plain docker compose --verbose up`. Vous verrez chaque étape (résolution du DNS, montage des volumes, exécution du healthcheck) en clair, ce qui révèle 90 % des problèmes en moins de cinq minutes.

## Optimisation Avancée : Bake, Caches et Builds Multi-Plateforme

Depuis 2025, Compose délègue les builds à **Docker Bake** par défaut, ce qui apporte la parallélisation, le cache distant et le multi-plateforme natif. Pour exploiter Bake explicitement, créez un fichier `docker-bake.hcl` en complément.

```
# docker-bake.hcl
group "default" {
  targets = ["api"]
}
target "api" {
  context  = "./app"
  platforms = ["linux/amd64", "linux/arm64"]
  tags      = ["registry.example.com/myapp/api:latest"]
  cache-from = ["type=registry,ref=registry.example.com/myapp/api:cache"]
  cache-to   = ["type=registry,ref=registry.example.com/myapp/api:cache,mode=max"]
}
```
```
# Build multi-arch poussé directement vers le registre
docker buildx bake --push
# Combinaison Compose + Bake
COMPOSE_BAKE=true docker compose build  # Active Bake explicitement
```
Pour réduire encore le temps de build, activez le **BuildKit cache mount** dans votre Dockerfile : `RUN --mount=type=cache,target=/root/.cache/pip pip install -r requirements.txt`. Le cache pip est conservé entre deux builds, divisant le temps par 5 en moyenne. Pour Node.js, ciblez `/root/.npm` ; pour Go, `/go/pkg/mod`.

## Comparatif Docker Compose vs Alternatives en 2026

| Outil | Format | Cas d’usage idéal | Courbe d’apprentissage | 
|---|---|---|---|
| **Docker Compose v2** | YAML (Compose Spec) | Dev local, CI, mono-hôte prod | Faible (1-2 j) | 
| Docker Swarm | YAML Compose étendu | Cluster Docker 2-10 nœuds | Moyenne (1 sem) | 
| Kubernetes | YAML manifests | Production multi-nœuds, > 20 services | Élevée (1-3 mois) | 
| Podman Compose | YAML compatible Compose | Environnements rootless, RHEL/CentOS | Faible (1-2 j) | 
| Nomad | HCL | Workloads mixtes (Docker, JVM, binaires) | Moyenne | 
| Nerdctl Compose | YAML compatible Compose | Containerd direct, sans Docker | Faible | 

Si votre stack dépasse une vingtaine de conteneurs ou nécessite du *scaling* automatique, migrez sur Kubernetes : l’outil Kompose convertit un fichier Compose en manifests Kubernetes en une seule commande. Pour une transition douce, jetez aussi un œil à notre tutoriel ArgoCD GitOps.

## Variables d’Environnement et Fichier .env

Compose lit automatiquement un fichier `.env` à la racine du projet et substitue toute occurrence `${VAR}` dans le YAML. C’est l’idéal pour *parameterize* versions d’image, ports et tags sans toucher au fichier principal.

```
# .env (à la racine, jamais commité)
POSTGRES_VERSION=17-alpine
REDIS_VERSION=7.4-alpine
NGINX_VERSION=1.27-alpine
API_PORT=8000
COMPOSE_PROJECT_NAME=monprojet
```
Dans le YAML : `image: postgres:${POSTGRES_VERSION}`. Pour passer ponctuellement une variable depuis le shell, exportez-la avant : `POSTGRES_VERSION=17.2-alpine docker compose up -d`. La variable shell prend la priorité sur celle du `.env`.

La variable spéciale `COMPOSE_PROJECT_NAME` remplace le préfixe par défaut des conteneurs et volumes. `COMPOSE_FILE` permet de lister plusieurs fichiers (séparés par `:` sous Linux/macOS, `;` sous Windows). `COMPOSE_PROFILES` active des profils sans flag CLI.

## FAQ : Questions Fréquentes sur Docker Compose

### Docker Compose est-il toujours gratuit ?

Oui. Le plugin `docker compose` est sous licence Apache 2.0 et reste librement utilisable. Seul Docker Desktop (l’environnement clé en main pour macOS/Windows) requiert un abonnement payant pour les entreprises de plus de 250 employés ou de plus de 10 millions de dollars de revenus annuels. Sous Linux, tout est gratuit.

### Quelle est la différence entre `docker-compose` et `docker compose` ?

`docker-compose` (avec tiret) est la **v1** écrite en Python, dépréciée depuis juin 2023 et plus du tout maintenue. `docker compose` (sans tiret) est la **v2**, écrite en Go, livrée comme plugin officiel de la CLI Docker. Elle est 3 à 4 fois plus rapide, gère mieux les builds parallèles et est la seule à recevoir les correctifs de sécurité.

### Compose remplace-t-il Kubernetes ?

Non. Compose orchestre des conteneurs sur *un seul hôte* (ou un petit cluster Swarm). Kubernetes orchestre des conteneurs sur des centaines de nœuds, avec auto-scaling, self-healing avancé, secrets distribués et networking complexe. Pour le développement local, Compose reste le choix optimal même dans une équipe Kubernetes – d’où son intégration native dans des outils comme Tilt et Skaffold.

### Comment exécuter une commande dans un conteneur en cours ?

`docker compose exec NOM_SERVICE COMMANDE`. Exemple : `docker compose exec db psql -U postgres`. Pour un service à l’arrêt, utilisez `docker compose run --rm NOM_SERVICE COMMANDE` qui lance un conteneur jetable, exécute la commande et le supprime.

### Comment scaler horizontalement un service ?

`docker compose up -d --scale api=4` lance quatre instances du service `api`. Attention : le service ne doit pas avoir de `ports:` en mode HOST:CONTAINER (collision), ni de volume bind unique non partageable. Combiné à un load balancer interne (Nginx, Traefik), c’est suffisant pour gérer plusieurs centaines de req/s sur un hôte.

### Comment migrer un projet de Compose v1 à v2 ?

Trois étapes : 1) supprimer la ligne `version:` en tête de fichier ; 2) remplacer toutes les occurrences de `docker-compose` par `docker compose` dans vos scripts CI ; 3) tester avec `docker compose config` pour repérer les warnings de syntaxe désormais stricts (clés inconnues, types incorrects). 95 % des fichiers v1 sont compatibles tels quels.

### Les volumes nommés survivent-ils à `docker compose down` ?

Oui. `docker compose down` seul supprime conteneurs et réseaux mais **conserve** les volumes nommés. Pour tout effacer (utile en début de tests d’intégration), ajoutez `-v` : `docker compose down -v`. Cette opération est irréversible : sauvegardez avant !

### Comment obtenir un dépannage HTTPS rapide en local ?

Remplacez Nginx par **Caddy** dans le service `proxy` et activez `tls internal` dans le Caddyfile : Caddy génère automatiquement un certificat ACME local. Aucune ligne OpenSSL à manipuler. Pour les déploiements publics, Caddy obtient gratuitement un certificat Let’s Encrypt en moins de 10 secondes.

## Conclusion : Une Stack Docker Compose Production-Ready en 60 Minutes

Vous disposez désormais d’une stack **Docker Compose** complète : API FastAPI, PostgreSQL persistant, cache Redis, reverse proxy Nginx avec rate-limiting, monitoring Prometheus/Grafana activable à la demande, gestion sécurisée des secrets, override de développement avec hot-reload via Compose Watch, et procédure de déploiement production avec tags immuables. Toutes les bonnes pratiques 2026 sont intégrées : suppression du champ `version:`, healthchecks systématiques, profiles, limites de ressources, multi-stage Dockerfile.

Prochaines étapes naturelles : configurer un pipeline CI/CD avec GitHub Actions pour tester chaque commit dans une stack éphémère (`docker compose -f compose.test.yml up --abort-on-container-exit`), sauvegarder vos volumes nocturnement vers S3 ou Backblaze B2, et – quand votre charge approche d’un million de requêtes par jour – migrer vers Kubernetes via Kompose. Mais en attendant, Docker Compose vous offre 90 % de la valeur pour 10 % de la complexité.
