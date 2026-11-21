---
id: collect-261121-veille-ia/gemini-google/installer-nextcloud-cloud-souverain-13-etapes-2026
title: "Installer Nextcloud : Cloud Souverain 13 Étapes [2026]"
domain: gemini-google
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["nextcloud", "cloud", "numerique", "souverain", "france", "installer", "souverainete", "etapes"]
source: https://tech-insider.org/fr/installer-nextcloud-cloud-souverain-13-etapes-2026/
source_anchor: ""
source_lines: [1, 459]
section: "Gemini_Google"
---
La **souveraineté numérique** est devenue un impératif stratégique en France et en Europe en 2026. Depuis le lancement par France Stratégie de l’Observatoire de la souveraineté numérique en janvier 2026 et la formalisation d’un indice de résilience numérique évaluant les dépendances technologiques selon 20 critères répartis en huit dimensions, les PME, ETI et collectivités cherchent des alternatives concrètes aux grands fournisseurs américains. Reprendre le contrôle de ses données passe souvent par une décision simple : **installer Nextcloud**, la plateforme open source qui transforme un serveur en cloud privé.

Ce tutoriel vous guide en **13 étapes** pour héberger Nextcloud en France sur votre propre serveur, avec Docker Compose, PostgreSQL, Redis et un reverse proxy Caddy gérant automatiquement le HTTPS. Comptez environ **45 minutes** pour un déploiement complet et fonctionnel, prêt pour une équipe de 10 à 20 personnes. À la fin, vous disposerez d’un **cloud souverain open source** conforme au RGPD, sans boîte noire ni droit extraterritorial, avec édition collaborative de documents et chiffrement TLS. Mis à jour le 16 septembre 2026, suite à la publication par Nextcloud GmbH de la version **35.0.0** via l’installeur web le 16 septembre 2026, désormais la version stable de référence.

## Pourquoi installer Nextcloud pour un cloud souverain en 2026

Le débat public français a basculé. Depuis le Sommet de Berlin du 18 novembre 2025, la doctrine officielle ne considère plus le numérique comme un simple support technique mais comme un « champ de compétition » géopolitique. L’État a réagi avec des outils mesurables : l’Observatoire de la souveraineté numérique, lancé par France Stratégie, a pour mission de cartographier les dépendances technologiques de la France pour mieux les réduire, sa première collecte se poursuivant jusqu’à fin février 2026. En parallèle, le SILL (Socle Interministériel de Logiciels Libres) référençait plus de 600 logiciels libres recommandés à l’administration en avril 2026.

Nextcloud s’inscrit exactement dans cette trajectoire. Là où Google Drive, Microsoft OneDrive ou Dropbox stockent vos fichiers sur des infrastructures soumises au CLOUD Act américain, un **auto-hébergement Nextcloud** place vos données sous votre seul contrôle juridique et technique. Les communications officielles de 2026 sur la souveraineté numérique insistent sur trois principes que Nextcloud respecte nativement : la **réversibilité** (vos données restent exportables), la **portabilité** (formats ouverts) et l’**immunité au droit extraterritorial**. Le mouvement est déjà massif : dans un billet publié le 24 mars 2026, Nextcloud GmbH revendiquait plus de **2 millions de nouveaux utilisateurs entreprise** gagnés en un an, une traction qui confirme l’argument ayant poussé de nombreuses administrations à quitter les suites américaines, comme nous l’avons analysé dans notre dossier sur l’abandon de Teams et Zoom.

Au-delà de la conformité, l’intérêt est aussi financier. Nextcloud est sous licence AGPLv3 : le logiciel est gratuit, sans coût de licence par utilisateur. Vous ne payez que l’infrastructure – un serveur dédié chez un hébergeur français comme Scaleway ou OVHcloud, ou même une machine sur site. Pour un cloud souverain européen, l’équation est imbattable : aucune redevance mensuelle par siège, un stockage limité uniquement par votre disque, et un écosystème de plus de 200 applications (Calendrier, Contacts, Talk pour la visioconférence, Collabora pour la bureautique en ligne). C’est le socle modulaire basé sur des standards ouverts que recommandent explicitement les plans d’action 2026 pour les PME et ETI.

Ce guide privilégie l’approche **Docker Compose manuelle** plutôt que Nextcloud All-in-One (AIO), dont l’installeur Docker vient d’atteindre la version **13.6.0** (tag publié le 18 août 2026 par Nextcloud sur GitHub, 290ᵉ version publiée au total). AIO reste plus simple et opinionné – Nextcloud GmbH a d’ailleurs publié le 23 avril 2026 une mise à jour de son interface de gestion apportant de nouvelles options pour les installeurs Windows et Linux – mais Compose offre un contrôle total sur les versions, la topologie réseau, le reverse proxy et les services externes (base de données, cache) – un atout décisif quand on vise la maîtrise complète exigée par une démarche de souveraineté.

## Prérequis : versions et matériel pour héberger Nextcloud

Avant de commencer, voici les composants logiciels et leurs versions de référence à la mi-2026. La branche stable de production de Nextcloud est désormais la **35.x** (version **35.0.0**, diffusée via l’installeur web le 16 septembre 2026, selon Nextcloud GmbH), et les pages d’installation officielles – y compris leurs versions française et allemande, toutes deux mises à jour le 16 septembre 2026 – ciblent déjà cette version 35.0.0 pour toute nouvelle installation. À titre de comparaison, l’archive serveur complète de la branche précédente, `34.0.3.zip`, pesait 315 Mo lors de sa publication le 13 août 2026, selon l’index des releases du serveur Nextcloud – un poids qui donne une idée du volume à télécharger si vous optez pour une installation manuelle plutôt que Docker. Nous utilisons les images officielles Docker, ce qui évite justement toute installation manuelle de PHP ou de serveur web.

| Composant | Version / Référence | Rôle dans l’architecture | 
|---|---|---|
| Système d’exploitation | Ubuntu Server 24.04 LTS | Hôte du serveur (support jusqu’en 2029) | 
| Nextcloud Server | 33 (image `nextcloud:33-apache` ) | Application principale, branche stable | 
| Docker Engine | Dernière version stable (v27+) | Moteur de conteneurs | 
| Docker Compose | v2 (plugin `docker compose` ) | Orchestration multi-conteneurs | 
| PostgreSQL | 16 (image `postgres:16-alpine` ) | Base de données relationnelle | 
| Redis | 7 (image `redis:7-alpine` ) | Cache mémoire et verrouillage de fichiers | 
| PHP | 8.3 (intégré à l’image Nextcloud) | Runtime applicatif | 
| Caddy | 2 (image `caddy:2-alpine` ) | Reverse proxy + HTTPS Let’s Encrypt auto | 
| Collabora Online | CODE (image `collabora/code` ) | Édition collaborative de documents | 

Côté matériel, le dimensionnement dépend du nombre d’utilisateurs et des fonctionnalités activées (prévisualisation d’images, recherche plein texte, bureautique en ligne). Voici des paliers réalistes pour un **cloud souverain auto-hébergé**.

| Usage | vCPU | RAM | Stockage | Commentaire | 
|---|---|---|---|---|
| Test / 1 à 3 utilisateurs | 2 vCPU | 2 Go | 40 Go SSD | Minimum viable, sans Collabora | 
| Petite équipe (10-20 users) | 4 vCPU | 8 Go | 200 Go SSD NVMe | Recommandé avec Collabora | 
| PME (50 users) | 8 vCPU | 16 Go | 1 To SSD | Prévoir recherche plein texte | 
| Usage intensif | 8+ vCPU | 32 Go | 2 To+ SSD | Collabora + prévisualisations lourdes | 

Vous aurez aussi besoin d’un **nom de domaine** (par exemple `cloud.votreentreprise.fr`) pointant vers l’adresse IP publique de votre serveur via un enregistrement DNS de type A, ainsi que des **ports 80 et 443 ouverts** au pare-feu. Caddy a besoin du port 80 pour valider les certificats Let’s Encrypt et du port 443 pour servir le trafic HTTPS. Si vous débutez avec la conteneurisation, notre tutoriel Docker en 13 étapes pose les fondamentaux utiles avant de poursuivre.

## Étape 1 – Préparer et sécuriser le serveur Ubuntu

Connectez-vous à votre serveur Ubuntu 24.04 LTS en SSH. La première règle de toute démarche souveraine est de partir d’une base saine et à jour. Mettez à jour le système et installez les outils de base, puis configurez le pare-feu UFW pour n’exposer que le strict nécessaire : SSH, HTTP et HTTPS.

```
# Mise à jour complète du système
sudo apt update && sudo apt upgrade -y
# Outils essentiels
sudo apt install -y curl git ufw fail2ban
# Pare-feu : on ouvre uniquement SSH, HTTP et HTTPS
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable
sudo ufw status verbose
```
Le paquet `fail2ban` protège votre accès SSH contre les attaques par force brute en bannissant automatiquement les adresses IP suspectes. C’est une protection essentielle dès qu’un serveur est exposé sur Internet. Vérifiez ensuite que votre fuseau horaire est correct (important pour les tâches planifiées et les journaux) avec `timedatectl set-timezone Europe/Paris`. Créez enfin un utilisateur dédié non-root si vous n’en avez pas déjà un, et désactivez l’authentification SSH par mot de passe au profit des clés.

**Sortie attendue** de `ufw status verbose` : vous devez voir les règles 22, 80 et 443 en « ALLOW IN », et la politique par défaut « deny (incoming) ». Si le port 443 n’apparaît pas, Caddy ne pourra pas obtenir de certificat à l’étape 7.

## Étape 2 – Installer Docker Engine et Docker Compose v2

Nous utilisons le dépôt officiel Docker plutôt que la version d’Ubuntu, généralement plus ancienne. Le script ci-dessous ajoute la clé GPG et le dépôt Docker, puis installe le moteur et le plugin Compose v2 (la commande moderne `docker compose` sans tiret, à ne pas confondre avec l’ancien `docker-compose`).

```
# Installation officielle de Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
# Permettre à votre utilisateur de lancer Docker sans sudo
sudo usermod -aG docker $USER
newgrp docker
# Vérification des versions installées
docker --version
docker compose version
```
**Sortie attendue** : `Docker version 27.x.x` et `Docker Compose version v2.x.x`. Le script officiel `get.docker.com` installe toujours la dernière version stable, le plugin Compose v2 et le démarrage automatique du service. Activez-le explicitement au démarrage avec `sudo systemctl enable --now docker`. Si la commande `docker compose version` renvoie une erreur « command not found », c’est que le plugin n’est pas installé : exécutez `sudo apt install docker-compose-plugin`.

Pour aller plus loin sur l’installation et le durcissement de Docker, notre guide d’installation de Docker sur Ubuntu 24.04 détaille la configuration du démon, la rotation des journaux et la gestion des volumes – des réglages qui valent la peine d’être appliqués sur un serveur de production.

## Étape 3 – Structurer le projet et les variables d’environnement

Une architecture propre facilite les sauvegardes et la réversibilité. Créez un répertoire dédié et un fichier `.env` qui centralisera tous vos secrets – jamais codés en dur dans le fichier Compose. Ce découplage est une bonne pratique de sécurité et simplifie la portabilité de votre installation vers un autre hébergeur français.

```
# Arborescence du projet
mkdir -p ~/nextcloud-souverain
cd ~/nextcloud-souverain
# Génération de mots de passe robustes (à copier dans .env)
openssl rand -base64 32   # pour POSTGRES_PASSWORD
openssl rand -base64 32   # pour REDIS_PASSWORD
openssl rand -base64 32   # pour NEXTCLOUD_ADMIN_PASSWORD
```
Créez ensuite le fichier `.env` avec un éditeur (`nano .env`) et collez-y vos valeurs. Remplacez les mots de passe par ceux générés ci-dessus et adaptez le domaine et l’e-mail.

```
# Fichier .env
# --- Base de données PostgreSQL ---
POSTGRES_DB=nextcloud
POSTGRES_USER=nextcloud
POSTGRES_PASSWORD=COLLEZ_VOTRE_MDP_POSTGRES
# --- Cache Redis ---
REDIS_PASSWORD=COLLEZ_VOTRE_MDP_REDIS
# --- Compte administrateur Nextcloud ---
NEXTCLOUD_ADMIN_USER=admin
NEXTCLOUD_ADMIN_PASSWORD=COLLEZ_VOTRE_MDP_ADMIN
# --- Domaine et messagerie ---
NC_DOMAIN=cloud.votreentreprise.fr
[email protected]
```
Sécurisez immédiatement ce fichier : `chmod 600 .env`. Il contient toutes vos clés et ne doit être lisible que par votre utilisateur. Si vous versionnez votre configuration avec Git, ajoutez impérativement `.env` à votre `.gitignore` pour ne jamais publier vos secrets.

## Étape 4 – Configurer le service PostgreSQL

Nextcloud recommande PostgreSQL pour les déploiements de production : il est plus performant que SQLite et plus robuste que MySQL sur les grosses bases. Nous allons commencer à construire le fichier `docker-compose.yml`. Voici le premier service, la base de données, avec un volume persistant pour ne jamais perdre les données même si le conteneur est recréé.

```
# docker-compose.yml (partie 1/3 : base de données)
services:
  db:
    image: postgres:16-alpine
    restart: unless-stopped
    volumes:
      - db_data:/var/lib/postgresql/data
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - nc_internal
```
Deux points méritent l’attention. Le `healthcheck` garantit que Nextcloud ne démarrera qu’une fois la base réellement prête à accepter des connexions, évitant les erreurs de connexion au premier lancement. Le réseau `nc_internal` est un réseau Docker privé : la base de données n’est **jamais exposée** sur Internet, elle n’est accessible que par les autres conteneurs. C’est un principe de sécurité fondamental – aucune raison d’ouvrir le port 5432 vers l’extérieur. Nous définirons les volumes et réseaux à la fin du fichier, à l’étape 7.

## Étape 5 – Ajouter le cache Redis pour les performances

Redis joue deux rôles critiques dans Nextcloud : il sert de cache mémoire (memcache) pour accélérer l’application et de backend de verrouillage transactionnel des fichiers (transactional file locking), qui évite la corruption lors d’accès concurrents. Sur une installation multi-utilisateurs, son absence provoque des lenteurs et des avertissements dans le panneau d’administration. Ajoutons-le au fichier Compose.

```
# docker-compose.yml (partie 2/3 : cache Redis)
  redis:
    image: redis:7-alpine
    restart: unless-stopped
    command: redis-server --requirepass ${REDIS_PASSWORD}
    volumes:
      - redis_data:/data
    healthcheck:
      test: ["CMD", "redis-cli", "-a", "${REDIS_PASSWORD}", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - nc_internal
```
L’option `--requirepass` protège Redis par mot de passe, indispensable même sur un réseau interne. Comme PostgreSQL, Redis reste confiné au réseau privé `nc_internal` et n’expose aucun port public. Nous brancherons Nextcloud sur ce cache via des variables d’environnement à l’étape suivante, ce qui activera automatiquement le verrouillage de fichiers transactionnel – l’un des réglages les plus impactants sur la stabilité d’un **auto-hébergement Nextcloud** en équipe.

## Étape 6 – Déployer le conteneur Nextcloud

Voici le cœur du système. Nous utilisons l’image `nextcloud:33-apache`, qui embarque PHP 8.3, le serveur web Apache et tous les modules requis. Les variables d’environnement connectent Nextcloud à PostgreSQL et à Redis, et pré-remplissent le compte administrateur pour une installation automatisée. Le paramètre `OVERWRITEPROTOCOL=https` est crucial : il indique à Nextcloud qu’il est servi derrière un reverse proxy en HTTPS, ce qui évite les redirections cassées et les avertissements de sécurité.

```
# docker-compose.yml (partie 3/3 : application Nextcloud)
  app:
    image: nextcloud:33-apache
    restart: unless-stopped
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_healthy
    volumes:
      - nc_data:/var/www/html
    environment:
      POSTGRES_HOST: db
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
      REDIS_HOST: redis
      REDIS_HOST_PASSWORD: ${REDIS_PASSWORD}
      NEXTCLOUD_ADMIN_USER: ${NEXTCLOUD_ADMIN_USER}
      NEXTCLOUD_ADMIN_PASSWORD: ${NEXTCLOUD_ADMIN_PASSWORD}
      NEXTCLOUD_TRUSTED_DOMAINS: ${NC_DOMAIN}
      OVERWRITEPROTOCOL: https
      OVERWRITECLIURL: https://${NC_DOMAIN}
    networks:
      - nc_internal
      - nc_proxy
```
Notez que le conteneur `app` est connecté à **deux réseaux** : `nc_internal` pour dialoguer avec la base et le cache, et `nc_proxy` pour recevoir le trafic du reverse proxy. Nextcloud n’expose ainsi **aucun port directement** sur l’hôte – tout passe par Caddy. C’est l’architecture la plus sûre : un seul point d’entrée chiffré. Le volume `nc_data` contient à la fois le code, la configuration (`config/config.php`) et les données utilisateurs.

## Étape 7 – Reverse proxy Caddy et HTTPS automatique

Caddy est le composant qui rend ce tutoriel particulièrement adapté à la souveraineté : il obtient et renouvelle **automatiquement** les certificats TLS via Let’s Encrypt, sans aucune configuration manuelle de Certbot. Ajoutons le service Caddy et finalisons le fichier Compose avec la déclaration des volumes et des réseaux.

```
# Fin du docker-compose.yml : reverse proxy + volumes + réseaux
  caddy:
    image: caddy:2-alpine
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile:ro
      - caddy_data:/data
      - caddy_config:/config
    depends_on:
      - app
    networks:
      - nc_proxy
volumes:
  db_data:
  redis_data:
  nc_data:
  caddy_data:
  caddy_config:
networks:
  nc_internal:
  nc_proxy:
```
Créez maintenant le fichier `Caddyfile` dans le même répertoire. Sa concision est remarquable : quelques lignes suffisent pour un reverse proxy HTTPS complet, avec les redirections recommandées par Nextcloud pour la découverte des services (CalDAV/CardDAV).

```
# Caddyfile
cloud.votreentreprise.fr {
    reverse_proxy app:80
    # Redirections recommandées par Nextcloud
    redir /.well-known/carddav /remote.php/dav 301
    redir /.well-known/caldav /remote.php/dav 301
    # En-tête de sécurité HSTS
    header Strict-Transport-Security "max-age=15768000;"
    encode gzip
}
```
Remplacez le domaine par le vôtre (il doit correspondre à `NC_DOMAIN`). Caddy détecte automatiquement le nom de domaine, contacte Let’s Encrypt et installe le certificat dès le premier démarrage. Aucune intervention n’est requise, et le renouvellement est géré en arrière-plan tous les 60 jours.

## Étape 8 – Lancer la pile et terminer l’installation web

Tout est en place. Lancez l’ensemble des conteneurs en arrière-plan et surveillez les journaux pour vérifier le bon déroulement, en particulier l’obtention du certificat par Caddy. Notez que cette étape correspond, côté documentation officielle, au flux du **Web Installer** (`setup-nextcloud.php`) dont Nextcloud GmbH a mis à jour la documentation le 17 juillet 2026 – utile si vous préférez suivre les instructions officielles en parallèle de ce tutoriel Docker.

```
# Démarrage de toute la pile
docker compose up -d
# Vérifier que les 4 conteneurs tournent
docker compose ps
# Suivre les journaux (Ctrl+C pour quitter)
docker compose logs -f caddy app
```
**Sortie attendue** de `docker compose ps` : quatre services (`db`, `redis`, `app`, `caddy`) avec le statut « running » ou « healthy ». Dans les journaux Caddy, recherchez la ligne `certificate obtained successfully`. Ouvrez ensuite votre navigateur sur `https://cloud.votreentreprise.fr` : vous devez voir l’assistant de connexion Nextcloud avec un cadenas valide. Comme nous avons pré-rempli les identifiants admin via le fichier `.env`, l’installation de la base de données se fait automatiquement au premier accès – connectez-vous simplement avec le compte `admin` et son mot de passe.

Si la page affiche une erreur « Accès interdit » ou « domaine non fiable », c’est que `NEXTCLOUD_TRUSTED_DOMAINS` n’a pas été pris en compte – nous corrigeons cela à l’étape 9. Si vous obtenez une erreur de certificat, vérifiez que votre DNS pointe bien vers le serveur et que le port 80 est ouvert.

## Étape 9 – Régler les domaines de confiance et le cron avec occ

`occ` est l’outil en ligne de commande de Nextcloud, l’équivalent d’un couteau suisse pour l’administration. On l’exécute à l’intérieur du conteneur `app`, en tant qu’utilisateur du serveur web `www-data`. Voici les commandes essentielles de post-installation.

```
# Vérifier l'état de l'instance
docker compose exec -u www-data app php occ status
# Ajouter / confirmer un domaine de confiance
docker compose exec -u www-data app php occ config:system:set \
  trusted_domains 1 --value=cloud.votreentreprise.fr
# Définir la région du téléphone par défaut (France)
docker compose exec -u www-data app php occ config:system:set \
  default_phone_region --value=FR
# Activer le mode cron pour les tâches d'arrière-plan
docker compose exec -u www-data app php occ background:cron
```
Le passage en mode `cron` est important : par défaut, Nextcloud exécute ses tâches d’arrière-plan (nettoyage, notifications, indexation) à chaque chargement de page (AJAX), ce qui est peu fiable. Pour un vrai système cron, ajoutez une tâche planifiée sur l’hôte qui appelle le script toutes les 5 minutes :

```
# Éditer la crontab de l'utilisateur
crontab -e
# Ajouter cette ligne (exécution toutes les 5 minutes)
*/5 * * * * docker compose -f /home/UTILISATEUR/nextcloud-souverain/docker-compose.yml exec -T -u www-data app php cron.php
```
Rendez-vous ensuite dans **Paramètres d’administration → Vue d’ensemble** dans l’interface web. Cette page liste les avertissements de configuration. À ce stade, le cache mémoire (Redis), le verrouillage transactionnel et la région téléphonique doivent être au vert. Toute alerte restante y est documentée avec un lien vers la solution.

## Étape 10 – Activer l’édition collaborative avec Collabora Online

Pour remplacer véritablement Google Docs ou Microsoft Office 365, il faut l’édition collaborative de documents bureautiques directement dans le navigateur. Collabora Online Development Edition (CODE) fournit cette capacité, en s’appuyant sur le moteur LibreOffice. Ajoutez ce service à votre `docker-compose.yml`.

```
# Service Collabora à ajouter dans docker-compose.yml
  collabora:
    image: collabora/code
    restart: unless-stopped
    environment:
      aliasgroup1: https://cloud.votreentreprise.fr:443
      extra_params: --o:ssl.enable=false --o:ssl.termination=true
    cap_add:
      - MKNOD
    networks:
      - nc_proxy
```
Ajoutez le routage dans le `Caddyfile` pour exposer Collabora sur un sous-domaine (créez d’abord l’enregistrement DNS `office.votreentreprise.fr`), puis relancez la pile avec `docker compose up -d`. Dans Nextcloud, installez l’application « Nextcloud Office » depuis l’App Store intégré, allez dans **Paramètres d’administration → Nextcloud Office**, choisissez « Utiliser votre propre serveur » et indiquez l’URL `https://office.votreentreprise.fr`. Vous pourrez désormais créer et co-éditer des documents .odt, .docx, .xlsx et .pptx à plusieurs, en temps réel, sans qu’aucune donnée ne quitte votre infrastructure. C’est la pierre angulaire d’une bureautique réellement souveraine.

## Étape 11 – Durcir la sécurité : 2FA, SSO et en-têtes

Un cloud souverain doit aussi être un cloud sûr. Commencez par activer l’authentification à deux facteurs : installez l’application « Two-Factor TOTP Provider » depuis l’App Store, puis imposez-la aux administrateurs et aux groupes sensibles via **Paramètres → Sécurité**. Cette mesure bloque l’écrasante majorité des compromissions de comptes par vol de mot de passe.

Pour une organisation de plus de 20 utilisateurs, envisagez un fournisseur d’identité (IdP) centralisé via SSO. **Authentik**, une solution open source européenne, s’intègre à Nextcloud en OpenID Connect et permet une gestion centralisée des comptes, des groupes et des politiques de mot de passe. C’est l’architecture modulaire – reverse proxy, IAM, base de données, stockage, bureautique – recommandée par les sources officielles 2026 sur la souveraineté numérique. Le principe du moindre privilège s’applique partout : créez des groupes par service, attribuez des quotas de stockage, et désactivez l’auto-inscription.

```
# Forcer le chiffrement HTTPS au niveau applicatif
docker compose exec -u www-data app php occ config:system:set \
  overwriteprotocol --value=https
# Activer l'app de sécurité TOTP en ligne de commande
docker compose exec -u www-data app php occ app:enable twofactor_totp
# Vérifier la note de sécurité depuis l'extérieur :
# rendez-vous sur https://scan.nextcloud.com et saisissez votre domaine
```
Utilisez le scanner officiel `scan.nextcloud.com` pour obtenir une note de A+ à F sur la configuration de votre serveur (en-têtes HTTP, version à jour, etc.). Caddy ajoute déjà l’en-tête HSTS ; visez le grade A. Enfin, pensez au RGPD : Nextcloud propose des applications de gestion du consentement et de journalisation des accès. La logique de conformité par conception rejoint celle que nous avons décrite pour l’hébergement local de modèles d’IA conformes au RGPD.

## Étape 12 – Sauvegardes et réversibilité des données

La réversibilité est un pilier de la souveraineté : vous devez pouvoir récupérer et migrer vos données à tout moment. Une sauvegarde Nextcloud complète comporte trois éléments : le **dump de la base PostgreSQL**, le **répertoire de données** (volume `nc_data`) et les fichiers de **configuration** (le `config.php` et le `docker-compose.yml`). Voici un script de sauvegarde robuste qui place d’abord Nextcloud en mode maintenance pour garantir la cohérence.

```
#!/bin/bash
# backup-nextcloud.sh
set -e
DEST=/srv/backups/nextcloud/$(date +%Y-%m-%d)
mkdir -p "$DEST"
cd ~/nextcloud-souverain
# 1. Mode maintenance pour figer les données
docker compose exec -T -u www-data app php occ maintenance:mode --on
# 2. Dump de la base PostgreSQL
docker compose exec -T db pg_dump -U nextcloud nextcloud > "$DEST/db.sql"
# 3. Copie du volume de données et de la config
docker run --rm -v nextcloud-souverain_nc_data:/data \
  -v "$DEST":/backup alpine tar czf /backup/nc_data.tar.gz -C /data .
# 4. Sortie du mode maintenance
docker compose exec -T -u www-data app php occ maintenance:mode --off
echo "Sauvegarde terminée : $DEST"
```
Rendez le script exécutable (`chmod +x backup-nextcloud.sh`) et planifiez-le chaque nuit via cron. Pour une vraie résilience, appliquez la règle **3-2-1** : 3 copies, sur 2 supports, dont 1 hors site – par exemple un stockage objet S3 chez un hébergeur français. Testez régulièrement la restauration : une sauvegarde jamais restaurée n’est qu’une hypothèse. Les plans d’action 2026 pour PME recommandent d’ailleurs explicitement de simuler un « scénario de rupture fournisseur » dans son PCA/PRA.

## Étape 13 – Mettre à jour et exploiter dans la durée

La maintenance dans le temps fait la différence entre un projet pérenne et une dette technique. Nextcloud suit un cycle de publication soutenu (plusieurs versions majeures par an dans la nouvelle ère Hub) et un support limité dans le temps : la version 31 (Hub 10), sortie le 25 février 2025, a reçu son correctif de maintenance 31.0.6 dès le 12 juin 2025 (checksum publié par Nextcloud GmbH) avant d’atteindre 31.0.8 le 14 août 2025, puis sa fin de vie en février 2026, soit tout juste 12 mois après sa sortie, d’après le wiki GitHub de Nextcloud. Entre ces deux jalons, la version 32.0.0 est sortie le 27 septembre 2025 chez Nextcloud GmbH, cette branche recevant encore un correctif de maintenance 32.0.15 le 13 août 2026 ; puis Hub 9 a introduit la version 33.0.0 le 18 février 2026, une refonte majeure de l’interface selon WinterFlow.io, avant que la branche stable n’atteigne la 34.x. Il est donc impératif de **ne jamais sauter de version majeure** : pour passer de la 33 à la 35, il faut transiter par la 34.0.4 avant d’atteindre la 35.0.0, diffusée le 16 septembre 2026 par Nextcloud GmbH et devenue depuis la version stable de référence. Avec Docker, la procédure est sûre et reproductible.

```
# Procédure de mise à jour (toujours sauvegarder avant !)
cd ~/nextcloud-souverain
# 1. Sauvegarde préalable
./backup-nextcloud.sh
# 2. Modifier le tag d'image dans docker-compose.yml
#    Exemple : nextcloud:33-apache -> nextcloud:34-apache
nano docker-compose.yml
# 3. Récupérer la nouvelle image et recréer le conteneur
docker compose pull app
docker compose up -d app
# 4. Lancer la mise à niveau de la base
docker compose exec -u www-data app php occ upgrade
docker compose exec -u www-data app php occ db:add-missing-indices
```
Surveillez l’exploitation au quotidien : la page **Vue d’ensemble** de l’administration, l’espace disque (`df -h`), et les journaux (`docker compose logs`). Activez les notifications par e-mail pour les alertes critiques. Pour la supervision avancée, Nextcloud expose un point de terminaison `serverinfo` compatible avec des outils comme Prometheus ou Zabbix. Une infrastructure souveraine bien exploitée tient des années sans surprise.

## 5 pièges courants à éviter lors de l’installation

Certaines erreurs reviennent systématiquement chez ceux qui débutent l’**auto-hébergement Nextcloud**. Les anticiper vous fera gagner des heures.

- **Oublier OVERWRITEPROTOCOL=https** : sans ce paramètre, Nextcloud derrière un reverse proxy génère des liens en HTTP, ce qui casse le chargement des ressources et déclenche des avertissements « contenu mixte ». C’est l’erreur n°1.
- **Exposer la base de données ou Redis** : ne mappez jamais les ports 5432 ou 6379 sur l’hôte. Ces services doivent rester sur le réseau Docker interne. Les exposer ouvre une faille de sécurité majeure.
- **Négliger la tâche cron** : laisser le mode AJAX par défaut entraîne des notifications manquantes, des fichiers non nettoyés et une indexation incomplète. Configurez toujours le cron système.
- **Sous-dimensionner la RAM avec Collabora** : Collabora Online est gourmand. Sur un serveur à 2 Go, il provoquera des plantages. Comptez au minimum 8 Go pour une équipe utilisant la bureautique.
- **Stocker les secrets en clair dans le Compose** : utilisez toujours un fichier`.env` avec`chmod 600` . Coder les mots de passe en dur dans le YAML, surtout s’il est versionné, est une fuite assurée.

## Dépannage : 8 erreurs fréquentes et leurs solutions

Voici les problèmes les plus rencontrés et la façon de les résoudre rapidement. Gardez cette table sous la main lors de votre premier déploiement.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| « Accès via un domaine non fiable » | Domaine absent de `trusted_domains` | `occ config:system:set trusted_domains 1 --value=...` | 
| 502 Bad Gateway | Caddy ne joint pas le conteneur app | Vérifier que `app` est « healthy » et sur le réseau`nc_proxy` | 
| Erreur de certificat TLS | DNS incorrect ou port 80 fermé | Vérifier l’enregistrement A et `ufw allow 80/tcp` | 
| « Internal Server Error » | `memory_limit` PHP trop bas | Définir `PHP_MEMORY_LIMIT=512M` dans l’environnement du conteneur | 
| Upload de gros fichiers échoue | Limite de taille du proxy/PHP | Augmenter `PHP_UPLOAD_LIMIT` (ex. 16G) | 
| Avertissement « pas de cache mémoire » | Redis mal connecté | Vérifier `REDIS_HOST` et`REDIS_HOST_PASSWORD` | 
| Le conteneur app redémarre en boucle | Base pas prête au démarrage | Vérifier le `healthcheck` de`db` et`depends_on` | 
| Collabora ne s’ouvre pas | URL WOPI ou SSL mal configurés | Vérifier `aliasgroup1` et le routage Caddy du sous-domaine | 

En cas de doute, les journaux sont votre meilleur allié. Consultez le journal applicatif Nextcloud avec `docker compose exec -u www-data app php occ log:tail`, et les journaux des conteneurs avec `docker compose logs --tail=100 app`. La page **Paramètres d’administration → Journalisation** affiche aussi les erreurs récentes directement dans l’interface.

## Astuces avancées pour un cloud souverain de production

Une fois la base maîtrisée, plusieurs optimisations transforment votre instance en plateforme de niveau professionnel. La **recherche plein texte** via l’application Full Text Search adossée à un moteur OpenSearch (open source) permet de retrouver instantanément le contenu de milliers de documents – un vrai gain de productivité pour les équipes documentaires.

Pour les performances, activez **HTTP/2** (natif dans Caddy) et configurez le module APCu en complément de Redis pour le cache local. Si votre stockage croît, externalisez le répertoire de données vers du stockage objet S3 compatible chez un hébergeur français : Nextcloud gère le « primary storage » S3 nativement, ce qui découple le calcul du stockage et facilite la montée en charge. Côté collaboration, l’application **Talk** ajoute la visioconférence chiffrée de bout en bout, alternative directe à Zoom et Teams, comme détaillé dans notre analyse de la migration des administrations françaises vers des suites souveraines.

Enfin, pour les déploiements critiques, envisagez la virtualisation sous-jacente avec un hyperviseur libre. Notre comparatif Proxmox vs VMware montre comment bâtir une infrastructure entièrement souveraine, de la couche matérielle jusqu’à l’application. Combiné à un hébergeur national comme Scaleway, vous obtenez une chaîne de souveraineté complète et auditable – exactement ce que vise l’indice de résilience numérique de France Stratégie.

## Nextcloud face aux alternatives propriétaires

Nextcloud n’est pas la seule brique d’un cloud souverain, mais c’est la plus mature pour le partage de fichiers et la collaboration. Voici comment il se positionne face aux solutions propriétaires américaines, sur les critères clés de la souveraineté.

| Critère | Nextcloud auto-hébergé | Google Workspace | Microsoft 365 | 
|---|---|---|---|
| Licence | Open source (AGPLv3) | Propriétaire | Propriétaire | 
| Coût par utilisateur / mois | 0 € (hors infra) | à partir de ~6 € | à partir de ~6 € | 
| Localisation des données | Votre serveur (France/UE) | Mondiale | Mondiale | 
| Soumis au CLOUD Act | Non | Oui | Oui | 
| Réversibilité des données | Totale (formats ouverts) | Partielle | Partielle | 
| Bureautique en ligne | Collabora / OnlyOffice | Google Docs | Office Online | 
| Visioconférence intégrée | Talk (chiffré E2E) | Meet | Teams | 

Le verdict est clair pour une organisation qui place la souveraineté au centre : Nextcloud offre la même couverture fonctionnelle (fichiers, agenda, contacts, bureautique, visio) sans aucun coût de licence ni exposition au droit extraterritorial. Le seul arbitrage réel concerne l’effort d’exploitation interne – c’est précisément ce que ce tutoriel réduit au minimum grâce à Docker et Caddy. Pour les structures sans équipe IT, des hébergeurs français proposent aussi du Nextcloud managé, conservant l’avantage souverain sans la charge opérationnelle.

## Questions fréquentes sur l’installation de Nextcloud

### Nextcloud est-il vraiment gratuit pour une entreprise ?

Oui, l’édition Community sous licence AGPLv3 est entièrement gratuite, sans limite d’utilisateurs ni de stockage. Vous ne payez que l’infrastructure (serveur, domaine, sauvegardes). Nextcloud GmbH propose en option un abonnement Enterprise payant qui ajoute le support professionnel, des fonctionnalités de gouvernance et des garanties de SLA, utile aux grandes organisations mais non obligatoire.

### Faut-il PostgreSQL ou MySQL pour Nextcloud ?

Les deux sont supportés, mais PostgreSQL est généralement recommandé pour la production en raison de sa robustesse sur les volumes importants et de sa gestion fine de la concurrence. SQLite ne convient qu’aux tests sur un seul utilisateur. Ce tutoriel utilise PostgreSQL 16, un choix éprouvé pour un **auto-hébergement Nextcloud** multi-utilisateurs.

### Mon installation Nextcloud est-elle conforme au RGPD ?

L’auto-hébergement vous donne le contrôle total nécessaire à la conformité : vos données restent sur un serveur situé en France ou dans l’UE, sous votre responsabilité juridique. Vous devez toutefois compléter par les mesures organisationnelles habituelles (registre des traitements, politique de conservation, gestion des consentements). Nextcloud facilite cela avec des applications dédiées de gouvernance des données.

### Puis-je héberger Nextcloud chez moi sur un Raspberry Pi ?

Oui pour un usage personnel ou très petite équipe : les images Docker Nextcloud existent en multi-architecture (ARM64). Un Raspberry Pi 5 avec un SSD USB suffit pour quelques utilisateurs, et vous pourrez administrer et synchroniser vos fichiers depuis un poste Linux grâce au client de bureau officiel en AppImage universelle – la version 4.0.4 (185 Mo, publiée le 15 décembre 2025), précédée de la 4.0.3 (signature de 833 octets datée du 3 décembre 2025) et, plus tôt, de la 3.17.3 (185 Mo, du 10 octobre 2025). Pour une équipe professionnelle de 10 à 20 personnes, en revanche, un serveur dédié ou un VPS chez un hébergeur français offrira de meilleures performances, une connexion plus stable et une sauvegarde hors site plus simple.

### AIO ou Docker Compose : quelle méthode choisir ?

Nextcloud All-in-One (AIO) est plus simple et automatise la plupart des composants via une interface de gestion – idéal pour démarrer vite. Docker Compose, l’approche de ce tutoriel, demande un peu plus de configuration mais offre un contrôle total sur les versions, le reverse proxy et la topologie. Pour une démarche de souveraineté où la maîtrise complète compte, Compose est le choix le plus pédagogique et le plus flexible.

### Comment migrer mes données depuis Google Drive ou Dropbox ?

Plusieurs voies existent : le client de bureau Nextcloud synchronise un dossier local, vous pouvez donc télécharger vos données puis les laisser remonter – le client Windows actuel, `Nextcloud-4.0.1-x64.msi` (159 Mo, daté du 27 octobre 2025), toujours complété par la branche de maintenance `Nextcloud-3.17.4-x64.msi` (160 Mo, datée du 20 novembre 2025) pour les postes plus anciens, et son équivalent macOS a connu plusieurs jalons successifs sur le serveur de téléchargement officiel de Nextcloud : `Nextcloud-3.16.2-macOS-vfs.pkg` (317 Mo, le 19 mars 2025), `Nextcloud-3.16.3-macOS-vfs.pkg` (356 Mo, le 16 avril 2025), puis `Nextcloud-4.0.1.pkg.tbz` (326 Mo, le 27 octobre 2025) et `Nextcloud-4.0.2-macOS-vfs.pkg.tbz` (353 Mo, le 25 novembre 2025), avant d’aboutir à la `Nextcloud-4.0.5-macOS-vfs.pkg` (355 Mo, publiée le 19 janvier 2026), selon Nextcloud GmbH. Pour une migration de masse, l’outil `rclone` (open source) transfère directement de Google Drive ou Dropbox vers Nextcloud via WebDAV. Nextcloud propose aussi une application « External storage » pour monter temporairement un service tiers le temps de la bascule.

### Quelle est la différence entre Collabora et OnlyOffice ?

Les deux fournissent l’édition collaborative de documents bureautiques dans Nextcloud. Collabora s’appuie sur le moteur LibreOffice et excelle sur les formats ouverts (ODF). OnlyOffice offre une fidélité supérieure avec les formats Microsoft (.docx, .xlsx). Les deux sont open source et auto-hébergeables ; le choix dépend de votre écosystème documentaire dominant. Ce tutoriel utilise Collabora (CODE), entièrement gratuit.

### Combien de temps prend la maintenance d’une instance Nextcloud ?

Pour une instance bien configurée comme celle de ce guide, comptez environ une à deux heures par mois : application des mises à jour mineures, vérification des sauvegardes, surveillance de l’espace disque et de la page Vue d’ensemble. Les mises à jour majeures (deux à trois fois par an) demandent une planification un peu plus soignée, toujours précédée d’une sauvegarde complète.

### Related Coverage

## Conclusion : reprendre le contrôle de ses données

En 13 étapes, vous avez bâti un **cloud souverain open source** complet : partage de fichiers, agenda, contacts, bureautique collaborative et visioconférence, le tout chiffré, conforme au RGPD et hors de portée du droit extraterritorial. Là où une suite américaine vous facture chaque siège et héberge vos données à l’autre bout du monde, votre instance Nextcloud tourne sur votre matériel, sous votre seule autorité, pour le coût de l’infrastructure.

C’est, à l’échelle d’une organisation, la traduction concrète de la souveraineté numérique que la France érige en priorité depuis 2026. L’Observatoire mesure les dépendances ; vous, vous les réduisez. Commencez petit avec une instance de test, validez vos usages, puis montez en charge sereinement. La réversibilité native de Nextcloud garantit qu’aucune décision n’est irréversible – l’inverse exact de l’enfermement propriétaire. À vous de jouer.

*Sources et ressources externes : documentation officielle Nextcloud, images Docker officielles Nextcloud, documentation Caddy, France Stratégie, CNIL – conformité RGPD.*
