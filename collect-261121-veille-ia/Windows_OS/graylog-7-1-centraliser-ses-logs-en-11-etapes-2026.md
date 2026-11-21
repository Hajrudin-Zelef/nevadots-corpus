---
id: collect-261121-veille-ia/windows-os/graylog-7-1-centraliser-ses-logs-en-11-etapes-2026
title: "Graylog 7.1 : Centraliser ses Logs en 11 Étapes [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["logs", "graylog", "centraliser", "serveur", "securite", "etapes", "open", "source"]
source: https://tech-insider.org/fr/graylog-centraliser-logs-securite-2026/
source_anchor: ""
source_lines: [1, 389]
section: "Windows_OS"
---
Un serveur piraté laisse toujours une trace dans les logs, sauf que personne ne les regarde avant qu’il soit trop tard. Le ministère de l’Intérieur a recensé 453 200 cyberattaques en France en 2025, soit une hausse de 87 % en cinq ans selon son rapport annuel sur la cybercriminalité. Face à ce volume, l’ANSSI pousse depuis 2026 les entreprises vers des outils de sécurité open source plutôt que vers des solutions propriétaires coûteuses. Graylog fait partie des rares plateformes capables de centraliser, indexer et analyser des millions de lignes de logs sans les frais de licence d’un Splunk. Ce tutoriel installe Graylog 7.1.8, publiée le 18 août 2026 et toujours la version la plus largement documentée à ce jour, sachant que la page de téléchargement officielle affiche désormais la 7.1.9 comme dernier correctif de la branche depuis sa mise à jour du 2 septembre 2026, sur un serveur Linux avec Docker, puis configure la collecte, le traitement et l’alerte en 11 étapes concrètes.

À la fin de ce guide, vous disposerez d’une pile fonctionnelle capable d’ingérer des dizaines de milliers de messages par seconde, de les structurer via des pipelines, et de déclencher une alerte dès qu’un comportement suspect apparaît. Le tout tient sur un seul serveur avec un investissement de temps d’environ 90 minutes, sans dépendre d’un fournisseur SaaS ni d’un budget de licence annuel.

## Pourquoi centraliser ses logs de sécurité avec Graylog en 2026

Un pare-feu, un serveur web, un annuaire Active Directory et une dizaine de machines Linux génèrent chacun leurs propres journaux, dans des formats différents, stockés sur des disques différents. Sans centralisation, un administrateur doit se connecter à chaque machine pour reconstituer la chronologie d’un incident. C’est précisément le scénario que décrit l’ENISA dans son Threat Landscape 2025 (version 1.2) : l’ingénierie sociale reste le point d’entrée principal des attaquants dans l’écosystème européen, et la détection tardive amplifie les dégâts.

Graylog répond à ce problème en agrégeant tous les flux de logs (syslog, GELF, logs applicatifs, logs Docker, logs Windows via NXLog) dans un moteur de recherche unique basé sur OpenSearch. L’intérêt n’est pas seulement le stockage : c’est la capacité à écrire une seule requête pour retrouver une tentative de connexion SSH échouée sur douze serveurs en même temps, ou à déclencher une alerte automatique dès qu’un compte utilisateur échoue cinq fois de suite. Sur le marché français, l’ANSSI a mis à jour sa politique open source en 2026 autour de quatre axes : publier des logiciels de sécurité sous licence libre, contribuer aux projets externes, renforcer l’écosystème et utiliser des solutions open source en interne. Graylog coche toutes ces cases pour une PME ou une ETI qui ne dispose pas du budget d’un SOC dédié.

Le panorama de la cybermenace publié par le CERT-FR confirme cette tendance : les incidents détectés tardivement, faute de centralisation des journaux, restent parmi les plus coûteux à traiter pour les entités concernées. Les métiers de la cybersécurité recrutent en masse en France en 2026 : ingénieurs cloud security, analystes SOC, responsables conformité NIS2. Tous ont un point commun, ils passent leurs journées dans un outil de gestion de logs. Savoir déployer et exploiter Graylog est donc une compétence directement monétisable, que l’on gère l’infrastructure d’une entreprise ou que l’on prépare une certification en sécurité opérationnelle.

## Graylog 7.1.8 : nouveautés, versions et calendrier de support

La branche 7.1 de Graylog est passée en disponibilité générale le 4 mai 2026, sous le nom de code “Spring 2026 Release” que l’éditeur a présenté comme son premier SIEM “AI-powered”. Elle a ensuite reçu une série de correctifs rapprochés : 7.1.1 le 8 mai, 7.1.3 le 3 juin, 7.1.5 le 8 juillet, 7.1.7 le 5 août, 7.1.8 le 18 août 2026 (la version que ce tutoriel installe), puis 7.1.9, devenue le dernier correctif listé sur la page de téléchargement officielle depuis sa mise à jour du 2 septembre 2026. Graylog a même publié une première bêta de la branche suivante, la 7.2.0-beta.1, dès le 31 août 2026, pour les équipes qui veulent tester en avance les prochaines fonctions. Cette cadence de publication mensuelle montre un projet activement maintenu, ce qui compte quand on choisit un outil de sécurité destiné à tourner plusieurs années en production.

La branche 7.1 introduit deux capacités qui rapprochent Graylog d’un véritable SIEM : les investigations automatisées et la détection native d’anomalies comportementales. Concrètement, l’outil ne se contente plus d’indexer des logs, il peut désormais regrouper automatiquement les événements liés à un même incident et signaler un comportement utilisateur qui s’écarte de la normale, sans règle de corrélation écrite à la main. Ces fonctions s’appuient sur les content packs Illuminate, dont la version 7.0.7 est sortie le 8 juin 2026 avec de nouveaux packs de traitement et des “spotlights” orientés détection.

| Branche | Date de sortie | Dernier correctif | Fin de support | Statut | 
|---|---|---|---|---|
| Graylog 7.1 | 4 mai 2026 | 7.1.8 (18 août 2026) | 4 mai 2027 (support entreprise jusqu’au 4 mai 2028) | Supportée, recommandée | 
| Graylog 7.0 | 3 novembre 2025 | 7.0.12 (5 août 2026) | 3 novembre 2026 | Supportée, en fin de vie proche | 
| Graylog 6.3 | 30 juin 2025 | 6.3.15 (5 août 2026) | 30 juin 2026 | Support terminé | 

Ce tableau justifie à lui seul le choix de la version 7.1.8 pour toute nouvelle installation : la branche 6.3 est déjà en fin de vie, et la 7.0 approche de son terme en novembre 2026. Un point technique à connaître avant de migrer : la matrice de compatibilité officielle exige au minimum MongoDB 7.x (maximum 8.2.x) et OpenSearch 2.0.x (maximum 2.19.5) pour les branches 7.0.x et 7.1.x. Graylog a définitivement abandonné le support d’Elasticsearch au profit d’OpenSearch depuis la version 5.0, un détail qui piège encore des administrateurs qui suivent d’anciens tutoriels.

## Graylog vs Wazuh vs ELK Stack : quel outil de logs choisir

Avant de se lancer dans l’installation, il faut savoir si Graylog est le bon outil pour le besoin. Trois familles de solutions open source dominent le marché de la gestion de logs et de la détection en 2026. Wazuh, en version 4.14.7 publiée le 29 juillet 2026, mise sur des agents installés directement sur chaque machine surveillée, ce qui en fait un outil de détection d’intrusion basée sur l’hôte (HIDS) avant d’être un outil de logs. Elastic, avec Elasticsearch 9.5.1 et Elastic Security 9.5 sorties le 3 août 2026, propose l’écosystème le plus riche mais demande le plus de travail d’intégration pour obtenir une expérience SIEM comparable à un produit clé en main.

Graylog se positionne entre les deux : il est plus léger et moins cher à exploiter qu’un Splunk sur le plan des licences, tout en offrant une interface de sécurité plus aboutie qu’un stack ELK construit à la main. Sa force réside dans le traitement structuré des logs via des pipelines, et dans le data tiering introduit en version 6.0, qui déplace automatiquement les données anciennes vers un stockage moins coûteux tout en gardant tout consultable. Ce mécanisme réduit la facture de stockage d’un ordre de grandeur par rapport à un index unique conservé indéfiniment sur du SSD rapide.

| Critère | Graylog 7.1 | Wazuh 4.14.7 | ELK / Elastic 9.5 | 
|---|---|---|---|
| Approche | Centralisation de logs + SIEM | Agents HIDS + SIEM | Recherche et analytics + module sécurité | 
| Composants requis | MongoDB + OpenSearch | Indexer + Manager + agents | Elasticsearch + Kibana + Beats | 
| Édition gratuite | Graylog Open | Complet, open source | Basic (fonctions limitées) | 
| Courbe d’apprentissage | Modérée | Modérée à élevée | Élevée | 
| Cas d’usage idéal | Centralisation multi-source, pipelines | Surveillance de postes et serveurs | Recherche full-text à grande échelle | 

Dans une architecture de sécurité mature, ces outils ne s’excluent pas. Beaucoup d’équipes font remonter les alertes de CrowdSec ou les résultats d’un scan OpenVAS/Greenbone vers Graylog pour disposer d’une vue unique. C’est cette approche que ce tutoriel construit pas à pas.

Le choix dépend surtout de la maturité de l’équipe qui exploitera l’outil au quotidien. Une petite structure qui débute dans la gestion de logs gagnera du temps avec Graylog, dont le pipeline de traitement et les dashboards se configurent depuis l’interface web sans écrire de requête complexe. Une équipe qui gère déjà un stack ELK pour d’autres usages (recherche produit, analytics métier) aura intérêt à mutualiser avec Elastic Security plutôt qu’ajouter une quatrième brique. Wazuh reste imbattable dès que la détection doit descendre au niveau du poste de travail, avec surveillance de l’intégrité des fichiers et détection de rootkits, une couche que ni Graylog ni Elastic ne couvrent nativement sans agent dédié.

## Prérequis : matériel, logiciels et versions nécessaires

Cette installation a été testée sur Ubuntu Server 24.04 LTS, mais fonctionne à l’identique sur Debian 12. Voici la configuration minimale recommandée pour un usage réel, pas seulement une démo.

- Système d’exploitation : Ubuntu Server 24.04 LTS ou Debian 12 (64 bits)
- RAM : 8 Go minimum pour un lab, 16 Go recommandés en production (OpenSearch consomme la majorité)
- CPU : 4 vCPU minimum
- Stockage : 50 Go de SSD pour un test, plusieurs centaines de Go en production selon le volume de logs ingérés
- Docker Engine version 27.x ou supérieure
- Docker Compose v2 (intégré au plugin docker-compose-plugin)
- MongoDB 8.0.x (compatible avec la plage 7.x à 8.2.x exigée par Graylog 7.1)
- OpenSearch 2.19.x (dernière version compatible avec Graylog 7.1.x)
- Graylog 7.1.8, image Docker officielle
- Accès root ou sudo sur le serveur
- Ports ouverts : 9000 (interface web), 12201 (GELF UDP/TCP), 1514 (Syslog), 27017 (MongoDB, interne uniquement)

Un point de vigilance mémoire : OpenSearch refuse de démarrer si la limite `vm.max_map_count` du noyau Linux est trop basse. C’est l’une des causes d’échec les plus fréquentes, traitée dans la section dépannage plus bas. La matrice de compatibilité officielle de Graylog reste la référence à consulter avant tout choix de version, car elle change à chaque montée de branche majeure.

## Dimensionner son serveur selon le volume de logs attendu

Le dimensionnement matériel dépend presque entièrement du volume quotidien de messages ingérés, pas du nombre de serveurs surveillés. Un parc de dix machines Linux qui produit surtout des logs d’authentification et des journaux applicatifs légers reste sous la barre du gigaoctet par jour. À l’inverse, un pare-feu d’entreprise qui journalise chaque connexion réseau, ou une flotte de conteneurs Kubernetes en mode debug, peut facilement dépasser 20 Go par jour à lui seul.

| Volume quotidien de logs | RAM recommandée | vCPU | Stockage SSD (rétention 30 jours) | 
|---|---|---|---|
| Moins de 1 Go/jour (lab, PME < 20 postes) | 8 Go | 4 | 50 Go | 
| 1 à 5 Go/jour (PME, quelques serveurs critiques) | 16 Go | 4 à 6 | 200 Go | 
| 5 à 20 Go/jour (ETI, infrastructure hybride) | 32 Go | 8 | 750 Go, cluster OpenSearch recommandé | 
| Plus de 20 Go/jour | 64 Go et plus, cluster multi-nœuds | 16+ | Plusieurs To, data tiering obligatoire | 

Ces chiffres restent des ordres de grandeur, pas des garanties absolues. Un pipeline mal écrit qui applique des expressions régulières coûteuses sur chaque message peut faire chuter le débit d’ingestion de moitié, même sur un serveur correctement dimensionné selon ce tableau. Il vaut toujours mieux commencer avec une marge de 30 % au-dessus de l’estimation initiale et surveiller la charge CPU d’OpenSearch pendant les deux premières semaines d’exploitation réelle.

## Étape 1 : Préparer le serveur et installer Docker

Sur un serveur fraîchement provisionné, on commence par mettre à jour le système puis installer Docker Engine depuis le dépôt officiel, pas depuis le paquet `docker.io` d’Ubuntu qui traîne souvent une version obsolète.

```
sudo apt update && sudo apt upgrade -y
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
docker --version
docker compose version
```
Il faut ensuite ajuster deux réglages noyau indispensables à OpenSearch : la limite de zones mémoire mappées et le nombre de fichiers ouverts. Sans ce réglage, le conteneur OpenSearch s’arrête immédiatement au démarrage avec une erreur “max virtual memory areas vm.max_map_count is too low”.

```
echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.conf
sudo sysctl -w vm.max_map_count=262144
echo "* soft nofile 65536" | sudo tee -a /etc/security/limits.conf
echo "* hard nofile 65536" | sudo tee -a /etc/security/limits.conf
```
## Étape 2 : Rédiger le fichier Docker Compose pour MongoDB, OpenSearch et Graylog

L’architecture de Graylog repose sur trois briques distinctes qui doivent communiquer entre elles : MongoDB stocke la configuration et les métadonnées (jamais les logs eux-mêmes), OpenSearch indexe et stocke le contenu des messages, et le serveur Graylog orchestre le tout via son interface web et son moteur de traitement. On les déploie ensemble avec un seul fichier `docker-compose.yml`.

Créez d’abord un répertoire de travail et un fichier `.env` pour ne jamais coder les secrets en dur.

```
mkdir -p ~/graylog-stack && cd ~/graylog-stack
# Génère un secret de session Graylog (obligatoire, 16+ caractères)
echo "GRAYLOG_PASSWORD_SECRET=$(openssl rand -hex 32)" > .env
# Génère le hash SHA-256 du mot de passe admin (remplacez MonMotDePasse!2026)
echo "GRAYLOG_ROOT_PASSWORD_SHA2=$(echo -n 'MonMotDePasse!2026' | sha256sum | cut -d' ' -f1)" >> .env
```
Le fichier `docker-compose.yml` suivant déploie la pile complète avec des volumes persistants, essentiels pour ne pas perdre les logs au moindre redémarrage de conteneur.

```
services:
  mongodb:
    image: mongo:8.0
    container_name: graylog-mongodb
    restart: unless-stopped
    volumes:
      - mongodb_data:/data/db
    networks:
      - graylog-net
  opensearch:
    image: opensearchproject/opensearch:2.19.5
    container_name: graylog-opensearch
    restart: unless-stopped
    environment:
      - "OPENSEARCH_JAVA_OPTS=-Xms2g -Xmx2g"
      - "discovery.type=single-node"
      - "plugins.security.disabled=true"
      - "bootstrap.memory_lock=true"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - opensearch_data:/usr/share/opensearch/data
    networks:
      - graylog-net
  graylog:
    image: graylog/graylog:7.1.8
    container_name: graylog-server
    restart: unless-stopped
    depends_on:
      - mongodb
      - opensearch
    environment:
      - GRAYLOG_PASSWORD_SECRET=${GRAYLOG_PASSWORD_SECRET}
      - GRAYLOG_ROOT_PASSWORD_SHA2=${GRAYLOG_ROOT_PASSWORD_SHA2}
      - GRAYLOG_HTTP_EXTERNAL_URI=http://localhost:9000/
      - GRAYLOG_MONGODB_URI=mongodb://mongodb:27017/graylog
      - GRAYLOG_ELASTICSEARCH_HOSTS=http://opensearch:9200
    ports:
      - "9000:9000"
      - "12201:12201/udp"
      - "12201:12201/tcp"
      - "1514:1514/udp"
    volumes:
      - graylog_data:/usr/share/graylog/data
    networks:
      - graylog-net
networks:
  graylog-net:
    driver: bridge
volumes:
  mongodb_data:
  opensearch_data:
  graylog_data:
```
Notez l’usage de l’image `graylog/graylog:7.1.8` et non un tag `latest` : figer la version évite qu’une mise à jour majeure surprenne la production un lundi matin.

## Étape 3 : Générer les secrets et démarrer la pile Graylog

Une fois le fichier en place, le démarrage se fait en une seule commande. Le premier lancement prend plusieurs minutes le temps qu’OpenSearch initialise ses index internes.

```
docker compose up -d
docker compose ps
docker compose logs -f graylog
```
Résultat attendu dans `docker compose ps` une fois la pile stabilisée :

```
NAME                 STATUS
graylog-mongodb      Up 2 minutes
graylog-opensearch    Up 2 minutes (healthy)
graylog-server        Up 1 minute
```
Si le conteneur `graylog-server` redémarre en boucle, c’est presque toujours parce qu’il a démarré avant qu’OpenSearch soit prêt à répondre. Patientez deux minutes supplémentaires avant de conclure à un problème de configuration.

## Étape 4 : Première connexion et configuration initiale de l’interface

Ouvrez un navigateur sur `http://IP_DU_SERVEUR:9000`. L’identifiant par défaut est `admin`, et le mot de passe est celui saisi en clair à l’étape 2 avant son hachage SHA-256 (dans notre exemple, `MonMotDePasse!2026`). Changez-le immédiatement depuis le menu “System > Users” si ce serveur est exposé au-delà d’un réseau de test.

La première tâche dans l’interface consiste à vérifier que l’index par défaut fonctionne, via “System > Indices”. Vous devez voir l’index set “Default index set” en statut vert, preuve que la communication avec OpenSearch est opérationnelle. C’est aussi le bon moment pour activer l’authentification à deux facteurs sur le compte admin, une pratique désormais recommandée par l’ANSSI pour tout accès à un outil de sécurité critique. Ceux qui utilisent déjà un dispositif de conformité NIS2 retrouveront ici une des exigences de traçabilité des accès imposées par la directive.

## Étape 5 : Configurer les inputs GELF et Syslog

Graylog ne reçoit aucun log tant qu’aucun “input” n’est déclaré. Un input est un point d’écoute réseau qui accepte un format de message précis. Rendez-vous dans “System > Inputs”, sélectionnez “GELF UDP” dans la liste déroulante, puis cliquez sur “Launch new input”. Renseignez le port 12201 et laissez l’adresse d’écoute sur 0.0.0.0 pour accepter les connexions de tout le réseau interne.

Répétez l’opération avec un input “Syslog UDP” sur le port 1514, utile pour les équipements réseau (pare-feu, switches) qui ne parlent que le protocole Syslog historique et ne savent pas produire du GELF. Une fois les deux inputs actifs, l’écran “System > Inputs” doit afficher un statut “Running” en vert pour chacun, avec un compteur de messages qui reste à zéro tant qu’aucune source n’envoie de données.

## Étape 6 : Envoyer les logs Linux et applicatifs vers Graylog

La méthode la plus simple pour un serveur Linux existant consiste à reconfigurer rsyslog pour qu’il duplique ses journaux vers Graylog en plus du stockage local. Éditez `/etc/rsyslog.d/60-graylog.conf` sur chaque machine à surveiller.

```
# /etc/rsyslog.d/60-graylog.conf
# Envoie tous les logs systeme vers Graylog en Syslog UDP
*.* @IP_DU_SERVEUR_GRAYLOG:1514
# Puis redemarrer le service
# sudo systemctl restart rsyslog
```
Pour les conteneurs Docker, la méthode recommandée passe par le driver de logging GELF natif, qui évite d’installer un agent supplémentaire dans chaque conteneur.

```
docker run -d \
  --log-driver=gelf \
  --log-opt gelf-address=udp://IP_DU_SERVEUR_GRAYLOG:12201 \
  --name mon-application \
  mon-image:latest
```
Pour Nginx, ajoutez simplement une règle rsyslog dédiée qui capte les fichiers `access.log` et `error.log` via un module imfile, ou redirigez directement le format de log Nginx vers syslog dans `nginx.conf` avec la directive `error_log syslog:server=IP_DU_SERVEUR_GRAYLOG:1514 warn;`. Une fois ces sources connectées, retournez sur “Search” dans l’interface Graylog : les messages doivent apparaître en quelques secondes, classés par ordre chronologique inverse.

## Étape 7 : Créer des pipelines et extracteurs pour structurer les logs

Un log brut sous forme de texte libre est difficile à interroger. Les pipelines Graylog appliquent des règles de traitement qui extraient des champs structurés (adresse IP source, code de statut HTTP, nom d’utilisateur) directement exploitables dans les recherches et les dashboards. Rendez-vous dans “System > Pipelines” pour créer une règle qui repère les tentatives de connexion SSH échouées.

```
rule "extraire echec SSH"
when
  contains(to_string($message.message), "Failed password")
then
  let extracted_ip = regex("from ([0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3}\\.[0-9]{1,3})", to_string($message.message));
  set_field("source_ip", extracted_ip["0"]);
  set_field("event_type", "ssh_auth_failure");
end
```
Une fois cette règle attachée au pipeline par défaut et connectée au flux “All messages”, chaque tentative de connexion SSH échouée génère automatiquement un champ `source_ip` et un champ `event_type` filtrables. C’est la base d’une détection de brute-force efficace, le même principe que celui utilisé par CrowdSec pour bloquer les attaques, mais ici appliqué à des fins d’investigation a posteriori plutôt que de blocage en temps réel.

## Étape 8 : Construire des dashboards et alertes de sécurité

Un dashboard Graylog agrège plusieurs widgets (graphiques, compteurs, cartes) sur un seul écran. Depuis “Dashboards > Create dashboard”, ajoutez un widget de type “Aggregation” filtré sur `event_type:ssh_auth_failure`, groupé par `source_ip`, pour visualiser en un coup d’œil quelles adresses martèlent vos serveurs.

Les alertes se configurent dans “Alerts > Event Definitions”. Créez une définition qui déclenche une notification dès que plus de 10 échecs SSH proviennent de la même IP sur une fenêtre de 5 minutes. Graylog 7.1 permet de router cette alerte vers un webhook, un e-mail SMTP, ou directement vers un canal Slack ou Microsoft Teams via une intégration HTTP.

```
curl -X POST http://localhost:9000/api/events/definitions \
  -u admin:MonMotDePasse!2026 \
  -H "Content-Type: application/json" \
  -H "X-Requested-By: cli" \
  -d '{
    "title": "Brute-force SSH detecte",
    "priority": 3,
    "config": {
      "type": "aggregation-v1",
      "query": "event_type:ssh_auth_failure",
      "group_by": ["source_ip"],
      "series": [{"function": "count"}],
      "conditions": {"expression": {"expr": ">", "left": {"expr": "number-ref", "ref": "count()"}, "right": {"expr": "number", "value": 10}}},
      "search_within_ms": 300000,
      "execute_every_ms": 60000
    }
  }'
```
C’est précisément ce type de règle qui exploite les capacités de détection comportementale natives ajoutées dans la branche 7.1, sans avoir besoin d’écrire de corrélation manuelle complexe comme c’était le cas dans les versions antérieures à 6.0.

## Étape 9 : Rétention des index, data tiering et volumétrie

Sans politique de rétention, un index Graylog grossit indéfiniment jusqu’à saturer le disque. Dans “System > Indices > Default index set”, configurez une rotation basée sur la taille (par exemple 5 Go par index) ou sur le temps (rotation quotidienne), puis définissez le nombre maximum d’index conservés avant suppression automatique. Une configuration courante pour une PME conserve 30 jours de logs “chauds” consultables instantanément, puis archive au-delà.

Le data tiering introduit en version 6.0 et toujours actif en 7.1 permet de basculer automatiquement les index les plus anciens vers un stockage “warm” moins coûteux (disque plus lent ou stockage objet), tout en les gardant interrogeables sans réimport manuel. Sur un volume de 50 Go de logs par jour, cette fonction peut diviser par dix le coût de stockage à un an par rapport à tout conserver sur du SSD rapide.

## Étape 10 : Durcir l’instance avec TLS, RBAC et pare-feu

Une instance Graylog qui centralise des logs de sécurité devient elle-même une cible de choix. Trois mesures de durcissement s’imposent avant toute mise en production.

D’abord, activez TLS sur l’interface web en générant un certificat (Let’s Encrypt ou une autorité interne) et en le déclarant via la variable `GRAYLOG_HTTP_EXTERNAL_URI=https://logs.monentreprise.fr/` couplée à un reverse proxy Nginx qui termine le TLS. Ensuite, créez des rôles distincts via “System > Roles” : un analyste SOC n’a pas besoin des droits d’administration système, seulement d’un accès en lecture aux dashboards et à la recherche. Enfin, restreignez au pare-feu l’accès aux ports 12201 et 1514 aux seules IP internes autorisées à envoyer des logs.

```
# Exemple avec UFW sur le serveur Graylog
sudo ufw allow from 10.0.0.0/24 to any port 12201 proto udp
sudo ufw allow from 10.0.0.0/24 to any port 1514 proto udp
sudo ufw allow from 10.0.0.0/24 to any port 9000 proto tcp
sudo ufw deny 12201
sudo ufw deny 1514
sudo ufw enable
```
## Étape 11 : Sauvegarder, mettre à jour et superviser Graylog

Trois éléments doivent être sauvegardés régulièrement : la base MongoDB (configuration, utilisateurs, dashboards), les snapshots OpenSearch (les données de logs elles-mêmes), et le fichier `docker-compose.yml` avec son `.env`. Un script cron simple suffit pour la partie MongoDB.

```
#!/bin/bash
# backup-graylog-mongo.sh
DATE=$(date +%Y%m%d)
docker exec graylog-mongodb mongodump --archive=/tmp/backup-$DATE.gz --gzip
docker cp graylog-mongodb:/tmp/backup-$DATE.gz /backups/graylog/mongo-$DATE.gz
find /backups/graylog/ -name "mongo-*.gz" -mtime +30 -delete
```
Pour la mise à jour vers une future version 7.1.x, comme la 7.1.9 apparue sur la page de téléchargement officielle le 2 septembre 2026, ne modifiez jamais le tag d’image en production sans avoir d’abord testé la migration sur un environnement de préproduction, et sauvegardez systématiquement avant de lancer `docker compose pull && docker compose up -d`. Évitez en revanche de basculer directement sur la bêta 7.2.0-beta.1, mise en ligne le 31 août 2026, tant qu’elle n’a pas atteint une disponibilité générale stable. Consultez toujours la calendrier de fin de vie des versions Graylog avant une montée de version majeure, car les exigences MongoDB et OpenSearch changent d’une branche à l’autre.

## Conservation des logs et conformité RGPD

Centraliser des logs de sécurité soulève une question juridique que beaucoup d’administrateurs négligent : ces journaux contiennent souvent des adresses IP, des identifiants de connexion, parfois des noms d’utilisateur complets, autant de données à caractère personnel au sens du RGPD. La CNIL recommande une durée de conservation proportionnée à la finalité du traitement, généralement limitée à six mois pour des logs de connexion à finalité de sécurité, sauf obligation légale contraire ou enquête en cours.

Concrètement, cela signifie que la politique de rétention configurée à l’étape 9 n’est pas qu’un paramètre technique de gestion d’espace disque : c’est aussi un engagement de conformité. Documentez la durée choisie, la justification métier associée, et assurez-vous que les index archivés en tier “warm” restent purgés au terme de la période retenue plutôt que conservés indéfiniment par simple oubli. Pour les entités soumises à la directive NIS2, cette traçabilité documentée fait partie des éléments vérifiés lors d’un contrôle par l’ANSSI, qui recommande depuis 2026 l’usage d’outils open source justement pour garder une maîtrise complète de la chaîne de traitement des données, sans dépendre d’un hébergeur tiers hors UE.

## Projet complet : une architecture de supervision prête pour une PME

En assemblant les onze étapes précédentes, voici l’architecture complète obtenue à la fin de ce tutoriel : un serveur Graylog 7.1.8 conteneurisé qui reçoit les logs Syslog des pare-feu et switches, les logs GELF des applications Docker, et les journaux systeme de chaque serveur Linux via rsyslog. Un pipeline extrait automatiquement les tentatives de connexion suspectes, un dashboard centralise la vue d’ensemble, et une alerte prévient l’équipe dès qu’un seuil de brute-force est franchi. La rétention des logs suit une politique à 30 jours “chauds” avec data tiering au-delà, et l’accès est protégé par TLS, des rôles RBAC et un pare-feu restrictif.

Cette base peut ensuite recevoir les résultats d’un scan OpenVAS/Greenbone pour croiser vulnérabilités connues et tentatives d’exploitation réelles, ou les résultats d’un scan réseau Nmap pour documenter l’inventaire des services exposés. Pour une entreprise soumise à la directive NIS2, cette architecture répond directement aux exigences de journalisation et de détection d’incident prévues par le texte.

## Erreurs fréquentes à éviter

- **Utiliser le tag Docker “latest”** — une mise à jour majeure non testée peut casser la compatibilité MongoDB/OpenSearch du jour au lendemain. Figez toujours un numéro de version précis comme 7.1.8.
- **Oublier le réglage vm.max_map_count** — OpenSearch refuse de démarrer sans cet ajustement noyau, une des causes d’échec les plus fréquentes en environnement conteneurisé.
- **Exposer le port 9000 directement sur Internet sans TLS ni pare-feu** — l’interface d’administration devient alors une cible directe pour du brute-force ou de l’exploitation de vulnérabilité.
- **Ne définir aucune politique de rétention d’index** — le disque se remplit progressivement jusqu’à un arrêt brutal du service par manque d’espace.
- **Confondre GELF et Syslog** — envoyer un flux Syslog vers un input configuré en GELF ne génère aucune erreur visible, mais aucun message n’apparaît jamais dans la recherche.
- **Ignorer les logs du conteneur Graylog lors d’un problème** — la commande docker compose logs -f graylog révèle presque toujours la cause exacte d’un échec de démarrage.
- **Sous-dimensionner le heap OpenSearch par précaution budgétaire** — allouer 512 Mo à un moteur censé indexer plusieurs gigaoctets par jour provoque des ralentissements puis des pertes de messages silencieuses sous forte charge.
- **Négocier les identifiants d’accès en clair dans le fichier docker-compose.yml versionné sur Git** — utilisez systématiquement un fichier .env exclu du dépôt via .gitignore, comme dans l’exemple de ce tutoriel.

## Dépannage : problèmes courants et solutions

**Le conteneur OpenSearch s’arrête immédiatement après le démarrage.** Vérifiez que `vm.max_map_count` vaut bien 262144 avec `sysctl vm.max_map_count`, et que la limite `memlock` est illimitée dans le fichier compose.

**Graylog reste bloqué sur “Waiting for indexer to become available”.** Le serveur Graylog a démarré trop tôt. Ajoutez un délai ou un mécanisme de retry, ou redémarrez simplement le conteneur Graylog après confirmation qu’OpenSearch répond sur le port 9200.

**Impossible de se connecter avec le mot de passe admin.** Le hash SHA-256 doit correspondre exactement au mot de passe en clair, sans retour à la ligne parasite. Régénérez-le avec `echo -n` (sans le “-n”, un caractère invisible s’ajoute et casse le hash).

**Les messages Syslog n’apparaissent jamais dans la recherche.** Vérifiez avec `tcpdump -i any port 1514` sur le serveur Graylog que les paquets arrivent bien, puis contrôlez que l’input Syslog est en statut “Running” et non “Failed”.

**L’interface web répond très lentement après quelques semaines d’utilisation.** C’est généralement un signe que la mémoire allouée à OpenSearch (`OPENSEARCH_JAVA_OPTS`) est sous-dimensionnée par rapport au volume de logs ingéré. Augmentez le heap à 4 Go si le serveur dispose de 16 Go de RAM ou plus.

**Erreur “too many open files” dans les logs OpenSearch.** La limite de fichiers ouverts du système n’a pas été appliquée. Vérifiez `/etc/security/limits.conf` et redémarrez la session ou le serveur pour que la limite prenne effet.

**Les pipelines ne modifient jamais les messages malgré une règle active.** La règle doit être explicitement rattachée à un pipeline, et ce pipeline doit être connecté au flux (“Stream”) concerné dans “System > Pipelines > Manage rules”. Une règle non connectée à un pipeline ne s’exécute jamais.

**Le volume de stockage Docker grossit anormalement vite.** Vérifiez la politique de rotation d’index dans “System > Indices” : un index configuré sans limite de taille ou de nombre continue de croître indéfiniment.

**Les alertes par e-mail ne partent jamais.** La configuration SMTP se règle dans le fichier de configuration serveur ou via les variables d’environnement `GRAYLOG_TRANSPORT_EMAIL_ENABLED` et associées, absentes par défaut du fichier Compose de ce tutoriel. Ajoutez-les explicitement si vous comptez notifier par e-mail plutôt que par webhook.

## Astuces avancées pour aller plus loin

Une fois la base opérationnelle, plusieurs optimisations méritent d’être explorées. Le Graylog Sidecar, en version 1.5.4, permet de déployer et gérer à distance des collecteurs (Filebeat, Winlogbeat) sur des dizaines de machines depuis l’interface Graylog centrale, sans se connecter en SSH sur chaque poste individuellement. Pour les architectures multi-sites, le Graylog Forwarder, passé en version 7.6 le 4 septembre 2026, relaie désormais les logs collectés sur des sites distants ou des environnements isolés vers le cluster central sans exposer directement OpenSearch à Internet. Les content packs Illuminate 7.0.7 fournissent des règles de détection prêtes à l’emploi pour des sources courantes (pare-feu Palo Alto, Microsoft 365, AWS CloudTrail), un gain de temps considérable comparé à l’écriture manuelle de chaque pipeline.

Pour les environnements qui dépassent un seul serveur, Graylog supporte un déploiement en cluster avec plusieurs nœuds de traitement partageant le même cluster OpenSearch, ce qui répartit la charge d’ingestion et élimine le point de défaillance unique. Enfin, pour les organisations qui doivent démontrer leur conformité NIS2 ou répondre à un audit ANSSI, l’export régulier des dashboards et des règles d’alerte vers un dépôt de configuration versionné (Git) constitue une preuve documentée de la posture de détection en place, un exercice que la directive NIS2 demande explicitement aux entités essentielles et importantes.

## Foire aux questions

**Graylog est-il vraiment gratuit ?**

L’édition Graylog Open est open source et gratuite pour un usage illimité. Les fonctions avancées (data tiering étendu, contenus de sécurité premium, support entreprise) relèvent des éditions commerciales Graylog Enterprise et Graylog Security, dont le tarif se négocie directement avec l’éditeur.

**Peut-on installer Graylog sans Docker ?**

Oui, une installation native sur Debian ou Ubuntu est possible via les paquets APT officiels, mais elle demande de gérer manuellement les versions Java, MongoDB et OpenSearch, ce qui multiplie les risques d’incompatibilité. Docker Compose reste la méthode la plus fiable pour une première installation.

**Combien de RAM faut-il vraiment pour un usage en production ?**

8 Go suffisent pour un lab ou un test avec quelques sources. Au-delà de 5 Go de logs ingérés par jour, comptez 16 Go minimum, majoritairement consommés par le heap OpenSearch.

**Graylog remplace-t-il un antivirus ou un pare-feu ?**

Non. Graylog centralise et analyse des logs déjà générés par d’autres équipements ; il ne bloque rien par lui-même. Il se combine avec des outils actifs comme un pare-feu ou une solution de blocage comportemental pour une protection complète.

**Quelle est la différence entre Graylog Open et Graylog Security ?**

Graylog Open couvre la centralisation, la recherche et les pipelines de base. Graylog Security ajoute les investigations automatisées, la détection comportementale avancée et les content packs de sécurité premium sous licence commerciale.

**Faut-il migrer d’Elasticsearch vers OpenSearch avant d’installer Graylog 7.1 ?**

Oui si vous partez d’une ancienne installation. Graylog a définitivement abandonné le support d’Elasticsearch au profit d’OpenSearch depuis la version 5.0 ; toute nouvelle installation en 2026 doit directement utiliser OpenSearch 2.19.x.

**Combien de temps faut-il pour déployer cette architecture complète ?**

Comptez environ 90 minutes pour suivre les onze étapes de ce tutoriel sur un serveur neuf, en incluant les tests de chaque input et la création d’une première alerte fonctionnelle.

**Graylog fonctionne-t-il pour surveiller des logs Windows ?**

Oui, via l’agent NXLog ou Winlogbeat déployé sur les serveurs Windows, configuré pour transmettre les journaux d’événements (sécurité, système) vers l’input GELF ou Syslog de Graylog, exactement comme pour une source Linux.
