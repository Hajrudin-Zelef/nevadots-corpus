---
id: collect-261121-veille-ia/windows-os/certbot-automatiser-le-https-en-13-etapes-2026
title: "Certbot : Automatiser le HTTPS en 13 Étapes [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["certbot", "certificat", "jours", "certificats", "votre", "lets", "encrypt", "sans"]
source: https://tech-insider.org/fr/certbot-automatiser-https-2026/
source_anchor: ""
source_lines: [1, 399]
section: "Windows_OS"
---
Le 15 mars 2026, une règle discrète votée par le CA/Browser Forum est entrée en vigueur : la durée de vie maximale d’un certificat TLS public est passée à 200 jours. Ce n’est que la première marche d’un calendrier qui va bouleverser la gestion du HTTPS pour des millions de sites : 100 jours en mars 2027, puis 47 jours en mars 2029. Chez Let’s Encrypt, la trajectoire est encore plus serrée, avec un profil court à 45 jours déjà ouvert en test depuis le 13 mai 2026. Gérer manuellement un renouvellement de certificat tous les 45 jours au lieu de tous les 90 jours, ou pire tous les ans comme le faisaient certaines équipes avec des certificats commerciaux, devient tout simplement intenable sans automatisation. C’est exactement le problème que résout Certbot, le client ACME développé par l’Electronic Frontier Foundation, aujourd’hui en version 5.8.0. Ce tutoriel détaille, en 13 étapes concrètes, comment installer Certbot, obtenir vos premiers certificats Let’s Encrypt, automatiser leur renouvellement, et préparer votre infrastructure à l’ère des certificats à durée de vie courte.

## Pourquoi l’automatisation TLS devient incontournable en 2026

Pendant longtemps, un certificat TLS commercial durait un an, parfois deux ou trois ans avant l’interdiction des certificats multi-annuels par les navigateurs. Cette époque touche à sa fin. Le ballot SC-081v3, adopté par le CA/Browser Forum, impose une réduction progressive et irréversible de la durée de validité maximale des certificats publiquement approuvés, ainsi que de la durée de réutilisation des validations de contrôle de domaine (DCV). Le calendrier est désormais public et gravé dans les exigences de référence du secteur.

Cette réduction n’est pas une lubie administrative. Elle répond à un vrai problème de sécurité : plus un certificat vit longtemps, plus la fenêtre d’exploitation d’une clé privée compromise, d’une mauvaise configuration ou d’un domaine détourné reste ouverte. Un certificat de 47 jours limite mécaniquement les dégâts en cas de fuite. Le revers de la médaille, c’est la charge opérationnelle : une équipe qui gérait deux ou trois renouvellements par an devra bientôt en gérer huit, sur chaque domaine, sans interruption de service. Sans automatisation fiable, cela se traduit par des pages d’erreur “votre connexion n’est pas privée” chez les visiteurs et des alertes de sécurité chez les clients professionnels.

Let’s Encrypt, l’autorité de certification gratuite et automatisée lancée en 2015, a anticipé ce virage. Sa page de statistiques officielle, mise à jour le 18 août 2026, publie en continu le nombre de certificats actifs et le volume de délivrance quotidienne, confirmant qu’elle reste l’infrastructure de référence pour le chiffrement automatisé du Web. C’est dans ce contexte que Certbot, son client ACME historique, prend toute son importance : il ne s’agit plus d’un confort, mais d’une brique d’infrastructure critique.

## Certbot et Let’s Encrypt : comment fonctionne le protocole ACME

Certbot est un client du protocole ACME (Automatic Certificate Management Environment), une norme ouverte qui permet à un serveur de prouver, sans intervention humaine, qu’il contrôle bien un nom de domaine, puis de recevoir automatiquement un certificat signé. Concrètement, le dialogue se déroule en trois temps. D’abord, votre serveur contacte l’autorité de certification (Let’s Encrypt dans la majorité des cas) et demande un certificat pour un ou plusieurs domaines. Ensuite, l’autorité lance un défi de validation : elle vous demande de déposer un fichier accessible publiquement sur votre serveur web (défi HTTP-01) ou d’ajouter un enregistrement TXT temporaire dans votre zone DNS (défi DNS-01). Enfin, une fois le défi validé, le certificat est émis et téléchargé automatiquement par Certbot, qui peut aussi reconfigurer votre serveur web pour l’utiliser immédiatement.

Cette mécanique entièrement scriptable est ce qui rend possible le renouvellement toutes les 45 ou 47 jours sans intervention manuelle. Let’s Encrypt travaille également sur l’extension ACME Renewal Information (ARI), qui permet à l’autorité de certification d’indiquer proactivement au client ACME la fenêtre de temps recommandée pour renouveler un certificat donné, plutôt que de laisser chaque client deviner. L’objectif est d’éviter les pics de renouvellement simultanés et de mieux répartir la charge sur l’infrastructure de l’autorité. Cette évolution s’inscrit dans une feuille de route plus large, incluant des certificats à durée de vie courte accessibles via des profils ACME, et même des certificats couvrant des adresses IP directement dans leurs noms alternatifs (SAN), une nouveauté distincte des certificats classiques de 90 jours pour noms de domaine.

## Prérequis techniques et versions nécessaires

Avant de commencer, assurez-vous de disposer des éléments suivants. Le tutoriel a été vérifié avec Certbot 5.8.0 (publié le 1er septembre 2026), mais reste valable pour toute version de la branche 5.x.

- Un serveur Linux avec accès root ou sudo : Ubuntu 24.04 LTS, Debian 12/13, ou Rocky Linux 9 sont couverts nativement par les paquets officiels
- Certbot 5.8.0 ou une version récente de la branche 5 (support maintenu depuis le 19 septembre 2026 pour cette branche)
- Un nom de domaine dont vous contrôlez la zone DNS, avec les enregistrements A/AAAA déjà pointés vers votre serveur
- Un serveur web installé : Nginx 1.24+ ou Apache 2.4+ selon le plugin choisi
- Les ports 80 et 443 ouverts sur votre pare-feu pour la validation HTTP-01 (ou un accès API à votre fournisseur DNS pour la validation DNS-01)
- Python 3.9 ou supérieur, requis par Certbot en interne
- Docker et Docker Compose (version 2.x) si vous suivez la partie sur le projet conteneurisé

Comptez environ 60 minutes pour dérouler l’ensemble des 13 étapes sur un serveur déjà opérationnel, incluant les tests de renouvellement et la mise en place de la surveillance.

## Étape 1 : choisir entre validation HTTP-01 et DNS-01

Le choix de la méthode de validation conditionne tout le reste de votre configuration. La validation HTTP-01 est la plus simple : Certbot dépose un fichier temporaire dans le répertoire servi par votre serveur web, et Let’s Encrypt vérifie qu’il est bien accessible publiquement sur le port 80. Elle fonctionne pour un domaine ou un sous-domaine précis, mais ne permet jamais d’obtenir un certificat wildcard (type `*.exemple.fr`), et elle échoue si le port 80 est bloqué ou si le serveur est derrière un load balancer qui ne route pas correctement la requête de validation.

La validation DNS-01, elle, consiste à ajouter un enregistrement TXT temporaire (`_acme-challenge.exemple.fr`) dans votre zone DNS. Elle est indispensable pour les certificats wildcard, fonctionne même si le port 80 est fermé, mais nécessite soit un accès API à votre fournisseur DNS (OVH, Cloudflare, Gandi, Route 53 sont couverts par des plugins officiels ou communautaires), soit une intervention manuelle à chaque renouvellement, ce qui casse l’automatisation si vous n’avez pas de plugin dédié. Pour un site vitrine classique avec un seul sous-domaine, HTTP-01 suffit largement. Pour une infrastructure avec plusieurs sous-domaines dynamiques ou un besoin de wildcard, DNS-01 avec un plugin API est la seule option viable à long terme.

## Étape 2 : installer Certbot 5.8.0 sur votre serveur

La méthode d’installation recommandée par l’EFF elle-même est le paquet Snap, qui garantit une mise à jour automatique vers la dernière version stable, contrairement aux dépôts de certaines distributions qui peuvent livrer une version plus ancienne (Fedora propose par exemple encore la 5.7.0 dans ses dépôts standards au moment de la rédaction).

```
# Sur Ubuntu 24.04 / Debian 12 - installation via Snap (recommandée)
sudo apt update
sudo apt install snapd -y
sudo snap install core
sudo snap refresh core
sudo snap install --classic certbot
sudo ln -s /snap/bin/certbot /usr/bin/certbot
# Vérifier la version installée
certbot --version
# Sortie attendue : certbot 5.8.0
```
Si vous préférez éviter Snap, le paquet `python3-certbot-nginx` ou `python3-certbot-apache` via `apt` fonctionne aussi, mais vérifiez toujours la version livrée avec `apt-cache policy certbot` avant de vous y fier pour de la production, car les dépôts Debian/Ubuntu accusent parfois plusieurs mois de retard sur la dernière release.

## Étape 3 : configurer le pare-feu et le DNS de votre domaine

Avant de lancer la moindre demande de certificat, vérifiez que votre domaine pointe correctement vers le serveur et que les ports nécessaires sont ouverts. C’est l’étape la plus souvent négligée, et la première source d’échec en production.

```
# Vérifier la résolution DNS depuis l'extérieur
dig +short A exemple.fr
dig +short AAAA exemple.fr
# Ouvrir les ports 80 et 443 avec ufw (Ubuntu)
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw reload
# Avec firewalld (Rocky Linux / RHEL)
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```
Si votre serveur est hébergé derrière un reverse proxy comme Traefik, vérifiez que les règles de routage laissent bien passer les requêtes vers `/.well-known/acme-challenge/` sans redirection HTTPS forcée, sinon la validation HTTP-01 échouera systématiquement. Ce point est d’ailleurs directement lié aux bonnes pratiques détaillées dans notre tutoriel Traefik pour le reverse proxy HTTPS, une configuration très répandue en environnement Docker.

## Étape 4 : obtenir votre premier certificat avec le plugin Nginx

Le plugin Nginx de Certbot est le plus intégré : il détecte automatiquement vos blocs `server` existants, obtient le certificat, puis modifie lui-même la configuration Nginx pour activer le HTTPS et rediriger le trafic HTTP.

```
# Installer le plugin Nginx
sudo snap install certbot-plugin-nginx
# Obtenir et installer automatiquement le certificat
sudo certbot --nginx -d exemple.fr -d www.exemple.fr
# Sortie attendue (extrait) :
# Successfully received certificate.
# Certificate is saved at: /etc/letsencrypt/live/exemple.fr/fullchain.pem
# Key is saved at:         /etc/letsencrypt/live/exemple.fr/privkey.pem
# This certificate expires on 2026-12-01.
# Deploying certificate
# Successfully deployed certificate for exemple.fr to /etc/nginx/sites-enabled/exemple.fr
# Congratulations! You have successfully enabled HTTPS
```
Certbot vous proposera aussi de forcer la redirection HTTP vers HTTPS automatiquement (option recommandée en production). Si vous préférez garder le contrôle total sur votre fichier de configuration Nginx plutôt que de laisser Certbot le modifier, utilisez l’option `certonly` à la place de `--nginx`, puis référencez manuellement les chemins des certificats dans votre bloc `server`.

## Étape 5 : obtenir un certificat avec le plugin Apache

Le fonctionnement est identique côté Apache, avec le plugin dédié `certbot-plugin-apache`.

```
sudo snap install certbot-plugin-apache
sudo a2enmod ssl
sudo certbot --apache -d exemple.fr -d www.exemple.fr
# Pour ne récupérer que le certificat sans toucher à la config Apache
sudo certbot certonly --apache -d exemple.fr
```
Sur les serveurs mutualisés ou les environnements où vous ne gérez pas directement Apache ou Nginx (par exemple derrière un CDN ou un load balancer géré), la sous-commande `certonly --standalone` ou `certonly --webroot` permet d’obtenir le certificat sans plugin de serveur web, à charge pour vous de le déployer manuellement à l’endroit voulu.

## Étape 6 : générer un certificat wildcard via le défi DNS-01

Pour couvrir tous les sous-domaines d’un coup (`*.exemple.fr`), seule la validation DNS-01 fonctionne. L’exemple ci-dessous utilise le plugin DNS Cloudflare, mais des plugins équivalents existent pour OVH, Gandi, Route 53, Google Cloud DNS et la plupart des grands fournisseurs, en plus du mode manuel pour les autres.

```
# Installer le plugin DNS Cloudflare
sudo snap install certbot-dns-cloudflare
# Créer le fichier d'identifiants API (permissions restreintes obligatoires)
sudo mkdir -p /etc/letsencrypt/secrets
sudo nano /etc/letsencrypt/secrets/cloudflare.ini
# Contenu : dns_cloudflare_api_token = VOTRE_TOKEN_API
sudo chmod 600 /etc/letsencrypt/secrets/cloudflare.ini
# Obtenir le certificat wildcard
sudo certbot certonly \
  --dns-cloudflare \
  --dns-cloudflare-credentials /etc/letsencrypt/secrets/cloudflare.ini \
  -d "exemple.fr" -d "*.exemple.fr"
```
Si votre fournisseur DNS n’a pas de plugin dédié, une solution courante consiste à déléguer la sous-zone `_acme-challenge` vers un service tiers comme acme-dns, ce qui évite de donner à Certbot un accès API complet à l’ensemble de votre zone DNS de production, une bonne pratique de moindre privilège à ne pas négliger.

## Étape 7 : automatiser le renouvellement avec systemd timer

Depuis l’installation via Snap ou les paquets récents, Certbot installe automatiquement un timer systemd qui exécute `certbot renew` deux fois par jour. C’est cette mécanique, et non une intervention manuelle, qui doit gérer le renouvellement au quotidien, y compris quand la durée de vie des certificats descendra à 47 jours.

```
# Vérifier que le timer est actif
sudo systemctl status snap.certbot.renew.timer
# Sortie attendue (extrait) :
# ● snap.certbot.renew.timer - Timer for snap application certbot.renew
#      Loaded: loaded (/etc/systemd/system/snap.certbot.renew.timer; enabled)
#      Active: active (waiting)
#     Trigger: Tue 2026-09-23 18:12:44 UTC; 6h left
# Lister les prochaines exécutions programmées
systemctl list-timers | grep certbot
```
Certbot ne renouvelle un certificat que s’il lui reste 30 jours ou moins avant expiration, quelle que soit sa durée de vie initiale. Cette logique reste valable pour les futurs certificats de 45 ou 47 jours : le seuil de déclenchement est une fenêtre relative, pas une date fixe, ce qui simplifie grandement la transition vers des cycles plus courts.

## Étape 8 : tester avec –dry-run et ajouter des hooks de déploiement

Ne faites jamais confiance à un timer sans l’avoir testé. La commande de simulation permet de vérifier que tout le processus de renouvellement fonctionne, sans consommer votre quota réel auprès de Let’s Encrypt (les limites de taux de production sont volontairement strictes, l’environnement de simulation utilise une autorité de test séparée).

```
# Simuler un renouvellement complet
sudo certbot renew --dry-run
# Sortie attendue (extrait) :
# Simulating renewal of an existing certificate for exemple.fr
# Congratulations, all simulated renewals succeeded:
#   /etc/letsencrypt/live/exemple.fr/fullchain.pem (success)
```
Le renouvellement ne sert à rien si le service web n’est pas rechargé pour prendre en compte le nouveau certificat. Ajoutez un hook de déploiement qui s’exécute uniquement quand un certificat a réellement été renouvelé, plutôt qu’à chaque exécution du timer.

```
# Créer un hook exécuté après chaque renouvellement réussi
sudo mkdir -p /etc/letsencrypt/renewal-hooks/deploy
sudo tee /etc/letsencrypt/renewal-hooks/deploy/reload-nginx.sh > /dev/null <<'EOF'
#!/bin/bash
systemctl reload nginx
EOF
sudo chmod +x /etc/letsencrypt/renewal-hooks/deploy/reload-nginx.sh
```
## Étape 9 : choisir entre ECDSA P-256 et RSA pour vos clés

Par défaut, Certbot génère des clés RSA 2048 bits, un choix pensé avant tout pour la compatibilité maximale avec les anciens clients. En 2026, pour une nouvelle configuration sur des systèmes récents, ECDSA avec la courbe NIST P-256 est généralement recommandée : les clés et les signatures sont beaucoup plus compactes, les opérations cryptographiques sont plus rapides, ce qui réduit la latence de négociation TLS, sans compromis de sécurité pour l'immense majorité des usages web modernes. RSA 2048 bits reste pertinent si vous devez impérativement supporter des clients très anciens ou des systèmes embarqués atypiques qui ne gèrent pas encore ECDSA.

```
# Générer un certificat avec une clé ECDSA P-256
sudo certbot certonly --nginx \
  -d exemple.fr \
  --key-type ecdsa \
  --elliptic-curve secp256r1
```
Vous pouvez faire coexister les deux types de clés sur un même serveur (une chaîne ECDSA et une chaîne RSA de secours), une technique appelée agilité cryptographique, utile en période de transition ou si votre audience inclut des équipements hérités.

## Étape 10 : activer HSTS et l'OCSP Stapling

Obtenir un certificat n'est que la moitié du travail : encore faut-il configurer le serveur web pour exploiter pleinement les protections TLS disponibles. HTTP Strict Transport Security (HSTS) indique au navigateur de toujours utiliser HTTPS pour votre domaine, même si l'utilisateur tape volontairement une URL en HTTP, ce qui neutralise les attaques de rétrogradation de protocole. L'OCSP Stapling permet au serveur de fournir lui-même la preuve de non-révocation du certificat, plutôt que de laisser chaque navigateur interroger l'autorité de certification séparément, ce qui accélère la connexion et réduit la charge sur l'infrastructure de Let's Encrypt.

```
server {
    listen 443 ssl;
    server_name exemple.fr;
    ssl_certificate     /etc/letsencrypt/live/exemple.fr/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/exemple.fr/privkey.pem;
    ssl_protocols       TLSv1.2 TLSv1.3;
    # OCSP Stapling
    ssl_stapling on;
    ssl_stapling_verify on;
    ssl_trusted_certificate /etc/letsencrypt/live/exemple.fr/chain.pem;
    resolver 1.1.1.1 8.8.8.8 valid=300s;
    # HSTS - 1 an, inclut les sous-domaines
    add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
}
```
Activez le préchargement HSTS (`preload`) uniquement une fois certain que tous vos sous-domaines fonctionnent correctement en HTTPS : la liste de préchargement des navigateurs est extrêmement difficile à quitter en cas d'erreur de configuration.

## Étape 11 : surveiller l'expiration et éviter la panne

Un timer systemd bien configuré échoue rarement, mais "rarement" ne veut pas dire "jamais" : changement de fournisseur DNS, token API expiré, quota de renouvellement dépassé après un test mal isolé, ou simplement une mise à jour système qui désactive le timer par erreur. La surveillance active de la date d'expiration reste indispensable, en complément de l'automatisation, pas à sa place.

```
# Vérifier rapidement la date d'expiration en ligne de commande
echo | openssl s_client -servername exemple.fr -connect exemple.fr:443 2>/dev/null \
  | openssl x509 -noout -enddate
# Sortie attendue :
# notAfter=Dec  1 08:14:22 2026 GMT
```
En production, préférez une surveillance externe automatisée plutôt qu'une vérification manuelle. L'exporter Prometheus Blackbox permet de sonder chaque domaine HTTPS et d'exposer le nombre de jours restants avant expiration sous forme de métrique, avec une alerte Grafana ou Alertmanager déclenchée par exemple 10 jours avant échéance. C'est une approche cohérente avec les principes de centralisation des journaux de sécurité déjà abordés dans notre tutoriel sur Graylog pour la centralisation des logs, où la même logique de détection proactive s'applique.

## Étape 12 : préparer la migration vers des sous-domaines et applications multiples

Dans un environnement réel, vous gérez rarement un seul domaine. Certbot permet d'étendre un certificat existant à de nouveaux noms sans repartir de zéro, ou de gérer plusieurs certificats indépendants selon vos besoins d'isolation.

```
# Ajouter un nouveau sous-domaine à un certificat existant
sudo certbot --nginx -d exemple.fr -d www.exemple.fr -d api.exemple.fr --expand
# Lister tous les certificats gérés sur le serveur
sudo certbot certificates
```
Pour des architectures avec de nombreux sous-domaines gérés dynamiquement (SaaS multi-tenant, environnements de préproduction générés à la volée), il est souvent plus robuste de combiner un certificat wildcard unique avec une architecture de reverse proxy centralisée, plutôt que de multiplier les certificats individuels et le risque d'oubli de renouvellement sur l'un d'entre eux.

## Étape 13 : passer aux certificats short-lived et aux profils ACME

Let's Encrypt a ouvert depuis le 13 mai 2026 un accès anticipé et volontaire à un profil de certificat à durée de vie réduite, baptisé `tlsserver`, valable 45 jours. La feuille de route officielle prévoit un abaissement à 64 jours pour le profil par défaut le 10 février 2027, puis 45 jours le 16 février 2028, en cohérence avec le calendrier plus large du CA/Browser Forum. Les clients ACME récents, dont Certbot, permettent de demander explicitement un profil via le paramètre dédié.

```
# Demander un certificat avec le profil short-lived (test recommandé en staging d'abord)
sudo certbot certonly --nginx \
  -d test.exemple.fr \
  --preferred-profile tlsserver \
  --staging
```
Testez systématiquement ce profil sur un sous-domaine non critique avant de l'appliquer en production. Un cycle de renouvellement à 45 jours laisse une marge de manœuvre beaucoup plus faible en cas d'incident : toute panne du timer, toute rotation de clé API DNS mal gérée, se traduit par une expiration réelle bien plus vite qu'avec un certificat classique de 90 jours. C'est précisément pour cette raison que l'étape de surveillance décrite plus haut devient non négociable dès lors que vous adoptez ces profils courts.

## Calendrier de réduction de la durée de vie des certificats TLS (2026-2029)

Le tableau ci-dessous résume les échéances officielles adoptées via le ballot SC-081v3 du CA/Browser Forum, qui s'appliquent à tous les certificats publiquement approuvés, au-delà du seul cas de Let's Encrypt.

| Date d'entrée en vigueur | Durée max. du certificat | Réutilisation validation domaine (DCV) | Statut au 23/09/2026 | 
|---|---|---|---|
| 15 mars 2026 | 200 jours | 200 jours | En vigueur | 
| 15 mars 2027 | 100 jours | 100 jours | À venir | 
| 15 mars 2029 | 47 jours | 10 jours | À venir | 
| 13 mai 2026 (Let's Encrypt) | 45 jours (profil tlsserver, opt-in) | Non concerné (profil test) | Disponible en accès anticipé | 
| 10 février 2027 (Let's Encrypt) | 64 jours (profil par défaut) | Non communiqué | Planifié | 
| 16 février 2028 (Let's Encrypt) | 45 jours (profil par défaut) | Non communiqué | Planifié | 

Notez que la première étape de mars 2026 touche surtout les certificats commerciaux classiques : les certificats Let's Encrypt étaient déjà limités à 90 jours depuis leur lancement, donc cette première marche ne réduit pas immédiatement leur cycle de vie. C'est la trajectoire propre à Let's Encrypt, avec ses profils courts optionnels puis généralisés, qui impactera le plus tôt les utilisateurs de Certbot.

## Certbot vs acme.sh vs win-acme vs lego vs Caddy : quel client choisir

Certbot n'est pas le seul client ACME disponible, et selon votre environnement, une alternative peut être plus adaptée. Voici les principales options du marché en 2026.

| Client | Langage / dépendances | Plateforme cible | Wildcard (DNS-01) | Cas d'usage idéal | 
|---|---|---|---|---|
| Certbot | Python 3 | Linux (Nginx, Apache) | Oui, via plugins DNS | Serveurs Linux classiques, plugins officiels nombreux | 
| acme.sh | Shell pur, zéro dépendance lourde | Linux, BSD, conteneurs légers | Oui, très large éventail de fournisseurs DNS | Environnements minimalistes, scripts d'automatisation custom | 
| win-acme | .NET | Windows Server, IIS | Oui, via plugins | Infrastructures Windows/IIS | 
| lego | Go, binaire unique | Multiplateforme (Linux, macOS, Windows) | Oui | Intégration dans des outils tiers (Traefik, Caddy s'en inspirent) | 
| Caddy (natif) | Go, serveur web intégré | Linux, macOS, Windows | Oui, HTTPS automatique par défaut | Nouveaux projets acceptant de changer de serveur web | 

Pour une infrastructure Linux déjà construite autour de Nginx ou Apache, Certbot reste le choix le plus documenté et le plus intégré, avec la communauté la plus large pour le support. acme.sh séduit les équipes qui veulent un script auto-contenu sans dépendance Python, notamment dans des images de conteneurs très légères. win-acme est incontournable dès qu'IIS entre en jeu. Caddy, de son côté, ne se contente pas d'automatiser Certbot : il intègre nativement la gestion ACME dans le serveur web lui-même, ce qui supprime une bonne partie de la configuration manuelle, au prix d'un changement de serveur web complet.

## Projet complet : stack Docker Compose Nginx + Certbot en production

Voici une stack fonctionnelle complète, pensée pour un déploiement Docker Compose, avec un conteneur Nginx et un conteneur Certbot dédié au renouvellement, sans dépendance à Snap sur l'hôte.

```
# docker-compose.yml
services:
  nginx:
    image: nginx:1.27-alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx/conf.d:/etc/nginx/conf.d:ro
      - ./certbot/conf:/etc/letsencrypt:ro
      - ./certbot/www:/var/www/certbot:ro
    restart: unless-stopped
  certbot:
    image: certbot/certbot:latest
    volumes:
      - ./certbot/conf:/etc/letsencrypt
      - ./certbot/www:/var/www/certbot
    entrypoint: >
      /bin/sh -c "trap exit TERM;
      while :; do
        certbot renew --webroot -w /var/www/certbot --quiet;
        sleep 12h & wait $${!};
      done"
```
```
# nginx/conf.d/exemple.fr.conf
server {
    listen 80;
    server_name exemple.fr;
    location /.well-known/acme-challenge/ {
        root /var/www/certbot;
    }
    location / {
        return 301 https://$host$request_uri;
    }
}
server {
    listen 443 ssl;
    server_name exemple.fr;
    ssl_certificate     /etc/letsencrypt/live/exemple.fr/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/exemple.fr/privkey.pem;
    location / {
        proxy_pass http://app:8080;
    }
}
```
Pour l'obtention initiale du certificat (avant que le conteneur ne puisse tourner en boucle de renouvellement), lancez une commande ponctuelle avec le même conteneur, en mode webroot, avant de démarrer la boucle permanente.

```
docker compose run --rm certbot certonly \
  --webroot -w /var/www/certbot \
  -d exemple.fr --email [email protected] \
  --agree-tos --no-eff-email
docker compose up -d
```
Cette architecture sépare clairement le renouvellement (conteneur Certbot autonome) du service (conteneur Nginx), une approche cohérente avec les principes d'infrastructure comme code déjà décrits dans notre tutoriel Traefik Docker, une alternative à Nginx qui gère nativement le renouvellement ACME sans conteneur Certbot séparé.

## Erreurs courantes à éviter avec Certbot et Let's Encrypt

La majorité des incidents liés à Certbot en production ne viennent pas d'un bug de l'outil, mais d'erreurs de configuration récurrentes, que voici.

- **Tester en production plutôt qu'en staging :** Let's Encrypt applique des limites de taux strictes (nombre de certificats par domaine par semaine). Multiplier les tentatives ratées en environnement réel épuise ce quota et vous bloque parfois plusieurs jours. Utilisez systématiquement`--staging` pour tout test de configuration.
- **Oublier le hook de rechargement du service :** un certificat renouvelé mais jamais rechargé par Nginx ou Apache ne sert à rien. Le service continue de présenter l'ancien certificat jusqu'à son redémarrage manuel, ce qui annule tout le bénéfice de l'automatisation.
- **Confondre HTTP-01 et wildcard :** un certificat`*.exemple.fr` ne peut être obtenu qu'avec une validation DNS-01. Tenter d'utiliser`--nginx` ou`--apache` seul pour un wildcard échoue systématiquement.
- **Permissions trop larges sur les identifiants API DNS :** un jeton API donnant un accès en écriture à l'ensemble de votre compte DNS, au lieu d'être restreint à la seule zone concernée, est un risque de sécurité disproportionné par rapport au bénéfice.
- **Ignorer la propagation DNS avant validation :** un enregistrement TXT tout juste créé peut mettre plusieurs minutes à se propager selon le TTL configuré. Certbot propose un délai d'attente paramétrable (`--dns-cloudflare-propagation-seconds` par exemple) qu'il faut ajuster si les validations échouent de façon intermittente.
- **Ne pas surveiller les emails d'expiration :** Let's Encrypt envoie des alertes automatiques à l'adresse renseignée lors de la première demande. Si cette adresse n'est plus valide ou n'est jamais consultée, le dernier filet de sécurité avant une panne disparaît.

## Dépannage : 8 problèmes fréquents et leurs solutions

Voici les erreurs les plus fréquemment rencontrées lors du déploiement de Certbot, avec leur cause probable et la correction associée.

- **"Timeout during connect (likely firewall problem)" :** le port 80 est bloqué par votre pare-feu, votre groupe de sécurité cloud, ou un load balancer en amont. Vérifiez avec`curl -I http://votredomaine.fr` depuis une machine externe.
- **"Too many certificates already issued for exact set of domains" :** vous avez dépassé la limite de taux de production de Let's Encrypt. Attendez la fin de la fenêtre (généralement une semaine glissante) ou continuez vos tests en`--staging` .
- **"DNS problem: NXDOMAIN looking up TXT" :** l'enregistrement`_acme-challenge` n'existe pas encore ou n'a pas fini de se propager. Vérifiez avec`dig TXT _acme-challenge.exemple.fr` et augmentez le délai de propagation du plugin DNS.
- **Certificat renouvelé mais l'ancien reste actif dans le navigateur :** le service web n'a pas été rechargé. Ajoutez ou corrigez le hook dans`/etc/letsencrypt/renewal-hooks/deploy/` .
- **"Permission denied" sur /etc/letsencrypt :** les commandes Certbot doivent presque toujours s'exécuter avec`sudo` , car les clés privées sont protégées avec des permissions restrictives par conception. Ne modifiez jamais ces permissions pour "simplifier" un script.
- **"Failed authorization procedure" sur un wildcard :** une tentative de validation HTTP-01 a été utilisée par erreur au lieu de DNS-01. Relancez la commande avec le plugin DNS approprié.
- **Le timer systemd est actif mais rien ne se passe :** vérifiez les journaux avec`journalctl -u snap.certbot.renew.service` pour identifier une erreur silencieuse, souvent liée à un identifiant API expiré ou changé sans mise à jour du fichier de credentials.
- **OCSP Stapling signalé comme non fonctionnel par un test SSL externe :** vérifiez que la directive`resolver` pointe vers un DNS accessible depuis le serveur, et que le fichier`chain.pem` (et non`fullchain.pem` ) est bien référencé dans`ssl_trusted_certificate` .

## Conformité NIS2 et Cyber Resilience Act : le rôle du chiffrement automatisé

Au-delà de la simple hygiène technique, l'automatisation du renouvellement TLS s'inscrit désormais dans un cadre réglementaire européen plus large. La directive NIS2, dont la transposition française reste suivie de près par l'ANSSI, impose aux entités essentielles et importantes de démontrer une gestion rigoureuse de leurs mesures de sécurité technique, ce qui inclut la disponibilité et l'intégrité des communications chiffrées. Un certificat expiré sur un service exposé n'est pas qu'un désagrément esthétique : c'est un indicateur direct de défaillance dans la gestion du cycle de vie de la sécurité, exactement le type de lacune que les audits de conformité recherchent. Nous détaillons les obligations concrètes de ce texte dans notre guide technique sur la directive NIS2.

Le Cyber Resilience Act, de son côté, pousse les fabricants de produits comportant des éléments numériques à documenter et sécuriser l'ensemble du cycle de vie de leurs logiciels, y compris les mécanismes de mise à jour et de gestion des secrets cryptographiques. Une automatisation robuste du renouvellement de certificats, avec surveillance et hooks de déploiement testés, correspond exactement à l'esprit de traçabilité et de résilience qu'exige ce règlement, sujet que nous approfondissons dans notre article sur le Cyber Resilience Act et la conformité SBOM. Dans les deux cas, la réduction imposée de la durée de vie des certificats agit comme un accélérateur : elle transforme une bonne pratique auparavant facultative en une nécessité opérationnelle documentée.

## Conseils avancés pour vos environnements de production

Une fois les bases posées, plusieurs raffinements permettent de fiabiliser durablement votre gestion des certificats. Pour les environnements avec plusieurs serveurs derrière un load balancer, centralisez l'obtention des certificats sur une seule machine, puis synchronisez les fichiers `fullchain.pem` et `privkey.pem` vers les autres nœuds via un outil comme rsync sécurisé par clé SSH, ou un gestionnaire de secrets centralisé, une approche que nous détaillons dans notre tutoriel sur HashiCorp Vault pour la gestion des secrets. Évitez à tout prix qu'un même domaine tente d'obtenir des certificats indépendamment depuis plusieurs serveurs sans coordination : cela multiplie inutilement la consommation de votre quota Let's Encrypt.

Adoptez également l'extension ACME Renewal Information (ARI) dès que votre client la supporte pleinement : elle laisse l'autorité de certification piloter le moment optimal de renouvellement plutôt que de vous fier uniquement à un seuil fixe de 30 jours, ce qui devient précieux à mesure que les cycles se raccourcissent. Documentez enfin une procédure de reprise manuelle pour le jour où l'automatisation échouera malgré tout : qui contacter, où se trouve le token API de secours, et comment obtenir un certificat en urgence via `certonly --manual` si toute votre chaîne d'automatisation tombe en panne simultanément. Cette procédure de secours n'est jamais du temps perdu : elle transforme un incident de plusieurs heures en correctif de quelques minutes.

## Questions fréquentes

**Certbot et Let's Encrypt sont-ils entièrement gratuits ?**

Oui. Let's Encrypt est une autorité de certification à but non lucratif financée par des dons et des sponsors, et Certbot est un logiciel libre développé par l'Electronic Frontier Foundation. Aucune limite de nombre de domaines gratuits n'existe, seules des limites de taux techniques s'appliquent pour éviter les abus.

**Combien de temps dure un certificat Let's Encrypt aujourd'hui ?**

Les certificats standards restent valables 90 jours en septembre 2026. Un profil optionnel à 45 jours (`tlsserver`) est disponible en test depuis le 13 mai 2026, avec une généralisation progressive prévue en 2027 et 2028.

**Certbot fonctionne-t-il avec Cloudflare, OVH ou Gandi ?**

Oui, via des plugins DNS-01 dédiés pour la plupart des grands fournisseurs, ce qui permet d'automatiser entièrement les certificats wildcard sans intervention manuelle à chaque renouvellement.

**Que se passe-t-il si mon certificat expire malgré tout ?**

Les navigateurs affichent un avertissement de sécurité bloquant pour les visiteurs, et la plupart des intégrations API tierces rejettent également la connexion. C'est pour cette raison que la surveillance active (étape 11) doit toujours accompagner l'automatisation, jamais la remplacer.

**Faut-il préférer un certificat wildcard ou plusieurs certificats individuels ?**

Un wildcard simplifie la gestion si vous ajoutez fréquemment de nouveaux sous-domaines, mais concentre le risque : la compromission de sa clé privée expose tous les sous-domaines simultanément. Des certificats individuels ou un certificat multi-domaines (SAN) limité offrent un meilleur cloisonnement.

**Certbot est-il adapté à une utilisation en entreprise, face à des solutions payantes ?**

Pour la très grande majorité des sites web et API publiques, Certbot avec Let's Encrypt couvre entièrement le besoin sans coût de licence. Les certificats commerciaux (EV, OV) restent pertinents dans des cas spécifiques où l'identité juridique de l'organisation doit apparaître directement dans le certificat, un besoin que Let's Encrypt ne couvre pas par conception.

**La directive NIS2 impose-t-elle explicitement Certbot ou Let's Encrypt ?**

Non, la directive ne cite aucun outil précis. Elle impose en revanche des obligations de sécurité technique et de gestion des risques dont une automatisation fiable du chiffrement TLS constitue une brique naturelle, sans imposer de fournisseur particulier.

**Comment tester le profil short-lived sans risquer une panne en production ?**

Créez d'abord un sous-domaine de test dédié, appliquez-y le profil `tlsserver` en environnement `--staging`, validez que votre hook de rechargement et votre surveillance fonctionnent correctement sur ce cycle court, puis étendez progressivement la configuration aux domaines de production.
