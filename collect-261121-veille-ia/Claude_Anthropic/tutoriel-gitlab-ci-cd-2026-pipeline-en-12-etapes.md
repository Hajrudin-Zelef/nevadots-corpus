---
id: collect-261121-veille-ia/claude-anthropic/tutoriel-gitlab-ci-cd-2026-pipeline-en-12-etapes
title: "Tutoriel GitLab CI/CD 2026 : Pipeline en 12 Étapes"
domain: claude-anthropic
role: reference
task: article
actors: ["Anthropic"]
dates: ["2026-03-19"]
keywords: ["gitlab", "version", "tutoriel", "selon", "pipeline", "minutes", "mois", "vous"]
source: https://tech-insider.org/fr/tutoriel-gitlab-ci-cd-pipeline-2026/
source_anchor: ""
source_lines: [1, 1030]
section: "Claude_Anthropic"
---
GitLab CI/CD est devenu l’outil de référence pour automatiser les tests, le build et le déploiement d’applications, porté par une croissance commerciale solide : au deuxième trimestre de son exercice fiscal 2027 (clos en juillet 2026), GitLab a déclaré un chiffre d’affaires de 286,3 millions de dollars (+21 % sur un an), tiré notamment par les abonnements CI/CD, avec une base clients passée de 10 338 à 11 114 comptes (+8 %) selon le 10-Q déposé auprès de la SEC. Avec la version 18.10, publiée le 19 mars 2026, la plateforme intègre désormais l’auto-remédiation des vulnérabilités et l’IA GitLab Duo pour le débogage de pipelines — une IA que GitLab met gratuitement à disposition de 100 % de ses clients sans abonnement Duo séparé depuis janvier 2026, selon DevOps.com — ainsi que les inputs dynamiques pour jobs manuels (menus déroulants en cascade), apparus dès la version 18.7 en février 2026 ; les correctifs 18.10.1, 18.9.3 et 18.8.7, sortis le 11 mars 2026, ont par ailleurs consolidé la CI/CD sur les éditions Community et Enterprise. Ce tutoriel GitLab CI/CD vous guide pas à pas, de l’installation à un pipeline de production complet avec Docker, tests automatisés, SAST et déploiement multi-environnements.

Que vous migriez depuis Jenkins (utilisé par 28 % des organisations) ou GitHub Actions (33 %), ou que vous découvriez l’intégration continue — pratique que 55 % des développeurs déclarent utiliser régulièrement selon le State of Developer Ecosystem 2025 de JetBrains, relayé par Quash en mars 2026, où GitLab CI ne recueille pour l’instant que 19 % d’adoption —, ce guide couvre chaque étape avec des fichiers `.gitlab-ci.yml` fonctionnels, des exemples de sortie console et des solutions aux erreurs les plus fréquentes. À la fin de ce tutoriel, vous disposerez d’un pipeline GitLab CI/CD de production prêt à l’emploi.

## Prérequis et Versions Nécessaires

Avant de commencer ce tutoriel GitLab CI/CD, assurez-vous de disposer des éléments suivants. Chaque version a été testée et validée pour garantir la compatibilité avec les exemples de ce guide.

| Outil | Version Minimale | Version Recommandée | Rôle | 
|---|---|---|---|
| GitLab (SaaS ou Self-Managed) | 17.0 | 18.10+ | Plateforme CI/CD | 
| Git | 2.39 | 2.47+ | Contrôle de version | 
| Docker Engine | 24.0 | 27.0+ | Conteneurisation des builds | 
| Node.js (projet démo) | 20 LTS | 22 LTS | Runtime applicatif | 
| GitLab Runner | 17.0 | 18.10+ | Exécution des jobs | 
| Compte GitLab | Free | Premium ou Ultimate | Accès aux fonctionnalités avancées | 

Le tier Free de GitLab offre 400 minutes CI/CD par mois sans frais par utilisateur ainsi que 10 Gio de stockage, un forfait confirmé aussi bien par l’aperçu d’eesel.ai en septembre 2025 que par la revue WorkflowAutomation.net d’avril 2026. Pour des projets d’entreprise, le tier Premium — facturé 24 $/utilisateur/mois selon TechRepublic en novembre 2025, puis relevé à 29 $/utilisateur/mois d’après la page tarifs officielle de GitLab mise à jour en août 2026 et confirmé par la revue Q4 2025 d’eesel.ai — inclut 10 000 minutes CI/CD mensuelles ainsi que les merge request approvals et les environnements protégés, tandis qu’Ultimate (99 $/utilisateur/mois, un tarif encore décrit par TechRepublic en novembre 2025 puis par Octopus Deploy en septembre 2026) débloque 50 000 minutes CI/CD, le DAST, le dependency scanning et les politiques de conformité. Au-delà de ce quota, GitLab facture désormais 10 $ par tranche de 1 000 minutes supplémentaires depuis juin 2026 selon le blueprint UsagePricing : à titre d’exemple, une équipe de 25 utilisateurs sur Premium paie 725 $/mois de licences plus 400 $ pour 40 000 minutes additionnelles. Pour suivre ce tutoriel dans son intégralité, le tier Free suffit pour les 10 premières étapes.

Vérifiez vos installations avec ces commandes :

```
git --version
# git version 2.47.1
docker --version
# Docker version 27.5.1, build 9f9e405
node --version
# v22.14.0
gitlab-runner --version
# Version: 18.10.0
```
## Étape 1 : Créer le Projet et Initialiser le Dépôt

La première étape de tout pipeline GitLab CI/CD consiste à créer un projet sur GitLab et à y pousser votre code. GitLab organise les projets en groupes (équivalent des organisations GitHub), ce qui facilite la gestion des permissions et des variables partagées.

Connectez-vous à `gitlab.com` (ou votre instance self-managed), cliquez sur **New project**, puis **Create blank project**. Nommez-le `demo-cicd-2026`, choisissez la visibilité Private, et cochez **Initialize repository with a README**.

Clonez ensuite le dépôt et créez la structure du projet Node.js :

```
git clone [email protected]:votre-groupe/demo-cicd-2026.git
cd demo-cicd-2026
# Initialiser le projet Node.js
npm init -y
npm install express --save
npm install jest supertest --save-dev
# Créer la structure
mkdir -p src tests
```
Créez le fichier `src/app.js` avec une API Express minimaliste :

```
// src/app.js
const express = require('express');
const app = express();
app.use(express.json());
app.get('/health', (req, res) => {
  res.json({ status: 'ok', version: process.env.APP_VERSION || '1.0.0' });
});
app.get('/api/users', (req, res) => {
  res.json([
    { id: 1, name: 'Alice Martin' },
    { id: 2, name: 'Bob Dupont' }
  ]);
});
module.exports = app;
// Démarrage si exécuté directement
if (require.main === module) {
  const PORT = process.env.PORT || 3000;
  app.listen(PORT, () => console.log(`Serveur démarré sur le port ${PORT}`));
}
```
Ajoutez un fichier de test `tests/app.test.js` :

```
// tests/app.test.js
const request = require('supertest');
const app = require('../src/app');
describe('API Health Check', () => {
  test('GET /health retourne status ok', async () => {
    const res = await request(app).get('/health');
    expect(res.statusCode).toBe(200);
    expect(res.body.status).toBe('ok');
  });
  test('GET /api/users retourne la liste', async () => {
    const res = await request(app).get('/api/users');
    expect(res.statusCode).toBe(200);
    expect(res.body).toHaveLength(2);
  });
});
```
Mettez à jour le `package.json` pour ajouter les scripts de test :

```
{
  "scripts": {
    "start": "node src/app.js",
    "test": "jest --coverage --forceExit",
    "lint": "eslint src/ tests/"
  }
}
```
Poussez le tout vers GitLab avec `git add . && git commit -m "feat: initial project setup" && git push origin main`. Le projet est maintenant prêt à recevoir son premier pipeline CI/CD.

## Étape 2 : Comprendre l’Architecture de GitLab CI/CD

Avant de rédiger le fichier `.gitlab-ci.yml`, il est essentiel de comprendre comment GitLab CI/CD orchestre l’exécution. L’architecture repose sur trois composants principaux : le **serveur GitLab** qui parse le fichier YAML et planifie les jobs, les **runners** qui exécutent physiquement les commandes, et le **registre d’artefacts** qui stocke les résultats entre les stages.

Un pipeline GitLab CI/CD se décompose en **stages** (étapes séquentielles) contenant des **jobs** (tâches parallèles). Par défaut, tous les jobs d’un même stage s’exécutent en parallèle, puis le stage suivant démarre uniquement si tous les jobs précédents ont réussi. Ce modèle permet d’accélérer les pipelines tout en maintenant l’intégrité du processus.

| Concept | Description | Exemple | 
|---|---|---|
| Pipeline | Ensemble ordonné de stages déclenché par un événement git | Push sur main, merge request | 
| Stage | Phase séquentielle contenant un ou plusieurs jobs | build, test, deploy | 
| Job | Unité d’exécution avec un script et une image Docker | unit-test, lint, sast-scan | 
| Runner | Agent qui exécute les jobs (partagé SaaS ou self-hosted) | Docker executor, Kubernetes executor | 
| Artefact | Fichier produit par un job, transmissible au stage suivant | rapport de couverture, binaire compilé | 
| Cache | Fichiers persistés entre pipelines pour accélérer les builds | node_modules/, .m2/repository | 
| Variable CI/CD | Valeur injectable dans les jobs (secrète ou publique) | $DOCKER_PASSWORD, $APP_ENV | 

Les runners partagés de GitLab SaaS utilisent des machines virtuelles éphémères sous Linux (architecture x86_64) avec Docker executor. Chaque job démarre dans un conteneur propre, ce qui garantit l’isolation. Pour les projets nécessitant plus de contrôle, vous pouvez enregistrer vos propres runners sur des serveurs dédiés ou dans un cluster Kubernetes.

Depuis que les **CI/CD Components** — accompagnés de leurs inputs typés — et leur catalogue sont passés en disponibilité générale avec GitLab 17.0 (sorti en mai 2025) — un statut toujours confirmé par les notes de version officielles GitLab actualisées en septembre 2026 —, la plateforme supporte pleinement ces blocs de configuration réutilisables que vous pouvez importer depuis le catalogue GitLab. Cela remplace les anciens templates `include` et offre un versionnement sémantique des configurations partagées, dont le processus de publication versionnée est détaillé dans la documentation mise à jour en août 2026.

## Étape 3 : Écrire Votre Premier Fichier .gitlab-ci.yml

Le fichier `.gitlab-ci.yml` placé à la racine du dépôt est le cœur de votre pipeline GitLab CI/CD. Voici un premier pipeline fonctionnel qui couvre le lint, les tests unitaires et le build Docker :

```
# .gitlab-ci.yml
stages:
  - validate
  - test
  - build
  - deploy
variables:
  NODE_ENV: "test"
  DOCKER_IMAGE: "$CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA"
# Cache global pour node_modules
cache:
  key: ${CI_COMMIT_REF_SLUG}
  paths:
    - node_modules/
# Stage: validate
lint:
  stage: validate
  image: node:22-alpine
  before_script:
    - npm ci
  script:
    - npx eslint src/ tests/ --max-warnings 0
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    - if: $CI_COMMIT_BRANCH == "main"
# Stage: test
unit-tests:
  stage: test
  image: node:22-alpine
  before_script:
    - npm ci
  script:
    - npm test -- --ci --reporters=default --reporters=jest-junit
  coverage: '/All files[^|]*\|[^|]*\s+([\d\.]+)/'
  artifacts:
    when: always
    reports:
      junit: junit.xml
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml
    paths:
      - coverage/
    expire_in: 30 days
# Stage: build
docker-build:
  stage: build
  image: docker:27
  services:
    - docker:27-dind
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - docker build -t $DOCKER_IMAGE .
    - docker push $DOCKER_IMAGE
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
# Stage: deploy (staging)
deploy-staging:
  stage: deploy
  image: alpine:3.20
  before_script:
    - apk add --no-cache openssh-client
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | ssh-add -
  script:
    - ssh -o StrictHostKeyChecking=no $DEPLOY_USER@$STAGING_HOST
      "docker pull $DOCKER_IMAGE && docker compose up -d"
  environment:
    name: staging
    url: https://staging.example.com
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
```
Ce pipeline définit quatre stages exécutés séquentiellement : `validate` vérifie le style du code, `test` lance les tests unitaires avec couverture, `build` crée et pousse l’image Docker vers le registre GitLab intégré, et `deploy` met à jour l’environnement de staging via SSH.

Les directives `rules` contrôlent quand chaque job s’exécute. Ici, le lint tourne sur les merge requests et les pushs sur main, tandis que le build et le déploiement ne se déclenchent que sur main. Cette approche évite de gaspiller des minutes CI/CD sur des branches de feature pour les étapes coûteuses.

Poussez ce fichier et observez le pipeline se déclencher automatiquement dans l’onglet **CI/CD > Pipelines** de votre projet GitLab.

## Étape 4 : Configurer les Variables CI/CD et les Secrets

Les variables CI/CD permettent d’injecter des valeurs sensibles (clés API, mots de passe, tokens) dans vos pipelines sans les stocker dans le code source. GitLab propose trois niveaux de portée : instance (admin), groupe et projet — et depuis mars 2025, les nouvelles variables d’environnement CI du monolithe sont préfixées `GLCI_` afin de les distinguer clairement des variables historiques. Depuis la version 18.0 (mai 2025), GitLab a également introduit des permissions de job token à granularité fine, renforçant le contrôle des accès inter-projets déclenchés par les pipelines, et la version 17.11 (février 2026) a ajouté les inputs CI/CD structurés pour sécuriser davantage le déclenchement des pipelines — une évolution que Releasebot a détaillée dans sa mise à jour du 5 mai 2026 (version 2026.05), soulignant l’apport de paramètres typés et de règles de validation pour ces inputs.

Pour ajouter une variable, naviguez vers **Settings > CI/CD > Variables** dans votre projet. Cliquez sur **Add variable** et configurez :

**Key** : le nom de la variable (ex. `SSH_PRIVATE_KEY`). **Value** : la valeur secrète. **Type** : Variable (texte) ou File (fichier temporaire). **Flags** : cochez *Protected* pour limiter l’accès aux branches protégées, et *Masked* pour masquer la valeur dans les logs.

Depuis GitLab 18.9, vous pouvez également utiliser les **job inputs** pour les jobs manuels. Cette fonctionnalité permet de passer des paramètres dynamiques à un job sans relancer tout le pipeline. Par exemple, un job de déploiement peut accepter un numéro de version en entrée :

```
# Exemple de job avec inputs (GitLab 18.9+)
deploy-production:
  stage: deploy
  image: alpine:3.20
  script:
    - echo "Déploiement version $DEPLOY_VERSION sur production"
    - ./deploy.sh --version $DEPLOY_VERSION --env production
  environment:
    name: production
    url: https://app.example.com
  rules:
    - if: $CI_COMMIT_TAG
      when: manual
  allow_failure: false
```
GitLab fournit aussi des **variables prédéfinies** accessibles dans tous les jobs. Les plus utiles sont `$CI_COMMIT_SHA` (hash complet du commit), `$CI_COMMIT_SHORT_SHA` (8 premiers caractères), `$CI_PIPELINE_ID`, `$CI_REGISTRY` (URL du registre Docker intégré), `$CI_PROJECT_DIR` (chemin du code cloné) et `$CI_MERGE_REQUEST_IID` (numéro de la merge request).

Pour les déploiements multi-environnements, utilisez les **variables d’environnement scopées**. Créez une variable `DATABASE_URL` avec des valeurs différentes pour les environnements staging et production. GitLab injectera automatiquement la bonne valeur selon l’environnement cible du job.

## Étape 5 : Installer et Configurer un GitLab Runner Self-Hosted

Les runners partagés de GitLab SaaS conviennent pour démarrer, mais les projets professionnels bénéficient d’un runner self-hosted pour des raisons de performance, de sécurité et de coût — une alternative gérée existe désormais pour les clients GitLab Dedicated, dont les runners CI/CD hébergés sont disponibles en accès limité depuis la version 17.8, sortie début 2025. Pour un runner self-hosted classique, l’installation prend moins de 10 minutes sur une machine Linux.

Installez le GitLab Runner sur votre serveur (Ubuntu/Debian) :

```
# Ajouter le dépôt officiel GitLab
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh" | sudo bash
# Installer le runner
sudo apt-get install gitlab-runner
# Vérifier l'installation
gitlab-runner --version
# Version: 18.10.0
# Git revision: abc1234d
# Git branch: 18-10-stable
# Enregistrer le runner auprès de votre instance GitLab
sudo gitlab-runner register \
  --url https://gitlab.com/ \
  --token glrt-VOTRE_TOKEN_ICI \
  --executor docker \
  --docker-image alpine:3.20 \
  --description "runner-docker-prod" \
  --tag-list "docker,linux,production"
# Démarrer le service
sudo gitlab-runner start
# Vérifier le statut
sudo gitlab-runner status
# gitlab-runner: Service is running
```
Le token d’enregistrement se trouve dans **Settings > CI/CD > Runners** de votre projet ou groupe. Depuis GitLab 17+, les tokens de type `glrt-` remplacent les anciens registration tokens pour une meilleure sécurité.

Pour optimiser les performances, configurez le fichier `/etc/gitlab-runner/config.toml` :

```
# /etc/gitlab-runner/config.toml
concurrent = 4
check_interval = 3
[[runners]]
  name = "runner-docker-prod"
  url = "https://gitlab.com/"
  token = "glrt-VOTRE_TOKEN"
  executor = "docker"
  [runners.docker]
    image = "alpine:3.20"
    privileged = false
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    volumes = ["/cache", "/var/run/docker.sock:/var/run/docker.sock"]
    shm_size = 0
    pull_policy = ["if-not-present"]
  [runners.cache]
    Type = "s3"
    Shared = true
    [runners.cache.s3]
      BucketName = "gitlab-runner-cache"
      BucketLocation = "eu-west-3"
```
Le paramètre `concurrent = 4` permet d’exécuter jusqu’à 4 jobs simultanément. La directive `pull_policy = ["if-not-present"]` évite de re-télécharger les images Docker à chaque job, ce qui réduit la durée d’exécution de 30 à 50 % selon la taille des images.

## Étape 6 : Optimiser le Cache et les Artefacts

La gestion du cache est l’un des leviers les plus efficaces pour accélérer vos pipelines GitLab CI/CD. Sans cache, chaque job télécharge et installe les dépendances depuis zéro, ce qui peut ajouter 2 à 5 minutes par pipeline pour un projet Node.js typique.

GitLab distingue deux mécanismes : le **cache** (persisté entre pipelines, pour les dépendances) et les **artefacts** (transmis entre stages du même pipeline, pour les résultats de build). Confondre les deux est une erreur fréquente.

| Caractéristique | Cache | Artefacts | 
|---|---|---|
| Persistance | Entre pipelines (best-effort) | Dans le même pipeline (garanti) | 
| Cas d’usage | node_modules/, pip cache, .m2/ | Binaires compilés, rapports de test | 
| Disponibilité | Même branche par défaut | Tous les jobs des stages suivants | 
| Politique | pull, push, pull-push | always, on_success, on_failure | 
| Stockage | Runner local ou S3 | Serveur GitLab | 
| Expiration | Configurable | expire_in (défaut : 30 jours) | 

Voici une configuration optimisée du cache pour un projet Node.js :

```
# Configuration cache optimisée
cache:
  key:
    files:
      - package-lock.json
  paths:
    - node_modules/
  policy: pull-push
# Job qui ne fait que lire le cache (plus rapide)
lint:
  stage: validate
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
    policy: pull  # Ne met pas à jour le cache
  script:
    - npx eslint src/
# Artefacts pour transmettre le build au stage deploy
build:
  stage: build
  script:
    - npm run build
  artifacts:
    paths:
      - dist/
    expire_in: 1 week
```
L’utilisation de `package-lock.json` comme clé de cache garantit que le cache est invalidé uniquement lorsque les dépendances changent. Les jobs en lecture seule utilisent `policy: pull` pour éviter les écritures inutiles. Pour les projets Python, remplacez par `requirements.txt` et le chemin `.pip-cache/`.

Un piège courant est de mettre `node_modules/` dans les artefacts au lieu du cache. Cela transfère des centaines de Mo via le serveur GitLab à chaque pipeline, ralentissant tout. Les dépendances vont dans le cache, les résultats de build dans les artefacts.

## Étape 7 : Intégrer Docker et le Container Registry GitLab

GitLab inclut un registre Docker intégré pour chaque projet, accessible via `$CI_REGISTRY_IMAGE`. Cette intégration élimine le besoin d’un Docker Hub ou d’un registre externe pour la plupart des cas d’usage.

Commencez par créer un `Dockerfile` optimisé pour la production :

```
# Dockerfile multi-stage optimisé
FROM node:22-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY src/ ./src/
FROM node:22-alpine AS production
WORKDIR /app
RUN addgroup -g 1001 appgroup && \
    adduser -u 1001 -G appgroup -s /bin/sh -D appuser
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/src ./src
COPY package.json ./
USER appuser
EXPOSE 3000
HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget -qO- http://localhost:3000/health || exit 1
CMD ["node", "src/app.js"]
```
Le build multi-stage réduit la taille de l’image finale en excluant les dépendances de développement. L’utilisateur non-root `appuser` renforce la sécurité du conteneur. Le `HEALTHCHECK` permet à Docker Compose et aux orchestrateurs de vérifier l’état de l’application.

Pour builder et pusher l’image dans le pipeline GitLab CI/CD, deux approches existent : Docker-in-Docker (DinD) et le montage du socket Docker. DinD est plus sécurisé mais légèrement plus lent :

```
# Approche Docker-in-Docker (recommandée pour SaaS)
docker-build:
  stage: build
  image: docker:27
  services:
    - docker:27-dind
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - docker build
      --cache-from $CI_REGISTRY_IMAGE:latest
      --tag $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA
      --tag $CI_REGISTRY_IMAGE:latest
      .
    - docker push $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA
    - docker push $CI_REGISTRY_IMAGE:latest
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
# Sortie attendue :
# Step 1/12 : FROM node:22-alpine AS builder
# ---> Using cache
# Successfully built a1b2c3d4e5f6
# Successfully tagged registry.gitlab.com/groupe/demo-cicd-2026:abc12345
# The push refers to repository [registry.gitlab.com/groupe/demo-cicd-2026]
# abc12345: digest: sha256:... size: 1234
```
La directive `--cache-from` réutilise les couches de l’image précédente, ce qui peut réduire le temps de build de 60 à 80 % lorsque seul le code applicatif change. Le double tag (SHA + latest) permet de déployer une version précise tout en gardant une référence à la dernière version stable.

## Étape 8 : Mettre en Place les Tests Automatisés et la Couverture

Un pipeline GitLab CI/CD robuste inclut plusieurs niveaux de tests. GitLab offre une intégration native avec les rapports JUnit, la couverture de code et les merge request widgets qui affichent les résultats directement dans l’interface.

Configurez des jobs séparés pour chaque type de test :

```
# Tests unitaires avec couverture
unit-tests:
  stage: test
  image: node:22-alpine
  before_script:
    - npm ci
  script:
    - npm test -- --ci --coverage --reporters=default --reporters=jest-junit
  coverage: '/All files[^|]*\|[^|]*\s+([\d\.]+)/'
  artifacts:
    when: always
    reports:
      junit: junit.xml
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml
    paths:
      - coverage/
    expire_in: 7 days
# Tests d'intégration
integration-tests:
  stage: test
  image: node:22-alpine
  services:
    - name: postgres:16-alpine
      alias: db
  variables:
    POSTGRES_DB: test_db
    POSTGRES_USER: test_user
    POSTGRES_PASSWORD: test_pass
    DATABASE_URL: "postgresql://test_user:test_pass@db:5432/test_db"
  before_script:
    - npm ci
  script:
    - npm run test:integration
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
# Sortie attendue des tests :
# PASS tests/app.test.js
#   API Health Check
#     ✓ GET /health retourne status ok (23 ms)
#     ✓ GET /api/users retourne la liste (8 ms)
#
# ----------|---------|----------|---------|---------|
# File      | % Stmts | % Branch | % Funcs | % Lines |
# ----------|---------|----------|---------|---------|
# All files |   94.12 |      100 |     100 |   93.33 |
#  app.js   |   94.12 |      100 |     100 |   93.33 |
# ----------|---------|----------|---------|---------|
```
L’expression régulière dans `coverage` permet à GitLab d’extraire le pourcentage de couverture depuis la sortie Jest. Ce chiffre apparaît ensuite dans les badges du projet et dans les merge requests. Le rapport Cobertura active la visualisation de la couverture ligne par ligne directement dans le diff de la merge request.

Les **services** dans le job d’intégration lancent un conteneur PostgreSQL éphémère accessible via l’alias `db`. Ce mécanisme est identique pour Redis, MongoDB ou tout autre service nécessaire aux tests. GitLab gère le cycle de vie des conteneurs de service automatiquement.

Pour les projets exigeant un seuil de couverture minimal, ajoutez une vérification dans le script :

```
unit-tests:
  script:
    - npm test -- --ci --coverage
    - |
      COVERAGE=$(cat coverage/coverage-summary.json | 
        python3 -c "import sys,json; print(json.load(sys.stdin)['total']['lines']['pct'])")
      echo "Couverture: ${COVERAGE}%"
      if [ $(echo "$COVERAGE < 80" | bc -l) -eq 1 ]; then
        echo "ERREUR: Couverture insuffisante (${COVERAGE}% < 80%)"
        exit 1
      fi
```
## Étape 9 : Ajouter le Scanning de Sécurité (SAST et Dependency Scanning)

L'un des avantages majeurs de GitLab CI/CD par rapport à GitHub Actions est l'intégration native des outils de sécurité. Le SAST (Static Application Security Testing) analyse le code source, tandis que le Dependency Scanning vérifie les vulnérabilités connues dans les bibliothèques tierces.

Pour activer le scanning de sécurité, ajoutez les templates officiels GitLab :

```
# Ajouter en haut du .gitlab-ci.yml
include:
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  - template: Security/Secret-Detection.gitlab-ci.yml
# Les jobs suivants sont automatiquement ajoutés :
# - semgrep-sast (analyse statique du code)
# - nodejs-scan-sast (scan spécifique Node.js)
# - gemnasium-dependency_scanning (vulnérabilités NPM)
# - secret_detection (clés API, tokens dans le code)
# Personnalisation optionnelle
variables:
  SAST_EXCLUDED_PATHS: "tests/, node_modules/, coverage/"
  DS_EXCLUDED_ANALYZERS: "bundler-audit"
  SECRET_DETECTION_EXCLUDED_PATHS: "tests/fixtures/"
```
Ces templates ajoutent automatiquement des jobs au stage `test` de votre pipeline. Les résultats apparaissent dans l'onglet **Security** du projet et directement dans les merge requests sous forme de widget. Depuis GitLab 18.9, l'auto-remédiation propose automatiquement des merge requests pour corriger les vulnérabilités détectées, avec des niveaux de sévérité configurables de LOW à CRITICAL.

Le Secret Detection est particulièrement critique : il empêche les commits contenant des clés API AWS, des tokens GitLab, des mots de passe en clair et d'autres secrets. Ce scan s'exécute avant le merge, bloquant les merge requests si un secret est détecté.

Pour les équipes sur le tier Ultimate, le DAST (Dynamic Application Security Testing) teste l'application déployée en environnement de staging, simulant des attaques XSS, injection SQL et CSRF sur les endpoints exposés. C'est un complément essentiel au SAST pour une couverture de sécurité complète.

## Étape 10 : Configurer le Déploiement Multi-Environnements

Un pipeline GitLab CI/CD mature déploie sur plusieurs environnements : staging pour la validation, production pour les utilisateurs finaux, et potionnellement des environnements éphémères par merge request (review apps).

Voici la configuration complète pour un déploiement multi-environnements :

```
# Déploiement staging automatique
deploy-staging:
  stage: deploy
  image: alpine:3.20
  before_script:
    - apk add --no-cache openssh-client curl
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | ssh-add -
  script:
    - ssh -o StrictHostKeyChecking=no deploy@$STAGING_HOST
      "docker pull $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA &&
       docker compose -f docker-compose.staging.yml up -d &&
       sleep 5 &&
       curl -f http://localhost:3000/health"
  environment:
    name: staging
    url: https://staging.example.com
    on_stop: stop-staging
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
# Arrêt de l'environnement staging
stop-staging:
  stage: deploy
  image: alpine:3.20
  script:
    - ssh deploy@$STAGING_HOST "docker compose -f docker-compose.staging.yml down"
  environment:
    name: staging
    action: stop
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
      when: manual
# Déploiement production manuel avec approbation
deploy-production:
  stage: deploy
  image: alpine:3.20
  before_script:
    - apk add --no-cache openssh-client curl
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY_PROD" | ssh-add -
  script:
    - echo "Déploiement de $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA en production"
    - ssh -o StrictHostKeyChecking=no deploy@$PRODUCTION_HOST
      "docker pull $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA &&
       docker compose -f docker-compose.prod.yml up -d --no-deps app &&
       sleep 10 &&
       curl -f http://localhost:3000/health || 
       (docker compose -f docker-compose.prod.yml rollback && exit 1)"
  environment:
    name: production
    url: https://app.example.com
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
      when: manual
  allow_failure: false
# Review Apps (environnement éphémère par MR)
deploy-review:
  stage: deploy
  image: docker:27
  services:
    - docker:27-dind
  script:
    - docker pull $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA
    - docker run -d --name review-$CI_MERGE_REQUEST_IID
      -p $((3000 + $CI_MERGE_REQUEST_IID)):3000
      $CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA
  environment:
    name: review/$CI_MERGE_REQUEST_SOURCE_BRANCH_NAME
    url: http://$CI_SERVER_HOST:$((3000 + $CI_MERGE_REQUEST_IID))
    on_stop: stop-review
    auto_stop_in: 1 week
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
```
Les review apps créent un environnement éphémère pour chaque merge request. L'équipe peut tester les changements sur une URL dédiée avant le merge. La directive `auto_stop_in: 1 week` garantit que les environnements orphelins sont automatiquement nettoyés.

Le déploiement production utilise `when: manual` pour exiger une validation humaine. Combiné avec les merge request approvals du tier Premium, cela crée un processus de release contrôlé et auditable. Le script inclut un rollback automatique si le health check échoue après déploiement.

## Étape 11 : Pipelines Avancés avec Rules, Needs et DAG

Les pipelines GitLab CI/CD avancés utilisent les DAG (Directed Acyclic Graphs) via le mot-clé `needs` pour paralléliser les stages et réduire drastiquement la durée d'exécution.

Sans `needs`, un pipeline à 4 stages s'exécute strictement en séquence : validate (1 min) → test (3 min) → build (2 min) → deploy (1 min) = 7 minutes. Avec `needs`, les jobs peuvent démarrer dès que leurs dépendances directes sont terminées, réduisant le chemin critique.

```
# Pipeline DAG optimisé
stages:
  - validate
  - test
  - build
  - deploy
lint:
  stage: validate
  script: npx eslint src/
security-scan:
  stage: validate
  script: echo "SAST scan"
unit-tests:
  stage: test
  needs: ["lint"]    # Démarre dès que lint est fini
  script: npm test
integration-tests:
  stage: test
  needs: ["lint"]    # Parallèle avec unit-tests
  script: npm run test:integration
docker-build:
  stage: build
  needs: ["unit-tests"]  # N'attend pas integration-tests
  script: docker build -t $DOCKER_IMAGE .
deploy-staging:
  stage: deploy
  needs: ["docker-build", "integration-tests", "security-scan"]
  script: ./deploy.sh staging
# Résultat : le pipeline passe de 7 min à ~4 min
# lint (1 min) → unit-tests + integration-tests (3 min, parallèle)
#                → docker-build (2 min, commence dès unit-tests fini)
#                → deploy (1 min, attend tous les prérequis)
```
Les **rules** avancées permettent de contrôler finement quand les jobs s'exécutent. Voici les patterns les plus utiles :

```
# Exécuter uniquement si des fichiers spécifiques changent
test-frontend:
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
      changes:
        - "src/frontend/**/*"
        - "package.json"
# Skip CI avec le message de commit
all-jobs:
  rules:
    - if: $CI_COMMIT_MESSAGE =~ /\[skip ci\]/
      when: never
    - when: on_success
# Pipelines programmés (cron)
nightly-tests:
  rules:
    - if: $CI_PIPELINE_SOURCE == "schedule"
  script: npm run test:e2e
# Déclenchement conditionnel par variable
deploy:
  rules:
    - if: $CI_COMMIT_TAG =~ /^v\d+\.\d+\.\d+$/
      variables:
        DEPLOY_ENV: "production"
    - if: $CI_COMMIT_BRANCH == "main"
      variables:
        DEPLOY_ENV: "staging"
```
Le déclenchement par `changes` est particulièrement utile pour les monorepos : les tests frontend ne s'exécutent que si le code frontend a changé, évitant des minutes CI/CD inutiles. Les pipelines programmés permettent de lancer des suites de tests e2e complètes la nuit sans bloquer les développeurs pendant la journée.

## Étape 12 : Utiliser les CI/CD Components et les Templates Réutilisables

GitLab 18.x a introduit les **CI/CD Components**, un système de configuration modulaire qui remplace les anciens fichiers `include`. Les components sont versionnés, testés et publiés dans un catalogue centralisé, et depuis GitLab 18.9 (mai 2026), une vue d'analytics du catalogue affiche le nombre d'utilisations par projet sur une fenêtre glissante de 30 jours — le détail par composant individuel restant toutefois réservé au tier Ultimate. Depuis août 2026, cette Component Analytics repère en plus les versions de components devenues obsolètes à travers l'ensemble des projets, facilitant les campagnes de mise à jour groupées.

Pour utiliser un component depuis le catalogue GitLab :

```
# Utiliser des components du catalogue
include:
  # Component officiel de déploiement
  - component: gitlab.com/components/[email protected]
    inputs:
      environment: staging
      strategy: rolling
  # Component de notification Slack
  - component: gitlab.com/components/[email protected]
    inputs:
      channel: "#deployments"
      webhook_url: $SLACK_WEBHOOK
  # Template local réutilisable
  - local: .gitlab/ci/templates/node-defaults.yml
# Créer votre propre template réutilisable
# .gitlab/ci/templates/node-defaults.yml
.node-defaults:
  image: node:22-alpine
  before_script:
    - npm ci
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
# Héritage du template
lint:
  extends: .node-defaults
  stage: validate
  script:
    - npx eslint src/
tests:
  extends: .node-defaults
  stage: test
  script:
    - npm test
```
Le mot-clé `extends` permet l'héritage de configuration, éliminant la duplication. Un template `.node-defaults` centralise l'image, le `before_script` et le cache. Chaque job qui en hérite n'a plus qu'à définir son stage et son script spécifique.

Pour les organisations avec plusieurs projets, créez un dépôt de templates partagés au niveau du groupe GitLab. Les projets enfants incluent ces templates via une URL relative, garantissant la cohérence des pipelines à travers l'organisation.

## 5 Pièges Courants et Comment les Éviter

Après des centaines de pipelines GitLab CI/CD — et alors que l'usage des pipelines CI/CD a bondi de 40 % en glissement annuel à fin septembre 2026 selon les points saillants de la conférence de résultats relayés par MarketBeat, tandis que le nombre de clients GitLab générant plus de 100 000 $ d'ARR est passé de 1 344 à 1 571 (+17 %) d'après le dernier 10-Q déposé auprès de la SEC —, certains pièges reviennent systématiquement. Voici les erreurs les plus fréquentes et leurs solutions.

**Piège 1 : Le cache qui ne fonctionne pas entre les branches.** Par défaut, la clé de cache utilise `$CI_COMMIT_REF_SLUG`, ce qui crée un cache par branche. Les branches de feature partent donc toujours d'un cache vide. Solution : utilisez `key: files: [package-lock.json]` pour partager le cache entre toutes les branches ayant les mêmes dépendances.

**Piège 2 : Docker-in-Docker qui échoue avec "Cannot connect to the Docker daemon".** Ce problème survient quand `DOCKER_TLS_CERTDIR` n'est pas configuré ou quand le service DinD n'a pas eu le temps de démarrer. Solution : ajoutez `DOCKER_TLS_CERTDIR: "/certs"` dans les variables et utilisez la même version majeure pour l'image et le service (ex. `docker:27` et `docker:27-dind`).

**Piège 3 : Les jobs qui tournent alors qu'ils ne devraient pas.** La syntaxe `only/except` est dépréciée et produit des comportements inattendus combinée avec `rules`. Solution : utilisez exclusivement `rules` avec des conditions explicites. Ne mélangez jamais `only/except` et `rules` dans le même pipeline.

**Piège 4 : Les artefacts qui font exploser le stockage GitLab.** Sans `expire_in`, les artefacts sont conservés indéfiniment. Un projet actif avec des rapports de couverture de 50 Mo peut consommer des Go de stockage en quelques semaines. Solution : définissez toujours `expire_in` (7 jours pour les rapports de test, 30 jours pour les builds).

**Piège 5 : Les variables masquées qui apparaissent quand même dans les logs.** Le flag `Masked` ne fonctionne que si la valeur fait au moins 8 caractères et ne contient pas de caractères spéciaux. Solution : encodez les secrets complexes en base64 avant de les stocker, et décodez-les dans le script du job.

## Dépannage : 8 Erreurs Fréquentes et Leurs Solutions

Voici les erreurs les plus courantes rencontrées dans les pipelines GitLab CI/CD, avec les commandes de diagnostic et les solutions testées.

| Erreur | Cause | Solution | 
|---|---|---|
| `This job is stuck because you don't have any active runners` | Aucun runner disponible avec les tags demandés | Vérifiez les tags dans le job et enregistrez un runner avec les tags correspondants | 
| `yaml invalid` | Erreur de syntaxe dans .gitlab-ci.yml | Utilisez le linter intégré : CI/CD > Editor > Validate pipeline | 
| `ERROR: Job failed: exit code 137` | Le conteneur a été tué par OOM (Out Of Memory) | Augmentez la mémoire du runner ou optimisez le build (ex. `NODE_OPTIONS=--max-old-space-size=4096` ) | 
| `Cannot connect to the Docker daemon` | DinD pas configuré ou pas démarré | Ajoutez le service `docker:27-dind` et`DOCKER_TLS_CERTDIR: "/certs"` | 
| `fatal: repository not found` | Le runner n'a pas accès au sous-module ou dépôt privé | Configurez `GIT_SUBMODULE_STRATEGY: recursive` et un deploy token | 
| `error during connect: no such host` | Le service (postgres, redis) n'est pas accessible par son alias | Vérifiez que l'alias correspond au hostname utilisé dans DATABASE_URL | 
| `Pulling docker image... access denied` | Authentification au registre manquante ou expirée | Ajoutez `docker login` dans`before_script` avec`$CI_REGISTRY_USER` | 
| `Pipeline filtered out by rules` | Aucune règle ne correspond au contexte actuel du pipeline | Ajoutez une règle par défaut `- when: on_success` en fin de liste | 

Pour diagnostiquer un job en échec, commencez par lire les logs complets dans l'onglet **Jobs** du pipeline. GitLab affiche la sortie standard et d'erreur du conteneur. Pour les problèmes intermittents, activez le mode debug avec la variable `CI_DEBUG_TRACE: "true"` (attention : cela expose les variables masquées dans les logs).

Le linter YAML intégré (**CI/CD > Editor**) valide la syntaxe avant le commit. Utilisez-le systématiquement pour éviter les pipelines qui échouent immédiatement à cause d'une indentation incorrecte ou d'un mot-clé mal orthographié.

Pour les runners self-hosted, consultez les logs du runner avec `sudo gitlab-runner --debug run` ou `journalctl -u gitlab-runner -f`. Les problèmes de DNS, de certificats TLS ou de permissions Docker sont les causes les plus fréquentes d'échec au niveau du runner.

## Astuces Avancées pour Pipelines de Production

Ces techniques avancées transforment un pipeline basique en un système de livraison continue de niveau entreprise.

**Pipelines parent-enfant pour les monorepos.** Si votre dépôt contient plusieurs services (API, frontend, worker), utilisez `trigger` pour déclencher des pipelines enfants indépendants :

```
# Pipeline parent (racine)
trigger-api:
  trigger:
    include: services/api/.gitlab-ci.yml
    strategy: depend
  rules:
    - changes:
        - "services/api/**/*"
trigger-frontend:
  trigger:
    include: services/frontend/.gitlab-ci.yml
    strategy: depend
  rules:
    - changes:
        - "services/frontend/**/*"
```
**Releases automatiques avec semantic versioning.** Combinez les tags Git avec les releases GitLab pour automatiser la création de releases :

```
create-release:
  stage: deploy
  image: registry.gitlab.com/gitlab-org/release-cli:latest
  rules:
    - if: $CI_COMMIT_TAG =~ /^v\d+\.\d+\.\d+$/
  script:
    - echo "Création de la release $CI_COMMIT_TAG"
  release:
    tag_name: $CI_COMMIT_TAG
    description: "Release $CI_COMMIT_TAG"
    assets:
      links:
        - name: "Image Docker"
          url: "https://$CI_REGISTRY_IMAGE:$CI_COMMIT_TAG"
```
**Matrice de tests multi-versions.** Testez votre application sur plusieurs versions de Node.js ou Python en parallèle :

```
test:
  stage: test
  image: node:${NODE_VERSION}-alpine
  parallel:
    matrix:
      - NODE_VERSION: ["20", "22"]
  script:
    - node --version
    - npm ci
    - npm test
```
**Notifications et intégrations.** GitLab s'intègre nativement avec Slack, Microsoft Teams, Jira et des dizaines d'autres outils. Configurez les notifications dans **Settings > Integrations** pour recevoir des alertes sur les pipelines échoués, les merge requests prêtes et les vulnérabilités détectées.

**GitLab Duo pour le débogage CI/CD.** Depuis GitLab 18.9, l'assistant IA GitLab Duo Chat peut analyser les logs de pipelines échoués et suggérer des corrections. Ouvrez le job en échec, cliquez sur l'icône Duo et décrivez le problème. L'IA accède au contexte du pipeline, aux logs et au fichier YAML pour proposer une solution contextuelle.

## Pipeline Complet : le Projet de A à Z

Voici le fichier `.gitlab-ci.yml` complet et fonctionnel qui rassemble toutes les techniques de ce tutoriel. Ce pipeline couvre le lint, les tests, la sécurité, le build Docker, le déploiement staging/production et les notifications.

```
# .gitlab-ci.yml — Pipeline de production complet
stages:
  - validate
  - test
  - build
  - deploy
  - release
variables:
  NODE_ENV: "test"
  DOCKER_IMAGE: "$CI_REGISTRY_IMAGE:$CI_COMMIT_SHORT_SHA"
include:
  - template: Security/SAST.gitlab-ci.yml
  - template: Security/Dependency-Scanning.gitlab-ci.yml
  - template: Security/Secret-Detection.gitlab-ci.yml
# Template partagé
.node-defaults:
  image: node:22-alpine
  before_script:
    - npm ci
  cache:
    key:
      files:
        - package-lock.json
    paths:
      - node_modules/
# === VALIDATE ===
lint:
  extends: .node-defaults
  stage: validate
  script:
    - npx eslint src/ tests/ --max-warnings 0
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    - if: $CI_COMMIT_BRANCH == "main"
# === TEST ===
unit-tests:
  extends: .node-defaults
  stage: test
  needs: ["lint"]
  script:
    - npm test -- --ci --coverage --reporters=default --reporters=jest-junit
  coverage: '/All files[^|]*\|[^|]*\s+([\d\.]+)/'
  artifacts:
    when: always
    reports:
      junit: junit.xml
      coverage_report:
        coverage_format: cobertura
        path: coverage/cobertura-coverage.xml
    paths:
      - coverage/
    expire_in: 7 days
integration-tests:
  extends: .node-defaults
  stage: test
  needs: ["lint"]
  services:
    - name: postgres:16-alpine
      alias: db
  variables:
    POSTGRES_DB: test_db
    POSTGRES_USER: test_user
    POSTGRES_PASSWORD: test_pass
    DATABASE_URL: "postgresql://test_user:test_pass@db:5432/test_db"
  script:
    - npm run test:integration
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
# === BUILD ===
docker-build:
  stage: build
  image: docker:27
  services:
    - docker:27-dind
  needs: ["unit-tests"]
  variables:
    DOCKER_TLS_CERTDIR: "/certs"
  before_script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
  script:
    - docker build
      --cache-from $CI_REGISTRY_IMAGE:latest
      --tag $DOCKER_IMAGE
      --tag $CI_REGISTRY_IMAGE:latest .
    - docker push $DOCKER_IMAGE
    - docker push $CI_REGISTRY_IMAGE:latest
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
# === DEPLOY ===
deploy-staging:
  stage: deploy
  image: alpine:3.20
  needs: ["docker-build", "integration-tests"]
  before_script:
    - apk add --no-cache openssh-client curl
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | ssh-add -
  script:
    - ssh -o StrictHostKeyChecking=no deploy@$STAGING_HOST
      "docker pull $DOCKER_IMAGE &&
       docker compose -f docker-compose.staging.yml up -d &&
       sleep 5 && curl -f http://localhost:3000/health"
  environment:
    name: staging
    url: https://staging.example.com
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
deploy-production:
  stage: deploy
  image: alpine:3.20
  needs: ["deploy-staging"]
  before_script:
    - apk add --no-cache openssh-client curl
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY_PROD" | ssh-add -
  script:
    - ssh -o StrictHostKeyChecking=no deploy@$PRODUCTION_HOST
      "docker pull $DOCKER_IMAGE &&
       docker compose -f docker-compose.prod.yml up -d"
  environment:
    name: production
    url: https://app.example.com
  rules:
    - if: $CI_COMMIT_BRANCH == "main"
      when: manual
  allow_failure: false
# === RELEASE ===
create-release:
  stage: release
  image: registry.gitlab.com/gitlab-org/release-cli:latest
  rules:
    - if: $CI_COMMIT_TAG =~ /^v\d+\.\d+\.\d+$/
  script:
    - echo "Release $CI_COMMIT_TAG"
  release:
    tag_name: $CI_COMMIT_TAG
    description: "Release $CI_COMMIT_TAG"
```
Ce pipeline s'exécute en environ 5 à 8 minutes grâce aux DAG (`needs`), au cache optimisé et au build Docker avec cache de couches. Sur un runner self-hosted avec SSD, les temps descendent à 3-4 minutes — des durées que vous pouvez désormais suivre plus finement grâce au backend d'analytics migré vers ClickHouse depuis GitLab 18.0, qui améliore la précision des métriques de pipeline (documentation mise à jour en août 2026).

## GitLab CI/CD vs GitHub Actions : Quand Choisir Quoi

La question du choix entre GitLab CI/CD et GitHub Actions revient fréquemment. Voici un comparatif objectif basé sur les fonctionnalités de 2026.

| Critère | GitLab CI/CD | GitHub Actions | 
|---|---|---|
| Hébergement | SaaS ou Self-Managed | SaaS uniquement (GHES limité) | 
| Sécurité intégrée | SAST, DAST, Secret Detection, Dependency Scanning | Via actions tierces (CodeQL) | 
| Container Registry | Intégré nativement | ghcr.io (séparé) | 
| Minutes gratuites | 400/mois (SaaS Free) | 2 000/mois (Free) | 
| Environnements | Natif avec review apps | Environments (basique) | 
| DAG / Parallélisme | needs + parallel matrix | needs (similaire) | 
| Compliance | Politiques de conformité (Ultimate) | Branch protection rules | 
| IA intégrée | GitLab Duo Chat | Copilot (payant séparé) | 

GitLab CI/CD est le meilleur choix pour les entreprises qui veulent une plateforme DevSecOps tout-en-un, avec le contrôle d'une instance self-managed et des fonctionnalités de conformité avancées. GitHub Actions est plus adapté aux projets open source et aux équipes qui utilisent déjà l'écosystème GitHub pour la gestion du code et des issues.

Pour les organisations européennes soumises au RGPD et au NIS2, GitLab self-managed offre un avantage significatif : l'hébergement des données CI/CD reste entièrement sous votre contrôle, sur vos propres serveurs ou un cloud souverain.

### Articles Connexes

Pour approfondir les sujets abordés dans ce tutoriel GitLab CI/CD, consultez nos guides complémentaires :

- Comment Construire un Pipeline CI/CD avec GitHub Actions — le concurrent direct de GitLab CI/CD
- Tutoriel Docker pour Débutants — maîtrisez les conteneurs utilisés par les runners GitLab
- Tutoriel Docker Compose — orchestrez vos applications multi-conteneurs
- GitHub vs GitLab 2026 — comparatif complet des deux plateformes
- Déployer avec Kubernetes et Helm — l'étape suivante après Docker pour vos déploiements
- Infrastructure AWS avec Terraform — automatisez votre infrastructure cloud

## FAQ : Questions Fréquentes sur GitLab CI/CD

### Combien coûte GitLab CI/CD ?

Le tier Free offre 400 minutes CI/CD par mois sur les runners partagés SaaS. Le tier Premium (29 $/utilisateur/mois) ajoute les merge request approvals et les environnements protégés. Le tier Ultimate (99 $/utilisateur/mois) débloque le DAST, le dependency scanning avancé et les politiques de conformité. Pour les runners self-hosted, seul le coût du serveur s'applique.

### Puis-je utiliser GitLab CI/CD avec un dépôt GitHub ?

Oui, GitLab supporte le mirroring de dépôts GitHub. Vous pouvez configurer un mirror automatique qui synchronise votre dépôt GitHub vers GitLab, déclenchant les pipelines CI/CD à chaque push. Cette approche est utile pour les équipes qui veulent conserver GitHub pour le code mais utiliser GitLab pour la CI/CD.

### Quelle est la différence entre cache et artefacts ?

Le cache persiste entre les pipelines (best-effort) et sert à stocker les dépendances (node_modules, pip packages). Les artefacts sont garantis dans le même pipeline et transmettent les résultats de build entre les stages. Utilisez le cache pour les dépendances, les artefacts pour les binaires compilés et les rapports de test.

### Comment déboguer un pipeline qui échoue silencieusement ?

Activez le mode debug avec la variable `CI_DEBUG_TRACE: "true"` dans les variables du pipeline (pas dans .gitlab-ci.yml pour éviter de commiter). Cela affiche chaque commande exécutée et les variables d'environnement. Attention : les variables masquées sont visibles en mode debug.

### GitLab CI/CD supporte-t-il les runners Windows et macOS ?

Oui, les runners GitLab sont disponibles pour Linux, Windows et macOS. Les runners SaaS partagés sont sous Linux uniquement. Pour Windows et macOS, vous devez enregistrer vos propres runners avec les executors Shell ou VirtualBox. Les runners macOS sont indispensables pour les builds iOS avec Xcode.

### Comment migrer de Jenkins vers GitLab CI/CD ?

GitLab propose un outil de migration automatique qui convertit les fichiers Jenkinsfile en .gitlab-ci.yml. Pour une migration manuelle, mappez chaque stage Jenkins à un stage GitLab, convertissez les plugins en jobs natifs et remplacez les agents Jenkins par des runners GitLab. Le processus prend généralement 1 à 3 jours pour un pipeline complexe.

### Quelle est la limite de taille des artefacts ?

Sur GitLab SaaS, la limite par défaut est de 1 Go par artefact. Pour les instances self-managed, cette limite est configurable dans les paramètres d'administration. Les artefacts volumineux (rapports de couverture, builds compilés) doivent avoir un `expire_in` court pour éviter de saturer le stockage.

### Comment sécuriser les pipelines contre les attaques supply chain ?

Activez le Secret Detection pour bloquer les commits contenant des secrets. Utilisez le Dependency Scanning pour détecter les vulnérabilités dans les packages NPM/PyPI. Limitez l'accès aux variables protégées aux branches protégées uniquement. Pour les images Docker, utilisez des images de base signées et scannez-les avec le Container Scanning de GitLab Ultimate.
