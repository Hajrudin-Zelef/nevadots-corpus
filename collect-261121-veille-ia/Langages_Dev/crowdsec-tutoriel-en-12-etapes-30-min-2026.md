---
id: collect-261121-veille-ia/langages-dev/crowdsec-tutoriel-en-12-etapes-30-min-2026
title: "CrowdSec : Tutoriel en 12 Étapes, 30 Min [2026]"
domain: langages-dev
role: reference
task: article
actors: []
dates: ["2026-05-11"]
keywords: ["crowdsec", "publie", "millions", "tutoriel", "etapes", "source", "logs", "environ"]
source: https://tech-insider.org/fr/crowdsec-tutoriel-proteger-serveur-linux-2026/
source_anchor: ""
source_lines: [1, 468]
section: "Langages_Dev"
---
Les serveurs exposés sur Internet subissent un déluge permanent de scans, de tentatives de brute-force SSH et d’injections web. Selon le bilan annuel *2025 Wrap* publié par CrowdSec, la communauté de l’éditeur bloque désormais près de **26 millions d’attaques par jour**, émanant de **1,5 million d’attaquants uniques quotidiens**, et a recensé environ **8 millions d’adresses IP malveillantes**. Face à cette pression, `fail2ban` montre ses limites : il agit en silo, ne partage rien et ne connaît que les attaques qu’il voit passer sur votre seule machine.

Ce tutoriel CrowdSec vous guide pas à pas, en 12 étapes et environ 30 minutes, pour installer, configurer et exploiter ce moteur de détection collaboratif sur un serveur Linux. À la fin, vous disposerez d’un projet complet et fonctionnel : SSH et Nginx protégés, un pare-feu applicatif (WAF) actif, une Console de supervision branchée, et votre serveur connecté à la plus grande source de renseignement sur les menaces (CTI) communautaire et open source du marché. Le tout avec un logiciel **français**, sous licence MIT, pensé pour la souveraineté numérique européenne.

## CrowdSec en 2026 : un IPS collaboratif made in France

CrowdSec est un moteur de sécurité open source qui détecte les comportements malveillants en analysant vos journaux (logs), puis applique des décisions de blocage via des composants appelés *bouncers*. Sa particularité : chaque IP malveillante détectée par un utilisateur, une fois validée, alimente une **liste de blocage communautaire** redistribuée à l’ensemble du réseau. Autrement dit, si un serveur en Allemagne se fait attaquer par une adresse IP, le vôtre en France peut la bloquer *avant* même qu’elle ne vous cible. C’est le principe du renseignement sur les menaces mutualisé, ou CTI.

Le projet est porté par une société française fondée en 2020 et basée à Montrouge, en région parisienne, par Philippe Humeau (CEO), Thibault Koechlin et Laurent Soubrevilla. CrowdSec a levé **14 millions d’euros** en série A en octobre 2022 (annonce officielle), menée par Supernova Invest aux côtés de l’investisseur historique Breega, pour un total d’environ 20,6 M$ sur trois tours. Le cœur du logiciel, écrit en Go 1.24, est publié sous **licence MIT** sur GitHub où il cumule plus de **14 000 étoiles** et totalise 221 releases au compteur du dépôt officiel, le tag v1.7.8 ayant été publié le 11 mai 2026 comme correctif de sécurité pour **CVE-2026-44982** et **CVE-2026-44981**. Le rythme des sorties s’est encore accéléré depuis : la branche **1.8.0**, publiée en août 2026, a ajouté une détection de bots directement dans le WAF ainsi qu’une source de logs native pour **Kubernetes**, avant que la **1.8.1** ne devienne, en septembre 2026, la dernière version disponible sur le dépôt GitHub officiel ; l’installeur Windows (MSI) de la 1.7.8, référencée comme la version la plus récente sur SourceForge, sortie le 11 mai 2026 et buildée le 27 mai 2026 sous le nom de code « alphaga » selon les relevés de Tech Insider, pèse quant à lui 90,6 Mo.

L’ampleur du réseau parle d’elle-même. Publié en janvier 2026, ce bilan 2025 révèle que les moteurs de sécurité CrowdSec ont analysé près de **320 milliards de lignes de logs** réparties sur **23 000 organisations**, avec une croissance de 65 % du nombre de moteurs connectés sur l’année ; le rapport souligne au passage que la branche Security Engine 1.7 a introduit de nouvelles métriques de parseurs facilitant le débogage sur le terrain. Environ 70 % des attaques détectées sont de type HTTP, et les États-Unis arrivent en tête des pays émetteurs avec 4,8 millions d’adresses IP malveillantes uniques. Ce volume fait de la blocklist CrowdSec un atout défensif difficile à égaler pour un outil local isolé.

## CrowdSec vs Fail2ban : pourquoi migrer en 2026

Beaucoup d’administrateurs arrivent à CrowdSec depuis `fail2ban`, l’outil historique de bannissement d’IP. Les deux partagent une idée commune – lire des logs, repérer des abus, bannir des adresses – mais leur philosophie diffère radicalement. Fail2ban est monolithique : il lit, décide et bannit dans le même processus, à l’aide d’expressions régulières (regex) propres à chaque *jail*. CrowdSec sépare la **détection** (le moteur) de la **remédiation** (les bouncers), ce qui le rend bien plus modulaire et adapté aux infrastructures distribuées.

| Critère | Fail2ban | CrowdSec | 
|---|---|---|
| Langage | Python | Go (binaire compilé) | 
| Détection | Regex locale par jail | Scénarios comportementaux + parseurs | 
| Renseignement (CTI) | Aucun | Blocklist communautaire mondiale | 
| Remédiation | iptables intégré | Bouncers découplés (pare-feu, Nginx, Cloudflare, etc.) | 
| Architecture | Monolithique | Moteur + LAPI + bouncers | 
| Supervision | Ligne de commande seule | Console SaaS gratuite (tableau de bord) | 
| Multi-serveur | Manuel | LAPI centralisée native | 
| Pare-feu applicatif (WAF) | Non | Oui (composant AppSec) | 
| Licence | GPLv2 | MIT | 

Le verdict n’est pas que CrowdSec « tue » fail2ban : pour un serveur unique et minimaliste, fail2ban reste léger et suffisant. Mais dès que vous gérez plusieurs machines, que vous voulez un tableau de bord, une protection web applicative ou bénéficier d’une intelligence collective, le rapport bénéfice/effort penche nettement en faveur de CrowdSec. C’est ce que confirme la communauté francophone : le guide de référence d’IT-Connect présente d’ailleurs CrowdSec comme « une alternative collaborative à Fail2ban ».

### L’architecture en clair : moteur, LAPI, bouncers, Hub et Console

Avant de taper la moindre commande, il faut comprendre les six briques qui composent un déploiement CrowdSec. Cette vision d’ensemble vous évitera 80 % des erreurs de débutant.

- **Le moteur de sécurité (Security Engine)** : l’agent qui lit les logs, les normalise via des*parseurs* , évalue le comportement contre des*scénarios* d’attaque, puis génère des décisions selon des*profils* . C’est la couche de détection (IDS), avec une option de pare-feu applicatif (WAF).
- **La LAPI (Local API)** : l’API locale qui stocke les décisions et fait le lien entre le moteur et les bouncers. Elle écoute par défaut sur`127.0.0.1:8080` .
- **Les bouncers (composants de remédiation)** : ils appliquent les décisions sans jamais les prendre eux-mêmes. Bouncer pare-feu (iptables/nftables), bouncer Nginx, bouncer Cloudflare, etc. C’est la couche de prévention (IPS).
- **Le Hub** : le dépôt communautaire de collections, parseurs et scénarios prêts à l’emploi (SSH, Nginx, WordPress, etc.).
- **La Console** : le tableau de bord SaaS gratuit hébergé sur app.crowdsec.net pour superviser vos moteurs, vos alertes et vos décisions.
- **La CAPI (Central API)** : l’API centrale qui mutualise le renseignement. C’est elle qui vous envoie la blocklist communautaire et reçoit, si vous y consentez, vos signaux anonymisés.

| Composant | Rôle | Port / Emplacement | 
|---|---|---|
| Security Engine | Détection (IDS) | service `crowdsec` | 
| LAPI | API locale des décisions | 8080/tcp (localhost) | 
| Firewall Bouncer | Remédiation réseau (IPS) | iptables / nftables | 
| Nginx Bouncer | Remédiation web | module Lua Nginx | 
| AppSec | Pare-feu applicatif (WAF) | 127.0.0.1:7422 | 
| Console | Tableau de bord SaaS | app.crowdsec.net (443) | 
| CAPI | CTI communautaire | api.crowdsec.net (443) | 

## Prérequis : versions et matériel requis

CrowdSec est remarquablement frugal : il tourne sur un VPS d’entrée de gamme, un Raspberry Pi ou un gros serveur de production sans configuration différente. Voici ce dont vous avez besoin avant de commencer l’installation de CrowdSec.

| Composant | Recommandation | Notes | 
|---|---|---|
| CrowdSec Security Engine | 1.7.8 (mai 2026) | Dernière version stable, paquets officiels | 
| Système d’exploitation | Debian 12/13, Ubuntu 22.04/24.04 | Aussi RHEL, Fedora, Alpine, FreeBSD | 
| RAM | 256 Mo minimum | ~128 Mo consommés en pratique | 
| Processeur | 1 vCPU | Empreinte CPU négligeable au repos | 
| Pare-feu | iptables ou nftables | Requis par le firewall-bouncer | 
| Accès | root ou sudo | Indispensable pour l’installation | 
| Port sortant | 443/tcp | Vers la CAPI et la Console | 

Un détail rassurant : aucune base de données externe n’est nécessaire pour démarrer. CrowdSec utilise par défaut une base SQLite locale (`/var/lib/crowdsec/data/crowdsec.db`), parfaitement adaptée à un serveur unique. Pour une architecture multi-serveur en production, vous pourrez basculer plus tard sur PostgreSQL ou MySQL, mais ce n’est pas requis pour ce tutoriel. Assurez-vous simplement que la date et l’heure du serveur sont correctes (via `chrony` ou `systemd-timesyncd`), car des horloges décalées faussent la corrélation des événements.

## Étapes 1 à 3 – Installer le moteur CrowdSec et le bouncer pare-feu

Les trois premières étapes constituent le socle : ajouter le dépôt officiel, installer le moteur de détection, puis installer le bouncer qui appliquera réellement les bannissements. C’est le point que beaucoup ratent : sans bouncer, CrowdSec *voit* les attaques mais ne *bloque* rien.

### Étape 1 – Ajouter le dépôt officiel CrowdSec

Commencez par mettre votre système à jour, puis exécutez le script d’installation du dépôt fourni par l’éditeur. Ce script POSIX détecte automatiquement votre distribution et configure le dépôt packagecloud adéquat.

```
# Mettre à jour la liste des paquets et le système
sudo apt update && sudo apt upgrade -y
# Ajouter le dépôt officiel CrowdSec (détection automatique de la distro)
curl -s https://install.crowdsec.net | sudo sh
```
Si vous êtes sur une distribution à base de Red Hat (RHEL, Rocky, AlmaLinux), le même script bascule sur `yum`/`dnf`. Pour les puristes méfiants vis-à-vis du *curl | sh*, vous pouvez télécharger le script, l’inspecter, puis l’exécuter manuellement – c’est une bonne habitude de sécurité.

### Étape 2 – Installer le moteur de sécurité

L’installation du paquet `crowdsec` est l’étape clé. Lors de cette opération, l’assistant analyse les services déjà présents sur la machine (SSH, Nginx, Apache…) et installe automatiquement les collections de détection correspondantes. C’est ce qui rend l’installation de CrowdSec si rapide.

```
# Installer le moteur de détection CrowdSec
sudo apt install crowdsec -y
# Vérifier que le service tourne
sudo systemctl status crowdsec --no-pager
```
Sortie attendue : la ligne `Active: active (running)` en vert confirme que le moteur est opérationnel. Dès cet instant, CrowdSec lit déjà vos logs et se connecte à la CAPI pour télécharger la blocklist communautaire.

### Étape 3 – Installer le bouncer pare-feu (remédiation)

Le bouncer est le bras armé du dispositif. Choisissez la variante adaptée à votre pare-feu : `iptables` (classique) ou `nftables` (par défaut sur Debian 11+ et Ubuntu récents). En cas de doute, vérifiez avec `nft list ruleset` : si la commande renvoie un résultat, optez pour nftables.

```
# Variante iptables (la plus répandue)
sudo apt install crowdsec-firewall-bouncer-iptables -y
# OU variante nftables (Debian/Ubuntu récents)
# sudo apt install crowdsec-firewall-bouncer-nftables -y
# Confirmer l'enregistrement du bouncer auprès de la LAPI
sudo cscli bouncers list
```
Le bouncer s’enregistre tout seul auprès de la LAPI grâce à une clé API générée à l’installation. Dans la sortie de `cscli bouncers list`, vous devez voir une ligne du type `cs-firewall-bouncer-...` avec un statut valide. Si le bouncer apparaît, votre chaîne détection → remédiation est désormais complète.

## Étape 4 – Vérifier l’installation et lire les métriques

Avant d’aller plus loin, validez que tout communique correctement. L’outil en ligne de commande `cscli` est votre couteau suisse : il interroge la LAPI pour afficher l’état du moteur, les collections actives, les bouncers connectés et les métriques de traitement.

```
# Vue d'ensemble des métriques (lignes lues, alertes, décisions)
sudo cscli metrics
# Collections de détection installées
sudo cscli collections list
# Bouncers connectés à la LAPI
sudo cscli bouncers list
# Moteurs (machines) enregistrés
sudo cscli machines list
# État de la connexion à la blocklist communautaire (CAPI)
sudo cscli capi status
```
Dans la sortie de `cscli metrics`, concentrez-vous sur la section **Acquisition Metrics** : elle indique combien de lignes ont été lues par source de logs. Si une source affiche zéro ligne lue alors que le service génère bien des journaux, c’est le signe d’un problème de chemin dans votre fichier d’acquisition – nous y reviendrons dans la section dépannage. La commande `cscli capi status` doit, elle, renvoyer un message confirmant que vous pouvez interagir avec la Central API (CAPI).

## Étape 5 – Maîtriser les collections, parseurs et scénarios du Hub

Le Hub est ce qui distingue CrowdSec d’un simple bloqueur d’IP. Une **collection** regroupe les parseurs (qui transforment une ligne de log brute en événement structuré) et les scénarios (qui décrivent un comportement malveillant, par exemple « plus de 5 échecs SSH en 10 secondes »). Vous pilotez tout cela via `cscli`.

```
# Parcourir tout le contenu du Hub disponible
sudo cscli hub list
# Installer la collection SSH (si elle n'a pas été ajoutée automatiquement)
sudo cscli collections install crowdsecurity/sshd
# Mettre à jour l'index du Hub puis les éléments installés
sudo cscli hub update
sudo cscli hub upgrade
# Recharger le moteur pour prendre en compte les changements
sudo systemctl reload crowdsec
```
Pensez à automatiser la mise à jour du Hub. Les scénarios évoluent constamment pour suivre les nouvelles techniques d’attaque ; un Hub à jour, c’est une détection à jour. Une tâche `cron` hebdomadaire exécutant `cscli hub update && cscli hub upgrade` suivie d’un `systemctl reload crowdsec` est une excellente pratique. Vous pouvez inspecter en détail un scénario installé avec `cscli scenarios inspect crowdsecurity/ssh-bf` pour comprendre ses seuils de déclenchement.

## Étape 6 – Sécuriser l’accès SSH contre le brute-force

SSH est la cible numéro un de tout serveur Linux exposé. La collection `crowdsecurity/sshd` détecte les attaques par force brute et les tentatives d’énumération d’utilisateurs en analysant `/var/log/auth.log` (ou le journal systemd selon votre distribution). Vérifions qu’elle est bien active.

```
# Confirmer que la collection SSH est chargée
sudo cscli collections list | grep sshd
# Lister les scénarios SSH actifs
sudo cscli scenarios list | grep ssh
# Voir les décisions actives (IP actuellement bannies)
sudo cscli decisions list
# Voir l'historique des alertes (déclenchements de scénarios)
sudo cscli alerts list
```
La distinction **decisions** / **alerts** est fondamentale et source de confusion fréquente. Une *alerte* est l’enregistrement historique d’un scénario qui s’est déclenché (« telle IP a tenté un brute-force SSH à telle heure »). Une *décision* est l’action en cours qui en découle (« telle IP est bannie pour 4 heures »). Une décision expire ; une alerte reste tracée. Pour bannir manuellement une IP de test et observer le mécanisme :

```
# Bannir manuellement une IP de test pendant 4 heures
sudo cscli decisions add --ip 203.0.113.42 --duration 4h --reason "test SSH"
# Vérifier qu'elle apparaît dans la liste des décisions
sudo cscli decisions list
# Lever le ban une fois le test terminé
sudo cscli decisions delete --ip 203.0.113.42
```
Sortie attendue : `cscli decisions list` affiche un tableau avec la source (Cscli), la valeur de l’IP, le motif, l’action (ban) et la durée restante. Si vous gérez l’accès SSH avec un VPN, pensez à compléter ce tutoriel par notre guide Tutoriel WireGuard : VPN Linux en 12 étapes pour réduire encore davantage la surface d’attaque.

## Étape 7 – Protéger un serveur web Nginx

Puisque 70 % des attaques détectées par la communauté sont de type HTTP, protéger votre couche web est prioritaire. La démarche comporte deux volets : faire *analyser* les logs Nginx par le moteur, puis installer un bouncer Nginx capable de *bloquer* les requêtes au niveau applicatif (renvoyer un 403 ou une page captcha).

```
# Installer la collection de détection Nginx
sudo cscli collections install crowdsecurity/nginx
# Déclarer les logs Nginx à analyser (acquisition)
sudo nano /etc/crowdsec/acquis.yaml
```
Ajoutez (ou vérifiez) un bloc d’acquisition pointant vers vos journaux Nginx. La clé `type: nginx` indique au moteur quel parseur appliquer.

```
filenames:
  - /var/log/nginx/access.log
  - /var/log/nginx/error.log
labels:
  type: nginx
```
Rechargez le moteur, puis installez le bouncer Nginx, qui s’intègre via un module Lua dans votre configuration Nginx :

```
# Recharger pour prendre en compte la nouvelle acquisition
sudo systemctl reload crowdsec
# Installer le bouncer Nginx (remédiation applicative)
sudo apt install crowdsec-nginx-bouncer -y
# Vérifier la lecture des logs Nginx
sudo cscli metrics | grep -i nginx
```
Le paquet du bouncer Nginx ajoute automatiquement sa configuration et génère sa propre clé API auprès de la LAPI. Après un `systemctl reload nginx`, toute IP frappée d’une décision se verra refuser l’accès directement par le serveur web, avant même d’atteindre votre application. C’est la double protection : réseau (firewall-bouncer) *et* applicatif (nginx-bouncer).

## Étape 8 – Brancher la Console et le CTI communautaire

La Console (app.crowdsec.net) est un tableau de bord SaaS **gratuit** qui transforme l’expérience CrowdSec. Vous y visualisez vos alertes en temps réel, la cartographie des attaquants, l’état de vos moteurs et la blocklist appliquée – sans plus dépendre uniquement de la ligne de commande. L’inscription est gratuite et n’impose aucun engagement.

Créez un compte sur la Console, copiez la clé d’enrôlement (*enrollment key*) affichée dans l’interface, puis liez votre moteur :

```
# Vérifier d'abord la connexion à la CAPI
sudo cscli capi status
# Enrôler ce moteur dans la Console (collez votre clé)
sudo cscli console enroll VOTRE_CLE_D_ENROLEMENT
# Recharger pour activer la liaison
sudo systemctl reload crowdsec
```
De retour dans la Console, validez la demande d’enrôlement : votre serveur apparaît en quelques secondes. Vous bénéficiez alors pleinement du renseignement communautaire. En partageant vos signaux d’attaque (anonymisés), vous « gagnez » l’accès à la blocklist mutualisée – un échange de bons procédés qui constitue le cœur du modèle CrowdSec. Cette logique de défense collective rejoint les ambitions de la stratégie nationale de cybersécurité de la France 2026-2030 en matière de mutualisation du renseignement.

## Étape 9 – Déployer le pare-feu applicatif (AppSec / WAF)

Introduit puis consolidé dans les versions récentes, le composant **AppSec** fait de CrowdSec un véritable pare-feu applicatif web (WAF), capable d’inspecter le contenu des requêtes HTTP en temps réel – pas seulement les logs *a posteriori*. Il s’appuie sur des règles de type OWASP et sur du *virtual patching* pour bloquer les exploits connus (injections, traversées de répertoire, CVE web). Depuis la version **1.8.0**, publiée en août 2026, ce WAF embarque également une détection de bots native qui vient compléter le virtual patching pour repérer directement les comportements automatisés (scrapers, credential stuffing) sans attendre la publication d’une CVE. La réactivité de ce mécanisme s’est illustrée avec **CVE-2025-14528** : CrowdSec a publié une règle de détection dès le 18 février 2026, soit deux jours seulement avant que l’exploitation de cette faille ne soit observée massivement sur Internet – un exemple concret de virtual patching qui protège avant même que l’attaque ne se généralise. C’est une fonctionnalité absente de fail2ban et l’un des grands arguments de la migration.

```
# Installer les collections AppSec (WAF)
sudo cscli collections install crowdsecurity/appsec-virtual-patching
sudo cscli collections install crowdsecurity/appsec-generic-rules
# Déclarer l'écouteur AppSec dans l'acquisition
sudo nano /etc/crowdsec/acquis.d/appsec.yaml
```
Créez le fichier d’acquisition AppSec qui fait écouter le moteur sur un port local dédié. Le bouncer Nginx transmettra les requêtes entrantes à ce point d’inspection.

```
listen_addr: 127.0.0.1:7422
appsec_config: crowdsecurity/appsec-default
name: appsec_serveur
source: appsec
labels:
  type: appsec
```
Configurez ensuite le bouncer Nginx pour activer le mode AppSec (paramètres `APPSEC_URL` et `ENABLE_INTERNAL_APPSEC` dans son fichier de configuration), rechargez le moteur puis Nginx. Désormais, une requête contenant une charge malveillante connue est rejetée *avant* d’atteindre votre application, même si l’IP émettrice n’a jamais été vue auparavant. Validez l’écoute avec `sudo cscli metrics`, section **Appsec Metrics**.

## Étape 10 – Gérer décisions, listes blanches et durées de ban

Une protection trop agressive est aussi dangereuse qu’une protection absente : se bannir soi-même ou bloquer un partenaire légitime est l’erreur la plus coûteuse. CrowdSec gère cela via les **listes blanches** (whitelists) et les **profils** de décision. Commençons par protéger vos adresses d’administration.

Créez un parseur de liste blanche pour vos IP de confiance (poste d’admin, monitoring, plages internes) :

`sudo nano /etc/crowdsec/parsers/s02-enrich/mes-whitelists.yaml````
name: crowdsecurity/whitelists
description: "Liste blanche des IP de confiance"
whitelist:
  reason: "IP d'administration et reseau interne"
  ip:
    - "198.51.100.7"
  cidr:
    - "10.0.0.0/8"
    - "192.168.0.0/16"
```
Rechargez avec `sudo systemctl reload crowdsec`. Pour ajuster la durée des bannissements, éditez `/etc/crowdsec/profiles.yaml` : la valeur `duration` par défaut est de 4 heures. Vous pouvez aussi remplacer un simple *ban* par un *captcha* (si un bouncer compatible est installé), une approche moins frontale pour les faux positifs potentiels :

```
# Allonger un ban a 24 h pour un recidiviste
sudo cscli decisions add --ip 203.0.113.42 --duration 24h --reason "recidive"
# Appliquer un captcha plutot qu'un blocage sec
sudo cscli decisions add --ip 203.0.113.99 --type captcha --duration 1h
# Purger toutes les decisions manuelles (prudence !)
sudo cscli decisions delete --all
```
## Étape 11 – Simuler une attaque pour valider la protection

Un dispositif de sécurité non testé est un dispositif théorique. Validons que la chaîne complète – détection, décision, remédiation – fonctionne réellement. La méthode la plus sûre consiste à utiliser `cscli explain`, qui rejoue un fichier de log à travers les parseurs et scénarios sans rien bloquer, idéal pour le débogage.

```
# Rejouer le journal d'authentification a travers le moteur
sudo cscli explain --file /var/log/auth.log --type syslog
# Tester une ligne precise de log Nginx
sudo cscli explain --log '...ligne de log nginx...' --type nginx
```
Pour un test « grandeur nature » du blocage réseau, bannissez votre propre IP de test (depuis une connexion secondaire, jamais votre seule session SSH !) et vérifiez que le pare-feu la rejette. Côté web, une requête simulant une traversée de répertoire vers une URL inexistante doit générer une alerte visible dans `cscli alerts list`. Si l’alerte apparaît puis qu’une décision en découle et que l’IP est bloquée, votre projet CrowdSec est pleinement fonctionnel.

```
# Observer en direct les nouvelles decisions
watch -n 2 "sudo cscli decisions list"
# Confirmer la regle appliquee par le bouncer pare-feu (iptables)
sudo iptables -L -n | grep -i crowdsec
```
## Anatomie d’une alerte : lire une détection CrowdSec

Savoir interpréter une alerte est essentiel pour distinguer une vraie attaque d’un faux positif. La commande `cscli alerts inspect` détaille un événement : l’IP source, son pays et son AS (l’opérateur réseau), le scénario déclenché, le nombre d’événements corrélés et la décision appliquée. Voici un exemple de sortie typique pour une attaque par force brute SSH.

```
# Inspecter une alerte en detail (ID visible via cscli alerts list)
sudo cscli alerts inspect 42 -d
# Sortie (extrait) :
#  - ID            : 42
#  - Scenario      : crowdsecurity/ssh-bf
#  - Source IP     : 203.0.113.42
#  - Country       : RU
#  - AS            : 12345 EXAMPLE-AS
#  - Events count  : 6
#  - Decision      : ban (4h)
```
Chaque champ raconte une histoire. « Events count : 6 » signifie que le scénario a corrélé six échecs d’authentification avant de déclencher, ce qui écarte un simple oubli de mot de passe d’un utilisateur légitime. Le couple pays/AS aide à juger la légitimité : un AS d’hébergeur (datacenter) qui tente de se connecter en SSH est presque toujours malveillant. Aller plus loin est immédiat : `cscli alerts list --scope ip --value 203.0.113.42` agrège tout l’historique d’une adresse, et la Console affiche sa réputation communautaire – signalée par combien d’autres moteurs, depuis quand et pour quels scénarios. C’est cette mise en contexte qui transforme une simple ligne de log en renseignement réellement actionnable, et c’est précisément ce que `fail2ban` ne sait pas faire.

## Étape 12 – Docker et déploiement multi-serveur

CrowdSec brille dans les environnements conteneurisés et distribués. Pour un déploiement Docker, l’image officielle `crowdsecurity/crowdsec` permet de tout configurer via variables d’environnement – le projet publie des versions à un rythme soutenu, comme en témoigne la cadence 2025-2026 des paquets Windows référencés sur SourceForge : la 1.7.0 (75,7 Mo, 1er septembre 2025, avec une refonte de l’auto-détection et des métriques), puis la 1.7.1 (79,6 Mo, 21 octobre 2025), la 1.7.8 (90,6 Mo, 11 mai 2026), puis la **1.8.0** en août 2026, qui a justement introduit une source de logs native pour **Kubernetes** – un atout direct pour ce type de déploiement conteneurisé – avant que la **1.8.1** ne prenne le relais comme dernière version publiée sur GitHub en septembre 2026. Montez vos logs en lecture seule, persistez la base de données et la configuration, et préférez épingler un tag de version précis (idéalement 1.8.x) plutôt que `latest` pour maîtriser vos mises à jour.

```
docker run -d \
  --name crowdsec \
  -e COLLECTIONS="crowdsecurity/sshd crowdsecurity/nginx" \
  -v /var/log:/var/log:ro \
  -v crowdsec-config:/etc/crowdsec \
  -v crowdsec-data:/var/lib/crowdsec/data \
  -p 8080:8080 \
  crowdsecurity/crowdsec:v1.7.8
```
Pour une architecture **multi-serveur**, le principe est d’avoir une LAPI centrale (sur un serveur dédié ou le plus stable) et plusieurs moteurs « agents » distribués qui lui remontent leurs alertes. Chaque agent distant s’enregistre auprès de la LAPI centrale, et les bouncers de chaque machine interrogent cette même API. On obtient une vision consolidée et une remédiation cohérente sur tout le parc.

```
# Sur la LAPI centrale : creer un identifiant pour un agent distant
sudo cscli machines add agent-web-01 --auto
# Sur l'agent distant : pointer vers la LAPI centrale
# (editer les identifiants dans /etc/crowdsec/local_api_credentials.yaml)
sudo cscli lapi register --machine agent-web-01 -u http://LAPI_CENTRALE:8080
# Valider l'enregistrement cote central
sudo cscli machines list
```
Cette topologie est idéale pour les hébergeurs, les agences web et les administrateurs gérant plusieurs VPS. Elle s’articule très bien avec un réseau privé maillé : voyez notre tutoriel Tailscale VPN Mesh pour relier vos moteurs et leur LAPI centrale sur un réseau chiffré privé, sans exposer le port 8080 sur Internet.

## Aide-mémoire : les commandes cscli essentielles

Gardez ce tableau sous la main : il couvre 95 % de l’exploitation quotidienne de CrowdSec en production.

| Commande | Rôle | 
|---|---|
| `cscli metrics` | Statistiques du moteur (acquisition, parseurs, scénarios) | 
| `cscli collections list` | Lister les collections installées | 
| `cscli hub update && cscli hub upgrade` | Mettre à jour le Hub et ses éléments | 
| `cscli decisions list` | Voir les IP actuellement bannies | 
| `cscli decisions add --ip X --duration 24h` | Bannir manuellement une IP | 
| `cscli decisions delete --ip X` | Lever un bannissement | 
| `cscli alerts list` | Historique des alertes déclenchées | 
| `cscli bouncers list` | Lister les bouncers connectés | 
| `cscli console enroll CLE` | Lier le moteur à la Console | 
| `cscli explain --file ... --type ...` | Déboguer parseurs et scénarios | 
| `cscli capi status` | Tester la connexion à la blocklist communautaire | 

## Pièges courants à éviter

Au fil des déploiements, les mêmes erreurs reviennent. Les anticiper vous fera gagner des heures de débogage.

- **Oublier le bouncer.** Installer le moteur seul ne protège rien : il détecte mais ne bloque pas. Sans au moins un bouncer (pare-feu et/ou Nginx), CrowdSec n’est qu’un système d’alerte. C’est de loin l’erreur n°1.
- **Se bannir soi-même.** Lors des tests SSH, ne bannissez jamais l’IP de votre unique session ouverte. Mettez toujours votre IP d’administration en liste blanche*avant* toute simulation d’attaque.
- **Confondre iptables et nftables.** Installer le bouncer iptables sur un système qui utilise nftables (ou l’inverse) aboutit à des règles non appliquées. Vérifiez votre pare-feu avant de choisir le paquet.
- **Mauvais chemin de logs.** Si`acquis.yaml` pointe vers un fichier inexistant ou si la distribution journalise via systemd plutôt qu’un fichier plat, le moteur lit zéro ligne. Contrôlez toujours la section Acquisition de`cscli metrics` .
- **Ne pas recharger après modification.** Toute modification d’acquisition, de liste blanche ou de profil nécessite un`systemctl reload crowdsec` . Sans cela, vos changements restent lettre morte.
- **Exposer la LAPI sur Internet.** Le port 8080 doit rester en localhost ou sur un réseau privé. Ne le publiez jamais directement ; passez par un VPN ou un tunnel chiffré pour l’architecture multi-serveur.

## Dépannage : 8 problèmes fréquents et solutions

Voici les incidents les plus courants rencontrés lors d’une installation et configuration de CrowdSec, avec leur résolution.

- **Le service ne démarre pas.** Lancez`sudo journalctl -u crowdsec -n 50 --no-pager` pour lire l’erreur exacte ; une faute de syntaxe YAML dans une acquisition ou un profil est la cause la plus fréquente. Validez avec`sudo cscli config show` .
- **Aucune IP n’est bloquée malgré des alertes.** Le moteur détecte mais le bouncer ne reçoit rien : vérifiez`cscli bouncers list` et la validité de la clé API du bouncer.
- **« unable to read API key » côté bouncer.** La clé du bouncer est manquante ou erronée. Régénérez-la avec`sudo cscli bouncers add mon-bouncer` et reportez-la dans le fichier de configuration du bouncer.
- **Zéro ligne lue (Acquisition).** Le chemin de logs est faux ou la distro journalise via systemd. Ajustez`acquis.yaml` ou ajoutez une source de type`journalctl` .
- **Les règles nftables ne s’appliquent pas.** Vous avez installé la variante iptables sur un système nftables. Désinstallez et installez`crowdsec-firewall-bouncer-nftables` .
- **La Console reste « déconnectée ».** Vérifiez`cscli capi status` et que le port sortant 443 n’est pas filtré par votre pare-feu sortant.
- **Trop de faux positifs.** Identifiez l’IP fautive dans`cscli alerts list` et ajoutez-la en liste blanche, puis rechargez le moteur.
- **La mise à jour du Hub échoue.** Exécutez`sudo cscli hub update` avant`upgrade` ; un index obsolète empêche la résolution des dépendances.

## Astuces avancées pour la production

Une fois les bases en place, ces optimisations font passer votre installation de CrowdSec au niveau professionnel.

- **Souscrivez à des blocklists supplémentaires.** En plus de la blocklist communautaire gratuite, la Console permet d’abonner gratuitement votre moteur à des listes thématiques (botnets, scanners, etc.) et, en option payante, à des listes premium à très faible taux de faux positifs.
- **Automatisez les mises à jour.** Une tâche cron hebdomadaire`cscli hub update && cscli hub upgrade && systemctl reload crowdsec` maintient votre détection alignée sur les dernières menaces.
- **Surveillez avec Prometheus.** Le moteur expose des métriques Prometheus (port 6060) ; couplées à Grafana, elles offrent une supervision fine des alertes et décisions.
- **Passez à PostgreSQL pour le multi-serveur.** Au-delà de quelques agents, remplacez SQLite par PostgreSQL pour gérer la concurrence d’écriture sur la LAPI centrale.
- **Combinez plusieurs bouncers.** Pare-feu + Nginx + Cloudflare offrent une défense en profondeur : le trafic malveillant est filtré au plus tôt, soulageant votre infrastructure.
- **Durcissez l’OS en parallèle.** CrowdSec complète, sans les remplacer, les bonnes pratiques : clés SSH plutôt que mots de passe, mises à jour automatiques de sécurité et pare-feu restrictif par défaut.

## Recevoir des alertes : notifications Slack, Discord et e-mail

Surveiller un tableau de bord en permanence n’est pas réaliste. CrowdSec intègre un système de *notifications* qui pousse les alertes vers vos canaux habituels – Slack, Discord, e-mail (SMTP), Microsoft Teams ou un webhook HTTP générique. Les plugins sont fournis d’origine ; il suffit de les configurer puis de les associer à un profil de décision.

```
# Lister les plugins de notification disponibles
ls /etc/crowdsec/notifications/
# Configurer le webhook HTTP (Slack, Discord, Teams...)
sudo nano /etc/crowdsec/notifications/http.yaml
# Activer la notification dans le profil de decision
sudo nano /etc/crowdsec/profiles.yaml
#   notifications:
#     - http_default
sudo systemctl reload crowdsec
```
Dans `profiles.yaml`, la clé `notifications` liste les canaux déclenchés à chaque décision. Vous pouvez filtrer pour n’être alerté que sur les bannissements de longue durée ou les scénarios critiques, afin d’éviter la fatigue d’alerte. Couplé à la Console et à un export Prometheus/Grafana, ce mécanisme ferme la boucle d’observabilité : détection, remédiation et, désormais, notification proactive de votre posture de sécurité. C’est l’un des chaînons qui font passer CrowdSec du simple outil défensif à une véritable plateforme de supervision.

## CrowdSec, RGPD et souveraineté numérique européenne

Pour les organisations européennes, CrowdSec présente un argument rarement mis en avant : c’est une solution de cybersécurité **européenne**, open source et transparente sur le traitement des données. À l’heure où la dépendance aux fournisseurs extra-européens est scrutée de près, déployer un moteur de détection français édité sous licence MIT s’inscrit pleinement dans une démarche de souveraineté numérique.

La question du RGPD se pose légitimement, puisqu’une adresse IP est une donnée à caractère personnel au sens de la CNIL. CrowdSec a conçu son modèle de partage en conséquence : lorsque vous contribuez à la communauté, seuls des signaux d’attaque réduits et pseudonymisés sont transmis (l’IP de l’attaquant, le scénario déclenché et un horodatage), et non le contenu de vos logs ni les données de vos utilisateurs légitimes. Le partage reste par ailleurs configurable. Cette approche défensive collective fait écho aux orientations de l’ANSSI et au renforcement de la posture cyber nationale.

Concrètement, pour une mise en conformité propre : documentez l’usage de CrowdSec dans votre registre de traitements, mentionnez la finalité (sécurité du système d’information, base légale de l’intérêt légitime), et configurez vos durées de rétention des alertes. L’open source facilite ici l’audit : le code étant public sur GitHub, rien n’est opaque. Pour aller plus loin sur la gestion centralisée des secrets et identités, notre guide Vaultwarden : auto-héberger Bitwarden complète idéalement cette démarche d’autonomie.

## Récapitulatif : votre projet CrowdSec complet

Voici la check-list du projet fonctionnel que vous venez de bâtir. Si chaque case est cochée, votre serveur dispose d’une défense collaborative de bout en bout, des couches réseau et applicative jusqu’à la supervision.

- **Moteur de sécurité installé** et actif (`cscli metrics` affiche des lignes lues).
- **Bouncer pare-feu** connecté à la LAPI (`cscli bouncers list` ).
- **SSH protégé** via la collection`crowdsecurity/sshd` .
- **Nginx protégé** en détection (collection) et en remédiation (bouncer Nginx).
- **AppSec / WAF** à l’écoute sur 127.0.0.1:7422 pour le virtual patching.
- **Console enrôlée** et CTI communautaire actif (`cscli capi status` ).
- **Listes blanches** en place pour vos IP d’administration.
- **Notifications** branchées sur Slack, Discord ou e-mail.
- **Mises à jour du Hub** automatisées par une tâche cron.

Ce socle couvre l’essentiel d’une protection sérieuse. Pour une infrastructure plus large, dupliquez le moteur sur chaque hôte et centralisez la LAPI comme décrit à l’étape 12, puis pilotez l’ensemble depuis une seule Console.

## Foire aux questions (FAQ)

### CrowdSec est-il vraiment gratuit ?

Oui. Le moteur de sécurité, les bouncers, le Hub, la blocklist communautaire et la Console (formule de base) sont gratuits et open source sous licence MIT. Seules certaines blocklists premium et des fonctionnalités d’entreprise sur la Console relèvent d’offres payantes, totalement optionnelles pour un usage personnel ou PME : depuis mai 2026, la grille tarifaire de CrowdSec propose par exemple un accès API au CTI à partir de **49 $/mois** pour 5 000 requêtes, des Platinum Blocklists à **900 $/mois** pour un usage individuel ou **3 900 $/mois** en usage illimité, une réplication locale du CTI (Local CTI Replication) à **9 000 $/mois**, ainsi qu’une offre Console Premium à **1 000 $/mois** – soit six formules payantes actives recensées en septembre 2026, aux côtés de l’offre gratuite qui reste, elle, inchangée.

### CrowdSec remplace-t-il mon pare-feu ?

Non, il le complète. CrowdSec décide *quelles* IP bloquer ; c’est votre pare-feu (via le bouncer iptables/nftables) qui applique le blocage. Conservez une configuration de pare-feu restrictive par défaut ; CrowdSec ajoute une couche dynamique de détection comportementale par-dessus.

### Quelle différence entre l’agent et le bouncer ?

L’agent (le moteur de sécurité) *détecte* en analysant les logs ; le bouncer *agit* en appliquant les décisions. C’est la séparation détection/remédiation, qui permet d’avoir un seul moteur et plusieurs bouncers de natures différentes (réseau, web, CDN).

### CrowdSec ralentit-il le serveur ?

L’empreinte est minime : quelques dizaines de mégaoctets de RAM et une charge CPU négligeable au repos. Écrit en Go, le moteur traite les logs de façon asynchrone. Sur un VPS modeste, l’impact sur les performances est imperceptible pour la grande majorité des charges de travail.

### Faut-il partager ses données pour utiliser CrowdSec ?

Le partage de signaux d’attaque pseudonymisés est le principe par défaut et conditionne l’accès à la blocklist communautaire, mais il reste paramétrable. Aucune donnée de vos utilisateurs légitimes ni le contenu de vos logs ne sont transmis : seuls l’IP de l’attaquant, le scénario et un horodatage circulent.

### CrowdSec fonctionne-t-il sous Windows, pfSense ou OPNsense ?

Oui. Au-delà de Linux, CrowdSec propose des paquets pour Windows, FreeBSD et s’intègre aux plateformes pare-feu comme OPNsense et pfSense via des plugins communautaires, ainsi qu’à Docker et Kubernetes. Sur OPNsense, la compatibilité du plugin CrowdSec avec la version 26.1.2 du firewall a d’ailleurs fait l’objet d’échanges sur le forum officiel OPNsense début février 2026, signe d’une communauté active qui suit de près chaque mise à jour majeure. Le moteur et le concept de bouncers restent identiques quelle que soit la plateforme.

### Comment mettre à jour CrowdSec et ses détections ?

Mettez à jour le moteur via votre gestionnaire de paquets (`sudo apt upgrade crowdsec`) et les détections via le Hub (`cscli hub update && cscli hub upgrade`), suivi d’un `systemctl reload crowdsec`. À titre de repère, le guide complet publié par LumaDock en juin 2026 ciblait déjà la version 1.7.8 du Security Engine, qui restait la ligne stable de référence en juillet 2026 – mais la branche a depuis évolué avec la 1.8.0 en août 2026 (détection de bots dans le WAF, source Kubernetes native) puis la 1.8.1, devenue en septembre 2026 la dernière version publiée sur GitHub : un bon indicateur pour vérifier que votre propre déploiement n’a pas pris de retard. Idéalement, automatisez la mise à jour du Hub par une tâche cron hebdomadaire.

### CrowdSec est-il compatible RGPD ?

Oui, à condition de le documenter correctement. Le traitement repose sur l’intérêt légitime (sécurité du SI), seules des données minimales et pseudonymisées sont partagées, et le code open source permet un audit complet. Inscrivez l’usage dans votre registre de traitements et définissez vos durées de rétention pour une conformité sereine.

### Pour aller plus loin – Articles liés

- Tutoriel WireGuard 2026 : VPN Linux en 12 étapes – chiffrez et réduisez la surface d’attaque de vos accès distants.
- Tutoriel Tailscale : VPN Mesh en 13 étapes – reliez vos moteurs et leur LAPI centrale sur un réseau privé.
- Vaultwarden : auto-héberger Bitwarden en 12 étapes – reprenez le contrôle de vos mots de passe et secrets.
- Faille Oracle PeopleSoft : 9,8/10, 100 cibles – pourquoi le virtual patching applicatif est crucial.
- Stratégie nationale de cybersécurité France 2026-2030 – le contexte souverain de la défense collective.
- Toute notre rubrique Cybersécurité – guides, alertes et analyses.

En suivant ces 12 étapes, vous avez transformé un serveur Linux exposé en cible activement défendue : SSH et Nginx protégés, pare-feu applicatif en place, supervision centralisée et accès à un renseignement sur les menaces alimenté par 23 000 organisations. CrowdSec illustre une idée simple mais puissante – face à des attaquants qui coopèrent, les défenseurs ont tout intérêt à coopérer aussi. Et avec une solution française, open source et frugale, cette défense collective est désormais à la portée de tous, du homelab à l’infrastructure de production.
