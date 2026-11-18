Les identifiants volés ou réutilisés restent l’un des premiers vecteurs d’intrusion informatique, et la CNIL recommande explicitement l’usage d’un gestionnaire de mots de passe pour générer et stocker des secrets uniques. Le problème : confier l’intégralité de votre coffre-fort numérique à un service cloud tiers – souvent hébergé hors d’Europe – soulève des questions de souveraineté et de conformité au RGPD. **Vaultwarden** répond à ce dilemme. C’est une réimplémentation en Rust du serveur Bitwarden (anciennement *bitwarden_rs*), créée par Daniel García, qui pèse entre 10 et 50 Mo de RAM là où le serveur officiel en réclame plusieurs gigaoctets.

Avec plus de 62 900 étoiles sur GitHub et une licence AGPL-3.0, Vaultwarden implémente l’API Bitwarden : les applications officielles (extension navigateur, application de bureau, mobile, CLI) s’y connectent sans modification. Mieux, il débloque gratuitement des fonctions facturées par Bitwarden (TOTP intégré, pièces jointes, organisations, clés matérielles). Ce guide vous montre comment **auto-héberger** un serveur **Vaultwarden** de production en 12 étapes sur Ubuntu 24.04 LTS, avec Docker Compose, un reverse proxy Caddy gérant le HTTPS automatiquement, un jeton d’administration chiffré en Argon2, les inscriptions verrouillées, une protection fail2ban et des sauvegardes chiffrées. Comptez 40 à 60 minutes. Version cible : **Vaultwarden 1.37.2**, publiée le 22 août 2026 selon ReleaseLog.org – la 85<sup>e</sup> version référencée sur le dépôt GitHub du projet – qui exige désormais des clients en version 2026.8.0 ou supérieure, et déjà proposée en téléchargement sur SourceForge sous la forme d’une archive source d’environ 1 Mo, également disponible via Snap Store (canal stable mis à jour le 27 juillet 2026, canal candidate le 23 août 2026). Elle fait suite à la 1.37.1, datée du 3 août 2026 selon l’avis de sécurité Fedora (le 29 juillet 2026 selon ThereIsAnOpenSourceForThat.com), qui corrige la faille **CVE-2026-31812** et a été empaquetée sous la référence 1.37.1-1.fc44 pour Fedora 44 le même jour selon l’avis de sécurité Fedora, elle-même héritière de la 1.37.0 du 24 juillet 2026 (archive source de 739,1 Ko, exigeant des clients en version 2026.7.0 ou supérieure) qui corrige des problèmes d’invitations ainsi qu’un souci OpenSSL sous Alpine. *Mis à jour le 28 août 2026.*

## Pourquoi auto-héberger son gestionnaire de mots de passe en 2026 ?

Un gestionnaire de mots de passe centralise vos secrets les plus sensibles : accès bancaires, messageries, infrastructures professionnelles. Le confier à un service cloud commercial signifie accepter trois compromis. D’abord, la **dépendance** : si le fournisseur ferme, augmente ses tarifs ou subit une fuite, vous n’avez aucun levier. Ensuite, la **souveraineté** : un coffre hébergé sur une infrastructure soumise au Cloud Act américain peut, en théorie, faire l’objet d’une réquisition extraterritoriale, même si les données sont chiffrées de bout en bout. Enfin, le **coût récurrent** des formules familiales ou professionnelles, qui s’accumule année après année.

L’auto-hébergement renverse ces contraintes. Vous gardez la maîtrise complète du chiffrement, de l’emplacement physique des données et du calendrier de mise à jour. Pour un organisme français ou européen soumis au RGPD, héberger le coffre sur un VPS situé en France ou en Allemagne simplifie la cartographie des traitements et la réponse à une analyse d’impact. La CNIL place la sécurité des données par défaut au cœur de la conformité, et la maîtrise de l’hébergement en fait partie. Cet enjeu de souveraineté numérique structure d’ailleurs la stratégie nationale de cybersécurité française.

Vaultwarden ne fait aucun compromis sur le modèle de sécurité de Bitwarden. Le chiffrement reste *zero-knowledge* : votre mot de passe maître ne quitte jamais l’appareil client, et le serveur ne stocke que des données déjà chiffrées. Auto-héberger n’affaiblit donc pas la cryptographie ; cela déplace simplement le point de confiance du fournisseur vers votre propre serveur. La contrepartie est une responsabilité opérationnelle réelle : sauvegardes, mises à jour de sécurité et durcissement deviennent votre affaire. C’est précisément ce que couvrent les étapes suivantes.

## Vaultwarden vs Bitwarden officiel vs solutions cloud

Avant de déployer, il faut comprendre ce qui distingue Vaultwarden du serveur Bitwarden officiel auto-hébergé et des offres cloud. La différence majeure tient à l’empreinte : Vaultwarden est un binaire Rust unique, là où le déploiement officiel classique orchestre plusieurs conteneurs (application, base SQL, identité, icônes). Cette légèreté permet de faire tourner Vaultwarden sur le plus petit VPS européen, voire sur un Raspberry Pi.

| Critère | Vaultwarden 1.36 | Bitwarden auto-hébergé | Bitwarden Cloud | 
|---|---|---|---|
| Langage / moteur | Rust (binaire unique) | C# / .NET + SQL Server | Géré par l’éditeur | 
| Empreinte RAM au repos | ≈ 10–50 Mo | ≈ 2 Go recommandés | Sans objet | 
| Nombre de conteneurs | 1 (+ reverse proxy) | Plusieurs (stack complète) | Sans objet | 
| Base de données | SQLite, MySQL ou PostgreSQL | MS SQL / PostgreSQL | Géré | 
| TOTP, pièces jointes, organisations | Inclus gratuitement | Selon licence | Abonnement payant | 
| Clés matérielles (FIDO2 / YubiKey) | Inclus | Selon licence | Premium | 
| Statut officiel | Non officiel, communautaire | Officiel | Officiel | 
| Coût logiciel | Gratuit (AGPL-3.0) | Gratuit à payant | Abonnement | 

### Ce que Vaultwarden débloque gratuitement

Parce qu’il réimplémente l’API côté serveur, Vaultwarden ne connaît pas la notion d’abonnement « premium ». L’authentificateur TOTP intégré, les pièces jointes chiffrées, les rapports d’hygiène, les organisations avec collections partagées, l’accès d’urgence et la prise en charge des clés matérielles sont tous actifs par défaut ; la création d’organisations elle-même a d’ailleurs été corrigée dans la version 1.35.2 (web vault 2025.12.1) en janvier 2026, après un bug remonté par la communauté. C’est l’argument central de l’auto-hébergement face aux gestionnaires commerciaux que nous avons comparés ailleurs : vous obtenez l’équivalent fonctionnel d’une formule familiale ou entreprise sans coût d’abonnement. La contrepartie, répétons-le, est que la disponibilité et les sauvegardes reposent entièrement sur vous.

Un point de vigilance juridique : Vaultwarden n’est pas affilié à Bitwarden (8bit Solutions LLC). La marque « Bitwarden » reste la propriété de l’éditeur, et le projet le rappelle clairement. Vous utilisez les *clients* officiels Bitwarden contre un *serveur* communautaire – une combinaison parfaitement fonctionnelle, mais sans support de l’éditeur.

## Comment fonctionne le chiffrement zero-knowledge

Comprendre le modèle cryptographique éclaire toutes les décisions de durcissement qui suivent. Lorsque vous saisissez votre mot de passe maître dans un client Bitwarden, celui-ci ne part jamais en clair vers le serveur. Le client dérive localement une clé via la fonction KDF (PBKDF2-SHA256 à 600 000 itérations par défaut, ou Argon2id). De cette clé découlent deux éléments : une *clé de chiffrement* qui protège le contenu du coffre, et un *hachage d’authentification* – seul ce dernier, lui-même re-haché côté serveur, sert à prouver votre identité.

Conséquence directe : votre serveur Vaultwarden ne stocke que des données chiffrées et un hachage d’authentification. Même un attaquant disposant d’un accès complet à la base SQLite ne peut pas déchiffrer vos mots de passe sans le mot de passe maître, qui n’y figure nulle part. C’est l’architecture *zero-knowledge* : le fournisseur – ici, vous-même – n’a techniquement pas connaissance des secrets. Cela explique aussi pourquoi un mot de passe maître perdu est définitivement perdu, et pourquoi le choix du KDF (étape 10) compte tant : il détermine le coût d’une attaque par force brute si la base venait à fuir.

Ce modèle ne dispense pas de sécuriser le serveur. Un attaquant capable d’altérer le code servi au navigateur pourrait théoriquement intercepter le mot de passe maître au moment de la saisie – d’où l’importance du HTTPS, des en-têtes de sécurité et des mises à jour. Mais il garantit qu’une simple fuite de la base de données, scénario le plus fréquent, ne compromet pas directement vos identifiants. C’est tout l’intérêt de coupler une cryptographie éprouvée à une infrastructure que vous maîtrisez.

## Architecture du projet et prérequis

L’architecture cible est volontairement minimaliste et robuste. Deux conteneurs Docker cohabitent sur un réseau interne : **Vaultwarden**, qui écoute en HTTP sur le port 80 à l’intérieur du réseau Docker, et **Caddy**, le reverse proxy qui termine le TLS et obtient automatiquement un certificat Let’s Encrypt. Seuls les ports 80 et 443 sont exposés à l’extérieur, et le port 80 ne sert qu’à la redirection HTTPS et au défi ACME. Le coffre n’est jamais accessible en clair.

Pourquoi le HTTPS est-il obligatoire et non optionnel ? Parce que les clients Bitwarden s’appuient sur la *Web Crypto API*, qui n’est disponible que dans un contexte sécurisé (HTTPS ou localhost). Sans certificat valide, l’interface web et le déverrouillage échouent. Tenter d’exposer Vaultwarden en HTTP simple est l’erreur la plus fréquente des tutoriels anciens. Voici les prérequis précis, avec leurs versions :

- **Un VPS** sous**Ubuntu 24.04 LTS** (1 vCPU et 1 Go de RAM suffisent largement), de préférence chez un hébergeur européen.
- **Docker Engine 27+** et le plugin**Docker Compose v2** (commande`docker compose` ).
- **Vaultwarden 1.36.0** (image`vaultwarden/server` ) et**Caddy 2.11.4** (image`caddy` ).
- **Un nom de domaine** que vous contrôlez, avec accès à la zone DNS pour créer un enregistrement`A` (et`AAAA` en IPv6).
- **Les ports 80 et 443 ouverts** en entrée sur le pare-feu et chez l’hébergeur.
- Un accès **SSH** avec un compte non-`root` disposant de`sudo` .
- **fail2ban** et**sqlite3** installés (nous les ajoutons dans les étapes).

Si Docker n’est pas encore en place, suivez d’abord notre guide dédié pour installer Docker sur Ubuntu 24.04 en 13 étapes, puis revenez ici. Tout au long du tutoriel, remplacez `coffre.exemple.fr` par votre domaine réel et adaptez les chemins à votre environnement.

## Étape 1 – Préparer et durcir le VPS Ubuntu 24.04

Connectez-vous en SSH puis mettez le système à jour. Nous créons un utilisateur dédié, activons le pare-feu UFW en n’ouvrant que le strict nécessaire, et installons les outils de base. Ce socle de durcissement est valable pour n’importe quel service exposé, pas seulement Vaultwarden.

```
# Mise à jour complète du système
sudo apt update && sudo apt full-upgrade -y
# Outils nécessaires aux étapes suivantes
sudo apt install -y ufw fail2ban sqlite3 curl ca-certificates gnupg
# Pare-feu : on n'ouvre que SSH, HTTP et HTTPS
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw --force enable
sudo ufw status verbose
```
La sortie de `ufw status verbose` doit confirmer que seules les règles 22 (OpenSSH), 80 et 443 sont autorisées en entrée. fail2ban démarre automatiquement avec une prison `sshd` par défaut, ce qui protège déjà votre accès SSH contre le bourrage d’identifiants. Nous ajouterons une prison spécifique à Vaultwarden à l’étape 12. Pensez également à désactiver l’authentification SSH par mot de passe au profit des clés, et à interdire la connexion `root` directe – une bonne pratique de base avant d’exposer le moindre service.

## Étape 2 – Installer Docker Engine et Docker Compose

Vaultwarden se déploie le plus proprement via Docker. Installez Docker Engine depuis le dépôt officiel afin d’obtenir une version récente (27 ou supérieure) et le plugin Compose v2. N’utilisez pas le paquet `docker.io` d’Ubuntu, souvent en retard de plusieurs versions.

```
# Dépôt officiel Docker
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | \
  sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/ubuntu $(. /etc/os-release; echo $VERSION_CODENAME) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io \
  docker-buildx-plugin docker-compose-plugin
# Autoriser votre utilisateur à piloter Docker sans sudo
sudo usermod -aG docker "$USER"
newgrp docker
# Vérification
docker --version
docker compose version
```
Les deux commandes de vérification doivent renvoyer des versions valides. Si `docker compose version` échoue mais que `docker-compose` (avec un tiret) existe, vous utilisez l’ancienne v1 : préférez systématiquement la v2 intégrée. Pour aller plus loin sur l’orchestration multi-conteneurs, notre guide Traefik pour un reverse proxy HTTPS détaille une alternative à Caddy que nous évoquerons en fin d’article.

## Étape 3 – Configurer le DNS et pointer le domaine

Caddy ne peut obtenir un certificat Let’s Encrypt que si votre domaine pointe déjà vers l’IP publique du VPS. Dans la zone DNS de votre registraire, créez un enregistrement `A` faisant correspondre `coffre.exemple.fr` à l’adresse IPv4 du serveur. Ajoutez un enregistrement `AAAA` si vous disposez d’une IPv6. Attendez la propagation, puis vérifiez depuis votre machine locale :

```
# Récupérer l'IP publique du VPS (depuis le serveur)
curl -s https://api.ipify.org; echo
# Vérifier la résolution DNS (depuis votre poste)
dig +short coffre.exemple.fr
# Sortie attendue : l'IPv4 de votre VPS, par exemple
# 203.0.113.42
```
Tant que `dig` ne renvoie pas l’IP exacte du VPS, n’allez pas plus loin : l’émission du certificat échouerait et Caddy entrerait dans une boucle de tentatives. La propagation DNS prend généralement quelques minutes, parfois jusqu’à une heure selon le TTL configuré. Profitez-en pour vérifier chez votre hébergeur qu’aucun pare-feu externe ne bloque les ports 80 et 443 – c’est une cause d’échec classique sur les VPS dont le panneau d’administration filtre par défaut.

## Étape 4 – Structurer le projet et écrire docker-compose.yml

Créez un répertoire de projet propre. Toutes les données persistantes (base SQLite, pièces jointes, certificats) y résideront via des montages de volumes, ce qui simplifie les sauvegardes.

```
sudo mkdir -p /opt/vaultwarden
sudo chown "$USER":"$USER" /opt/vaultwarden
cd /opt/vaultwarden
mkdir -p vw-data caddy-data caddy-config
```
Rédigez ensuite le fichier `docker-compose.yml`. Vaultwarden n’expose aucun port directement vers l’hôte : seul Caddy publie 80 et 443. La communication entre les deux conteneurs passe par un réseau Docker interne nommé `vw-net`.

```
# /opt/vaultwarden/docker-compose.yml
services:
  vaultwarden:
    image: vaultwarden/server:1.36.0
    container_name: vaultwarden
    restart: unless-stopped
    env_file: .env
    volumes:
      - ./vw-data:/data
    networks:
      - vw-net
  caddy:
    image: caddy:2.11.4
    container_name: caddy
    restart: unless-stopped
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile:ro
      - ./caddy-data:/data
      - ./caddy-config:/config
    networks:
      - vw-net
networks:
  vw-net:
```
Le mapping `443:443/udp` active HTTP/3 (QUIC) côté Caddy. L’épinglage des versions d’image (`:1.36.0` plutôt que `:latest`) est une bonne pratique : il rend vos déploiements reproductibles et vous évite une mise à jour majeure non maîtrisée. Le tableau suivant récapitule les variables d’environnement essentielles que nous allons définir dans le fichier `.env`.

| Variable | Valeur recommandée | Rôle | 
|---|---|---|
| `DOMAIN` | `https://coffre.exemple.fr` | URL canonique complète, indispensable au bon fonctionnement | 
| `SIGNUPS_ALLOWED` | `false` (après création) | Verrouille les inscriptions publiques | 
| `SIGNUPS_VERIFY` | `true` | Exige une vérification e-mail (nécessite SMTP) | 
| `INVITATIONS_ALLOWED` | `true` | Autorise l’ajout d’utilisateurs par invitation | 
| `ADMIN_TOKEN` | Hachage Argon2 | Protège le panneau d’administration | 
| `LOG_FILE` | `/data/vaultwarden.log` | Active la journalisation pour fail2ban | 
| `LOG_LEVEL` | `warn` | Niveau de verbosité des journaux | 
| `PUSH_ENABLED` | `false` (par défaut) | Notifications push mobiles (optionnel) | 

## Étape 5 – Générer l’ADMIN_TOKEN Argon2 et le fichier .env

Le panneau d’administration de Vaultwarden permet de gérer les utilisateurs et la configuration. Le protéger par un jeton en clair serait risqué : depuis les versions récentes, Vaultwarden accepte un hachage **Argon2id** au format PHC, bien plus sûr. Générez-le directement avec le binaire embarqué dans l’image :

```
# Générer un hachage Argon2id pour le jeton admin
docker run --rm -it vaultwarden/server:1.36.0 /vaultwarden hash
# Saisissez un mot de passe fort, deux fois.
# Sortie (exemple) :
# Generating an Argon2id PHC string...
# $argon2id$v=19$m=65540,t=3,p=4$bWluZWQ...$cXVlbHF1ZWNob3Nl...
```
Copiez l’intégralité de la chaîne `$argon2id$...`. Créez ensuite le fichier `.env` dans `/opt/vaultwarden`. Point crucial et source d’erreur n°1 : dans un fichier lu par Docker Compose, **chaque `$` du hachage doit être doublé en `$$`**, faute de quoi Compose tentera d’interpréter la chaîne comme des variables et corrompra le jeton.

```
# /opt/vaultwarden/.env
DOMAIN=https://coffre.exemple.fr
# Inscriptions : laissez "true" UNIQUEMENT le temps de créer le 1er compte
SIGNUPS_ALLOWED=true
SIGNUPS_VERIFY=false
INVITATIONS_ALLOWED=true
# Jeton admin Argon2id – NOTEZ les $ doublés en $$
ADMIN_TOKEN=$$argon2id$$v=19$$m=65540,t=3,p=4$$bWluZWQ...$$cXVlbHF1ZWNob3Nl...
# Journalisation (requise pour fail2ban)
LOG_FILE=/data/vaultwarden.log
LOG_LEVEL=warn
# SMTP (optionnel) – décommentez et adaptez pour activer la vérification e-mail
# SMTP_HOST=smtp.exemple.fr
# [email protected]
# SMTP_PORT=587
# SMTP_SECURITY=starttls
# [email protected]
# SMTP_PASSWORD=motdepasse_smtp
```
Restreignez les permissions du fichier : `chmod 600 .env`. Il contient le hachage du jeton admin et, éventuellement, des identifiants SMTP. Si vous n’avez pas de serveur SMTP, laissez `SIGNUPS_VERIFY=false` : la vérification par e-mail sera simplement désactivée, sans bloquer le service.

## Étape 6 – Mettre en place le reverse proxy HTTPS avec Caddy

Caddy est idéal pour ce rôle car il obtient et renouvelle les certificats Let’s Encrypt sans configuration manuelle. Le `Caddyfile` tient en quelques lignes : il déclare le domaine, active la compression et transmet tout le trafic au conteneur Vaultwarden sur son port 80 interne.

```
# /opt/vaultwarden/Caddyfile
coffre.exemple.fr {
    encode gzip zstd
    # Transmet HTTP et WebSocket vers Vaultwarden
    reverse_proxy vaultwarden:80 {
        header_up X-Real-IP {remote_host}
    }
    # En-têtes de sécurité
    header {
        Strict-Transport-Security "max-age=31536000; includeSubDomains; preload"
        X-Content-Type-Options "nosniff"
        X-Frame-Options "DENY"
        Referrer-Policy "no-referrer"
    }
}
```
Un détail capital concernant les **WebSockets** : depuis Vaultwarden 1.29, les notifications temps réel (synchronisation instantanée entre appareils) transitent par le même port HTTP que le reste de l’application. Vous n’avez donc *plus* besoin du port 3012 séparé que réclamaient les anciens guides. La directive `reverse_proxy vaultwarden:80` suffit : Caddy détecte et relaie automatiquement la mise à niveau WebSocket. Ajouter une configuration 3012 obsolète provoquerait des erreurs inutiles.

## Étape 7 – Démarrer la stack et vérifier le déploiement

Tout est en place. Lancez les deux conteneurs en arrière-plan, puis surveillez les journaux pour confirmer que Caddy obtient bien son certificat et que Vaultwarden démarre.

```
cd /opt/vaultwarden
docker compose up -d
# Suivre les journaux des deux services
docker compose logs -f
# Vérifier l'état des conteneurs
docker compose ps
```
Dans les journaux de Caddy, recherchez une ligne indiquant `certificate obtained successfully`. Côté Vaultwarden, vous verrez le démarrage du serveur Rocket. Testez ensuite l’accès depuis l’extérieur en interrogeant le point de santé intégré :

```
# Point de santé : renvoie l'heure UTC du serveur si tout va bien
curl -s https://coffre.exemple.fr/alive
# Sortie attendue (exemple) :
# "2026-06-26T09:14:03.512Z"
# Version exposée par l'API
curl -s https://coffre.exemple.fr/api/version
# Sortie attendue (exemple) :
# "1.36.0"
```
Ouvrez enfin `https://coffre.exemple.fr` dans un navigateur : la page d’inscription Bitwarden doit s’afficher avec un cadenas valide. Si le certificat est marqué comme non sécurisé, c’est presque toujours un problème de DNS (étape 3) ou de port 80 bloqué empêchant le défi ACME.

## Étape 8 – Créer le premier compte et verrouiller les inscriptions

Sur la page d’accueil, cliquez sur « Créer un compte » et enregistrez votre utilisateur principal avec une adresse e-mail et un **mot de passe maître** long et unique. Ce mot de passe est la clé de tout votre coffre : il n’est jamais transmis au serveur et ne peut pas être réinitialisé sans indice de récupération. Notez-le hors ligne pendant la phase d’installation.

Une fois le compte créé, **fermez immédiatement les inscriptions publiques**. Sans cela, n’importe qui découvrant votre URL pourrait créer un compte. Modifiez le `.env` et redémarrez la pile :

```
# Dans /opt/vaultwarden/.env, passez :
# SIGNUPS_ALLOWED=false
sed -i 's/^SIGNUPS_ALLOWED=true/SIGNUPS_ALLOWED=false/' /opt/vaultwarden/.env
# Appliquer le changement
docker compose up -d
# Vérifier l'état du service après redémarrage
docker compose ps
```
À partir de là, l’ajout de nouveaux utilisateurs (membres de la famille, collègues) se fera par **invitation** depuis le panneau d’administration ou une organisation, grâce à `INVITATIONS_ALLOWED=true`. C’est le modèle recommandé : aucune inscription anonyme, chaque accès est nominatif et tracé.

## Étape 9 – Connecter les clients Bitwarden officiels

L’intérêt de Vaultwarden est de pouvoir utiliser les applications Bitwarden officielles. Dans chaque client – extension navigateur, application de bureau, mobile ou CLI – il faut indiquer l’URL de votre serveur auto-hébergé *avant* de se connecter.

- **Extension / application :** sur l’écran de connexion, cliquez sur l’icône d’engrenage (Paramètres régionaux), choisissez « Auto-hébergé » et renseignez`https://coffre.exemple.fr` dans le champ « URL du serveur ».
- **Mobile (iOS / Android) :** même principe, le réglage de l’environnement se trouve sur l’écran de connexion avant la saisie des identifiants.
- **CLI :** configurez le serveur puis authentifiez-vous.

```
# Installer le client en ligne de commande Bitwarden
npm install -g @bitwarden/cli
# Pointer la CLI vers votre serveur auto-hébergé
bw config server https://coffre.exemple.fr
# Se connecter (l'e-mail et le mot de passe maître seront demandés)
bw login
# Déverrouiller et lister un élément pour valider
export BW_SESSION="$(bw unlock --raw)"
bw list items | head
```
Si `bw config server` est oublié, la CLI tentera de joindre le cloud Bitwarden officiel et la connexion échouera avec une erreur d’identifiants : c’est le piège le plus courant côté client. Une fois connecté sur un appareil, créez quelques entrées de test et observez la synchronisation instantanée sur les autres clients – preuve que les WebSockets fonctionnent.

## Étape 10 – Renforcer la sécurité : 2FA, Argon2id et politiques

Le coffre est en ligne ; il faut maintenant le durcir au niveau applicatif. Trois leviers sont prioritaires.

### Activer la double authentification

Dans les paramètres de sécurité de votre compte (via l’interface web), activez la 2FA. Vaultwarden gère gratuitement le TOTP (application d’authentification), les clés matérielles FIDO2/WebAuthn comme une YubiKey, et l’e-mail. Pour un compte administrateur de coffre, la clé matérielle est l’option la plus résistante au hameçonnage. Sauvegardez les codes de récupération hors ligne.

### Passer la dérivation de clé en Argon2id

La robustesse de votre mot de passe maître dépend de la fonction de dérivation de clé (KDF). Par défaut, les clients Bitwarden utilisent PBKDF2-SHA256 avec 600 000 itérations. Vous pouvez basculer sur **Argon2id**, plus résistant aux attaques par matériel spécialisé, depuis Paramètres > Sécurité > Clé de chiffrement. La documentation Bitwarden sur les algorithmes KDF détaille les paramètres recommandés (mémoire, itérations, parallélisme). C’est un réglage côté client : il rechiffre votre coffre lors du changement.

Pour une organisation, définissez aussi des **politiques** via le panneau admin ou l’organisation : longueur minimale du mot de passe maître, 2FA obligatoire pour tous les membres, désactivation de l’export individuel. Accédez au panneau d’administration sur `https://coffre.exemple.fr/admin` et authentifiez-vous avec le mot de passe choisi à l’étape 5. C’est là que vous invitez de nouveaux utilisateurs et consultez l’état du service.

## Étape 11 – Mettre en place des sauvegardes chiffrées automatiques

Sans sauvegarde, l’auto-hébergement est une roulette russe. La base de Vaultwarden est en SQLite, en mode WAL : copier bêtement le fichier pendant que le service tourne risque de produire une sauvegarde corrompue. La méthode correcte est la commande `.backup` de `sqlite3`, qui réalise une copie cohérente à chaud. Le script suivant sauvegarde la base, les pièces jointes et les clés, puis chiffre le tout en AES-256 avec GPG.

```
#!/usr/bin/env bash
# /opt/vaultwarden/backup.sh
set -euo pipefail
SRC="/opt/vaultwarden/vw-data"
DEST="/opt/vaultwarden/backups"
STAMP="$(date +%F_%H%M)"
mkdir -p "$DEST"
# 1) Copie cohérente de la base SQLite (mode WAL)
sqlite3 "$SRC/db.sqlite3" ".backup '$DEST/db-$STAMP.sqlite3'"
# 2) Archive des fichiers : pièces jointes, sends, clés, config
tar czf "$DEST/files-$STAMP.tar.gz" -C "$SRC" \
  attachments sends config.json rsa_key.pem 2>/dev/null || true
# 3) Chiffrement AES-256 de la base
gpg --batch --yes --symmetric --cipher-algo AES256 \
    --passphrase-file /root/.vw-backup-pass \
    "$DEST/db-$STAMP.sqlite3"
rm -f "$DEST/db-$STAMP.sqlite3"
# 4) Rotation : conserver 14 jours
find "$DEST" -type f -mtime +14 -delete
echo "Sauvegarde $STAMP terminée."
```
Rendez le script exécutable (`chmod +x backup.sh`), stockez la phrase secrète dans `/root/.vw-backup-pass` (permissions `600`), puis planifiez une exécution quotidienne via cron :

```
# Éditer la crontab root
sudo crontab -e
# Ajouter : sauvegarde tous les jours à 3h15
15 3 * * * /opt/vaultwarden/backup.sh >> /var/log/vw-backup.log 2>&1
```
Une sauvegarde n’a de valeur que si la **restauration** est testée. Pour restaurer : déchiffrez l’archive (`gpg --decrypt db-DATE.sqlite3.gpg > db.sqlite3`), arrêtez la pile (`docker compose down`), remplacez les fichiers dans `vw-data`, puis relancez. Copiez régulièrement les sauvegardes chiffrées hors du serveur (stockage objet européen, disque externe) pour survivre à une perte totale du VPS.

## Étape 12 – Fail2ban, mises à jour et supervision

Dernière étape : protéger le coffre contre le bourrage d’identifiants et garder la pile à jour – un impératif rappelé par le CVE-2025-24365, divulgué le 27 janvier 2025, corrigé dans la version 1.33.0 et dont la fiche du NVD (NIST) a encore été mise à jour en juin 2026 selon SentinelOne. L’urgence n’a rien de théorique : une étude d’infrastructure Censys recensait encore, en mai 2026, environ 1 700 serveurs Vaultwarden exposés et vulnérables à ces failles critiques de 2025, preuve qu’une version datée ne doit jamais rester en production. Comme nous avons activé `LOG_FILE` à l’étape 5, Vaultwarden journalise les échecs de connexion, ce que fail2ban peut exploiter. Créez un filtre puis une prison.

```
# /etc/fail2ban/filter.d/vaultwarden.conf
[Definition]
failregex = ^.*Username or password is incorrect\. Try again\. IP: <ADDR>\. Username:.*$
ignoreregex =
```
```
# /etc/fail2ban/jail.d/vaultwarden.local
[vaultwarden]
enabled  = true
filter   = vaultwarden
logpath  = /opt/vaultwarden/vw-data/vaultwarden.log
port     = 80,443
maxretry = 5
bantime  = 14400
findtime = 14400
```
Rechargez fail2ban (`sudo systemctl restart fail2ban`) puis vérifiez la prise en compte avec `sudo fail2ban-client status vaultwarden`. Au bout de 5 tentatives ratées en 4 heures, l’adresse fautive est bannie. Pour la **mise à jour**, modifiez le tag d’image dans `docker-compose.yml` vers une version récente – par exemple la **1.37.2** du 22 août 2026, ou a minima la **1.36.0** du 3 mai 2026, qui embarque le web vault 2026.4.1, mitige plusieurs avis de sécurité selon WinterFlow et a clos à elle seule 6 avis de sécurité en ajoutant l’archivage d’éléments après une série de 13 avis de sécurité publiés entre février et mai 2026 sur les versions 1.35.3 à 1.36.0 selon WZ-IT, sans oublier l’étape intermédiaire de la **1.35.8** du 25 avril 2026, qui a mis à jour la toolchain Rust, corrigé des soucis DNS ainsi que la politique de mot de passe maître et d’authentification, et réglé un bug affectant les codes de récupération et les jetons de rafraîchissement (*refresh tokens*) selon WinterFlow.io – puis :

```
cd /opt/vaultwarden
# Sauvegarde AVANT toute mise à jour
./backup.sh
# Récupérer la nouvelle image et redéployer
docker compose pull
docker compose up -d
# Nettoyer les anciennes images
docker image prune -f
```
Surveillez les notes de version sur GitHub avant chaque montée : elles signalent les changements de comportement et les éventuels correctifs de sécurité – Release-monitoring.org retrace d’ailleurs cette cadence dès les correctifs mineurs 1.33.2 du 9 février 2025 et 1.34.1 du 27 mai 2025, avant que la 1.34.0 (web vault v2025.5.0, mai 2025) n’introduise l’inscription avec vérification e-mail obligatoire, tandis que la 1.35.0 (web vault v2025.12.0), sortie le 27 décembre 2025, a ajouté le support de l’authentification unique OpenID Connect (SSO), une attestation de release renforçant l’intégrité des binaires publiés, et étendu la compatibilité aux clients mobiles 2026.1.0 et suivants, selon WinterFlow.io, Tweakers et SourceForge – une cadence de publication toujours aussi soutenue, puisque le dépôt communautaire Debian livrait déjà le paquet 1.37.2-1 avec le web vault 2026.7.0-1 dès le 28 août 2026, quelques jours seulement après sa sortie officielle, selon le dépôt vaultwarden-deb. Évitez l’automatisation aveugle des mises à jour majeures sur un service aussi sensible – une sauvegarde fraîche suivie d’un `pull` manuel reste la voie la plus sûre.

## Choisir un hébergeur européen pour la souveraineté des données

L’argument de souveraineté ne tient que si le VPS est lui-même hébergé en Europe. Plusieurs fournisseurs proposent des machines adaptées à Vaultwarden, dont l’empreinte minimale autorise les offres les plus modestes. Les tarifs ci-dessous sont indicatifs au moment de la rédaction et doivent être vérifiés sur les sites des fournisseurs ; les spécifications, elles, sont stables.

| Hébergeur | Localisation | Config d’entrée typique | Tarif indicatif | Atout | 
|---|---|---|---|---|
| OVHcloud | France (Gravelines, Roubaix) | 1 vCPU / 2 Go | ≈ 5–7 €/mois | Acteur français, RGPD natif | 
| Scaleway | France (Paris) | 1–2 vCPU / 2 Go | ≈ 5–8 €/mois | Souveraineté, API moderne | 
| Infomaniak | Suisse | 2 vCPU / 2 Go | ≈ 6–9 €/mois | Neutralité, énergie renouvelable | 
| Hetzner | Allemagne / Finlande | 2 vCPU / 4 Go | ≈ 4–6 €/mois | Rapport perf/prix imbattable | 
| Auto-hébergement | Domicile / entreprise | Raspberry Pi 5 / mini-PC | Matériel ponctuel | Contrôle physique total | 

Pour un usage strictement personnel ou familial, un Raspberry Pi 5 à domicile suffit amplement, à condition d’avoir une IP publique stable ou un tunnel. Pour un usage professionnel ou multi-utilisateurs, un VPS européen géré offre une meilleure disponibilité. Vous pouvez vérifier l’empreinte réelle de votre instance en production avec une seule commande :

```
# Mesurer la consommation réelle du conteneur
docker stats --no-stream vaultwarden
# Sortie (exemple) :
# CONTAINER     CPU %   MEM USAGE / LIMIT   MEM %
# vaultwarden   0.00%   18.4MiB / 1.94GiB   0.93%
```
Avec moins de 20 Mo consommés au repos, l’empreinte mémoire dérisoire de Vaultwarden signifie que le facteur limitant sera presque toujours le réseau et la stratégie de sauvegarde, jamais le CPU ou la RAM. C’est précisément ce qui rend l’auto-hébergement accessible à quasiment n’importe quel matériel.

## 5 pièges courants à éviter

- **Exposer Vaultwarden en HTTP.** Les clients exigent un contexte sécurisé (Web Crypto API). Sans HTTPS valide, le déverrouillage échoue. Le reverse proxy avec certificat n’est pas optionnel.
- **Oublier de doubler les `$` du jeton Argon2.** Dans`.env` et`docker-compose.yml` , chaque`$` doit devenir`$$` , sinon Compose corrompt le hachage et le panneau admin devient inaccessible.
- **Laisser `SIGNUPS_ALLOWED=true` en production.** Après création de votre compte, verrouillez les inscriptions, sinon votre URL devient un point d’entrée ouvert.
- **Copier le fichier SQLite à chaud.** En mode WAL, un simple`cp` peut produire une sauvegarde incohérente. Utilisez toujours`sqlite3 .backup` .
- **Configurer un port WebSocket 3012 séparé.** Obsolète depuis la 1.29 : les WebSockets passent par le port HTTP principal. Cette config héritée des vieux tutoriels génère des erreurs.

## Dépannage : 8 erreurs fréquentes

| Symptôme | Cause probable | Solution | 
|---|---|---|
| Certificat invalide / non sécurisé | DNS non propagé ou port 80 bloqué | Vérifier `dig` , ouvrir le port 80 pour le défi ACME | 
| Le panneau `/admin` refuse le jeton | `$` non doublés dans le hachage Argon2 | Remplacer chaque `$` par`$$` , redéployer | 
| « Username or password is incorrect » côté client | URL serveur non configurée | `bw config server` / réglage « Auto-hébergé » | 
| Pas de synchronisation temps réel | WebSocket non relayé par le proxy | Proxy vers le port 80, supprimer toute config 3012 | 
| Erreur 502 Bad Gateway | Conteneur Vaultwarden non démarré | `docker compose logs vaultwarden` | 
| Boucle de renouvellement Caddy | Domaine ne pointe pas vers le VPS | Corriger l’enregistrement DNS `A` | 
| Inscription impossible (voulue plus tard) | `SIGNUPS_ALLOWED=false` | Inviter l’utilisateur depuis l’admin/organisation | 
| E-mails de vérification non envoyés | SMTP non ou mal configuré | Renseigner les variables `SMTP_*` dans`.env` | 

En cas de doute, le premier réflexe est `docker compose logs -f` : les journaux de Vaultwarden et de Caddy y sont explicites. Pour les erreurs de certificat, les journaux de Caddy précisent toujours l’étape exacte du défi ACME qui a échoué, ce qui pointe presque systématiquement vers le DNS ou le pare-feu.

## Astuces avancées pour aller plus loin

Une fois la base solide, plusieurs optimisations valent le détour. Côté **collaboration**, créez une *organisation* pour partager des collections de mots de passe entre membres d’une équipe ou d’une famille, avec des permissions granulaires. L’**accès d’urgence** permet de désigner un contact de confiance qui pourra récupérer votre coffre après un délai d’attente – une fonctionnalité premium chez Bitwarden, gratuite ici.

Pour une instance à forte charge ou multi-utilisateurs, migrez la base de SQLite vers **PostgreSQL ou MySQL** via la variable `DATABASE_URL` ; SQLite reste néanmoins parfait pour la plupart des usages. Si vous préférez Traefik à Caddy, notre tutoriel Traefik reverse proxy HTTPS montre comment le brancher via des labels Docker. Enfin, pour ne *jamais* exposer le coffre sur l’internet public, restreignez l’accès à un réseau privé : couplez Vaultwarden à un tunnel WireGuard ou à un réseau mesh Tailscale, de sorte que seuls vos appareils authentifiés atteignent le serveur. C’est l’approche la plus sûre pour un coffre familial.

Activez aussi les **notifications push mobiles** en enregistrant un identifiant d’installation gratuit auprès du relais Bitwarden, ce qui évite aux applications mobiles de scruter le serveur en continu. Et pensez à activer le *fail2ban* spécifique au panneau d’administration en plus de la prison principale : un attaquant qui découvre `/admin` sera ainsi banni après quelques tentatives.

## FAQ – Auto-héberger Vaultwarden

### Vaultwarden est-il sûr pour stocker des mots de passe sensibles ?

Oui. Vaultwarden réimplémente l’API Bitwarden mais conserve son modèle *zero-knowledge* : le chiffrement et le déchiffrement se font côté client, votre mot de passe maître ne quitte jamais l’appareil. Le serveur ne stocke que des données déjà chiffrées. La sécurité dépend surtout de votre durcissement : HTTPS, 2FA, jeton admin Argon2, sauvegardes et mises à jour régulières.

### Quelle différence avec le serveur Bitwarden officiel ?

Vaultwarden est un projet communautaire non officiel, écrit en Rust, qui tient dans un seul conteneur léger (10–50 Mo de RAM) et débloque gratuitement des fonctions premium ; sa version la plus récente, la 1.36.0, embarque le web vault v2026.4.1 depuis mai 2026 selon WinterFlow.io, tandis que la branche 1.37.x a déjà atteint la 1.37.2 du 22 août 2026, soit la 85<sup>e</sup> version publiée sur le dépôt GitHub du projet, également distribuée via Snap Store. Le serveur officiel, écrit en C#/.NET, est plus lourd (plusieurs conteneurs, ~2 Go de RAM) mais bénéficie du support de l’éditeur. Les deux fonctionnent avec les mêmes clients Bitwarden officiels.

### Puis-je utiliser les applications Bitwarden officielles ?

Absolument. C’est tout l’intérêt : extension navigateur, application de bureau, mobile et CLI fonctionnent contre Vaultwarden. Il suffit de configurer l’URL de votre serveur auto-hébergé dans les paramètres de chaque client avant de vous connecter.

### Le HTTPS est-il vraiment obligatoire ?

Oui, sans exception en production. Les clients Bitwarden s’appuient sur la Web Crypto API, disponible uniquement en contexte sécurisé. Un reverse proxy comme Caddy ou Traefik, qui obtient automatiquement un certificat Let’s Encrypt, est la solution standard. Le HTTP simple ne fonctionnera pas au-delà de localhost.

### Combien de ressources faut-il pour héberger Vaultwarden ?

Très peu. Au repos, Vaultwarden consomme entre 10 et 50 Mo de RAM. Un VPS à 1 vCPU et 1 Go de RAM, ou même un Raspberry Pi, suffit pour une famille ou une petite équipe. Le facteur limitant est généralement la stratégie de sauvegarde, pas la puissance de calcul.

### Comment sauvegarder et restaurer mon coffre ?

Sauvegardez la base SQLite avec `sqlite3 .backup` (jamais un simple `cp` à chaud), archivez le dossier des pièces jointes et des clés, chiffrez le tout, et planifiez via cron. Pour restaurer, arrêtez la pile, replacez les fichiers déchiffrés dans le volume `vw-data`, puis relancez. Testez la restauration au moins une fois : une sauvegarde non testée n’en est pas une.

### Vaultwarden est-il conforme au RGPD ?

L’auto-hébergement facilite la conformité en gardant les données sur une infrastructure que vous contrôlez, idéalement sur un VPS européen. Vous restez responsable de traitement : documentez l’hébergement, sécurisez l’accès, gérez les sauvegardes et les droits des utilisateurs. Le logiciel ne « rend » pas conforme à lui seul, mais il supprime la dépendance à un cloud tiers extra-européen.

### Que se passe-t-il si je perds mon mot de passe maître ?

Par conception *zero-knowledge*, le mot de passe maître n’est pas récupérable : sans lui, le coffre est indéchiffrable, même pour vous en tant qu’administrateur du serveur. Configurez un **indice de récupération**, conservez les **codes de récupération 2FA** hors ligne, et envisagez l’accès d’urgence pour désigner un contact de confiance. C’est la contrepartie d’un chiffrement réellement étanche.

### Related Coverage

## Conclusion : un coffre-fort souverain en moins d’une heure

En douze étapes, vous disposez d’un serveur **Vaultwarden** de production : déployé via Docker Compose, sécurisé par un reverse proxy Caddy en HTTPS automatique, verrouillé contre les inscriptions sauvages, protégé par fail2ban et sauvegardé de manière chiffrée. Vous bénéficiez gratuitement de l’équivalent d’une formule entreprise Bitwarden – TOTP, organisations, clés matérielles, accès d’urgence – tout en gardant vos secrets sur une infrastructure que vous maîtrisez, idéalement en Europe.

La responsabilité opérationnelle est le prix de cette liberté : tenez vos sauvegardes à jour, testez vos restaurations, suivez les notes de version et appliquez les correctifs de sécurité. Pour durcir davantage votre infrastructure, parcourez notre section Cybersécurité et envisagez de placer votre coffre derrière un VPN privé. Auto-héberger son gestionnaire de mots de passe n’a jamais été aussi accessible – ni aussi pertinent à l’heure de la souveraineté numérique.