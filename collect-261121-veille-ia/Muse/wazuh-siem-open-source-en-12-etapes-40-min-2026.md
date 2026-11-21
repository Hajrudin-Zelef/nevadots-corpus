---
id: collect-261121-veille-ia/muse/wazuh-siem-open-source-en-12-etapes-40-min-2026
title: "Wazuh : SIEM Open Source en 12 Étapes, 40 Min [2026]"
domain: muse
role: reference
task: article
actors: ["Meta"]
dates: ["2026-08-16"]
keywords: ["wazuh", "siem", "source", "open", "detection", "licence", "sous", "etapes"]
source: https://tech-insider.org/fr/wazuh-tutoriel-siem-open-source-2026/
source_anchor: ""
source_lines: [1, 383]
section: "Muse"
---
*Mis à jour le 16 août 2026.* Pendant que les licences Splunk et la facturation au gigaoctet de Microsoft Sentinel explosent les budgets, une alternative open source s’est imposée dans les SOC européens : **Wazuh**. Ce SIEM/XDR gratuit sous licence GPLv2 cumule plus de 16 000 étoiles sur GitHub et couvre, sans surcoût de licence, la détection d’intrusion, la surveillance d’intégrité des fichiers, la détection de vulnérabilités et la réponse automatisée aux incidents. Dans ce tutoriel, vous allez déployer **Wazuh 4.14.7** (version stable publiée le 29 juillet 2026, listée comme dernière version dans la documentation officielle) de A à Z, enrôler un agent, détecter une vraie attaque et automatiser sa neutralisation – en 12 étapes, environ 40 minutes. Les plus curieux pourront aussi garder un œil sur la future branche majeure, déjà accessible sous forme de préversion **Wazuh 5.0.0-beta2** publiée en mai 2026.

Au-delà de la technique, l’enjeu est réglementaire. Avec la transposition de la directive NIS 2 qui vise près de 15 000 entités françaises et impose une notification d’incident sous 24 puis 72 heures, disposer d’un système capable de *détecter, journaliser et corréler* les événements de sécurité n’est plus optionnel. Un SIEM open source auto-hébergé comme Wazuh répond à cette obligation tout en gardant les journaux sur votre infrastructure – un atout de souveraineté et de conformité au RGPD que les plateformes cloud américaines peinent à offrir.

## Pourquoi Wazuh, le SIEM open source, séduit l’Europe en 2026

La vague de cyberattaques de 2025-2026 a rappelé une évidence : sans visibilité sur ses journaux, une organisation est aveugle. La fuite de données de l’ANTS ou l’exfiltration de 350 Go subie par la Commission européenne partagent un point commun : la détection tardive. Un SIEM (Security Information and Event Management) centralise et analyse en temps réel les journaux de tous vos serveurs, postes et équipements réseau pour repérer les comportements anormaux avant qu’ils ne deviennent une brèche.

Là où Splunk facture à l’ingestion et Microsoft Sentinel au gigaoctet ingéré dans Azure, Wazuh reste **entièrement gratuit** et auto-hébergé – même la propre offre cloud gérée de son éditeur illustre l’écart de coût : selon le comparateur MSP Compared, Wazuh Cloud facturait en juin 2026 à partir de 571 $/mois pour 100 agents, jusqu’à 923 $/mois pour 250 agents et 1 467 $/mois pour 500 agents, quand la version auto-hébergée de ce tutoriel reste, elle, à coût de licence nul quel que soit le nombre d’agents. Développé par la société américaine Wazuh, Inc. mais distribué sous licence libre GPLv2, le projet affichait déjà plus de 47 900 commits sur son dépôt GitHub principal en juin 2026, preuve d’un rythme de développement soutenu, et il combine dans un seul produit ce que la concurrence vend en modules séparés : SIEM, XDR (détection et réponse étendues), surveillance d’intégrité des fichiers (FIM), évaluation de configuration (SCA), détection de vulnérabilités et réponse active. Pour une PME, une collectivité ou un hébergeur français soumis à NIS 2, l’équation est simple : la souveraineté des données, un coût de licence nul et une conformité facilitée. Cela s’inscrit dans la logique de la stratégie nationale de cybersécurité 2026-2030 qui met l’accent sur l’autonomie technologique.

Ce choix de l’open source rejoint une tendance de fond : comme pour l’auto-hébergement d’un gestionnaire de mots de passe avec Vaultwarden ou le blocage d’attaques avec CrowdSec, reprendre le contrôle de sa sécurité passe par des briques libres, éprouvées et maîtrisées de bout en bout. Wazuh en est la pièce maîtresse côté détection.

## Wazuh, c’est quoi ? L’architecture SIEM/XDR décryptée

Avant d’installer Wazuh, il faut comprendre ses quatre composants. Une installation « tout-en-un » les regroupe sur un seul serveur, mais en production, l’indexeur et le tableau de bord peuvent être répartis sur des machines dédiées pour absorber la montée en charge.

### Les quatre briques de Wazuh

- **Wazuh Manager (serveur)** : le cœur du système. Il reçoit les données des agents, applique les règles de détection, corrèle les événements, déclenche les alertes et pilote la réponse active.
- **Wazuh Indexer** : moteur de recherche et de stockage basé sur OpenSearch. Il indexe les alertes et les journaux pour permettre des recherches quasi instantanées, même sur des téraoctets de données.
- **Wazuh Dashboard** : l’interface web (basée sur OpenSearch Dashboards). Elle offre tableaux de bord, visualisations, exploration des alertes, cartographie MITRE ATT&CK et rapports de conformité.
- **Wazuh Agent** : un collecteur léger (environ 35 Mo de RAM en moyenne) installé sur chaque machine surveillée. Il remonte journaux, événements FIM, inventaire et résultats d’analyses vers le manager.

La communication agent-manager est chiffrée et s’appuie sur des clés d’enregistrement uniques. Un agent consomme très peu de ressources, ce qui permet de couvrir un parc de milliers de machines depuis un seul serveur Wazuh correctement dimensionné. Le tableau ci-dessous positionne Wazuh face aux principaux SIEM du marché en 2026.

| Solution | Type | Licence / coût | Hébergement | Points forts | 
|---|---|---|---|---|
| **Wazuh 4.14.7** | SIEM + XDR open source | GPLv2, gratuit | Auto-hébergé (souverain) | FIM, SCA, réponse active, MITRE, sans coût de licence | 
| Splunk Enterprise Security | SIEM commercial | Licence à l’ingestion / au workload | Cloud ou on-premise | Écosystème d’apps, langage SPL, maturité | 
| Microsoft Sentinel | SIEM cloud natif | Facturation au Go ingéré (Azure) | Cloud Azure | Intégration Microsoft 365 / Entra ID | 
| Elastic Security | SIEM / analytics | Abonnement (offre gratuite limitée) | Cloud ou auto-hébergé | Recherche, machine learning | 
| IBM QRadar | SIEM commercial | Licence (EPS / volume) | Cloud ou on-premise | Corrélation avancée, grands comptes | 

La documentation officielle détaille chaque composant sur la page Getting started. Retenez l’essentiel : Wazuh offre en open source ce que les autres facturent, avec une architecture modulaire qui grandit avec vos besoins.

## Ce que vous allez construire dans ce tutoriel Wazuh

Ce guide ne se limite pas à une installation. À la fin des 12 étapes, vous disposerez d’un **projet complet et fonctionnel** : un serveur Wazuh tout-en-un supervisant un serveur web Linux, avec surveillance d’intégrité des fichiers sensibles, détection des vulnérabilités logicielles, contrôle de configuration CIS, et surtout une chaîne de détection-réponse capable de repérer une attaque SSH par force brute et de **bannir automatiquement l’adresse IP de l’attaquant** en quelques secondes. Nous cartographierons enfin les alertes sur le référentiel MITRE ATT&CK et alignerons le déploiement sur les exigences NIS 2 et RGPD.

- **Machine 1** – le serveur Wazuh (manager + indexeur + tableau de bord), Ubuntu 24.04 LTS, IP d’exemple`10.0.0.10` .
- **Machine 2** – un serveur web à surveiller (agent Wazuh), Ubuntu 24.04 LTS, IP d’exemple`10.0.0.21` , nom`web01` .
- **Résultat** – un SIEM opérationnel, des alertes en temps réel, une réponse active fonctionnelle et un socle de conformité.

Toutes les adresses IP utilisées (plages `203.0.113.0/24` et `198.51.100.0/24`) sont des blocs de documentation réservés : remplacez-les par vos valeurs réelles. Passons aux prérequis.

## Prérequis : versions, matériel et ports réseau

Le principal écueil d’une installation de Wazuh est le sous-dimensionnement. L’indexeur, basé sur OpenSearch (donc sur la JVM), a besoin de mémoire : en dessous de **8 Gio de RAM**, l’assistant d’installation refuse de continuer. Voici les besoins matériels officiels pour un déploiement tout-en-un, en fonction du nombre d’agents et d’une rétention de 90 jours.

| Composant | Agents surveillés | CPU | RAM | Stockage (90 j) | 
|---|---|---|---|---|
| Serveur tout-en-un | 1 à 25 | 4 vCPU | 8 Gio | 50 Go | 
| Serveur tout-en-un | 25 à 50 | 8 vCPU | 8 Gio | 100 Go | 
| Serveur tout-en-un | 50 à 100 | 8 vCPU | 8 Gio | 200 Go | 
| Agent Wazuh | – | négligeable | ~35 Mo | quelques Mo | 

Côté système d’exploitation, les composants centraux exigent un Linux 64 bits (x86_64 ou ARM64). Sont officiellement pris en charge : **Ubuntu 16.04 à 24.04**, **Red Hat Enterprise Linux 7 à 10**, **CentOS Stream 10**, **Amazon Linux 2 et 2023**. Pour ce tutoriel, nous utilisons Ubuntu 24.04 LTS. Les agents Wazuh, eux, tournent sur Linux, Windows, macOS, Solaris, AIX et HP-UX : la couverture est quasi universelle.

### Ports réseau à ouvrir

Avant de commencer, planifiez votre pare-feu. Ces ports doivent être joignables entre agents, serveur et administrateurs (jamais exposés à Internet sans filtrage).

| Port | Protocole | Usage | 
|---|---|---|
| 1514 | TCP | Communication agent → manager (flux chiffré des événements) | 
| 1515 | TCP | Enregistrement des agents (service `authd` ) | 
| 1516 | TCP | Communication interne du cluster Wazuh | 
| 514 | UDP | Réception Syslog (optionnel, équipements réseau) | 
| 55000 | TCP | API RESTful du manager Wazuh | 
| 9200 | TCP | API de l’indexeur Wazuh (OpenSearch) | 
| 443 | TCP | Tableau de bord Wazuh (HTTPS) | 

Vous aurez enfin besoin d’un accès `root` ou `sudo` sur les deux machines, d’une connexion Internet pour télécharger les paquets, et d’un navigateur récent. Vérifiez aussi que l’heure système est synchronisée (NTP) : un décalage d’horloge fausse l’horodatage des alertes et désynchronise les agents.

## Étape 1 – Préparer le serveur hôte

Connectez-vous à la machine qui hébergera Wazuh (notre `10.0.0.10`). On commence par mettre le système à jour, fixer le nom d’hôte, régler le fuseau horaire sur celui de la France et installer les quelques outils nécessaires. Un fuseau et une horloge corrects sont essentiels : c’est le socle d’une corrélation fiable des événements.

```
sudo hostnamectl set-hostname wazuh-server
sudo timedatectl set-timezone Europe/Paris
sudo apt update && sudo apt upgrade -y
sudo apt install -y curl tar apt-transport-https gnupg
sudo systemctl enable --now systemd-timesyncd
timedatectl status | grep -E "Time zone|synchronized"
```
La dernière commande doit confirmer que l’horloge est synchronisée. Exemple de sortie attendue :

```
System clock synchronized: yes
                Time zone: Europe/Paris (CEST, +0200)
```
Assurez-vous enfin de disposer d’au moins 8 Gio de RAM libres et de 50 Go d’espace disque sur la partition qui accueillera `/var/lib/wazuh-indexer`. Sur une machine virtuelle sous-dimensionnée, l’installation échouera à l’étape suivante.

## Étape 2 – Installer les composants centraux de Wazuh

Wazuh fournit un assistant d’installation « tout-en-un » qui déploie le manager, l’indexeur et le tableau de bord, génère les certificats SSL et crée les mots de passe. C’est la méthode recommandée pour découvrir Wazuh ou couvrir jusqu’à une centaine d’agents. Une seule commande suffit :

`curl -sO https://packages.wazuh.com/4.14/wazuh-install.sh && sudo bash ./wazuh-install.sh -a`
L’option `-a` (pour *all-in-one*) enchaîne l’installation des trois composants. Comptez 8 à 15 minutes selon votre bande passante et votre CPU. L’assistant télécharge les paquets, configure OpenSearch, applique les certificats et démarre les services. À la fin, il affiche une ligne décisive :

```
INFO: --- Summary ---
INFO: You can access the web interface https://10.0.0.10:443
    User: admin
    Password: <MOT_DE_PASSE_GENERE>
INFO: Installation finished.
```
Notez soigneusement ce mot de passe. L’assistant crée également une archive `wazuh-install-files.tar` contenant tous les identifiants et certificats. Conservez-la précieusement et hors ligne. Vérifions maintenant que les trois services tournent :

`sudo systemctl status wazuh-manager wazuh-indexer wazuh-dashboard --no-pager`
Les trois doivent afficher `active (running)`. Si l’indexeur reste en `activating` ou tombe en échec, c’est presque toujours un manque de RAM : consultez la section dépannage. La documentation de référence pour cette étape est le guide de démarrage rapide officiel.

## Étape 3 – Accéder au tableau de bord Wazuh

Ouvrez votre navigateur sur `https://10.0.0.10:443` (remplacez par l’IP de votre serveur). Le certificat étant auto-signé par défaut, votre navigateur affichera un avertissement de sécurité : acceptez l’exception pour continuer. Connectez-vous avec l’utilisateur `admin` et le mot de passe relevé à l’étape 2.

Si vous avez égaré le mot de passe, extrayez-le de l’archive générée par l’installateur :

`sudo tar -O -xvf wazuh-install-files.tar wazuh-install-files/wazuh-passwords.txt`
Cette commande affiche l’ensemble des identifiants (utilisateurs `admin`, `kibanaserver`, `wazuh-wui`, etc.). Une fois connecté, vous arrivez sur la vue d’ensemble de Wazuh : modules de sécurité, agents, alertes récentes et état de conformité. Le tableau de bord est encore vide car aucun agent n’est enrôlé – c’est l’objet de l’étape suivante.

Bonne pratique : changez immédiatement le mot de passe `admin` par défaut. Le script dédié se trouve dans le répertoire de l’indexeur (`/usr/share/wazuh-indexer/plugins/opensearch-security/tools/wazuh-passwords-tool.sh`). Nous détaillerons le durcissement complet à l’étape 11.

## Étape 4 – Déployer votre premier agent Wazuh

Un SIEM sans agent ne voit rien. Connectez-vous à la machine à surveiller (`web01`, `10.0.0.21`) et installez l’agent Wazuh. La méthode la plus propre passe par le dépôt APT officiel, avec une variable d’environnement qui pointe l’agent vers votre manager et lui donne un nom lisible.

```
curl -s https://packages.wazuh.com/key/GPG-KEY-WAZUH | sudo gpg --no-default-keyring \
  --keyring gnupg-ring:/usr/share/keyrings/wazuh.gpg --import
sudo chmod 644 /usr/share/keyrings/wazuh.gpg
echo "deb [signed-by=/usr/share/keyrings/wazuh.gpg] https://packages.wazuh.com/4.x/apt/ stable main" \
  | sudo tee /etc/apt/sources.list.d/wazuh.list
sudo apt update
sudo WAZUH_MANAGER="10.0.0.10" WAZUH_AGENT_NAME="web01" apt install -y wazuh-agent
sudo systemctl daemon-reload
sudo systemctl enable --now wazuh-agent
```
L’agent s’enregistre automatiquement auprès du manager via le port 1515, puis bascule sur le port 1514 pour transmettre ses événements. Retournez sur le serveur Wazuh et listez les agents connectés :

`sudo /var/ossec/bin/agent_control -l````
Wazuh agent_control. List of available agents:
   ID: 000, Name: wazuh-server (server), IP: 127.0.0.1, Active/Local
   ID: 001, Name: web01, IP: 10.0.0.21, Active
```
Le statut `Active` confirme que la liaison fonctionne. Dans le tableau de bord, le nouvel agent apparaît sous *Agents management*. Pour un poste Windows, la démarche est identique via le programme d’installation MSI, en passant les mêmes paramètres `WAZUH_MANAGER` et `WAZUH_AGENT_NAME`. Répétez l’opération pour chaque machine de votre parc.

## Étape 5 – Surveiller l’intégrité des fichiers (FIM)

La surveillance d’intégrité des fichiers (File Integrity Monitoring) détecte toute création, modification ou suppression dans les répertoires sensibles – un signal fort de compromission ou d’action malveillante. C’est aussi une exigence explicite de nombreux référentiels de conformité (PCI DSS, ISO 27001). Sur l’agent `web01`, éditez `/var/ossec/etc/ossec.conf` et adaptez le bloc `<syscheck>` :

```
<syscheck>
  <disabled>no</disabled>
  <frequency>43200</frequency>
  <directories check_all="yes" realtime="yes">/etc,/usr/bin,/usr/sbin</directories>
  <directories check_all="yes" whodata="yes">/home/admin/.ssh,/var/www</directories>
  <ignore>/etc/mtab</ignore>
  <ignore>/etc/random-seed</ignore>
  <alert_new_files>yes</alert_new_files>
</syscheck>
```
L’attribut `realtime="yes"` déclenche une alerte instantanée grâce à l’API inotify du noyau ; `whodata="yes"` va plus loin en identifiant *quel utilisateur* a modifié le fichier, via le sous-système d’audit Linux. Redémarrez l’agent pour appliquer :

`sudo systemctl restart wazuh-agent`
Testez immédiatement en modifiant un fichier surveillé, par exemple `sudo touch /etc/test-fim.conf`. En quelques secondes, une alerte remonte dans le tableau de bord, module *File Integrity Monitoring*, avec le chemin, l’utilisateur, l’empreinte avant/après et l’horodatage. Attention toutefois : activer le temps réel sur d’énormes arborescences (comme `/var` entier) génère une charge CPU importante – ciblez les répertoires réellement sensibles.

## Étape 6 – Activer la détection de vulnérabilités

Le module de détection de vulnérabilités de Wazuh croise l’inventaire logiciel remonté par chaque agent avec des flux de CVE (bases NVD et catalogues éditeurs). Il signale ainsi les paquets obsolètes ou vulnérables sur l’ensemble du parc, sans installer de scanner tiers. Depuis la refonte du moteur (versions 4.8 et suivantes), le module a continué de gagner en précision : la version 4.11.0 (20 février 2025) a amélioré la détection de vulnérabilités et la fiabilité de l’inventaire logiciel, tandis que la 4.12.0 (7 mai 2025) a ajouté le support des composants centraux sur architecture ARM et migré vers OpenSearch 2.19.1. La configuration se fait côté manager dans `/var/ossec/etc/ossec.conf` :

```
<vulnerability-detection>
  <enabled>yes</enabled>
  <index-status>yes</index-status>
  <feed-update-interval>60m</feed-update-interval>
</vulnerability-detection>
```
Redémarrez le manager avec `sudo systemctl restart wazuh-manager`. Au premier lancement, Wazuh télécharge les flux de vulnérabilités (plusieurs minutes) puis évalue l’inventaire de chaque agent. Les résultats apparaissent dans le module *Vulnerability Detection*, classés par sévérité (Critique, Élevée, Moyenne, Faible) avec le CVE, le paquet concerné et la version corrective recommandée.

C’est un atout majeur pour la conformité NIS 2, qui impose une gestion active des vulnérabilités. Vous obtenez, sans licence supplémentaire, une cartographie continue de votre dette de sécurité, exportable en rapport. Couplez-le au module SCA (étape suivante) pour une vision complète : « quels logiciels sont vulnérables » d’un côté, « quelles configurations sont dangereuses » de l’autre.

## Étape 7 – Évaluer la configuration avec le module SCA (CIS)

Le module SCA (Security Configuration Assessment) audite automatiquement vos systèmes par rapport aux référentiels de durcissement CIS (Center for Internet Security). Il vérifie des centaines de points de contrôle : permissions de fichiers, paramètres SSH, politiques de mot de passe, services inutiles, etc. Wazuh embarque des politiques prêtes à l’emploi pour Ubuntu, Debian, RHEL, Windows Server (avec une politique SCA dédiée à Windows Server 2025 introduite dans la 4.14.2 du 14 janvier 2026) et macOS.

Le module est activé par défaut ; ses résultats sont visibles dans le tableau de bord, module *Security Configuration Assessment*, pour chaque agent. Chaque politique affiche un score de conformité (par exemple « 68 % – 214 réussis, 99 échoués ») et détaille chaque contrôle échoué avec la remédiation exacte. Pour forcer une réévaluation immédiate depuis l’agent :

```
# Redémarre l'agent et relance les scans SCA
sudo systemctl restart wazuh-agent
# Lister les politiques SCA disponibles
ls /var/ossec/ruleset/sca/
```
Traitez en priorité les contrôles « Élevés » et les échecs liés à SSH, aux comptes et aux permissions. Chaque correction améliore mécaniquement votre score et réduit votre surface d’attaque. Un serveur passant de 60 % à 90 % de conformité CIS, c’est concrètement des dizaines de portes d’entrée fermées aux attaquants.

## Comprendre les niveaux de règles et la corrélation Wazuh

Avant de déclencher une détection, il faut saisir comment Wazuh « pense ». Chaque événement remonté par un agent suit une chaîne de traitement : un *décodeur* extrait les champs pertinents du journal brut (IP source, utilisateur, action), puis le moteur de *règles* évalue ces champs. Si une règle correspond, Wazuh lui attribue un **niveau de gravité** et, au-delà d’un seuil, génère une alerte. Comprendre ces niveaux est indispensable pour régler le bruit et prioriser les réponses.

Les règles Wazuh sont classées sur une échelle de 0 à 15. Plus le niveau est élevé, plus l’événement est grave. Voici la grille de lecture généralement retenue par les analystes.

| Niveau | Gravité | Signification | Exemple typique | 
|---|---|---|---|
| 0 à 3 | Faible | Événements informatifs ou autorisés | Connexion réussie, notification système | 
| 4 à 7 | Moyenne | Erreurs et anomalies mineures | Échec d’authentification isolé, erreur applicative | 
| 8 à 11 | Élevée | Activité suspecte, attaque probable | Force brute, modification d’un fichier critique | 
| 12 à 15 | Critique | Compromission probable ou attaque réussie | Rootkit détecté, élévation de privilèges | 

La vraie force de Wazuh réside dans la **corrélation**. Un unique échec de connexion SSH (règle 5716, niveau 5) n’a rien d’alarmant. Mais une règle *composite*, dotée d’attributs `frequency` et `timeframe`, surveille la répétition : si le même IP source déclenche par exemple huit échecs en 120 secondes, Wazuh remonte automatiquement l’événement au niveau 10 avec la règle 5712 « brute force ». C’est ce mécanisme de fenêtre temporelle qui transforme un flot d’événements anodins en une alerte de sécurité exploitable – et c’est exactement ce que nous allons déclencher à l’étape suivante.

Conseil pratique : par défaut, seules les alertes de niveau 3 et plus sont indexées. Ajustez ce seuil (`<alerts><log_alert_level>`) selon votre tolérance au bruit. Trop bas, vous noyez les analystes ; trop haut, vous manquez des signaux faibles. Un réglage à 5 ou 7 constitue souvent un bon compromis pour démarrer, quitte à l’affiner en observant vos propres volumes d’alertes.

## Étape 8 – Détecter une attaque SSH par force brute

Passons au cœur du projet : la détection en temps réel. Wazuh embarque des milliers de règles prêtes à l’emploi, dont un jeu complet pour `sshd`. Par défaut, l’agent surveille déjà `/var/log/auth.log` (Ubuntu/Debian) ou `/var/log/secure` (RHEL). Vérifiez la présence de ce bloc dans l’`ossec.conf` de l’agent :

```
<localfile>
  <log_format>syslog</log_format>
  <location>/var/log/auth.log</location>
</localfile>
```
Pour simuler une attaque, tentez plusieurs connexions SSH échouées depuis une autre machine (par exemple `ssh [email protected]` avec un mauvais mot de passe, répété rapidement). Wazuh corrèle les échecs successifs et déclenche la règle **5712** (« brute force ») dès que le seuil est franchi. Voici une alerte réelle telle qu’elle apparaît dans les journaux d’alertes (`/var/ossec/logs/alerts/alerts.log`) :

```
** Alert 1751709322.148763: - syslog,sshd,authentication_failures,
2026 Jul 05 10:15:22 (web01) 10.0.0.21->/var/log/auth.log
Rule: 5712 (level 10) -> 'sshd: brute force trying to get access to the system.'
Src IP: 203.0.113.45
Src User: root
Jul  5 10:15:22 web01 sshd[20481]: Failed password for root from 203.0.113.45 port 54122 ssh2
```
Décryptage : la règle 5712 est de niveau 10 (élevé), l’IP source est identifiée, l’utilisateur ciblé aussi. Dans le tableau de bord, module *Threat Hunting*, l’alerte est enrichie et interrogeable. Vous venez de *voir* une attaque. Reste à la *stopper* automatiquement.

## Étape 9 – Automatiser la réponse : bannir l’attaquant

La réponse active (Active Response) de Wazuh exécute automatiquement une commande lorsqu’une alerte franchit un seuil. Le script intégré `firewall-drop` ajoute l’IP malveillante aux règles de pare-feu local (iptables/nftables) pour une durée déterminée. Configurons le bannissement automatique sur détection de force brute SSH. Sur le manager, dans `/var/ossec/etc/ossec.conf`, ajoutez :

```
<active-response>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>5710,5712,5763</rules_id>
  <timeout>600</timeout>
</active-response>
```
Ici, dès qu’une des règles 5710, 5712 ou 5763 se déclenche, l’agent bannit l’IP source pendant 600 secondes (10 minutes), puis la débloque automatiquement. Redémarrez le manager avec `sudo systemctl restart wazuh-manager`, puis relancez votre attaque de test. Surveillez le journal de réponse active sur l’agent :

`sudo tail -f /var/ossec/logs/active-responses.log````
2026/07/05 10:15:23 wazuh-execd: Starting firewall-drop
2026/07/05 10:15:23 active-response/bin/firewall-drop: Adding rule for 203.0.113.45
2026/07/05 10:25:23 active-response/bin/firewall-drop: Removing rule for 203.0.113.45
```
Félicitations : vous avez une chaîne détection → réponse entièrement automatisée. L’attaquant est neutralisé en quelques secondes, sans intervention humaine. Pour une défense en profondeur, combinez cette réponse active avec un blocage réseau en amont : notre guide CrowdSec mutualise les listes d’IP malveillantes à l’échelle communautaire, un excellent complément à Wazuh.

## Étape 10 – Cartographier les alertes sur MITRE ATT&CK

Détecter, c’est bien ; comprendre *quelle technique adverse* a été employée, c’est mieux. Wazuh mappe nativement ses règles sur le référentiel MITRE ATT&CK, la base de connaissances mondiale des tactiques et techniques d’attaquants. Notre attaque SSH par force brute, par exemple, est associée à la technique **T1110 (Brute Force)**.

Dans le tableau de bord, le module *MITRE ATT&CK* présente une matrice interactive : chaque colonne est une tactique (Accès initial, Exécution, Persistance, Exfiltration…), et les alertes s’y positionnent en temps réel. Vous visualisez ainsi la *progression* d’un attaquant dans votre système d’information, et non plus une simple liste d’événements isolés. C’est le langage commun des analystes SOC et des rapports d’incident.

Concrètement, cette cartographie transforme Wazuh en outil de *threat hunting* : filtrez par technique, identifiez les tactiques les plus sollicitées contre votre parc, et priorisez vos défenses. Pour un rapport NIS 2 ou une investigation post-incident, pouvoir dire « l’attaquant a utilisé T1110 puis T1078 (Valid Accounts) » apporte une rigueur que les décideurs et les autorités attendent désormais.

## Étape 11 – Sécuriser et fiabiliser votre déploiement Wazuh

Un SIEM est une cible de choix : il concentre les journaux et les secrets de toute l’infrastructure. Le durcir est impératif. Voici les mesures prioritaires après une installation Wazuh.

- **Changer tous les mots de passe par défaut** avec`wazuh-passwords-tool.sh` , y compris ceux des comptes internes de l’indexeur.
- **Remplacer les certificats auto-signés** par des certificats de confiance (Let’s Encrypt ou PKI interne) via l’outil`wazuh-certs-tool.sh` , pour supprimer les avertissements navigateur et chiffrer proprement.
- **Filtrer les ports** 55000, 9200 et 443 au pare-feu : n’exposez jamais l’API ni l’indexeur à Internet. Réservez l’accès au tableau de bord à un VPN.
- **Isoler les flux inter-sites** dans un tunnel chiffré. Pour relier des agents distants au manager, faites transiter le trafic par un VPN WireGuard ou un réseau maillé Tailscale.
- **Centraliser l’authentification** du tableau de bord via SSO. Wazuh s’intègre à un fournisseur d’identité OIDC/SAML comme Keycloak, pour appliquer MFA et politiques d’accès.
- **Planifier les sauvegardes** de`/var/ossec/etc` , des règles personnalisées et des index critiques.

Pensez également à la rétention. L’indexeur peut saturer le disque : mettez en place une politique de gestion du cycle de vie des index (Index State Management) pour supprimer ou archiver automatiquement les données au-delà de votre durée légale de conservation. Nous y revenons dans la section conformité et dans les astuces avancées.

## Étape 12 – Aligner Wazuh sur NIS 2 et le RGPD

Dernière étape, et non des moindres pour une organisation française ou européenne : transformer votre déploiement Wazuh en actif de conformité. La directive NIS 2 impose aux entités essentielles et importantes de détecter les incidents, de conserver des preuves et de notifier l’ANSSI sous 24 heures (alerte précoce) puis 72 heures (notification complète). Wazuh adresse directement ces obligations.

- **Détection et journalisation** : la corrélation en temps réel et l’archivage des événements (`/var/ossec/logs/archives/` ) fournissent la traçabilité exigée.
- **Gestion des vulnérabilités** : le module dédié documente en continu votre exposition, un pilier de NIS 2.
- **Preuves d’incident** : les alertes horodatées, enrichies MITRE et exportables constituent un dossier de notification prêt à l’emploi.
- **Souveraineté et RGPD** : parce que Wazuh est auto-hébergé, les journaux – qui contiennent souvent des données personnelles (IP, identifiants) – restent sur votre infrastructure, en Europe, sous votre seul contrôle. Aucun transfert vers un cloud tiers.

Attention à la contrepartie RGPD : les journaux étant des données personnelles, définissez une durée de conservation proportionnée et documentée (souvent 6 à 12 mois pour les journaux de sécurité), et restreignez leur accès. La politique de rétention des index n’est donc pas qu’une question d’espace disque : c’est aussi une obligation légale. Bien configuré, Wazuh devient la colonne vertébrale de votre conformité, à coût de licence nul. Le cadre européen est détaillé par l’ENISA et l’ANSSI.

## 6 pièges courants à éviter avec Wazuh

La plupart des déploiements Wazuh qui échouent butent sur les mêmes erreurs. Les connaître vous fera gagner des heures.

- **Sous-dimensionner la RAM.** En dessous de 8 Gio, l’indexeur OpenSearch se fait tuer par le noyau (OOM) et le tableau de bord devient inaccessible. C’est l’erreur numéro un.
- **Négliger la synchronisation NTP.** Un décalage d’horloge entre agent et manager fausse l’horodatage, désynchronise les flux et fait apparaître des agents « déconnectés » à tort.
- **Exposer l’interface à Internet.** Publier les ports 443, 55000 ou 9200 sans filtrage transforme votre SIEM en cible. Passez par un VPN.
- **Activer le FIM temps réel partout.** Surveiller`/var` ou`/` en`realtime` sature le CPU. Ciblez uniquement les répertoires sensibles.
- **Oublier la rétention des index.** Sans politique de cycle de vie, l’indexeur remplit le disque en quelques semaines et bloque l’ingestion.
- **Mélanger les versions.** Un agent dont la version est supérieure à celle du manager n’est pas garanti. Mettez toujours le manager à niveau en premier.

## Dépannage : 10 erreurs fréquentes et leurs solutions

Un problème lors de l’installation ou de l’enrôlement ? Ce tableau couvre les symptômes les plus courants rencontrés avec Wazuh et leur résolution.

| Symptôme | Cause probable | Solution | 
|---|---|---|
| Agent en « Never connected » | Ports 1514/1515 filtrés ou mauvaise IP manager | Ouvrir le pare-feu, vérifier `WAZUH_MANAGER` , redémarrer l’agent | 
| Agent « Disconnected » par intermittence | Décalage d’horloge (NTP) | Synchroniser l’heure sur agent et manager | 
| Installateur : « not enough RAM » | Moins de 8 Gio de mémoire | Augmenter la RAM à 8 Gio minimum | 
| « Failed to connect to indexer » (9200) | Service `wazuh-indexer` arrêté ou heap JVM trop petit | Redémarrer l’indexeur, ajuster `jvm.options` | 
| Tableau de bord inaccessible (443) | Service `wazuh-dashboard` down ou certificat invalide | Vérifier le service, régénérer les certificats | 
| Mot de passe admin perdu | Archive d’installation non conservée | Utiliser `wazuh-passwords-tool.sh` | 
| Agent connecté mais aucune alerte | Bloc `localfile` ou`syscheck` absent | Vérifier `ossec.conf` , redémarrer l’agent | 
| Indexeur qui consomme toute la RAM | Heap JVM mal réglé | Fixer le heap à 50 % de la RAM (max 32 Go) | 
| Disque plein en quelques semaines | Aucune politique de rétention d’index | Configurer l’Index State Management (ISM) | 
| Réponse active ne bannit pas l’IP | Règle non déclenchée ou droits iptables manquants | Vérifier `rules_id` et`active-responses.log` | 

En cas de doute, deux fichiers sont vos meilleurs alliés : `/var/ossec/logs/ossec.log` côté manager et agent, et `/var/ossec/logs/active-responses.log` pour la réponse active. La commande `sudo /var/ossec/bin/wazuh-control status` liste l’état de tous les démons.

## Astuces avancées pour aller plus loin avec Wazuh

Une fois les bases maîtrisées, Wazuh révèle une profondeur remarquable. Voici les pistes qui font la différence en environnement de production.

- **Cluster haute disponibilité.** Déployez plusieurs managers en mode cluster (un nœud*master* , plusieurs*workers* ) et répartissez la charge des agents pour tolérer les pannes et absorber les gros volumes.
- **Règles et décodeurs personnalisés.** Créez vos propres règles dans`/var/ossec/etc/rules/local_rules.xml` pour couvrir vos applications métier, et des décodeurs dans`local_decoder.xml` pour parser des formats de log exotiques.
- **Intégrations SOAR et alerting.** Le module*integrator* pousse les alertes vers Slack, un e-mail, TheHive ou Shuffle. Automatisez ainsi la création de tickets et l’orchestration de la réponse.
- **Ingestion des logs réseau.** Envoyez en Syslog (port 514) les journaux de votre pare-feu pfSense ou OPNsense vers Wazuh pour corréler événements réseau et endpoints.
- **API RESTful.** Pilotez Wazuh par programmation via l’API (port 55000) et un jeton JWT. Idéal pour l’automatisation et l’intégration CI/CD.

Exemple d’authentification à l’API et de récupération de la liste des agents actifs :

```
TOKEN=$(curl -u wazuh-wui:VOTRE_MOT_DE_PASSE -k -X POST \
  "https://10.0.0.10:55000/security/user/authenticate?raw=true")
curl -k -X GET "https://10.0.0.10:55000/agents?pretty=true&status=active" \
  -H "Authorization: Bearer $TOKEN"
```
Enfin, gardez votre déploiement à jour. Wazuh publie des correctifs réguliers sur la branche 4.14.x : la 4.14.3 (11 février 2026), la 4.14.4 (17 mars 2026), la 4.14.5 (23 avril 2026) puis la 4.14.6 (1er juillet 2026) – qui a par exemple retiré des options de transport SSL/TLS obsolètes du cluster et mis à jour ses bibliothèques eBPF, avec libbpf en version 1.7.0 et bpftool en version 7.7.0 – ont chacune apporté leur lot de correctifs cumulatifs, confirmant un rythme d’au moins trois publications sur le seul premier semestre 2026, jusqu’à la 4.14.7 du 29 juillet 2026, aujourd’hui la version stable recommandée. Suivez les notes de version officielles et planifiez vos montées de version, en commençant toujours par le manager.

## Foire aux questions sur Wazuh

### Wazuh est-il vraiment gratuit ?

Oui. Tous les composants de Wazuh (manager, indexeur, tableau de bord, agents) sont open source sous licence GPLv2 et totalement gratuits, sans limite d’agents ni de volume de données. Wazuh, Inc. propose par ailleurs une offre cloud gérée payante, optionnelle, pour ceux qui ne veulent pas auto-héberger : d’après le comparateur MSP Compared, son tarif d’entrée démarrait à 571 $/mois pour 100 agents en juin 2026 – un coût que la version auto-hébergée décrite dans ce tutoriel permet d’éviter entièrement.

### Quelle est la dernière version de Wazuh en 2026 ?

La dernière version stable est Wazuh 4.14.7, publiée le 29 juillet 2026. La branche 4.14 a introduit courant 2025-2026 la fonctionnalité IT Hygiene, un tableau de bord Microsoft Graph et le support des politiques SCA pour Windows Server 2025 et macOS 26 ; les utilisateurs de la branche de maintenance 4.10.x reçoivent eux aussi des correctifs réguliers, comme la version 4.10.4 du 21 mai 2026. Une version 5.0 est en développement : sa branche a été fusionnée dans la branche principale le 29 juin 2026, avec un premier bump de version vers l’agent 5.0.1 peu après ; selon un suivi de Tech Insider, elle restait toutefois encore au stade bêta à cette date de juin 2026 (après la préversion 5.0.0-beta2 de mai 2026), une étape clé mais pas encore définitive vers cette prochaine génération de Wazuh.

### Quelle différence entre Wazuh et Splunk ?

Splunk est une plateforme commerciale facturée à l’ingestion de données, très puissante mais coûteuse à grande échelle. Wazuh est open source et gratuit, avec des capacités SIEM et XDR intégrées (FIM, SCA, détection de vulnérabilités, réponse active). Pour un budget contraint ou une exigence de souveraineté, Wazuh est souvent le meilleur rapport valeur/coût.

### Combien d’agents un seul serveur Wazuh peut-il gérer ?

Un déploiement tout-en-un correctement dimensionné (8 vCPU, 8 Gio de RAM, 200 Go de disque) supervise confortablement jusqu’à 100 agents avec 90 jours de rétention. Au-delà, on répartit l’indexeur et le tableau de bord sur des machines dédiées, et l’on passe le manager en cluster pour atteindre plusieurs milliers d’agents – à titre de comparaison, la formule cloud gérée équivalente de l’éditeur facturait, en juin 2026 selon MSP Compared, 923 $/mois pour 250 agents et jusqu’à 1 467 $/mois pour 500 agents, un argument de poids en faveur du cluster auto-hébergé pour les parcs de grande taille.

### Wazuh fonctionne-t-il sur Windows ?

Les composants centraux (manager, indexeur, tableau de bord) nécessitent un serveur Linux 64 bits. En revanche, l’agent Wazuh s’installe sur Windows (via un MSI), macOS, Solaris, AIX et HP-UX, en plus de Linux. Vous pouvez donc surveiller un parc mixte Windows/Linux depuis un unique serveur Wazuh.

### Wazuh aide-t-il à la conformité NIS 2 ?

Oui, directement. Wazuh couvre la détection d’incidents, la journalisation, la gestion des vulnérabilités et la production de preuves horodatées – autant d’exigences de NIS 2. Étant auto-hébergé, il garantit aussi la souveraineté des journaux, un atout pour le RGPD. Il ne remplace pas une démarche de conformité globale mais en constitue un pilier technique majeur.

### Faut-il un agent sur chaque machine ?

L’agent offre la visibilité la plus riche (FIM, SCA, inventaire, réponse active). Toutefois, Wazuh peut aussi ingérer des données sans agent via Syslog ou API, pour les équipements réseau, pare-feu ou services cloud qui n’acceptent pas d’agent. La stratégie idéale combine les deux approches.

### Quelle est la configuration matérielle minimale ?

Pour un serveur tout-en-un supervisant jusqu’à 25 agents : 4 vCPU, 8 Gio de RAM et 50 Go de disque, sous Ubuntu, RHEL ou Amazon Linux 64 bits. L’agent, lui, ne consomme qu’environ 35 Mo de RAM en moyenne, ce qui le rend déployable même sur des systèmes très contraints.

### Couverture associée

- CrowdSec : bloquer les attaques en 12 étapes – le complément idéal de Wazuh pour la mise en liste noire collaborative.
- Directive NIS 2 : 15 000 entités visées – le cadre réglementaire qui rend un SIEM incontournable.
- Tutoriel WireGuard : VPN Linux en 12 étapes – pour chiffrer les flux entre agents distants et manager.
- Tutoriel Keycloak 26 : SSO et OIDC – pour sécuriser l’accès au tableau de bord Wazuh.
- Vaultwarden : auto-héberger Bitwarden – dans la même logique d’outils de sécurité souverains.
- pfSense vs OPNsense 2026 – pour alimenter Wazuh avec les journaux de votre pare-feu.

Explorez l’ensemble de nos guides dans la rubrique Cybersécurité. Avec ce déploiement Wazuh opérationnel – installation, agents, FIM, détection de vulnérabilités, réponse active et conformité NIS 2 – vous disposez désormais d’un SIEM open source de niveau professionnel, souverain et sans coût de licence. Le code source et les dernières évolutions sont disponibles sur le dépôt GitHub officiel de Wazuh.
