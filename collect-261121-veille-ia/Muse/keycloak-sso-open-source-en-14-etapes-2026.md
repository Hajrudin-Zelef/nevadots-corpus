---
id: collect-261121-veille-ia/muse/keycloak-sso-open-source-en-14-etapes-2026
title: "Keycloak : SSO Open Source en 14 Étapes [2026]"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-08-31"]
keywords: ["keycloak", "version", "vous", "elle", "projet", "etapes", "equipes", "publiee"]
source: https://tech-insider.org/fr/keycloak-tutoriel-sso-open-source-2026/
source_anchor: ""
source_lines: [1, 432]
section: "Muse"
---
Les identifiants volés restent la porte d’entrée numéro un des cyberattaques en 2026. Après la fuite chez l’Agence nationale des titres sécurisés, qui a exposé 11,7 millions de comptes, une question revient sans cesse dans les équipes techniques : qui contrôle vraiment les accès à vos applications, et sur quels serveurs vivent ces identifiants ? Keycloak, la plateforme de gestion des identités et des accès (IAM) portée par la Fondation Eclipse, répond directement à cette question. Elle est gratuite, publiée sous licence Apache 2.0, et elle tourne sur votre propre infrastructure plutôt que sur les serveurs d’un fournisseur tiers.

Keycloak centralise la connexion unique (SSO), l’authentification multifacteur, la fédération d’annuaires LDAP et Active Directory, ainsi que les protocoles OIDC, OAuth 2.0 et SAML 2.0. La version stable actuelle, la 26.7.3, est listée comme la plus récente par endoflife.date au 31 août 2026 ; elle succède à la 26.7.0 du 9 juillet 2026, qui avait introduit un aperçu de l’API SCIM, la haute disponibilité multi-cluster v2 et l’authentification SAML step-up d’après Skycloak, elle-même précédée par la 26.6.3 du 4 juin 2026 selon le service de suivi de versions VeRSSion, le tout tournant sur OpenJDK 21 en conteneur. Le jalon GitHub du projet visait initialement la version majeure 27.0.0 pour le 31 mars 2026, mais l’équipe a préféré continuer d’itérer sur la branche 26.x plutôt que de précipiter une rupture de compatibilité. Rester sur une branche ancienne coûte cher en sécurité : la série 26.0, marquée comme terminée par endoflife.date dès le 15 janvier 2025 (dernier correctif 26.0.8 daté du 13 janvier 2025), est concernée par la CVE-2025-11538 publiée en décembre 2025 par GitHub Security Advisory pour toute version antérieure à 26.4.4, et Red Hat a dû publier en 2025 deux avis distincts, RHSA-2025:19925 pour la RHBK 26.0.17 et RHSA-2025:12016 listant sept CVE. C’est le projet qui sert de socle au Red Hat Build of Keycloak (RHBK), la version commerciale supportée par Red Hat, ce qui donne une idée du sérieux de sa base de code en production — et de l’intérêt de suivre le rythme des mises à jour plutôt que de rester figé sur une version ancienne.

Ce tutoriel vous fait construire, de zéro, un déploiement Keycloak complet avec Docker Compose : base PostgreSQL, royaume dédié, client OIDC, utilisateurs et rôles, authentification multifacteur avec passkeys, fédération LDAP, reverse proxy TLS et bascule en mode production. Quatorze étapes, un peu moins d’une heure, et un projet fonctionnel que vous pourrez brancher sur une vraie application à la fin.

Ce guide s’adresse aux développeurs backend, aux administrateurs système et aux équipes DevOps qui gèrent déjà Docker au quotidien mais n’ont jamais mis les mains dans un serveur d’identité. Aucune connaissance préalable de Keycloak n’est nécessaire : chaque commande est expliquée, chaque piège est signalé avant qu’il ne vous morde, et chaque étape part du principe que vous partez d’une machine vierge. Si vous gérez déjà un cluster Kubernetes en production, les principes restent identiques, seul le mode de déploiement change.

## Pourquoi Keycloak s’impose en 2026

Trois forces poussent les équipes techniques vers l’auto-hébergement de leur IAM cette année. D’abord la directive européenne NIS2, dont l’application s’est durcie en 2025 et 2026 : elle pousse les opérateurs essentiels et importants à démontrer qu’ils maîtrisent leur chaîne d’authentification, pas seulement à la sous-traiter. Le Cyber Resilience Act, avec ses amendes pouvant atteindre 15 millions d’euros, ajoute une pression réglementaire similaire côté produits connectés. Ensuite, la généralisation des passkeys et de WebAuthn change la manière dont les utilisateurs s’attendent à se connecter, sans mot de passe, avec une clé biométrique ou matérielle. Keycloak a mûri son support de ces standards au fil des versions 22 à 26 : la 26.6.0, publiée le 8 avril 2026 d’après le dépôt GitHub du projet, a introduit à la fois le JWT Authorization Grant (RFC 7523) et un moteur de workflows, avant que la branche ne reçoive sa quatrième mise à jour mineure, la 26.6.4 du 26 juin 2026 rapportée par OpenStandia, preuve d’un rythme de maintenance soutenu sur cette seule lignée. Enfin, la souveraineté des données : héberger son propre serveur d’identité en Europe évite de faire transiter les données personnelles de connexion par des clouds soumis à des juridictions étrangères.

Sur le plan financier, l’argument est tout aussi direct. Keycloak ne facture rien par utilisateur : vous ne payez que l’infrastructure. Face à des solutions SaaS dont la facture grimpe avec chaque compte créé, ce modèle change complètement l’équation pour une startup qui passe de 500 à 50 000 utilisateurs.

Il y a aussi un changement de philosophie derrière cette bascule. Dans une architecture Zero Trust, l’identité remplace le pare-feu périmétrique comme véritable frontière de sécurité : chaque requête, qu’elle vienne du bureau ou d’un café à l’autre bout du pays, doit prouver qui elle est avant d’accéder à quoi que ce soit. Un serveur d’authentification que vous maîtrisez de bout en bout, avec ses journaux, ses règles de force brute et sa politique de sessions entièrement sous votre contrôle, s’intègre nettement mieux dans ce modèle qu’un service tiers où vous ne voyez qu’un tableau de bord.

## Prérequis : versions et matériel nécessaires

Avant de lancer la première commande, vérifiez que votre environnement correspond à ces exigences. Rien d’exotique : un serveur Linux standard ou une machine de développement suffisent pour suivre ce tutoriel de bout en bout.

| Composant | Version ou exigence minimale | 
|---|---|
| Docker Engine | 25.0 ou supérieur | 
| Docker Compose | v2.20 ou supérieur (plugin intégré à Docker) | 
| Image Keycloak | quay.io/keycloak/keycloak:26.7.0 | 
| Java embarqué dans l’image | OpenJDK 21 (conteneur), OpenJDK 25 en mode natif | 
| Base de données | PostgreSQL 15 ou supérieur (recommandée par le projet) | 
| RAM | 1 Go minimum, 2 Go ou plus en production | 
| CPU | 2 vCPU recommandés | 
| Espace disque | 5 Go minimum pour les images et les volumes | 
| Nom de domaine + DNS | requis pour le mode production et le TLS | 
| Certificat TLS | Let’s Encrypt via Traefik, ou certificat interne | 

Vérifiez vos versions installées avec deux commandes rapides avant de continuer :

```
docker --version
docker compose version
```
Si l’une des deux commandes échoue ou affiche une version trop ancienne, mettez à jour Docker avant d’aller plus loin. Les variables d’environnement Keycloak utilisées dans ce tutoriel (KC_BOOTSTRAP_ADMIN_USERNAME notamment) n’existent qu’à partir de la branche 26, une image plus ancienne ne fonctionnera pas de la même façon.

## Étape 1 : Préparer l’arborescence du projet

Créez un dossier de travail avec une structure claire. Elle sépare la configuration, les volumes de données et les certificats, ce qui facilite les sauvegardes et évite de mélanger les données persistantes avec le code de configuration versionné dans git.

```
mkdir -p keycloak-stack/{data/postgres,data/keycloak,certs}
cd keycloak-stack
touch docker-compose.yml .env
git init
```
Le dossier `data/postgres` contiendra les fichiers de la base, `data/keycloak` stockera les exports de royaume et les thèmes personnalisés, et `certs` accueillera vos certificats si vous ne passez pas par un reverse proxy pour le TLS. Ajoutez un fichier `.gitignore` qui exclut `data/` et `.env` avant de committer quoi que ce soit : ces dossiers contiennent des données sensibles et des identifiants.

## Étape 2 : Configurer les variables d’environnement

Le fichier `.env` centralise tous les secrets et paramètres que docker-compose injectera dans les conteneurs. Générez des mots de passe robustes, pas les valeurs d’exemple ci-dessous.

```
# Base de données
POSTGRES_DB=keycloak
POSTGRES_USER=keycloak
POSTGRES_PASSWORD=changez-ce-mot-de-passe-maintenant
# Administration Keycloak (remplace l'ancien KEYCLOAK_ADMIN, deprecie depuis la v26)
KC_ADMIN_USER=admin
KC_ADMIN_PASSWORD=un-autre-mot-de-passe-solide
# Nom d'hote public utilise en production
KC_HOSTNAME=auth.votre-domaine.fr
```
C’est le premier piège classique du déploiement Keycloak : les tutoriels rédigés avant fin 2024 utilisent encore les variables `KEYCLOAK_ADMIN` et `KEYCLOAK_ADMIN_PASSWORD`. Elles fonctionnent toujours en 26.7.1 mais affichent un avertissement de dépréciation dans les journaux. Les variables recommandées aujourd’hui sont `KC_BOOTSTRAP_ADMIN_USERNAME` et `KC_BOOTSTRAP_ADMIN_PASSWORD`, utilisées dans le fichier docker-compose ci-dessous. Gardez aussi un œil sur le rythme des correctifs de sécurité : la 26.5.7, publiée en avril 2026, corrige à elle seule sept CVE d’après le projet Keycloak, et Clever Cloud a répercuté cette même mise à jour sur son offre Keycloak hébergée dès avril 2026, signe que le projet publie des correctifs à un rythme soutenu qu’il vaut mieux suivre plutôt qu’ignorer. Même les bibliothèques clientes suivent ce tempo : la version 26.0.10 a été annoncée le 30 juin 2026 par Peter Skopek sur le groupe Google Keycloak Dev, et la 26.0.12 figure comme version des sources téléchargeables sur la page Downloads officielle de keycloak.org au 1er septembre 2026 — un rappel qu’il vaut mieux épingler une version précise plutôt que suivre une branche à l’aveugle dans vos dépendances Maven ou npm.

## Étape 3 : Écrire le fichier docker-compose.yml

Voici la configuration complète pour démarrer Keycloak en mode développement, adossé à PostgreSQL plutôt qu’à la base H2 embarquée par défaut. Utiliser PostgreSQL dès le premier test évite d’avoir à migrer une base plus tard.

```
services:
  postgres:
    image: postgres:16
    container_name: keycloak-postgres
    restart: unless-stopped
    environment:
      POSTGRES_DB: ${POSTGRES_DB}
      POSTGRES_USER: ${POSTGRES_USER}
      POSTGRES_PASSWORD: ${POSTGRES_PASSWORD}
    volumes:
      - ./data/postgres:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U ${POSTGRES_USER}"]
      interval: 10s
      timeout: 5s
      retries: 5
    networks:
      - keycloak-net
  keycloak:
    image: quay.io/keycloak/keycloak:26.7.0
    container_name: keycloak
    command: start-dev
    restart: unless-stopped
    depends_on:
      postgres:
        condition: service_healthy
    environment:
      KC_DB: postgres
      KC_DB_URL: jdbc:postgresql://postgres:5432/${POSTGRES_DB}
      KC_DB_USERNAME: ${POSTGRES_USER}
      KC_DB_PASSWORD: ${POSTGRES_PASSWORD}
      KC_BOOTSTRAP_ADMIN_USERNAME: ${KC_ADMIN_USER}
      KC_BOOTSTRAP_ADMIN_PASSWORD: ${KC_ADMIN_PASSWORD}
      KC_METRICS_ENABLED: "true"
      KC_HEALTH_ENABLED: "true"
    ports:
      - "8080:8080"
      - "9000:9000"
    volumes:
      - ./data/keycloak:/opt/keycloak/data
    networks:
      - keycloak-net
networks:
  keycloak-net:
    driver: bridge
```
Le port 9000 mérite une explication : depuis la branche 26, Keycloak sépare son interface de gestion (santé, métriques Prometheus) du port applicatif 8080. C’est une bonne pratique de sécurité, elle évite d’exposer les endpoints de supervision sur la même interface que le trafic public. Vous ne l’ouvrirez d’ailleurs jamais publiquement en production, uniquement à votre système de monitoring interne.

## Étape 4 : Démarrer les conteneurs et vérifier les journaux

Lancez la stack en arrière-plan, puis suivez les journaux de Keycloak pour confirmer que le démarrage se termine sans erreur.

```
docker compose up -d
docker compose logs -f keycloak
```
Un démarrage réussi affiche une sortie proche de celle-ci, avec un temps de démarrage généralement compris entre 5 et 15 secondes sur une machine de développement :

```
keycloak  | Keycloak 26.7.0 on JVM (powered by Quarkus 3.x) started in 8.412s
keycloak  | Listening on: http://0.0.0.0:8080
keycloak  | Management interface listening on http://0.0.0.0:9000
keycloak  | Profile prod activated.
keycloak  | Config source PropertiesConfigSource loaded
```
Si le conteneur redémarre en boucle à la place, la cause la plus fréquente est une base PostgreSQL pas encore prête. Le `healthcheck` et la condition `service_healthy` définis à l’étape précédente sont justement là pour éviter ce cas, mais vérifiez tout de même les journaux du service postgres avec `docker compose logs postgres` si le problème persiste.

## Étape 5 : Première connexion à la console d’administration

Ouvrez `http://localhost:8080/admin/master/console/` dans votre navigateur. Connectez-vous avec les identifiants définis dans `KC_ADMIN_USER` et `KC_ADMIN_PASSWORD`. Vous atterrissez dans le royaume `master`, celui qui gère Keycloak lui-même : ne créez jamais vos utilisateurs applicatifs ici, il sert uniquement à l’administration de la plateforme.

Première chose à faire une fois connecté : si vous comptez exposer cette instance au-delà de votre machine locale, changez immédiatement le mot de passe administrateur depuis l’interface, même s’il provient d’un fichier `.env` que vous jugez robuste. Un compte admin par défaut est la cible numéro un des scans automatisés dès qu’un service Keycloak est détecté sur un port public.

## Étape 6 : Créer un royaume dédié à votre application

Un royaume (realm) est un espace isolé : ses utilisateurs, ses rôles et sa configuration ne se mélangent jamais avec un autre royaume. C’est la brique qui permet, par exemple, de séparer proprement un environnement de recette et un environnement de production sur la même instance Keycloak, ou d’isoler chaque client si vous construisez une plateforme multi-tenant — pour ce dernier cas, la fonctionnalité Organizations, passée en support complet dès la version 26.0.0 d’octobre 2024 et dont le statut a été reconfirmé par la documentation Keycloak en janvier 2025, offre désormais une alternative plus légère à la multiplication des royaumes. Depuis le menu déroulant en haut à gauche de la console, cliquez sur **Create realm**, nommez-le par exemple `entreprise-prod`, puis validez.

Vous pouvez aussi automatiser cette étape avec `kcadm.sh`, le client en ligne de commande embarqué dans l’image Keycloak, particulièrement utile si vous scriptez un déploiement reproductible :

```
docker exec -it keycloak /opt/keycloak/bin/kcadm.sh config credentials \
  --server http://localhost:8080 --realm master \
  --user admin --password 'un-autre-mot-de-passe-solide'
docker exec -it keycloak /opt/keycloak/bin/kcadm.sh create realms \
  -s realm=entreprise-prod -s enabled=true
```
## Étape 7 : Durcir la politique de sécurité du royaume

Avant de créer le moindre utilisateur, réglez la politique de mots de passe et la protection contre les attaques par force brute. Dans **Realm settings → Security defenses**, activez la détection de force brute et fixez un seuil raisonnable, par exemple un verrouillage temporaire après 5 tentatives échouées. Dans **Authentication → Policies**, définissez une longueur minimale de mot de passe d’au moins 12 caractères et exigez au moins un chiffre et une casse mixte.

Réglez également la durée de vie des sessions dans **Realm settings → Sessions**. La valeur par défaut convient pour une démonstration, mais en production, un timeout d’inactivité de 30 minutes et une durée de vie maximale de session de 10 heures constituent un point de départ raisonnable pour une application interne d’entreprise.

## Étape 8 : Créer un client OIDC et configurer les URI de redirection

Un client représente une application qui va déléguer son authentification à Keycloak. Dans **Clients → Create client**, choisissez le protocole `openid-connect`, donnez-lui un identifiant comme `mon-app-web`, puis passez à l’écran de configuration des flux.

### Client confidentiel ou client public

Un client confidentiel possède un secret partagé et convient à une application côté serveur, qui peut garder ce secret hors de portée du navigateur. Un client public, sans secret, s’utilise pour une application mobile ou un frontend JavaScript pur, en s’appuyant sur PKCE (Proof Key for Code Exchange) pour sécuriser l’échange malgré l’absence de secret. Pour une application web classique avec un backend, choisissez confidentiel. Pour une SPA React ou Vue sans backend dédié, choisissez public avec PKCE activé.

Renseignez ensuite les **Valid redirect URIs**, par exemple `https://mon-app.exemple.fr/callback`. C’est le champ le plus souvent mal configuré du tutoriel : une erreur de casse, un port manquant ou une barre oblique finale absente provoquent immédiatement une erreur au moment du login. Gardez la valeur strictement identique à celle utilisée par votre application.

## Étape 9 : Créer des utilisateurs, groupes et rôles

Dans **Users → Add user**, créez un premier compte de test avec une adresse e-mail et un nom d’utilisateur. Dans l’onglet **Credentials**, définissez un mot de passe temporaire en cochant **Temporary**, ce qui forcera l’utilisateur à le changer à sa première connexion.

Créez ensuite au moins deux rôles dans **Realm roles**, par exemple `utilisateur` et `administrateur`, puis assignez-les depuis l’onglet **Role mapping** du profil utilisateur. Pour un scénario multi-équipes, régler des groupes dans **Groups** plutôt que d’assigner les rôles individuellement facilite grandement la maintenance à mesure que le nombre de comptes augmente. Le même résultat en ligne de commande :

```
docker exec -it keycloak /opt/keycloak/bin/kcadm.sh create users \
  -r entreprise-prod \
  -s username=test.utilisateur \
  -s [email protected] \
  -s enabled=true
```
## Étape 10 : Activer le MFA, WebAuthn et les passkeys

Dans **Authentication → Required actions**, activez **Configure OTP** pour le TOTP classique (Google Authenticator, Aegis, ou équivalent), et **Webauthn Register Passwordless** pour les passkeys. Vous pouvez rendre l’une de ces actions obligatoire pour tous les nouveaux comptes en cochant **Set as default action**.

### WebAuthn et passkeys, la vraie nouveauté de ce cycle de versions

Le support des passkeys s’est nettement stabilisé depuis la version 22 et arrive à maturité dans la branche 26. Concrètement, un utilisateur peut désormais se connecter avec Face ID, Windows Hello ou une clé de sécurité physique, sans jamais saisir de mot de passe. Réglez la politique WebAuthn dans **Authentication → Policies → WebAuthn Passwordless Policy** : signature algorithm ES256, attestation non requise pour un usage interne, et `discoverable credentials` réglé sur `required` si vous voulez une connexion sans même saisir de nom d’utilisateur.

## Étape 11 : Connecter un annuaire LDAP ou Active Directory

Si votre organisation dispose déjà d’un annuaire, inutile de recréer tous les comptes à la main, et c’est justement l’un des cas d’usage où Keycloak brille par rapport à une solution SaaS qui exige souvent un connecteur payant pour la fédération LDAP. Dans **User federation → Add provider → ldap**, renseignez l’URL de connexion (`ldaps://votre-serveur:636`), le DN de liaison, le mot de passe du compte de service et le DN de base de recherche des utilisateurs. Réglez **Edit mode** sur `READ_ONLY` si Keycloak ne doit jamais écrire dans votre annuaire existant, ce qui est le choix le plus sûr pour une première intégration. Réservez le mode `WRITABLE` aux cas où vous voulez explicitement gérer la création de comptes depuis Keycloak plutôt que depuis votre annuaire d’origine.

Lancez une synchronisation manuelle avec le bouton **Sync all users** pour valider la configuration avant de programmer une synchronisation périodique. Keycloak ne remplace pas Active Directory : il vient s’y brancher pour offrir le SSO et les protocoles modernes (OIDC, SAML) à des applications qui ne savent pas parler LDAP nativement.

## Étape 12 : Déployer un reverse proxy TLS avec Traefik

Keycloak ne doit jamais servir de trafic HTTP en clair sur un domaine public. Les jetons d’accès, les cookies de session et les mots de passe transiteraient alors en clair sur le réseau, ce qui annule d’un coup tout le travail fait sur le MFA et la politique de mots de passe des étapes précédentes. Ajoutez Traefik à votre docker-compose.yml pour gérer automatiquement le certificat Let’s Encrypt et rediriger tout le trafic vers HTTPS. Nginx ou Caddy fonctionnent tout aussi bien si votre équipe les maîtrise déjà mieux.

```
  traefik:
    image: traefik:v3.2
    container_name: traefik
    restart: unless-stopped
    command:
      - "--providers.docker=true"
      - "--providers.docker.exposedbydefault=false"
      - "--entrypoints.websecure.address=:443"
      - "--certificatesresolvers.le.acme.httpchallenge=true"
      - "--certificatesresolvers.le.acme.httpchallenge.entrypoint=web"
      - "[email protected]"
      - "--certificatesresolvers.le.acme.storage=/certs/acme.json"
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./certs:/certs
    networks:
      - keycloak-net
    labels:
      - "traefik.http.routers.keycloak.rule=Host(`auth.votre-domaine.fr`)"
      - "traefik.http.routers.keycloak.tls.certresolver=le"
      - "traefik.http.services.keycloak.loadbalancer.server.port=8080"
```
Ajoutez ensuite deux variables à la section `environment` du service `keycloak` : `KC_PROXY_HEADERS=xforwarded` et `KC_HOSTNAME=${KC_HOSTNAME}`. Sans ces deux réglages, Keycloak continue de générer des liens en `http://` et sur le port interne 8080 même derrière un proxy HTTPS, ce qui casse silencieusement le flux de connexion pour vos utilisateurs.

## Étape 13 : Tester le flux d’authentification OIDC de bout en bout

Avant de brancher une vraie application, validez le flux directement en ligne de commande. Récupérez un jeton d’accès avec le grant `password`, pratique pour tester mais à réserver aux environnements de développement :

```
curl -X POST \
  "https://auth.votre-domaine.fr/realms/entreprise-prod/protocol/openid-connect/token" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "client_id=mon-app-web" \
  -d "client_secret=le-secret-du-client" \
  -d "grant_type=password" \
  -d "username=test.utilisateur" \
  -d "password=le-mot-de-passe"
```
Une réponse réussie ressemble à ceci (jeton tronqué par souci de lisibilité) :

```
{
  "access_token": "eyJhbGciOiJSUzI1NiIsInR5cCIgOiAi...",
  "expires_in": 300,
  "refresh_expires_in": 1800,
  "refresh_token": "eyJhbGciOiJIUzI1NiIsInR5cCIgOi...",
  "token_type": "Bearer",
  "not-before-policy": 0,
  "scope": "openid email profile"
}
```
Côté application, la validation d’un jeton se fait en vérifiant sa signature contre les clés publiques exposées par Keycloak, sans jamais interroger le serveur à chaque requête. Un exemple minimal en Python avec la bibliothèque `PyJWT` :

```
import jwt
from jwt import PyJWKClient
realm_url = "https://auth.votre-domaine.fr/realms/entreprise-prod"
jwks_client = PyJWKClient(f"{realm_url}/protocol/openid-connect/certs")
def verifier_jeton(token: str):
    signing_key = jwks_client.get_signing_key_from_jwt(token)
    return jwt.decode(
        token,
        signing_key.key,
        algorithms=["RS256"],
        audience="mon-app-web",
        issuer=realm_url,
    )
```
Si `verifier_jeton` renvoie le contenu décodé sans lever d’exception, votre flux OIDC fonctionne de bout en bout, de la connexion de l’utilisateur jusqu’à la validation côté application.

## Étape 14 : Sauvegarder, exporter et passer en production

Une fois la configuration validée, exportez le royaume complet pour pouvoir le reconstruire ailleurs de façon identique, un principe d’infrastructure as code appliqué à l’IAM :

```
docker exec -it keycloak /opt/keycloak/bin/kc.sh export \
  --dir /opt/keycloak/data/export \
  --realm entreprise-prod \
  --users realm_file
```
Programmez aussi une sauvegarde régulière de la base PostgreSQL, la vraie source de vérité de votre déploiement :

```
docker exec keycloak-postgres pg_dump -U keycloak keycloak \
  | gzip > backup-keycloak-$(date +%F).sql.gz
```
Pour la mise en production, remplacez la commande `start-dev` par un vrai build optimisé. Créez un Dockerfile minimal qui exécute `kc build` avec les fonctionnalités dont vous avez réellement besoin, puis démarrez avec `start --optimized` plutôt qu’en mode développement, qui désactive plusieurs protections et expose des endpoints de debug à ne jamais laisser accessibles publiquement.

## Aller plus loin : protéger une API avec l’introspection de jeton

Une fois le SSO en place pour vos interfaces web, la question suivante arrive presque toujours : comment un microservice backend vérifie-t-il qu’un jeton présenté par un appelant est encore valide ? Deux approches coexistent. La première, déjà couverte à l’étape 13, valide la signature du jeton localement via les clés publiques JWKS, sans appeler Keycloak à chaque requête : c’est la méthode la plus rapide et la plus scalable. La seconde, l’introspection, interroge Keycloak en direct pour chaque jeton, ce qui coûte une requête réseau supplémentaire mais permet de révoquer un accès immédiatement, même avant l’expiration naturelle du jeton.

L’introspection convient particulièrement aux API qui manipulent des données sensibles, où le délai entre une révocation d’accès et sa prise en compte doit être quasi nul. Depuis janvier 2026, Keycloak documente d’ailleurs officiellement ce même mécanisme pour un cas d’usage émergent : servir de serveur d’autorisation pour des serveurs MCP (Model Context Protocol), la même logique de jetons et de scopes s’appliquant désormais aux agents IA qui appellent vos API. Voici l’appel type, à exécuter depuis votre service backend plutôt que depuis le navigateur :

```
curl -X POST \
  "https://auth.votre-domaine.fr/realms/entreprise-prod/protocol/openid-connect/token/introspect" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -u "mon-app-api:le-secret-du-client-api" \
  -d "token=eyJhbGciOiJSUzI1NiIsInR5cCIgOiAi..."
```
Keycloak répond avec un objet JSON qui indique si le jeton est actif, ses scopes et son propriétaire :

```
{
  "active": true,
  "scope": "openid email profile",
  "client_id": "mon-app-api",
  "username": "test.utilisateur",
  "exp": 1752345600,
  "realm_access": {
    "roles": ["utilisateur"]
  }
}
```
Si `active` vaut `false`, rejetez la requête avec un code 401, quelle que soit la raison (jeton expiré, révoqué ou signé par un autre royaume). Pour une API à fort trafic, réservez l’introspection aux endpoints les plus sensibles et gardez la validation locale JWKS pour le reste : le compromis entre latence et réactivité de révocation se règle service par service, pas au niveau de toute l’architecture.

## Keycloak face à Auth0, Okta et Authentik : le comparatif des coûts

La question du prix revient systématiquement dès qu’une équipe évalue Keycloak face à une alternative SaaS. Les chiffres ci-dessous correspondent aux tarifs publics affichés par les éditeurs, en dollars, à titre indicatif : ils évoluent régulièrement et méritent d’être vérifiés au moment de votre décision.

| Solution | Licence | Hébergement | Tarif indicatif | 
|---|---|---|---|
| Keycloak | Apache 2.0 (open source) | Auto-hébergé | Gratuit, coût d’infrastructure uniquement | 
| Red Hat build of Keycloak | Apache 2.0 + support commercial | Auto-hébergé | Abonnement de support sur devis | 
| Auth0 (Okta) | Propriétaire | SaaS | Gratuit en développement, puis environ 25 à 50 $ par utilisateur actif et par mois en entreprise | 
| Okta Identity Engine | Propriétaire | SaaS | Environ 6 à 10 $ par utilisateur et par mois, jusqu’à 20 $ ou plus pour les options avancées | 
| Authentik | Open source, ou SaaS géré | Auto-hébergé ou SaaS | Gratuit en auto-hébergement, environ 15 $ par utilisateur et par mois en SaaS | 

Le calcul penche presque toujours en faveur de Keycloak dès que le nombre de comptes dépasse quelques centaines, à condition d’avoir en interne les compétences pour l’exploiter. C’est justement le compromis : Auth0 et Okta vendent de la tranquillité d’esprit et un support contractuel, Keycloak vend la maîtrise complète de la pile au prix du temps d’administration système. Une équipe de deux ou trois développeurs sans administrateur système dédié mettra probablement plus de temps à durcir correctement son instance Keycloak que ne coûterait un abonnement Auth0 pendant la même période, ce qui remet le calcul en perspective : le logiciel est gratuit, l’exploitation ne l’est jamais complètement.

Authentik occupe une position intermédiaire intéressante pour les équipes qui veulent l’esprit open source de Keycloak avec une interface jugée plus moderne par certains administrateurs, au prix d’un écosystème et d’une communauté nettement plus restreints. Le choix final dépend surtout de la taille de votre parc applicatif et de votre tolérance à opérer vous-même un service critique.

## Les erreurs les plus fréquentes lors du déploiement de Keycloak

Sept erreurs reviennent systématiquement dans les déploiements Keycloak, y compris chez des équipes expérimentées qui découvrent l’outil. Aucune n’est propre à Keycloak en particulier, ce sont globalement les mêmes pièges que pour n’importe quel service critique déployé en conteneur, mais leurs conséquences sont plus visibles ici puisqu’un incident touche immédiatement la capacité de tous vos utilisateurs à se connecter.

- **Garder la base H2 embarquée en production.** Elle est parfaite pour un test de cinq minutes, mais elle ne supporte ni la montée en charge ni des sauvegardes fiables. Basculez sur PostgreSQL dès le premier déploiement réel.
- **Laisser les identifiants administrateur par défaut.** Un compte admin avec un mot de passe prévisible est la première cible des scans automatisés dès qu’un service Keycloak répond sur un port exposé.
- **Mal configurer les URI de redirection du client.** Une casse différente, un port manquant ou une barre oblique finale absente provoquent une erreur immédiate au moment du login.
- **Oublier KC_PROXY_HEADERS et KC_HOSTNAME derrière un reverse proxy.** Sans ces réglages, Keycloak continue de générer des liens en HTTP interne, ce qui casse le flux de connexion pour l’utilisateur final.
- **Laisser start-dev tourner en production.** Ce mode désactive plusieurs protections et n’est pas conçu pour encaisser du trafic public.
- **Ne jamais exporter le royaume avant une mise à jour majeure.** Un export récent transforme une mise à jour ratée en simple restauration plutôt qu’en incident majeur.
- **Distribuer le rôle d’administration du royaume trop largement.** Réservez les rôles realm-management aux comptes qui en ont réellement besoin, et documentez qui les détient.

## Dépannage : 10 problèmes courants et leurs solutions

Voici les symptômes les plus fréquemment rencontrés en suivant ce type de déploiement, avec leur cause probable et la correction à appliquer. Gardez cette table sous la main lors de votre première mise en production, la majorité des tickets de support liés à Keycloak se résument à l’un de ces dix cas.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| “Invalid parameter: redirect_uri” | L’URI de redirection du client ne correspond pas exactement à celle enregistrée | Vérifiez la casse, le port et la barre oblique finale dans la configuration du client | 
| Erreur 404 “Realm does not exist” | Le nom du royaume dans l’URL ne correspond à aucun royaume créé | Vérifiez l’orthographe exacte du royaume, sensible à la casse | 
| Boucle de redirection infinie après connexion | KC_HOSTNAME ou KC_PROXY_HEADERS mal configuré derrière le reverse proxy | Définissez explicitement KC_HOSTNAME et activez KC_PROXY_HEADERS=xforwarded | 
| Le conteneur Keycloak redémarre en boucle | La base PostgreSQL n’est pas encore prête au démarrage | Ajoutez une condition depends_on avec healthcheck sur le service postgres | 
| “Failed to obtain JDBC connection” | Identifiants incorrects ou base de données non initialisée | Vérifiez KC_DB_URL, KC_DB_USERNAME, KC_DB_PASSWORD et l’état du conteneur postgres | 
| Console admin inaccessible après passage en mode start | Le hostname strict bloque les accès sur un autre domaine que celui configuré | Vérifiez KC_HOSTNAME et utilisez le nom de domaine exact dans l’URL | 
| “Client not found” lors de l’appel au token endpoint | Le client_id est mal orthographié ou appartient à un autre royaume | Vérifiez le royaume dans l’URL et l’exactitude du client_id | 
| Codes OTP toujours rejetés | Décalage d’horloge entre le serveur et l’appareil de l’utilisateur | Synchronisez l’horloge du serveur via NTP | 
| L’import du royaume JSON échoue silencieusement | Le fichier exporté référence des identifiants qui n’existent plus sur la cible | Réexportez avec kc.sh export –realm et réimportez sur une base propre | 
| Le endpoint /metrics renvoie une erreur 404 | L’interface de gestion n’est pas exposée sur le port 9000 | Vérifiez que le port 9000 est publié et que KC_METRICS_ENABLED=true est défini | 

## Conseils avancés pour un déploiement en production

Une fois le socle stable, plusieurs réglages font la différence entre une instance qui tient une charge de test et une instance qui encaisse le trafic réel d’une entreprise. Ces réglages ne sont pas nécessaires le premier jour, mais mieux vaut les connaître avant qu’un pic de trafic ne les impose dans l’urgence.

### Cache distribué et haute disponibilité

Pour faire tourner plusieurs nœuds Keycloak derrière un répartiteur de charge, activez le cache distribué Infinispan en mode cluster plutôt que le cache local par défaut. Chaque nœud partage alors les sessions actives, ce qui évite qu’un utilisateur soit déconnecté simplement parce que sa requête suivante atterrit sur un autre nœud. Depuis la version 26.5.0 de janvier 2026, Keycloak expose également un support natif d’OpenTelemetry pour les métriques et les journaux, ce qui facilite son intégration dans une chaîne d’observabilité existante sans dépendre uniquement du endpoint Prometheus du port 9000. C’est le seuil à partir duquel il devient pertinent d’envisager l’Operator Keycloak officiel pour Kubernetes plutôt que Docker Compose.

Au-delà de la haute disponibilité, quatre autres réglages valent le détour pour une instance qui tourne en continu :

- **Supervision Prometheus.** Le endpoint /metrics du port 9000 s’intègre directement dans un tableau de bord Grafana pour suivre le taux d’échec de connexion et le temps de réponse du token endpoint.
- **Thèmes personnalisés.** Un thème custom déposé dans /opt/keycloak/themes permet d’habiller l’écran de connexion aux couleurs de votre marque, sans toucher au cœur de Keycloak.
- **Extensions SPI.** Les Service Provider Interfaces en Java permettent d’ajouter une règle métier, par exemple une validation de mot de passe spécifique à votre politique interne.
- **Limitation de débit en amont.** Un rate limiting configuré au niveau de Traefik ou d’un WAF freine une tentative de force brute avant même qu’elle n’atteigne Keycloak, en complément de la protection native du royaume.

## Le projet complet : ce que vous venez de construire

À ce stade, votre dossier `keycloak-stack` contient une stack Docker Compose à trois services (PostgreSQL, Keycloak, Traefik), un fichier `.env` avec vos secrets, un export JSON du royaume reproductible sur une autre machine, et un script de sauvegarde de base de données prêt à être placé dans une tâche cron. Ce socle gère l’authentification centralisée, le MFA avec passkeys, la fédération LDAP optionnelle et le TLS automatique, sans dépendre d’un fournisseur tiers.

Pour aller plus loin, committez le docker-compose.yml et l’export du royaume dans un dépôt git (jamais le fichier .env), ce qui transforme votre configuration Keycloak en infrastructure as code que n’importe quel membre de l’équipe peut reconstruire à l’identique en quelques minutes.

## Foire aux questions

### Keycloak est-il vraiment gratuit ?

Oui. Keycloak est publié sous licence Apache 2.0 par la Fondation Eclipse, sans coût de licence ni limite de comptes utilisateurs. Vous ne payez que l’infrastructure sur laquelle vous l’hébergez : serveur, base de données et certificat TLS.

### Quelle est la différence entre Keycloak et le Red Hat Build of Keycloak ?

Le Red Hat Build of Keycloak (RHBK) est la distribution commerciale construite sur le projet open source. Le code reste le même, mais Red Hat ajoute un support contractuel, des correctifs certifiés et une compatibilité garantie avec ses propres produits, notamment OpenShift.

### Keycloak peut-il remplacer Active Directory ?

Pas totalement. Keycloak gère l’authentification et les accès applicatifs, tandis qu’Active Directory gère aussi les postes de travail, les stratégies de groupe (GPO) et l’annuaire réseau Windows au sens large. Dans la pratique, la plupart des entreprises connectent Keycloak à leur Active Directory existant via la fédération LDAP plutôt que de le remplacer entièrement : Keycloak devient la couche SSO moderne posée par-dessus un annuaire qui continue de gérer le reste du parc informatique.

### Combien d’utilisateurs Keycloak peut-il gérer ?

Le logiciel n’impose aucune limite fixe. Le nombre réel dépend surtout de votre base de données et de votre configuration de cache. Avec PostgreSQL correctement dimensionné et le cache distribué Infinispan activé, la communauté documente des déploiements de plusieurs centaines de milliers de comptes, une échelle cohérente avec les chiffres publiés par la CNCF en août 2026 : plus de 12 000 utilisateurs actifs du projet et plus de 2 millions d’utilisateurs à travers tout son écosystème. Le nombre de projets qui dépendent de Keycloak a lui aussi bondi de 68,6 %, passant de 4 947 à 8 342 projets recensés d’ici juin 2025 d’après les données Linux Foundation LFX Insights. L’enquête communautaire de mars 2026, qui a recueilli plus de 360 réponses, confirme cette dynamique d’adoption.

### Keycloak est-il conforme au RGPD et à la directive NIS2 ?

Keycloak est un outil, pas un service certifié : il ne délivre aucune conformité automatique. Mais l’auto-hébergement en Europe facilite le respect du RGPD, puisque les données d’identité restent sous votre contrôle direct, et répond à l’esprit de NIS2, qui pousse les opérateurs essentiels à maîtriser leur chaîne d’authentification plutôt qu’à la sous-traiter hors de l’UE.

### Faut-il Kubernetes pour faire tourner Keycloak en production ?

Non. Docker Compose, comme dans ce tutoriel, suffit largement pour une PME ou un projet interne. Kubernetes, via l’Operator Keycloak officiel, devient pertinent à partir du moment où vous avez besoin d’une haute disponibilité multi-nœuds avec bascule automatique.

### Comment migrer depuis Auth0 ou Okta vers Keycloak ?

La migration passe généralement par trois étapes : exporter les utilisateurs via l’API du fournisseur d’origine, les réimporter dans Keycloak via l’API Admin ou un script kcadm.sh, puis migrer progressivement les applications clientes vers les nouveaux endpoints OIDC. Le point le plus délicat concerne les mots de passe : la plupart des fournisseurs SaaS ne les exportent jamais en clair pour des raisons évidentes de sécurité, ce qui oblige généralement à forcer une réinitialisation groupée ou à s’appuyer sur un algorithme de hachage compatible si le fournisseur le documente. Prévoyez une période de coexistence des deux systèmes, avec les deux fournisseurs d’identité actifs en parallèle, pour éviter toute coupure de service pendant la bascule.

### Quelle est la meilleure base de données pour Keycloak en production ?

PostgreSQL est la base recommandée par le projet et la plus testée en production. MySQL, MariaDB, Oracle et Microsoft SQL Server sont également supportés. La base H2 embarquée livrée par défaut ne doit jamais servir au-delà d’un test local : elle ne supporte ni la montée en charge ni des sauvegardes fiables.

### À lire aussi

**Sources et documentation officielle :** Keycloak.org, documentation d’administration Keycloak, dépôt GitHub du projet, Fondation Eclipse, spécification OpenID Connect, RFC 7519 (JSON Web Token) et la directive NIS2 de la Commission européenne.
