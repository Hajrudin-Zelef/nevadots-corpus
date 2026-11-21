---
id: collect-261121-veille-ia/google-ecosystem/security-onion-surveillance-reseau-nsm-en-13-etapes
title: "Security Onion : Surveillance Réseau NSM en 13 Étapes"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["security", "onion", "reseau", "detection", "capture", "linux", "siem", "surveillance"]
source: https://tech-insider.org/fr/security-onion-surveillance-reseau-nsm-2026/
source_anchor: ""
source_lines: [1, 292]
section: "Google_Ecosystem"
---
Security Onion 3.3.0 est sorti le 11 septembre 2026, dix jours seulement avant la rédaction de ce tutoriel. Cette distribution Linux gratuite empile Suricata, Zeek, Wazuh et la suite Elastic dans un seul système capable de surveiller un réseau d’entreprise ou un lab de formation, sans payer une licence SIEM à cinq chiffres. Ce guide détaille l’installation complète, du téléchargement de l’ISO jusqu’à la première chasse aux menaces dans la console SOC, avec les vrais prérequis matériels, les pièges qui bloquent 90 % des débutants et une checklist de dépannage.

## Qu’est-ce que Security Onion et pourquoi l’installer en 2026

Security Onion est une distribution Linux libre, construite sur Rocky Linux 9, entièrement dédiée à la surveillance de sécurité réseau (Network Security Monitoring, NSM) et à la détection d’intrusion. Contrairement à un pare-feu ou un antivirus classique, l’outil ne bloque rien par défaut : il capture le trafic réseau, l’analyse avec plusieurs moteurs de détection en parallèle, puis centralise les alertes dans une console web unique appelée Security Onion Console (SOC).

La force du projet tient à l’intégration. Plutôt que d’installer et de faire dialoguer séparément Suricata pour la détection d’intrusion réseau, Zeek pour l’analyse de protocoles, Wazuh pour la supervision des postes et des logs, et la suite Elastic (Elasticsearch, Logstash, Kibana) pour l’indexation et la recherche, Security Onion Solutions livre le tout préconfiguré, avec des scripts d’installation qui gèrent les dépendances, les certificats et les flux de données entre composants. Le code source est public sur GitHub, gratuit à l’usage, et sans limite de nombre de capteurs contrairement aux offres SIEM commerciales facturées au volume de données ingérées.

Pour un service informatique en France, l’intérêt dépasse le simple aspect budgétaire. La directive NIS2 impose désormais aux entités essentielles et importantes une capacité de détection et de remontée d’incident dans des délais serrés. Disposer d’une plateforme NSM auto-hébergée, dont les données ne transitent jamais chez un tiers américain, répond directement à cette exigence de souveraineté tout en gardant un budget compatible avec une PME ou une collectivité.

La différence entre un SIEM classique et une plateforme NSM comme Security Onion mérite d’être clarifiée avant de se lancer. Un SIEM traditionnel se contente le plus souvent de collecter et corréler des journaux déjà produits par vos équipements (pare-feu, serveurs, applications). Security Onion va plus loin : il capture directement le trafic réseau brut, ce qui lui permet de détecter des activités que les journaux applicatifs ne mentionnent jamais, comme une exfiltration de données via un protocole détourné ou une communication de commande et contrôle chiffrée mais reconnaissable à ses métadonnées. Cette capacité de capture complète (full packet capture) transforme chaque alerte en dossier d’investigation exploitable, puisque l’analyste peut rejouer littéralement le paquet réseau à l’origine de la détection plutôt que de se contenter d’une ligne de log résumée.

## Prérequis : matériel, logiciels et versions exactes

Avant de lancer quoi que ce soit, vérifiez que votre machine (physique ou machine virtuelle) répond aux exigences officielles. La documentation Security Onion est stricte sur ce point : sous-dimensionner la RAM est la cause numéro un des installations qui échouent au démarrage des services Elasticsearch.

| Composant | Minimum absolu (standalone/éval) | Recommandé (trafic réel) | 
|---|---|---|
| Version testée | Security Onion 3.3.0 (build 3.3.0-20260911) | Idem, dernière release stable | 
| CPU | 4 cœurs | 8 cœurs ou plus | 
| RAM | 24 Go (avec swap conseillé) | 32 Go ou plus | 
| Stockage | 200 Go | 500 Go+ selon la rétention visée | 
| Interfaces réseau | 2 NIC (management + monitoring) | 2 NIC, la carte de capture en mode promiscuous | 
| Système de base | Rocky Linux 9 (ISO fournie ou installation minimale) | Rocky Linux 9 à jour | 

Ces chiffres proviennent directement de la documentation officielle (docs.securityonion.net) pour un déploiement standalone, c’est-à-dire un nœud unique qui cumule le rôle de manager et de capteur. Avec seulement 24 Go de RAM, l’installateur avertit qu’il faudra probablement activer de l’espace swap pour éviter que le service Elasticsearch ne soit tué par l’OOM killer du noyau Linux. Pour un lab de test avec un trafic faible, 24 Go suffisent. Pour surveiller ne serait-ce qu’un petit segment réseau de production, comptez 32 Go dès le départ.

Côté logiciels, vous aurez besoin de :

- L’image ISO officielle Security Onion 3.3.0, téléchargée exclusivement depuis le dépôt GitHub Security-Onion-Solutions/securityonion
- Un hyperviseur si vous testez en VM : VMware Workstation/ESXi, VirtualBox ou Proxmox VE
- Une clé USB de 8 Go minimum si vous installez sur du matériel physique
- Un accès Internet stable pendant l’installation (les paquets sont téléchargés depuis les dépôts officiels)
- Un navigateur récent (Chrome, Firefox ou Edge) pour accéder à la console SOC

Le fichier ISO doit systématiquement être vérifié par son empreinte SHA256 avant l’installation. Le dépôt GitHub inclut un fichier DOWNLOAD_AND_VERIFY_ISO.md qui documente la procédure officielle de vérification, une étape que beaucoup de tutoriels non officiels omettent et qui expose à un risque d’image corrompue ou modifiée.

## Étape 1 : télécharger et vérifier l’image ISO

Rendez-vous sur le dépôt GitHub officiel du projet. Ne téléchargez jamais l’ISO depuis un miroir tiers ou un lien partagé sur un forum : c’est un vecteur classique de compromission pour un outil dont le rôle est précisément de détecter des compromissions. Une fois le fichier récupéré, vérifiez son intégrité en ligne de commande.

```
# Calculer le hash SHA256 de l'ISO téléchargée
sha256sum securityonion-3.3.0-20260911.iso
# Comparer avec le hash publié sur la page de release GitHub
# Les deux valeurs doivent être strictement identiques
cat securityonion-3.3.0-20260911.iso.sha256
```
Si les deux empreintes ne correspondent pas, supprimez le fichier et retéléchargez-le. N’installez jamais une ISO dont le hash ne correspond pas : c’est une règle non négociable pour un outil de sécurité qui va ensuite avoir un accès privilégié à tout votre trafic réseau.

## Étape 2 : préparer la machine virtuelle ou le serveur physique

Pour un premier test, une machine virtuelle est largement suffisante et vous permet de faire des snapshots avant chaque étape critique. Dans votre hyperviseur, créez une VM avec les caractéristiques suivantes : 4 vCPU, 24 Go de RAM minimum, un disque de 200 Go en provisionnement fin, et deux cartes réseau virtuelles distinctes.

La première interface réseau (management) doit être connectée à votre réseau administratif classique, celui depuis lequel vous accéderez à la console SOC via HTTPS. La seconde interface (monitoring) doit être branchée sur un port SPAN, un port mirror de votre switch, ou une TAP réseau, de façon à recevoir une copie du trafic que vous souhaitez surveiller. Cette interface de capture ne doit jamais avoir d’adresse IP configurée dessus : elle fonctionne en mode promiscuous, invisible sur le réseau qu’elle observe.

Sur VMware ou Proxmox, pensez à activer le mode promiscuous au niveau du commutateur virtuel pour l’interface de monitoring, sinon Security Onion ne recevra jamais le trafic mirroré, même si la configuration du port SPAN côté switch physique est correcte.

## Étape 3 : démarrer l’installateur et choisir le mode d’installation

Bootez la VM sur l’ISO. L’écran d’accueil de Security Onion propose deux chemins : une installation graphique guidée (recommandée pour une première prise en main) ou une installation sur une base Rocky Linux 9 Minimal déjà en place, pour les administrateurs qui préfèrent maîtriser chaque paquet du système de base.

Pour ce tutoriel, choisissez l’installation via ISO complète. L’assistant vous demande successivement : le fuseau horaire (Europe/Paris), la configuration réseau de l’interface management (IP statique fortement recommandée en production), le nom d’hôte, et un compte administrateur local avec mot de passe robuste. Ce compte servira à la fois pour l’accès SSH et pour la première connexion à la console web.

## Étape 4 : choisir le type d’installation Security Onion

Une fois le système de base Rocky Linux installé, le script setup de Security Onion démarre automatiquement. C’est ici que se joue la décision la plus importante du déploiement : le type de nœud.

| Type de nœud | Rôle | Cas d’usage typique | 
|---|---|---|
| Standalone | Manager + capteur sur une seule machine | Lab, PME, test de faisabilité, petit réseau | 
| Manager | Gère la configuration, la console SOC et l’orchestration du cluster | Déploiement distribué multi-sites | 
| Search node | Nœud dédié à l’indexation et la recherche Elasticsearch | Scaling horizontal quand le volume de logs augmente | 
| Sensor | Capture le trafic, exécute Suricata et Zeek | Chaque site distant à surveiller | 

Pour une première installation ou un environnement de moins de 200 postes, le mode Standalone est le bon choix : il regroupe tous les rôles sur une seule machine et simplifie considérablement la maintenance. Les déploiements distribués (Manager séparé des Sensors) ne se justifient qu’à partir d’une volumétrie de trafic ou d’un nombre de sites qui dépasse la capacité d’un seul serveur, typiquement au-delà de plusieurs centaines de mégabits par seconde de trafic soutenu.

## Étape 5 : configurer l’interface de capture réseau

L’installateur vous demande ensuite quelle interface réseau utiliser pour le management (accès web, SSH) et laquelle utiliser pour la capture (monitoring). C’est une confusion fréquente : inverser les deux rend la console web inaccessible et empêche toute capture de trafic utile.

```
# Vérifier après installation quelles interfaces sont détectées
ip link show
# Vérifier que l'interface de monitoring est bien en mode promiscuous
ip -d link show eth1 | grep -i promisc
# Consulter le statut des services de capture
sudo so-status
```
Le script so-status est votre premier réflexe après chaque redémarrage ou modification de configuration : il liste l’état (running, stopped, degraded) de chaque conteneur Docker composant la stack (Suricata, Zeek, Wazuh, Elasticsearch, Logstash, Kibana, SOC). Une installation saine affiche tous les services en statut “running” au bout de 10 à 15 minutes après le premier démarrage, le temps que les images Docker soient extraites et que les index Elasticsearch soient initialisés.

## Étape 6 : finaliser l’installation et le premier démarrage

Une fois toutes les questions répondues, l’installateur télécharge et configure automatiquement l’ensemble des composants : Suricata pour la détection d’intrusion signature-based, Zeek pour l’analyse de protocoles et la génération de logs réseau enrichis, Wazuh pour la collecte de logs système et la détection sur les hôtes, puis la suite Elastic (Elasticsearch, Logstash, Kibana) pour l’indexation et la visualisation. Cette phase dure généralement entre 20 et 45 minutes selon la vitesse de votre connexion Internet et la puissance du CPU alloué.

À la fin du processus, le serveur redémarre et affiche une invite de connexion classique. Notez l’adresse IP de l’interface management affichée à l’écran : c’est par cette adresse, en HTTPS, que vous accéderez à la console SOC.

## Étape 7 : première connexion à la console Security Onion (SOC)

Ouvrez un navigateur et rendez-vous sur https://IP-DE-VOTRE-SERVEUR. Le certificat TLS généré par défaut est auto-signé, votre navigateur affichera donc un avertissement de sécurité au premier accès : c’est normal, acceptez l’exception ou importez le certificat racine généré par l’installateur si vous souhaitez éviter l’avertissement sur tous vos postes d’administration.

Connectez-vous avec le compte administrateur créé pendant l’installation. La console SOC s’ouvre sur un tableau de bord (Dashboards) qui agrège les métriques clés : nombre d’alertes par sévérité, volume de trafic capturé, top des adresses IP sources et destinations, statut des capteurs. C’est le point d’entrée unique pour toute l’exploitation quotidienne, vous n’avez normalement plus besoin d’accéder séparément à Kibana ou aux interfaces natives de chaque outil.

## Étape 8 : explorer les alertes et lancer une première chasse aux menaces

Le menu Alerts regroupe toutes les détections remontées par Suricata (signatures réseau) et Wazuh (règles hôtes). Chaque alerte affiche sa sévérité, la règle qui l’a déclenchée, les adresses IP et ports concernés, ainsi qu’un lien direct vers le paquet réseau brut capturé au moment de l’événement grâce à l’intégration avec l’outil de capture complète (full packet capture). Un clic sur une alerte ouvre une vue détaillée qui affiche non seulement le paquet incriminé, mais aussi l’ensemble de la session réseau qui l’entoure, ce qui permet de reconstituer le contexte complet d’une tentative d’intrusion sans quitter la console.

Une bonne pratique pour les premières semaines d’exploitation consiste à trier les alertes par sévérité décroissante et à traiter en priorité les événements classés critiques et élevés, plutôt que de tenter de traiter l’intégralité du flux dès le premier jour. Le volume d’alertes générées par une installation fraîchement configurée peut atteindre plusieurs centaines par jour sur un réseau d’entreprise de taille moyenne, avant même le travail de calibration des règles évoqué plus loin dans ce guide.

Le module Hunt permet d’interroger manuellement l’ensemble des logs indexés, qu’ils proviennent de Zeek, de Suricata ou de Wazuh, avec une syntaxe de requête proche de celle de Kibana. C’est l’outil central pour la threat hunting proactive : rechercher une adresse IP suspecte sur les dernières 24 heures, isoler tous les flux DNS vers un domaine donné, ou croiser des logs d’authentification avec des connexions réseau sortantes anormales.

```
# Exemple de requête dans le module Hunt pour isoler
# tout le trafic DNS sortant vers un domaine suspect
event.dataset:dns AND dns.query.name:"domaine-suspect.example"
# Rechercher toutes les alertes de sévérité critique
# sur les dernières 24 heures
event.severity_label:"critical" AND @timestamp:[now-24h TO now]
```
## Comprendre le rôle exact de chaque composant intégré

Avant d’aller plus loin dans la configuration, il est utile de comprendre précisément ce que fait chaque brique de la stack, car les alertes que vous verrez dans la console SOC proviennent de moteurs très différents dans leur logique de fonctionnement.

Suricata fonctionne par signatures : il compare chaque paquet réseau à une base de règles qui décrivent des schémas d’attaque connus (exploitation d’une CVE spécifique, communication vers un serveur de commande et contrôle identifié, tentative d’exfiltration par un protocole détourné). C’est un moteur rapide et fiable sur les menaces déjà documentées, mais par nature incapable de repérer une attaque totalement inédite qui ne correspond à aucune signature existante. Zeek, à l’inverse, ne cherche pas de signatures : il journalise en continu tous les métadonnées de chaque connexion réseau (protocole utilisé, volume échangé, certificats TLS vus, requêtes DNS, fichiers transférés) sous forme de logs structurés. C’est ce moteur qui permet la threat hunting rétrospective, en recherchant a posteriori un comportement anormal qu’aucune signature n’aurait détecté au moment des faits.

Wazuh complète ce duo réseau par une surveillance côté hôte : intégrité des fichiers système, analyse des journaux d’authentification, détection de rootkits, conformité à des référentiels de durcissement. C’est le seul des trois moteurs à s’exécuter directement sur les postes et serveurs surveillés, via un agent léger installé sur chaque machine, plutôt qu’en écoute passive sur le réseau. Enfin, la suite Elastic (Elasticsearch pour l’indexation, Logstash pour l’ingestion et la transformation des données, Kibana comme moteur de visualisation sous-jacent à la console SOC) sert de colonne vertébrale technique : c’est elle qui stocke, indexe et rend interrogeables en quelques secondes des dizaines de millions d’événements générés chaque jour par les trois moteurs de détection.

Cette répartition des rôles explique pourquoi une alerte critique dans Security Onion croise très souvent plusieurs sources : une connexion sortante suspecte détectée par Zeek, confirmée par une signature Suricata correspondant à un malware connu, et corrélée à une modification de fichier système détectée par l’agent Wazuh sur le poste à l’origine du trafic. C’est cette corrélation multi-source, impossible à obtenir avec un seul outil pris isolément, qui constitue la véritable valeur ajoutée de la plateforme par rapport à un déploiement de chaque brique en silo.

## Étape 9 : intégrer CyberChef et Playbook pour l’analyse avancée

Security Onion embarque directement CyberChef, accessible depuis le menu principal de la console, pour décoder rapidement des payloads suspects extraits d’une alerte : base64, hexadécimal, chiffrement simple, extraction de fichiers depuis une capture réseau. C’est un gain de temps considérable par rapport à devoir exporter les données vers un outil externe.

Le module Playbook, quant à lui, gère les règles de détection actives. Chaque règle Suricata ou Wazuh peut être activée, désactivée ou modifiée directement depuis l’interface, sans toucher aux fichiers de configuration bruts sur le serveur. C’est particulièrement utile pour ajuster le taux de faux positifs les premières semaines suivant le déploiement, une phase de calibrage que beaucoup d’équipes sous-estiment.

## Étape 10 : configurer la rétention des données et le stockage

Par défaut, Elasticsearch conserve les index selon une politique de rétention basée sur l’espace disque disponible : les données les plus anciennes sont automatiquement purgées lorsque le seuil configuré est atteint. Pour une conformité NIS2 ou un besoin d’investigation forensique, ajustez cette politique dans le fichier de configuration du gestionnaire d’index.

```
# Consulter l'espace disque utilisé par les index Elasticsearch
sudo so-elastic-index-size
# Forcer un rafraîchissement de la politique de rétention
sudo salt-call state.apply elasticsearch.ilm_policy
```
Avec 200 Go de disque, comptez généralement une rétention de quelques jours à deux semaines pour un trafic soutenu, ou plusieurs mois pour un petit segment réseau peu chargé. La documentation officielle recommande de dimensionner le stockage en fonction du volume de trafic réel observé pendant les deux premières semaines d’exploitation plutôt que de se fier à une estimation théorique.

## Étape 11 : ajouter un nœud sensor distant (déploiement distribué)

Si votre besoin dépasse un seul site, Security Onion permet d’ajouter des nœuds sensor distants qui remontent leurs données vers le manager central. Sur chaque nouveau site, relancez l’installateur ISO, mais choisissez cette fois le type de nœud Sensor au lieu de Standalone, puis renseignez l’adresse IP et la clé d’enregistrement générée sur le manager.

```
# Sur le manager : générer une clé d'enregistrement pour un nouveau sensor
sudo so-user add-sensor --site "Agence-Lyon"
# Sur le nouveau sensor : rejoindre le cluster existant
sudo so-setup --join-manager 192.168.10.5
```
Cette architecture distribuée est celle recommandée pour toute organisation multi-sites, car elle permet de centraliser la corrélation d’alertes tout en gardant la capture de trafic au plus près de chaque segment réseau surveillé.

## Étape 12 : sécuriser l’accès et gérer les comptes utilisateurs

Une fois l’installation validée, créez des comptes nominatifs pour chaque analyste plutôt que de partager le compte administrateur initial. La console SOC gère nativement des rôles (analyste, administrateur, lecture seule) qui permettent de limiter les actions possibles selon le niveau de responsabilité.

```
# Ajouter un nouvel analyste avec un rôle restreint
sudo so-user add analyste.dupont --role analyst
# Lister les comptes existants et leurs rôles
sudo so-user list
```
Pensez également à restreindre l’accès à la console SOC via un pare-feu ou un VPN plutôt que de l’exposer directement sur Internet. L’outil manipule des données réseau sensibles, et sa console d’administration ne devrait jamais être accessible depuis l’extérieur sans une couche d’authentification supplémentaire.

## Étape 13 : maintenir la plateforme à jour

Security Onion publie des mises à jour régulières qui incluent aussi bien des correctifs de sécurité que des mises à jour de signatures Suricata et Zeek. La commande de mise à jour officielle gère l’ensemble du cycle, y compris la mise à jour des conteneurs Docker sous-jacents.

```
# Lancer la mise à jour complète de la plateforme
sudo soup
# Vérifier la version actuellement installée
sudo so-version
```
Planifiez cette mise à jour lors d’une fenêtre de maintenance : certaines montées de version majeures nécessitent un redémarrage complet des services et peuvent interrompre la capture de trafic pendant quelques minutes.

## Comparatif : Security Onion face aux alternatives

Avant de vous engager, il est utile de situer Security Onion par rapport aux options que vous connaissez peut-être déjà, notamment si votre équipe a testé Suricata, Zeek ou Wazuh séparément.

| Solution | Coût | Composants inclus | Effort d’intégration | 
|---|---|---|---|
| Security Onion | Gratuit, open source | Suricata + Zeek + Wazuh + Elastic Stack + SOC préintégrés | Faible, tout est packagé | 
| Suricata seul | Gratuit, open source | IDS/IPS réseau uniquement | Élevé, corrélation et visualisation à construire | 
| Zeek seul | Gratuit, open source | Analyse de protocoles uniquement | Élevé, pas d’interface d’alerte native | 
| Wazuh seul | Gratuit, open source | HIDS et gestion de logs uniquement | Moyen, pas de capture réseau native | 
| Graylog | Gratuit (édition open) à payant | Centralisation de logs, sans capture réseau intégrée | Moyen | 
| Splunk / Sentinel | Facturé au volume ingéré, souvent 5 à 6 chiffres/an | SIEM complet avec support commercial | Faible mais coûteux | 

L’argument central de Security Onion est le temps d’intégration : monter individuellement Suricata, Zeek, Wazuh puis les faire dialoguer avec une stack Elastic demande généralement plusieurs semaines de travail à un ingénieur sécurité expérimenté. Security Onion réduit cet effort à quelques heures, au prix d’une flexibilité un peu moindre sur la personnalisation fine de chaque composant pris isolément. Pour une comparaison plus large des solutions SIEM commerciales, consultez notre analyse Sentinel vs Splunk vs Elastic Security.

Le second argument, moins évident au premier abord, concerne la portabilité des compétences. Un analyste formé sur Security Onion maîtrise en réalité Suricata, Zeek, Wazuh et les bases d’Elasticsearch de façon transférable : ces compétences restent valables même si l’organisation bascule un jour vers un SIEM commercial qui embarque tout ou partie de ces mêmes moteurs open source en coulisses, ce qui est le cas d’un nombre croissant de solutions propriétaires construites elles-mêmes sur des fondations Elastic ou Suricata sous licence.

## 5 pièges courants à éviter

- **Sous-dimensionner la RAM.** Installer avec moins de 24 Go provoque des crashs répétés d’Elasticsearch au bout de quelques heures d’utilisation, une fois que le volume de logs commence à remplir les index en mémoire.
- **Inverser les interfaces management et monitoring.** C’est l’erreur numéro un des débutants : la console web devient inaccessible et le capteur ne voit passer aucun trafic utile.
- **Oublier le mode promiscuous sur le commutateur virtuel.** Sous VMware ou Proxmox, si le port group de l’interface de monitoring n’autorise pas le mode promiscuous, Security Onion ne recevra jamais la copie du trafic réseau, même avec un port SPAN correctement configuré côté switch physique.
- **Négliger la calibration des règles Suricata après l’installation.** Les signatures par défaut génèrent souvent un volume élevé de faux positifs les premières semaines ; sans ajustement via le module Playbook, les analystes se noient rapidement sous les alertes non pertinentes.
- **Exposer la console SOC directement sur Internet.** Cette interface d’administration centralise l’accès à l’ensemble des données de sécurité collectées ; elle doit systématiquement rester derrière un VPN ou un pare-feu restrictif, jamais accessible publiquement.

## Exemple de sortie attendue après une installation réussie

Une fois l’installation terminée et tous les services démarrés, la commande so-status doit afficher un résultat similaire à celui-ci, confirmant que l’ensemble de la stack fonctionne correctement :

```
$ sudo so-status
so-elasticsearch    running (healthy)
so-logstash         running (healthy)
so-kibana           running (healthy)
so-soc              running (healthy)
so-suricata         running (healthy)
so-zeek              running (healthy)
so-wazuh-manager     running (healthy)
so-redis             running (healthy)
Statut global : OPERATIONNEL - 8/8 services actifs
```
Si un ou plusieurs services affichent “degraded” ou “stopped” au lieu de “running (healthy)”, ne poursuivez pas la configuration avant d’avoir résolu le problème : chaque service dépend souvent des autres, et une base instable génère des comportements erratiques difficiles à diagnostiquer plus tard.

## Dépannage : 8 problèmes fréquents et leurs solutions

- **La console SOC affiche une page blanche après connexion.** Videz le cache du navigateur et vérifiez que le service so-soc est bien “running” via so-status ; un redémarrage du service suffit dans la majorité des cas.
- **Aucune alerte n’apparaît après plusieurs heures.** Vérifiez que l’interface de monitoring reçoit effectivement du trafic avec sudo tcpdump -i eth1 -c 20 ; si aucun paquet n’apparaît, le problème vient du port SPAN ou du mode promiscuous, pas de Security Onion lui-même.
- **Elasticsearch refuse de démarrer avec une erreur de mémoire virtuelle.** Le paramètre noyau vm.max_map_count doit être ajusté à au moins 262144 ; l’installateur le configure normalement automatiquement, mais une réinstallation ou une migration peut le réinitialiser.
- **La mise à jour soup échoue avec une erreur de dépôt.** Vérifiez la connectivité Internet du serveur et la résolution DNS ; les dépôts Security Onion doivent être joignables en HTTPS sortant sans proxy transparent qui intercepte le TLS.
- **Le disque se remplit rapidement malgré une politique de rétention configurée.** Vérifiez le volume réel de trafic capturé avec so-elastic-index-size ; un trafic plus élevé que prévu consomme l’espace plus vite que la politique de rétention par âge ne peut le libérer.
- **Les signatures Suricata ne se mettent pas à jour automatiquement.** Contrôlez le statut du service de mise à jour des règles dans le module Playbook et vérifiez que le serveur a bien accès aux flux de règles externes configurés.
- **Un nœud sensor distant n’apparaît pas dans le manager.** Vérifiez que le port de communication entre sensor et manager (généralement le port de registration Salt) n’est pas bloqué par un pare-feu intermédiaire entre les deux sites.
- **La console devient lente après plusieurs semaines d’utilisation.** C’est généralement un signe que les index Elasticsearch ont atteint une taille critique par rapport à la RAM allouée ; ajoutez de la mémoire ou réduisez la durée de rétention des données.

## Astuces avancées pour aller plus loin

Une fois la plateforme stabilisée, plusieurs axes permettent d’en tirer davantage de valeur opérationnelle. D’abord, intégrez des flux de renseignement sur les menaces (threat intelligence) externes pour enrichir automatiquement les alertes avec un contexte de réputation IP ou de domaine, via les intégrations disponibles dans le module Playbook.

Ensuite, configurez des règles de corrélation personnalisées qui combinent des événements Wazuh (comportement anormal sur un poste) avec des événements réseau Zeek ou Suricata, pour détecter des scénarios d’attaque en plusieurs étapes que chaque outil pris isolément ne verrait pas. C’est l’un des véritables avantages d’une stack intégrée par rapport à des outils cloisonnés.

Enfin, pour les organisations soumises à des obligations réglementaires, exportez régulièrement les journaux d’audit et les statistiques d’alertes vers un stockage froid externe, séparé du serveur Security Onion lui-même. En cas d’incident majeur touchant le serveur de surveillance, ces exports garantissent que les preuves collectées avant l’incident restent exploitables pour l’investigation post-mortem, un point souvent négligé mais essentiel pour la conformité NIS2 et pour toute procédure judiciaire ultérieure.

## Ressources et documentation officielle

Pour approfondir chaque composant intégré dans Security Onion, plusieurs sources font autorité. La documentation officielle Security Onion détaille l’ensemble des scénarios de déploiement, y compris les architectures distribuées à grande échelle. Le dépôt GitHub du projet reste la référence pour le téléchargement de l’ISO et le suivi des notes de version.

Pour les moteurs de détection sous-jacents, le site officiel de Suricata publie régulièrement les mises à jour de signatures, tandis que le projet Zeek documente en détail son langage de scripting pour créer des détections personnalisées. Côté supervision des hôtes, la documentation Wazuh couvre la configuration avancée des agents. Enfin, pour comprendre le fonctionnement interne de la couche d’indexation, la documentation officielle de la suite Elastic reste incontournable pour optimiser les performances de recherche à grande échelle.

## Cas d’usage concrets : PME, collectivités et établissements scolaires

Le profil type d’organisation qui bénéficie le plus de Security Onion est celui qui a besoin d’une visibilité réseau sérieuse mais ne dispose ni du budget ni de l’équipe pour justifier un SIEM commercial facturé au volume de données. Une PME de 50 à 300 postes peut déployer un nœud standalone sur un unique serveur physique ou une VM dédiée, brancher l’interface de monitoring sur le port SPAN du switch cœur de réseau, et obtenir en une après-midi une couverture de détection qui aurait nécessité plusieurs dizaines de milliers d’euros de licence annuelle avec une solution propriétaire équivalente.

Pour une collectivité territoriale, l’argument de la souveraineté des données pèse souvent autant que le coût. Un serveur Security Onion hébergé dans les locaux de la collectivité, ou dans un datacenter français sous contrat SecNumCloud, garantit qu’aucune donnée de trafic réseau ne transite par un cloud étranger soumis à une juridiction extraterritoriale. C’est un point d’attention de plus en plus scruté par les services juridiques des collectivités depuis le renforcement des obligations de la directive NIS2 et les débats récurrents autour du Cloud Act américain.

Dans le monde académique, plusieurs universités et écoles d’ingénieurs utilisent Security Onion comme support pédagogique pour leurs formations en cybersécurité défensive : la plateforme permet aux étudiants de manipuler un SOC complet, de la capture réseau à l’investigation, sans avoir à assembler manuellement chaque composant. C’est également l’environnement de référence utilisé dans de nombreux CTF (Capture The Flag) orientés détection et réponse à incident, où les participants doivent identifier une chaîne d’attaque à partir des alertes générées par la plateforme.

## Questions fréquentes

**Security Onion est-il vraiment gratuit ?**

Oui. Le logiciel est open source, le code source est public sur GitHub, et il n’existe aucune limite de nombre de capteurs, de volume de données ou de fonctionnalités bridées derrière une offre payante. Security Onion Solutions propose en revanche des formations et un support commercial optionnel pour les entreprises qui le souhaitent.

**Peut-on installer Security Onion sur un Raspberry Pi ou un mini-PC ?**

Non, pas dans une configuration standard. Les exigences minimales de 24 Go de RAM et 4 cœurs CPU dépassent largement les capacités d’un Raspberry Pi. Un mini-PC avec 32 Go de RAM peut en revanche convenir pour un lab domestique ou une petite structure.

**Quelle est la différence entre Security Onion et Wazuh seul ?**

Wazuh se concentre sur la supervision des hôtes (logs, intégrité de fichiers, conformité) sans capture réseau native. Security Onion inclut Wazuh comme composant, mais y ajoute Suricata et Zeek pour la surveillance du trafic réseau, ainsi qu’une console unifiée qui corrèle les deux types de données.

**Faut-il des compétences Linux avancées pour l’administrer ?**

Une bonne maîtrise de la ligne de commande Linux facilite grandement le dépannage, mais l’installation et l’usage quotidien via la console SOC restent accessibles à un administrateur système avec des bases solides. Les tâches les plus courantes (consultation d’alertes, recherche de logs, activation de règles) se font entièrement depuis l’interface web.

**Security Onion peut-il remplacer un SIEM commercial comme Splunk ou Microsoft Sentinel ?**

Pour une PME ou une collectivité avec un budget limité, oui dans une large mesure : la couverture fonctionnelle (détection réseau, hôte, corrélation, recherche) est comparable. Les grandes organisations qui ont besoin d’un support contractuel garanti, d’intégrations SOAR poussées ou d’une conformité à des certifications spécifiques peuvent néanmoins préférer une solution commerciale.

**Combien de temps faut-il pour une installation complète ?**

Comptez entre 1 et 2 heures pour une installation standalone de bout en bout, incluant le téléchargement de l’ISO, l’installation du système et le premier démarrage de tous les services. La calibration des règles de détection pour réduire les faux positifs demande en revanche plusieurs semaines d’ajustement progressif.

**Comment surveiller plusieurs sites distants avec une seule console ?**

Déployez un nœud Manager central et un ou plusieurs nœuds Sensor sur chaque site distant, qui remontent leurs données vers le manager. Cette architecture distribuée est documentée à l’étape 11 de ce guide et constitue le modèle recommandé pour toute organisation multi-sites.

**Que faire si l’installation échoue à mi-parcours ?**

Revenez à un snapshot de VM pris avant l’installation si vous en avez créé un, et relancez le processus depuis le début en vérifiant scrupuleusement les prérequis matériels. Une interruption réseau pendant le téléchargement des paquets est la cause la plus fréquente d’échec en cours d’installation.
